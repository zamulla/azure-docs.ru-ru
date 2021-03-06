---
title: Ограничения и известные проблемы при импорте API в службу управления API Azure | Документация Майкрософт
description: Сведения об известных проблемах и ограничениях при импорте в службу управления API Azure с помощью форматов Open API, WSDL и WADL.
services: api-management
documentationcenter: ''
author: vladvino
manager: vlvinogr
editor: ''
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: apipm
ms.openlocfilehash: 03d785898398cb0bcd7b43e8d7feab705bce4b34
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/01/2018
ms.locfileid: "34598476"
---
# <a name="api-import-restrictions-and-known-issues"></a>Ограничения и известные проблемы при импорте API
## <a name="about-this-list"></a>Об этом списке
При импорте API вы можете столкнуться с некоторыми ограничениями или выявить проблемы, которые необходимо исправить, чтобы успешно завершить импорт. В этой статье собраны необходимые сведения. Они сгруппированы по форматам импорта API.

## <a name="open-api"> </a>OpenAPI/Swagger
Если возникают ошибки при импорте документа OpenAPI, то необходимо проверить, утвержден ли этот процесс. Это можно сделать с помощью конструктора на новом портале Azure ("Конструктор > Интерфейс > Редактор спецификаций OpenAPI") или инструмента стороннего разработчика, такого как <a href="http://www.swagger.io">редактор Swagger</a>.

* Поддерживается только формат JSON для OpenAPI.
* Обязательные параметры пути и запроса должны иметь уникальные имена. (В OpenAPI имя параметра должно быть уникальным только в пределах расположения (например, путь, запрос, заголовок).  Тем не менее в управлении API имеется возможность различать операции по параметрам пути и запроса (которые не поддерживается в OpenAPI). Поэтому необходимо указать уникальные имена параметров в пределах всего шаблона URL-адреса.)
* Схемы, ссылки на которые добавлены с помощью свойств **$ref**, не должны содержать другие свойства **$ref**.
* Указатели **$ref** не должны использоваться для внешних файлов.
* Поддерживаются только расширения **x-ms-paths** и **x-servers**.
* Настраиваемые расширения игнорируются при импорте и не сохраняются для экспорта.

> [!IMPORTANT]
> Важные сведения и советы, связанные с импортом OpenAPI, см. в этом [документе](https://blogs.msdn.microsoft.com/apimanagement/2018/03/28/important-changes-to-openapi-import-and-export/).

## <a name="wsdl"> </a>WSDL
WSDL-файлы используются для создания сквозных интерфейсов API SOAP, а также служат в качестве серверной части интерфейса API между SOAP и REST.
* **Привязки SOAP.** Поддерживаются только привязки SOAP шифрования типа document или literal. Шифрование SOAP типа rpc не поддерживается.
* **WSDL:import.** Этот атрибут не поддерживается. Клиентам следует объединить импортированные элементы в один документ.
* **Сообщения, состоящие из нескольких частей.** Такие типы сообщений не поддерживаются.
* **WCF wsHttpBinding.** Службы SOAP, созданные с помощью технологии Windows Communication Foundation, должны использовать значение basicHttpBinding. Значение wsHttpBinding не поддерживается.
* **MTOM.** Службы, использующие MTOM, <em>могут</em> работать. Но официальная поддержка в данный момент не предоставляется.
* **Рекурсия.** APIM не поддерживает типы, которые определены рекурсивно (например, которые ссылаются на включающий их массив).

## <a name="wadl"> </a>WADL
У нас нет сведений о проблемах импорта с помощью формата WADL.
