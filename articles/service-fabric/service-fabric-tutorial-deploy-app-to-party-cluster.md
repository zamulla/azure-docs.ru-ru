---
title: Развертывание приложения Service Fabric в кластер в Azure | Документы Майкрософт
description: Сведения о развертывании приложения в кластере из Visual Studio.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/28/2018
ms.author: ryanwi,mikhegn
ms.custom: mvc
ms.openlocfilehash: f83ebcce68a7abe53d7b8eaeff5913a907e3df9a
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344195"
---
# <a name="tutorial-deploy-a-service-fabric-application-to-a-cluster-in-azure"></a>Руководство по развертыванию приложения Service Fabric в кластере в Azure

Это руководство из цикла. В нем показано, как развернуть приложение Azure Service Fabric в новый кластер в Azure непосредственно из Visual Studio.

Из этого руководства вы узнали, как выполнять такие задачи:
> [!div class="checklist"]
> * Создание кластера с помощью Visual Studio.
> * Развертывание приложения на удаленный кластер с помощью Visual Studio

Из этого цикла руководств вы узнаете, как выполнять такие задачи:
> [!div class="checklist"]
> * [Создание приложения .NET Service Fabric](service-fabric-tutorial-create-dotnet-app.md).
> * Развертывание приложения в удаленном кластере.
> * [Добавление конечной точки HTTPS к интерфейсной службе ASP.NET Core](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
> * [Настройка непрерывной интеграции и непрерывного развертывания с помощью Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md).
> * [Настройка мониторинга и диагностики приложения](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Предварительные требования

Перед началом работы с этим руководством выполните следующие действия:

* Если у вас еще нет подписки Azure, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Установите Visual Studio 2017](https://www.visualstudio.com/), а также рабочие нагрузки **разработка Azure** и **ASP.NET и веб-разработка**.
* [Установите пакет SDK для Service Fabric](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a>Скачивание примера приложения для голосования

Если вы не создавали пример приложения для голосования [в первой части этой серии руководств](service-fabric-tutorial-create-dotnet-app.md), вы можете скачать его. В окне терминала выполните следующую команду, чтобы клонировать репозиторий с примером приложения на локальный компьютер.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="create-a-service-fabric-cluster"></a>Создание кластера Service Fabric

Теперь, когда приложение готово, можно развернуть его в кластер напрямую из Visual Studio. [Кластер Service Fabric](/service-fabric/service-fabric-deploy-anywhere.md) — это подключенный к сети набор виртуальных машин или физических компьютеров, в котором выполняется развертывание микрослужб и управление ими.

В Visual Studio доступны два варианта развертывания:

* Создание кластера в Azure с помощью Visual Studio. Этот вариант позволяет создать безопасный кластер непосредственно из Visual Studio с нужными конфигурациями. Этот тип кластера идеально подходит для тестовых сценариев, где вы можете создать кластер, а затем опубликовать его непосредственно в Visual Studio.
* Выполните публикацию в имеющийся кластер в своей подписке.  Кластеры Service Fabric можно создавать с помощью [портала Azure](https://portal.azure.com), скриптов [PowerShell](./scripts/service-fabric-powershell-create-secure-cluster-cert.md) или [Azure CLI](./scripts/cli-create-cluster.md) либо [шаблона Azure Resource Manager](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

В этом руководстве объясняется, как создать кластер с помощью Visual Studio. Если кластер уже развернут, вы можете скопировать и вставить конечную точку подключения или выбрать ее из своей подписки.
> [!NOTE]
> Многие службы используют обратный прокси-сервер для взаимодействия между собой. У кластеров, созданных из Visual Studio, и сторонних кластеров есть обратный прокси-сервер, включенный по умолчанию.  При использовании имеющегося кластера [в нем необходимо включить обратный прокси-сервер](service-fabric-reverseproxy.md#setup-and-configuration).

### <a name="find-the-votingweb-service-endpoint"></a>Поиск конечной точки службы VotingWeb

Сначала найдите конечную точку интерфейсной веб-службы.  Интерфейсная веб-служба ожидает передачи данных через определенный порт.  Если приложение развертывается в кластере в Azure, то приложение и кластер запускаются под контролем Azure Load Balancer.  Чтобы входящий трафик поступал в веб-службу, необходимо открыть порт приложения в Azure Load Balancer.  Порт (например, 8080) указан в файле *VotingWeb/PackageRoot/ServiceManifest.xml* в элементе **Endpoint**:

```xml
<Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
```

На следующем шаге укажите этот порт на вкладке **Дополнительно** диалогового окна **Создание кластера**.  Если вы развертываете приложение в существующем кластере, этот порт можно открыть в Azure Load Balancer с помощью [скрипта PowerShell](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) или на [портале Azure](https://portal.azure.com).

### <a name="create-a-cluster-in-azure-through-visual-studio"></a>Создание кластера в Azure с помощью Visual Studio

Щелкните правой кнопкой мыши проект приложения в обозревателе решений, а затем выберите **Опубликовать**.

Войдите в систему, используя свою учетную запись Azure, чтобы получить доступ к своим подпискам. Если вы используете сторонний кластер, этот шаг является необязательным.

Щелкните раскрывающийся список для **конечной точки подключения** и выберите параметр **<Create New Cluster...>**.

![Диалоговое окно "Публикация"](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

В диалоговом окне **Создание кластера** измените следующие параметры:

1. Укажите имя кластера в поле **Имя кластера**, а также подписку и расположение, которые нужно использовать.
2. Необязательно. Можно изменить количество узлов. По умолчанию у вас есть три узла — минимальное количество, необходимое для тестирования сценариев Service Fabric.
3. Выберите вкладку **Сертификат**. На этой вкладке введите пароль, используемый для защиты сертификата кластера. С помощью этого сертификата вы защитите кластер. Также можно изменить путь, используемый для сохранения сертификата. Visual Studio также может импортировать сертификат автоматически, так как это необходимый шаг для публикации приложения в кластер.
4. Перейдите на вкладку **Сведения о виртуальной машине**. Укажите пароль, который вы хотите использовать для виртуальных машин, входящих в состав кластера. Имя пользователя и пароль можно использовать для удаленного подключения к виртуальным машинам. Также необходимо выбрать размер виртуальной машины и при необходимости изменить ее образ.
5. На вкладке **Дополнительно** можно изменить список портов, которые нужно открыть в подсистеме Azure Load Balancer, создаваемой вместе с кластером.  Добавьте конечную точку службы VotingWeb, найденную на предыдущем шаге. Вы также можете добавить существующий ключ Application Insights для направления файлов журналов приложения.
6. После внесения всех необходимых изменений в параметры нажмите кнопку **Создать**. Процесс создания займет несколько минут. Когда кластер будет полностью создан, в окне вывода отобразится соответствующее уведомление.

![Диалоговое окно создания кластера](./media/service-fabric-tutorial-deploy-app-to-party-cluster/create-cluster.png)

## <a name="deploy-the-sample-application"></a>Развертывание примера приложения

После подготовки нужного кластера щелкните правой кнопкой мыши проект приложения и выберите **Опубликовать**.

После завершения публикации вы сможете отправить запрос к приложению через браузер.

Откройте любой браузер и введите адрес кластера (конечную точку подключения без указания порта — например, win1kw5649s.westus.cloudapp.azure.com).

Вы должны увидеть такой же результат, как и при локальном запуске приложения.

![Ответ API из кластера](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как выполнить следующие задачи:

> [!div class="checklist"]
> * Создание кластера с помощью Visual Studio.
> * Развертывание приложения на удаленный кластер с помощью Visual Studio

Перейдите к следующему руководству:
> [!div class="nextstepaction"]
> [Включение HTTPS](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
