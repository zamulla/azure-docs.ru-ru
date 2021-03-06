---
title: Поддерживаемые целевые и выходные форматы данных в средстве подготовки данных службы "Машинное обучение Azure" | Документация Майкрософт
description: В этой статье содержится полный список целевых и выходных форматов данных, поддерживаемых средством подготовки данных службы "Машинное обучение Azure"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 4aee24150524c270084ae8ec22f09df94b6e9f36
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34831716"
---
# <a name="supported-data-exports-for-this-preview"></a>Поддерживаемые форматы экспорта данных в предварительной версии 
Данные можно экспортировать в нескольких разных форматах. Эти форматы можно использовать для хранения промежуточных результатов подготовки данных перед их дальнейшей интеграцией в рабочий процесс службы "Машинное обучение".

## <a name="types"></a>Типы 
### <a name="csv-file"></a>CSV-файл 
Запись файла данных с разделителями-запятыми в хранилище.

#### <a name="options"></a>Параметры
- Символы конца строки
- Замена значений Null
- Замена ошибочных значений 
- Разделитель


### <a name="parquet"></a>PARQUET 
Запись набора данных в формате PARQUET в хранилище.

Формат PARQUET может принимать разные формы в хранилище. Иногда для наборов данных меньшего размера используется один PARQUET-файл. Разные библиотеки Python поддерживают операции чтения и записи в отдельные файлы в этом формате. 

Сейчас Azure Machine Learning Workbench выполняет запись данных в формате PARQUET во время локального интерактивного использования с помощью библиотеки Python PyArrow. Это означает, что сейчас формат PARQUET для отдельных файлов — это единственный формат выходных данных PARQUET, поддерживаемый при локальном интерактивном использовании.

При масштабировании (в Spark) Azure Machine Learning Workbench зависит от функций чтения и записи PARQUET Spark. Структура стандартного формата выходных данных PARQUET (сейчас — единственный поддерживаемый формат) в Spark во многом напоминает набор данных Hive. Это означает, что папка содержит большое число файлов PARQUET, каждый из которых входит в состав небольшой секции более крупного набора данных. 

#### <a name="caveats"></a>Предупреждения 
PARQUET как формат появился относительно недавно. Он имеет некоторые несоответствия реализации в различных библиотеках. Например, в Spark применяются ограничения символов, допустимых в именах столбцов при записи в формате PARQUET. В PyArrow эти ограничения не применяются. Имя столбца не может содержать следующие символы: 
- ,
- ;
- {}
- ()
- \\n
- \\t
- =

>[!NOTE]
>- Для обеспечения совместимости со Spark при каждой операции записи данных в формате PARQUET все вхождения этих символов в именах столбцов заменяются символом подчеркивания (_).
>- Для обеспечения согласованности со Spark при локальном использовании и масштабировании всех данных, записанных в формате PARQUET при работе с приложением, Python и Spark имеют собственные соответствующие имена столбцов. Перед записью данных в формате PARQUET удалите символы, на которые распространяются ограничения в Spark, чтобы получить ожидаемые имена столбцов.



## <a name="locations"></a>Расположения 
### <a name="local"></a>Local 
Локальный жесткий диск или сопоставленное сетевое расположение для хранения.

### <a name="azure-blob-storage"></a>Хранилище больших двоичных объектов Azure
Для работы с хранилищем BLOB-объектов Azure требуется подписка Azure.

