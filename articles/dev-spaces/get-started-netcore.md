---
title: Создание пространства разработки Kubernetes в облаке с помощью .NET Core и VS Code | Документация Майкрософт
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 07/09/2018
ms.topic: tutorial
description: Быстрая разработка в Kubernetes с использованием контейнеров и микрослужб в Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
manager: douge
ms.openlocfilehash: 4da5b42ddd235fa26834e582a911140116692d34
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38299565"
---
# <a name="get-started-on-azure-dev-spaces-with-net-core"></a>Начало работы в Azure Dev Spaces с .NET Core

[!INCLUDE[](includes/learning-objectives.md)]

[!INCLUDE[](includes/see-troubleshooting.md)]

Теперь вы готовы создать среду разработки на основе Kubernetes в Azure.

[!INCLUDE[](includes/portal-aks-cluster.md)]

## <a name="install-the-azure-cli"></a>Установка Azure CLI
Для Azure Dev Spaces требуется минимальная настройка локального компьютера. Большая часть конфигурации среды разработки хранится в облаке и доступна для других пользователей. Начните со скачивания и запуска [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest). 

> [!IMPORTANT]
> Если интерфейс Azure CLI уже установлен, убедитесь, что используется версия 2.0.38 или выше.

[!INCLUDE[](includes/sign-into-azure.md)]

[!INCLUDE[](includes/use-dev-spaces.md)]

[!INCLUDE[](includes/install-vscode-extension.md)]

Пока вы ждете создания кластера, вы можете начать писать код.

## <a name="create-a-web-app-running-in-a-container"></a>Создание веб-приложения для работы в контейнере

В этом разделе вы создадите веб-приложение ASP.NET Core и запустите его в контейнере в Kubernetes.

### <a name="create-an-aspnet-core-web-app"></a>Создание веб-приложения ASP.NET Core
Если у вас установлен [.NET Core](https://www.microsoft.com/net), вы можете быстро создать веб-приложение ASP.NET Core в папке с именем `webfrontend`.
    
```cmd
dotnet new mvc --name webfrontend
```

Можно также **скачать пример кода из GitHub**, перейдя по ссылке https://github.com/Azure/dev-spaces и выбрав **Clone or Download** (Клонировать или скачать), чтобы скачать репозиторий GitHub в локальную среду. Код для этого руководства находится в папке `samples/dotnetcore/getting-started/webfrontend`.

[!INCLUDE[](includes/azds-prep.md)]

[!INCLUDE[](includes/build-run-k8s-cli.md)]

### <a name="update-a-content-file"></a>Обновление файла содержимого
Azure Dev Spaces — это не просто среда выполнения кода в Kubernetes. Она позволяет быстро и итеративно видеть, как изменения вашего кода вступают в силу в среде Kubernetes в облаке.

1. Найдите файл `./Views/Home/Index.cshtml` и внесите изменения в HTML. Например, измените строку 70 `<h2>Application uses</h2>` строкой примерно такого содержания: `<h2>Hello k8s in Azure!</h2>`
1. Сохраните файл. Через несколько мгновений в окне терминала вы увидите сообщение о том, что файл в запущенном контейнере обновлен.
1. Вернитесь в браузер и обновите страницу. Должна отобразиться веб-страница с обновленным HTML.

Что произошло? Изменения файлов содержимого, таких как HTML и CSS, не требуют перекомпиляции в веб-приложении .NET Core, поэтому активная команда `azds up` автоматически синхронизирует любые измененные файлы содержимого в запущенный контейнер в Azure, чтобы можно было сразу же видеть изменения содержимого.

### <a name="update-a-code-file"></a>Обновление файла кода
Для обновления файлов кода требуется немного больше работы, так как приложение .NET Core должно перестроить и создать обновленные двоичные файлы приложений.

1. В окне терминала нажмите клавишу `Ctrl+C` (чтобы остановить `azds up`).
1. Откройте файл кода с именем `Controllers/HomeController.cs` и измените сообщение, которое будет отображаться на странице About (Сведения): `ViewData["Message"] = "Your application description page.";`
1. Сохраните файл.
1. Запустите `azds up` в окне терминала. 

Эта команда перестроит образ контейнера и повторно развернет диаграмму Helm. Чтобы увидеть, как изменения вашего кода вступили в силу в работающем приложении, перейдите в меню "Сведения" в веб-приложении.


Но есть еще один *более быстрый метод* разработки кода, который вы рассмотрите в следующем разделе. 

## <a name="debug-a-container-in-kubernetes"></a>Отладка контейнера в Kubernetes

[!INCLUDE[](includes/debug-intro.md)]

[!INCLUDE[](includes/init-debug-assets-vscode.md)]


### <a name="select-the-azds-debug-configuration"></a>Выбор конфигурации отладки AZDS
1. Чтобы открыть представление отладки, щелкните значок "Отладка" на **панели действия** сбоку VS Code.
1. Выберите **.NET Core Launch (AZDS)** (Запуск .NET Core (AZDS)) как активную конфигурацию отладки.

![](media/get-started-netcore/debug-configuration.png)

> [!Note]
> Если вы не видите никаких команд Azure Dev Spaces на палитре команд, убедитесь, что вы установили расширение VS Code для Azure Dev Spaces. Убедитесь, что рабочая область, которую вы открыли в VS Code, — это папка, содержащая файл azds.yaml.


### <a name="debug-the-container-in-kubernetes"></a>Отладка контейнера в Kubernetes
Нажмите клавишу **F5**, чтобы отладить свой код в Kubernetes.

Как и команда `up`, код синхронизируется со средой разработки, а контейнер создается и развертывается в Kubernetes. На этот раз, конечно, отладчик подключен к удаленному контейнеру.

[!INCLUDE[](includes/tip-vscode-status-bar-url.md)]

Установите точку останова в файле кода на стороне сервера, например в функции `Index()` в исходном файле `Controllers/HomeController.cs`. Обновление страницы браузера инициирует срабатывание точки останова.

У вас есть полный доступ к отладочной информации, как если бы код выполнялся локально, например к стеку вызовов, локальным переменным, информации об исключениях и т. д.

### <a name="edit-code-and-refresh"></a>Изменение и обновление кода
С помощью активного отладчика отредактируйте код. Например, измените сообщение страницы About (Сведения) в `Controllers/HomeController.cs`. 

```csharp
public IActionResult About()
{
    ViewData["Message"] = "My custom message in the About page.";
    return View();
}
```

Сохраните файл и в области **действий отладки** нажмите кнопку **Обновить**. 

![](media/get-started-netcore/debug-action-refresh.png)

Вместо того, чтобы перестраивать и повторно развертывать новый образ контейнера при каждой правке кода, что часто занимает много времени, Azure Dev Spaces пошагово перекомпилирует код в существующем контейнере, чтобы ускорить цикл редактирования и отладки.

В браузере обновите веб-приложение и перейдите на страницу About (Сведения). Вы должны увидеть настраиваемое сообщение в пользовательском интерфейсе.

**Теперь у вас есть метод быстрой итерации кода и отладки непосредственно в Kubernetes**. Далее вы узнаете, как создать и вызвать второй контейнер.

## <a name="next-steps"></a>Дополнительная информация

> [!div class="nextstepaction"]
> [См. дополнительные сведения о коллективной разработке](team-development-netcore.md)