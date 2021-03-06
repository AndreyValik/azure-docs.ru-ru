---
title: Создание пользовательского слова для активации
description: Создание пользовательских слов для активации с помощью пакета SDK для речевых устройств.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 615a901c70fff92141442699ea6e4b8fce1c9ace
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282579"
---
# <a name="create-a-custom-wake-word-using-speech-service"></a>Создание пользовательских слов для активации с помощью службы "Речь"

Устройство всегда ожидает на звуковом входе слово (или фразу) для активации. Например, фраза "Hey Cortana" (Эй, Кортана) используется для активации помощника Cortana. Когда пользователь произносит слово для активации, устройство передает в облако всю следующую за этим словом законченную фразу. Настройка слова для активации — это эффективный способ выделить устройство среди конкурентов и укрепить позиции торговой марки.

Из этой статьи вы узнаете, как создать собственное слово для активации вашего устройства.

## <a name="choosing-an-effective-wake-word"></a>Выбор эффективного слова для активации

При выборе слова для активации придерживайтесь следующих рекомендаций:

* Это должно быть слово или короткая фраза на английском языке. Произнесение этого слова не должно занимать более двух секунд.

* Лучше всего подходят слова с количеством слогов от 4 до 7. Например, фраза "Hey, Computer" (Эй, компьютер) хорошо подходит для активации, а просто "Hey" (Эй) — плохо.

* Слова для активации должны соответствовать стандартным правилам произношения для английского языка.

* Очень редкие или даже вымышленные слова с традиционно английским произношением помогут сократить количество ложных срабатываний. Например, хорошим словом для активации может оказаться "computerama".

* Не выбирайте распространенные слова. Например, слова "eat" и "go" пользователи часто произносят в обычном разговоре. Такие слова будут часто вызывать ложную активацию устройства.

* Старайтесь не применять слово для активации, которое имеет несколько вариантов произношения. Пользователям придется выяснять "правильный" вариант произношения, чтобы добиться ответа от устройства. Например, число 509 на английском может произноситься как "five zero nine", "five oh nine" или "five hundred and nine". Аббревиатуру R.E.I. можно прочитать по буквам R E I (ар, и, ай) или как слово Ray (рэй). Live имеет варианты произношения [līv] и [liv].

* Не используйте специальные символы и (или) цифры. Например "Go#" или "20 + cats" не подойдут для активации. Но эти же варианты в форме "go sharp" или "twenty plus cats" будут работать гораздо лучше. Такой подход позволит вам сохранить эти символы в фирменной символике, дополнив маркетинговые материалы и документацию напоминаниями о правильном произношении.

> [!NOTE]
> Если в качестве слова для активации вы выберете зарегистрированный товарный знак, вы должны являться владельцем этого товарного знака или получить у настоящего владельца разрешение на его использование. Корпорация Майкрософт не несет юридической ответственности за проблемы, возникающие из-за выбора слова для активации.

## <a name="creating-your-wake-word"></a>Создание слова для активации

Прежде чем применить пользовательское слово для активации устройства, нужно создать это слово с помощью службы от Майкрософт для создания пользовательских слов для активации. Вы передаете в эту службу слово для активации, а служба создает для вас специальный файл, который вам нужно развернуть в пакете средств разработки, чтобы применить к устройству выбранное слово для активации.

1. Откройте [портал Пользовательской службы распознавания речи](https://cris.ai/).

2. Создайте учетную запись с адресом электронной почты, на который вы получили приглашение для регистрации в Azure Active Directory. 

    ![Создание учетной записи](media/speech-devices-sdk/wake-word-1.png)
 
3.  После входа заполните предложенную форму и щелкните **Start the journey** (Начать работу).

    ![Экран, отображаемый после успешного входа](media/speech-devices-sdk/wake-word-3.png)
 
4. Страница **пользовательских слов для активации** недоступна обычным посетителям, и вы не можете попасть на нее по ссылкам на портале. Вместо этого щелкните ссылку https://cris.ai/customkws или скопируйте и вставьте ее.

    ![Скрытая страница](media/speech-devices-sdk/wake-word-4.png)
 
6. Введите выбранное слово для активации и щелкните **Submit** (Отправить).

    ![Ввод выбранного слова для активации](media/speech-devices-sdk/wake-word-5.png)
 
7. Создание нужных файлов может занять несколько минут. На вкладке браузера в это время будет отображаться вращающийся круг. Через некоторое время появится информационная панель с предложением скачать файл `.zip`.

    ![Получение ZIP-файла](media/speech-devices-sdk/wake-word-6.png)

8. Сохраните этот файл `.zip` на локальный компьютер. Он потребуется вам, чтобы развернуть пользовательское слово для активации в пакете средств разработки. Инструкции по этому процессу см. в статье [Начало работы с пакетом SDK для речевых устройств](speech-devices-sdk-qsg.md).

9. После этого можно **выйти из системы**.

## <a name="next-steps"></a>Дополнительная информация

Для начала работы получите [бесплатную учетную запись Azure](https://azure.microsoft.com/free/) и зарегистрируйтесь для получения пакета SDK для речевых устройств.

> [!div class="nextstepaction"]
> [Регистрация для получения пакета SDK для речевых устройств](get-speech-devices-sdk.md)

