---
title: Отображение данных дорожного движения с помощью службы "Карты Azure" | Документация Майкрософт
description: Как отобразить данные дорожного движения на карте Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 6ff7a0270509c244fc97bd04d8ba648fd262dc58
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34600125"
---
# <a name="show-traffic-on-the-map"></a>Отображение данных дорожного движения на карте

В этой статье показано, как отобразить на карте данные дорожного движения и сведения о проблемах на дороге. 

## <a name="understand-the-code"></a>Изучение кода

<iframe height='456' scrolling='no' title='Отображение данных дорожного движения на карте' src='//codepen.io/azuremaps/embed/WMLRPw/?height=456&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Просмотрите фрагмент кода для <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>отображения данных дорожного движения на карте</a> службы "Карты Azure" (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) в <a href='https://codepen.io'>CodePen</a>.
</iframe>

В первом блоке приведенного выше кода создается объект карты. См. инструкции по [созданию карты](map-create.md).

Во втором блоке кода используется функция класса карты [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#settraffic). Она позволяет отобразить на карте данные дорожного движения и сведения о проблемах на дороге.

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о классах и методах, которые используются в этой статье: 
* класс [Map](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest);
    * метод [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#settraffic).

Дополнительные примеры кода для добавления в карты см. в следующих статьях: 
* [Взаимодействие с картой — события мыши](./map-events.md)
* [Создание карты](./map-accessibility.md)

Чтобы узнать о других сценариях работы с картами, перейдите на эту [страницу с примерами кода](http://aka.ms/AzureMapsSamples).
