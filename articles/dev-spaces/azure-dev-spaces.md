---
title: Azure Dev Spaces | Документация Майкрософт
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 06/01/2018
ms.topic: tutorial
description: Быстрая разработка в Kubernetes с использованием контейнеров и микрослужб в Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
manager: douge
ms.openlocfilehash: 93440b8a1c9fd1b386931e5998c70133071a079e
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823050"
---
# <a name="azure-dev-spaces"></a>Рабочие среды Azure для разработчиков
Рабочие среды Azure Dev Spaces предоставляют быстрый итеративный интерфейс разработки Kubernetes для команд. С минимальной настройкой компьютера разработчика можно итеративно запускать и проводить отладку контейнера непосредственно из службы Azure Kubernetes (AKS). Разрабатывайте приложения для Windows, Mac или Linux с помощью таких привычных средств, как Visual Studio, Visual Studio Code или из командной строки.

[!INCLUDE[](includes/dev-spaces-preview.md)]

## <a name="how-azure-dev-spaces-simplifies-kubernetes-development"></a>Как служба Azure Dev Spaces упрощает разработку Kubernetes 

Рабочая среда Azure Dev Spaces помогает командам разработчиков стать более продуктивными при использовании Kubernetes следующим образом.
- Минимизирует настройку локальной машины для каждого члена команды и работает непосредственно в AKS (управляемом кластере Kubernetes в Azure).
- Быстро выполняет итерацию и отладку кода непосредственно в Kubernetes с помощью Visual Studio 2017 или Visual Studio Code.
- Создает конфигурационные файлы Docker и Kubernetes, которые можно использовать от разработки до производства. 
- Поделится управляемым кластером Kubernetes с командой для совместной работы. Изолированное написание кода и комплексное тестирование вместе с другими компонентами без репликации или имитации зависимостей.

[!INCLUDE[](includes/get-started.md)]

![](media/azure-dev-spaces/vscode-overview.png)

## <a name="see-also"></a>См. также

[Служба Azure Kubernetes (AKS)](/azure/aks)