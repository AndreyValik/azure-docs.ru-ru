---
title: 'Маршрутизация сетевого трафика: Azure CLI | Документация Майкрософт'
description: Из этой статьи вы узнаете, как маршрутизировать трафик с помощью таблицы маршрутов и Azure CLI.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 3217524ccfd03184a93a4e6637696195b3f0bf16
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2018
ms.locfileid: "42146661"
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-cli"></a>Маршрутизация сетевого трафика с помощью таблицы маршрутов и Azure CLI

По умолчанию Azure автоматически маршрутизирует трафик между всеми подсетями в виртуальной сети. Для переопределения маршрутизации Azure по умолчанию можно создать собственные маршруты. Возможность создания настраиваемых маршрутов полезна, если, к примеру, требуется маршрутизировать трафик между подсетями через виртуальный сетевой модуль. В этой статье раскрываются следующие темы:

* Создание таблицы маршрутов
* Создание маршрута
* Создание виртуальной сети с несколькими подсетями
* Связывание таблицы маршрутов с подсетью
* создание виртуального сетевого модуля, который перенаправляет трафик;
* развертывание виртуальных машин в разных подсетях;
* направление трафика из одной подсети в другую через виртуальный сетевой модуль.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Если вы решили установить и использовать CLI локально, для выполнения инструкций из этого руководства вам потребуется Azure CLI 2.0.28 или более поздней версии. Чтобы узнать версию, выполните команду `az --version`. Если вам необходимо выполнить установку или обновление, см. статью [Установка Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="create-a-route-table"></a>Создание таблицы маршрутов

Перед созданием таблицы маршрутов создайте группу ресурсов с помощью команды [az group create](/cli/azure/group#az_group_create) для всех ресурсов, созданных в рамках этой статьи. 

```azurecli-interactive
# Create a resource group.
az group create \
  --name myResourceGroup \
  --location eastus
``` 

Создайте таблицу маршрутов с помощью команды [az network route-table create](/cli/azure/network/route-table#az-network-route-table-create). В следующем примере создается таблица маршрутов с именем *myRouteTablePublic*. 

```azurecli-interactive 
# Create a route table
az network route-table create \
  --resource-group myResourceGroup \
  --name myRouteTablePublic
```

## <a name="create-a-route"></a>Создание маршрута

Создайте маршрут в таблице маршрутов с помощью команды [az network route-table route create](/cli/azure/network/route-table/route#az-network-route-table-route-create). 

```azurecli-interactive
az network route-table route create \
  --name ToPrivateSubnet \
  --resource-group myResourceGroup \
  --route-table-name myRouteTablePublic \
  --address-prefix 10.0.1.0/24 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address 10.0.2.4
``` 

## <a name="associate-a-route-table-to-a-subnet"></a>Связывание таблицы маршрутов с подсетью

Чтобы связать таблицу маршрутов с подсетью, необходимо создать виртуальную сеть и подсеть. Создайте виртуальную сеть с одной подсетью при помощи команды [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create).

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefix 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

Создайте две дополнительные подсети с помощью команды [az network vnet subnet create](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create).

```azurecli-interactive
# Create a private subnet.
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24

# Create a DMZ subnet.
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name DMZ \
  --address-prefix 10.0.2.0/24
```

Свяжите таблицу маршрутов *myRouteTablePublic* с подсетью *Public* при помощи команды [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update).

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Public \
  --resource-group myResourceGroup \
  --route-table myRouteTablePublic
```

## <a name="create-an-nva"></a>Создание виртуального сетевого модуля

Виртуальный сетевой модуль представляет собой виртуальную машину, которая выполняет сетевые функции, такие как маршрутизация, защита брандмауэром или оптимизация доступа к глобальной сети.

Создайте виртуальный сетевой модуль в подсети *DMZ* с помощью команды [az vm create](/cli/azure/vm#az_vm_create). По умолчанию при создании виртуальной машины Azure создает и назначает ей общедоступный IP-адрес. Параметр `--public-ip-address ""` указывает Azure, что для этой виртуальной машины не нужно создавать и назначать общедоступный IP-адрес, так как ей не требуется подключение из Интернета. Также команда создает ключи SSH, если они не существуют в расположении ключей по умолчанию. Чтобы использовать определенный набор ключей, используйте параметр `--ssh-key-value`.

```azure-cli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmNva \
  --image UbuntuLTS \
  --public-ip-address "" \
  --subnet DMZ \
  --vnet-name myVirtualNetwork \
  --generate-ssh-keys
```

Создание виртуальной машины занимает несколько минут. Переходите к следующему шагу только после того, как Azure завершит создание виртуальной машины и вернет выходные данные о ней. 

В сетевом интерфейсе должна быть включена IP-пересылка, чтобы он мог перенаправлять отправленный ему сетевой трафик, который не предназначен для его собственного IP-адреса. Включите IP-пересылку для сетевого интерфейса с помощью команды [az network nic update](/cli/azure/network/nic#az_network_nic_update).

```azurecli-interactive
az network nic update \
  --name myVmNvaVMNic \
  --resource-group myResourceGroup \
  --ip-forwarding true
```

Операционная система или приложение, работающие на виртуальной машине, также должны уметь перенаправлять сетевой трафик. Чтобы включить IP-пересылку в операционной системе виртуальной машины, используйте [az vm extension set](/cli/azure/vm/extension#az_vm_extension_set):

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVmNva \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
```
Выполнение команды может занять до минуты.

## <a name="create-virtual-machines"></a>Создание виртуальных машин

Создайте в виртуальной сети две виртуальные машины, чтобы на следующем шаге с их помощью проверить, что трафик из подсети *Public* направляется в подсеть *Private* через виртуальный сетевой модуль. 

Создайте виртуальную машину в подсети *Public* с помощью команды [az vm create](/cli/azure/vm#az_vm_create). Параметр `--no-wait` позволяет Azure выполнять команду в фоновом режиме, поэтому вы можете перейти к следующей команде. Для упрощения работы с этой статьей используется пароль. Ключи обычно используются в рабочей среде. Если вы используете ключи, необходимо также настроить перенаправление агента SSH. Дополнительные сведения см. в документации по своему клиенту SSH. В следующей команде замените значение `<replace-with-your-password>` паролем по своему выбору.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmPublic \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --admin-username azureuser \
  --admin-password $adminPassword \
  --no-wait
```

Создайте виртуальную машину в подсети *Private*.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPrivate \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --admin-username azureuser \
  --admin-password $adminPassword
```

Создание виртуальной машины занимает несколько минут. После создания виртуальной машины в Azure CLI отображаются примерно такие данные: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmPrivate",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.1.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```
Запишите значение **publicIpAddress**. На следующем шаге этот адрес потребуется для доступа к виртуальной машине из Интернета.

## <a name="route-traffic-through-an-nva"></a>Перенаправление трафика через виртуальный сетевой модуль

Используйте следующую команду для создания сеанса SSH с виртуальной машиной *myVmPrivate*. Замените *<publicIpAddress>* общедоступным IP-адресом, который присвоен виртуальной машине. В примере выше используется IP-адрес *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

При запросе пароля введите пароль, выбранный при работе с разделом [Создание виртуальных машин](#create-virtual-machines).

С помощью следующей команды установите средство трассировки маршрута на виртуальную машину *myVmPrivate*:

```bash 
sudo apt-get install traceroute
```

Используя следующую команду, протестируйте маршрутизацию сетевого трафика к виртуальной машине *myVmPublic* от виртуальной машины *myVmPrivate*.

```bash
traceroute myVmPublic
```

Ответ будет выглядеть примерно так:

```bash
traceroute to myVmPublic (10.0.0.4), 30 hops max, 60 byte packets
1  10.0.0.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```

Вы увидите, что трафик направляется непосредственно от виртуальной машины *myVmPrivate* на виртуальную машину *myVmPublic*. Маршруты Azure по умолчанию выполняют маршрутизацию трафика напрямую между подсетями. 

Используя следующую команду, создайте SSH-подключение к виртуальной машине *myVmPublic* с виртуальной машины *myVmPrivate*.

```bash 
ssh azureuser@myVmPublic
```

С помощью следующей команды установите средство трассировки маршрута на виртуальную машину *myVmPublic*:

```bash 
sudo apt-get install traceroute
```

Используя следующую команду, протестируйте маршрутизацию сетевого трафика к виртуальной машине *myVmPrivate* от виртуальной машины *myVmPublic*.

```bash
traceroute myVmPrivate
```

Ответ будет выглядеть примерно так:

```bash
traceroute to myVmPrivate (10.0.1.4), 30 hops max, 60 byte packets
1  10.0.2.4 (10.0.2.4)  0.781 ms  0.780 ms  0.775 ms
2  10.0.1.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```
Можно заметить, что первым прыжком указан частный IP-адрес виртуального сетевого модуля (10.0.2.4). Второй прыжок — 10.0.1.4. Это частный IP-адрес виртуальной машины *myVmPrivate*. Из-за маршрута, добавленного в таблицу маршрутов *myRouteTablePublic* и связанного с подсетью *Public*, трафик в Azure был маршрутизирован через NVA, а не напрямую в подсеть *Private*.

Закройте сеансы SSH к виртуальным машинам *myVmPublic* и *myVmPrivate*.

## <a name="clean-up-resources"></a>Очистка ресурсов

Вы можете удалить ненужную группу ресурсов и все содержащиеся в ней ресурсы, выполнив команду [az group delete](/cli/azure/group#az_group_delete).

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Дополнительная информация

При работе с этой статьей вы создали таблицу маршрутов и связали ее с подсетью. Вы также создали простой виртуальный сетевой модуль, который направляет трафик из общедоступной подсети в частную подсеть. [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking) предоставляет широкий выбор предварительно настроенных виртуальных сетевых модулей для разных сетевых функций, например модули брандмауэра и оптимизация доступа к глобальной сети. См. дополнительные сведения о [маршрутизации](virtual-networks-udr-overview.md) и [управлении таблицей маршрутов](manage-route-table.md).

Хотя в виртуальной сети можно развернуть множество ресурсов Azure, ресурсы для некоторых служб PaaS Azure развернуть невозможно. Тем не менее вы все же можете ограничить доступ к ресурсам некоторых служб PaaS в Azure трафиком только из подсети виртуальной сети. Сведения о том, как это сделать, см. в статье [Ограничение сетевого доступа к ресурсам PaaS посредством конечных точек службы виртуальной сети с помощью Azure CLI](tutorial-restrict-network-access-to-resources-cli.md).
