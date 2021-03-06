---
title: Повышение производительности за счет сжатия файлов в Azure CDN | Документация Майкрософт
description: В этом разделе описано, как ускорить передачу файла и повысить производительность загрузки страниц с помощью сжатия файлов в CDN Azure.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: v-deasim
ms.openlocfilehash: bdff57275cf123079004ada732fe782d98399d71
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260402"
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a>Повышение производительности за счет сжатия файлов в Azure CDN
Сжатие файлов — это простой и эффективный способ, который позволяет повысить скорость передачи файлов и увеличить производительность загрузки страницы за счет уменьшения размера файлов перед их отправкой с сервера. Этот способ позволяет снизить потребление пропускной способности и обеспечивает более высокую скорость работы для пользователей.

Сжатие файлов можно активировать двумя способами.

- Включить сжатие на сервере-источнике. В этом случае сеть доставки содержимого Azure проходит по сжатым файлам и доставляет их клиентам, которые запросили эти файлы.
- Включить сжатие непосредственно на серверах POP сети CDN (*сжатие в режиме реального времени*). В таком случае CDN сжимает файлы и доставляет их пользователям, даже если эти файлы не были сжаты на сервере-источнике.

> [!IMPORTANT]
> Применение изменений конфигурации Azure CDN по сети может занять некоторое время. 
- Для профилей **Azure CDN категории "Стандартный" от Майкрософт** распространение обычно выполняется в течение 10 минут. 
- Для профилей **Azure CDN уровня "Стандартный" от Akamai** распространение обычно завершается в течение одной минуты. 
- Для профилей **Azure CDN уровня "Стандартный" от Verizon** и **Azure CDN уровня "Премиум" от Verizon** распространение обычно выполняется в течение 10 минут. 
>
> Если сжатие для конечной точки CDN задается впервые, перед устранением неполадок следует подождать 1–2 часа, чтобы настройки сжатия гарантированно распространились на серверы POP.
> 
> 

## <a name="enabling-compression"></a>Включение сжатия
Уровни CDN "Стандартный" и "Премиум" предоставляют одинаковые возможности сжатия, но разные пользовательские интерфейсы. Дополнительные сведения о различиях между уровнями CDN "Стандартный" и "Премиум" см. в разделе [Обзор Azure CDN](cdn-overview.md).

### <a name="standard-cdn-profiles"></a>Профили CDN уровня "Стандартный" 
> [!NOTE]
> Сведения в этом разделе относятся к профилям **Azure CDN уровня "Стандартный" от Майкрософт**, **Azure CDN уровня "Стандартный" от Verizon** и **Azure CDN уровня "Стандартный" от Akamai**.
> 
> 

1. На странице профиля CDN выберите конечную точку CDN, которой вы хотите управлять.
   
    ![Конечные точки профиля CDN](./media/cdn-file-compression/cdn-endpoints.png)
   
    Откроется страница конечной точки CDN.
2. Выберите **Сжатие**.

    ![Выбор сжатия CDN](./media/cdn-file-compression/cdn-compress-select-std.png)
   
    Откроется страница сжатия.
3. Чтобы включить сжатие, выберите значение **Вкл.**
   
    ![Параметры сжатия файла CDN](./media/cdn-file-compression/cdn-compress-standard.png)
4. Используйте типы MIME по умолчанию либо измените список, удалив или добавив типы MIME.
   
   > [!TIP]
   > Хотя это и возможно, не рекомендуем применять сжатие для сжатых форматов. Например, ZIP, MP3, MP4 или JPG.
   > 
 
5. После внесения изменений выберите **Сохранить**.

### <a name="premium-cdn-profiles"></a>Профили CDN уровня "Премиум"
> [!NOTE]
> Сведения в этом разделе относятся только к профилям **Azure CDN уровня "Премиум" от Verizon**.
> 

1. На странице профиля CDN выберите **Управление**.
   
    ![Выбор управления CDN](./media/cdn-file-compression/cdn-manage-btn.png)
   
    Откроется портал управления CDN.
2. Наведите указатель мыши на вкладку **HTTP Large** (Большая платформа HTTP), а затем наведите указатель мыши на всплывающий элемент **Параметры кэша**. Выберите **Сжатие**.

    ![Выбор сжатия CDN](./media/cdn-file-compression/cdn-compress-select.png)
   
    Отобразятся параметры сжатия.
   
    ![Параметры сжатия файла CDN](./media/cdn-file-compression/cdn-compress-files.png)
3. Включите сжатие, выбрав параметр **Включить сжатие**. Введите типы MIME для сжатия в виде списка с разделителями-запятыми (без пробелов) в текстовом поле **Типы файлов**.
   
   > [!TIP]
   > Хотя это и возможно, не рекомендуем применять сжатие для сжатых форматов. Например, ZIP, MP3, MP4 или JPG.
   > 
    
4. После внесения изменений выберите **Обновить**.

## <a name="compression-rules"></a>Правила сжатия

### <a name="azure-cdn-standard-from-microsoft-profiles"></a>Профили Azure CDN уровня "Стандартный" от Майкрософт

Для профилей **Azure CDN уровня "Стандартный" от Майкрософт** могут сжиматься все файлы. Но должен использоваться файл типа MIME, для которого [в настройках разрешено сжатие](#enabling-compression).

Эти профили поддерживают следующие алгоритмы сжатия:
- gzip (GNU zip)
- brotli 
 
Если запрос поддерживает несколько типов сжатия, эти типы сжатия обладают приоритетом над сжатием brotli.

Если в запросе ресурса указано сжатие gzip и запрос приводит к сбросу кэша, Azure CDN выполняет сжатие gzip прямо на сервере POP. После этого сжатый файл используется из кэша.

### <a name="azure-cdn-from-verizon-profiles"></a>Профили Azure CDN от Verizon

Для профилей **Azure CDN уровня "Стандартный" от Verizon** и **Azure CDN уровня "Премиум" от Verizon** будут сжаты только подходящие файлы. Сжатие допускается для следующих файлов:
- более 128 байт;
- менее 1 МБ.
 
Эти профили поддерживают следующие алгоритмы сжатия:
- gzip (GNU zip)
- DEFLATE
- bzip2
- brotli 
 
Если запрос поддерживает несколько типов сжатия, эти типы сжатия обладают приоритетом над сжатием brotli.

Если в запросе ресурса указано сжатие brotli (заголовок HTTP `Accept-Encoding: br`) и запрос приводит к сбросу кэша, Azure CDN выполняет сжатие brotli непосредственно на сервере POP. После этого сжатый файл используется из кэша.

### <a name="azure-cdn-standard-from-akamai-profiles"></a>Профили Azure CDN уровня "Стандартный" от Akamai

Для профилей **Azure CDN уровня "Стандартный" от Akamai** сжатие допускается для всех файлов. Но должен использоваться файл типа MIME, для которого [в настройках разрешено сжатие](#enabling-compression).

Эти профили поддерживают только алгоритм сжатия gzip. Когда конечная точка профиля запрашивает файл в формате gzip, он всегда запрашивается с сервера-источника, независимо от запроса клиента. 

## <a name="compression-behavior-tables"></a>Варианты использования сжатия
В приведенных ниже таблицах описан принцип работы сжатия CDN Azure для всех сценариев.

### <a name="compression-is-disabled-or-file-is-ineligible-for-compression"></a>Сжатие отключено или для файла сжатие недопустимо
| Запрошенный клиентом формат (через заголовок Accept-Encoding) | Формат кэшированного файла | Ответ CDN клиенту | Примечания&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
| --- | --- | --- | --- |
| сжатые; |сжатые; |сжатые; | |
| сжатые; |Без сжатия |Без сжатия | |
| сжатые; |Не кэширован |Сжатый или несжатый |Ответ источника определяет, будет ли CDN выполнять сжатие. |
| Без сжатия |сжатые; |Без сжатия | |
| Без сжатия |Без сжатия |Без сжатия | |
| Без сжатия |Не кэширован |Без сжатия | |

### <a name="compression-is-enabled-and-file-is-eligible-for-compression"></a>Сжатие включено и для файла допускается сжатие
| Запрошенный клиентом формат (через заголовок Accept-Encoding) | Формат кэшированного файла | Ответ CDN клиенту | Заметки |
| --- | --- | --- | --- |
| сжатые; |сжатые; |сжатые; |CDN перекодирует из одного поддерживаемого формата в другой. |
| сжатые; |Без сжатия |сжатые; |CDN выполняет сжатие. |
| сжатые; |Не кэширован |сжатые; |CDN выполняет сжатие, если источник возвращает несжатый файл. <br/>**Azure CDN от Verizon** передает несжатый файл при первом запросе, а затем сжимает файл и помещает его в кэш для последующих запросов. <br/>Файлы с заголовком `Cache-Control: no-cache` никогда не сжимаются. |
| Без сжатия |сжатые; |Без сжатия |CDN проводит распаковку. |
| Без сжатия |Без сжатия |Без сжатия | |
| Без сжатия |Не кэширован |Без сжатия | |

## <a name="media-services-cdn-compression"></a>Сжатие CDN для служб мультимедиа
Для конечных точек с включенной потоковой передачей CDN служб мультимедиа по умолчанию включено сжатие для следующих типов MIME: 
- application/vnd.ms-sstr+xml; 
- application/dash+xml;
- application/vnd.apple.mpegurl;
- application/f4m+xml 

## <a name="see-also"></a>См. также
* [Устранение неполадок со сжатием файлов CDN](cdn-troubleshoot-compression.md)    

