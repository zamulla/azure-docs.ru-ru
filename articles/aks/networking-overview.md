---
title: Конфигурация сети в службе Azure Kubernetes (AKS)
description: Узнайте о базовой и расширенной конфигурации сети в службе Azure Kubernetes (AKS).
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/15/2018
ms.author: marsma
ms.openlocfilehash: 207accc30e10c4e2bed5b713fc59e2f9ad86a876
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/21/2018
ms.locfileid: "36311098"
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Конфигурация сети в службе Azure Kubernetes (AKS)

При создании кластера службы Azure Kubernetes (AKS) можно выбрать один из двух параметров сети: **Базовый** или **Расширенный**.

## <a name="basic-networking"></a>Сетевое взаимодействие уровня "Базовый"

Параметр сети **Базовый** — это конфигурация по умолчанию для создания кластера AKS. Конфигурация сети кластера и его контейнеров pod полностью управляется Azure и подходит для развертываний, для которых не требуется пользовательская конфигурация виртуальной сети. Если для сетевого взаимодействия выбрать параметр "Базовый", вы не сможете управлять конфигурацией сети, а именно подсетями и диапазонами IP-адресов, назначенными кластеру.

В кластере AKS узлы, для которых выбран параметр сети "Базовый", используют подключаемый модуль Kubernetes [kubenet][kubenet].

## <a name="advanced-networking"></a>Сетевое взаимодействие уровня "Расширенный"

При выборе параметра сети **Расширенный** ваши контейнеры pod помещаются в виртуальную сеть Azure, которую вы настраиваете, в результате чего они автоматически подключаются к ресурсам виртуальной сети и интегрируются с обширным набором возможностей виртуальных сетей.
Сетевое взаимодействие уровня "Расширенный" доступно при развертывании кластеров AKS с помощью [портала Azure][portal], Azure CLI или шаблона Resource Manager.

Узлы в кластере AKS, настроенном для сетевого взаимодействия "Расширенный", используют подключаемый модуль Kubernetes [сетевого интерфейса работы с контейнерами Azure (CNI)][cni-networking].

![Схема, демонстрирующая два узла с мостами, подключающими каждый из них к одной виртуальной сети Azure][advanced-networking-diagram-01]

## <a name="advanced-networking-features"></a>Возможности сетевого взаимодействия уровня "Расширенный"

Сетевое взаимодействие уровня "Расширенный" обеспечивает следующие преимущества:

* Развертывание кластера AKS в имеющуюся виртуальную сеть или создание виртуальной сети и подсети для кластера.
* Каждому контейнеру pod в кластере назначается IP-адрес в виртуальной сети. Они могут напрямую взаимодействовать с другими контейнерами pod в кластере, а также другими узлами в виртуальной сети.
* Группа контейнеров pod может подключаться к другим службам одноранговой виртуальной сети, а также к локальным сетям через ExpressRoute и VPN типа "сеть — сеть" (S2S). Контейнеры pod также доступны локально.
* Предоставление службы Kubernetes на внешнем или внутреннем уровне через Azure Load Balancer. Кроме того, предоставляется возможность сетевого взаимодействия "Базовый".
* Контейнеры pod в подсети с включенными конечными точками службы могут безопасно подключаться к службам Azure (например, к службе хранилища или базе данных SQL).
* Трафик из контейнеров pod к виртуальным сетевым модулям можно маршрутизировать через определяемые пользователем маршруты.
* Контейнеры pod могут получать доступ к ресурсам в общедоступном Интернете. Кроме того, предоставляется возможность сетевого взаимодействия "Базовый".

> [!IMPORTANT]
> Каждый узел в кластере AKS, для которого настроен параметр сетевого взаимодействия уровня "Расширенный", может содержать не более **30 контейнеров pod**, если для его настройки используется портал Azure.  Изменить максимальное значение можно только путем изменения свойства maxPods при развертывании кластера с помощью шаблона Resource Manager. Каждая виртуальная сеть, подготовленная для использования с подключаемым модулем Azure CNI, ограничена **4096 настроенными IP-адресами**.

## <a name="advanced-networking-prerequisites"></a>Предварительные требования к расширенному сетевому взаимодействию

* Виртуальная сеть для кластера AKS должна разрешать исходящее подключение к Интернету.
* Не создавайте больше одного кластера AKS в одной подсети.
* Расширенное взаимодействие для AKS не поддерживает виртуальные сети, использующие частные зоны DNS.
* Кластер AKS не может использовать `169.254.0.0/16`, `172.30.0.0/16` или `172.31.0.0/16` для диапазона адресов службы Kubernetes.
* Субъект-служба, используемая для кластера AKS, должна иметь разрешения `Contributor` для группы ресурсов, содержащей имеющуюся виртуальную сеть.

## <a name="plan-ip-addressing-for-your-cluster"></a>Планирование назначения IP-адресов для кластера

Для кластеров с настроенным расширенным сетевым взаимодействием требуется дополнительное планирование. Размер виртуальной сети и подсети должен быть достаточным для количества контейнеров pod, которые будут одновременно запускаться, а также соответствовать количеству узлов в кластере.

IP-адреса для контейнеров pod и узлов кластера назначаются из определенной подсети в виртуальной сети. Для каждого узла настраивается основной IP-адрес, который является IP-адресом узла, и 30 дополнительных IP-адресов, предустановленных Azure CNI и назначенных контейнерам pod, запланированным на узле. При масштабировании кластера каждый узел настраивается аналогичным образом с использованием IP-адресов из подсети.

План IP-адреса для кластера AKS содержит виртуальную сеть, по крайней мере одну подсеть для узлов и контейнеров pod, а также диапазон адресов службы Kubernetes.

| Диапазон адресов / ресурс Azure | Установления размера и ограничения |
| --------- | ------------- |
| Виртуальная сеть | Виртуальная сеть Azure может достигать размера /8, но для нее можно настроить только 4096 IP-адресов. |
| Подсеть | Должна иметь размер, достаточный для размещения узлов и контейнеров pod. Вычислить минимальный размер подсети можно таким образом: (количество узлов) + (количество узлов * количество контейнеров pod в каждом узле). Для кластера с 50 узлами: (50) + (50 * 30) = 1550 (подсеть должна иметь размер минимум /21). |
| Диапазон адресов службы Kubernetes | Этот диапазон не должен использоваться элементом в этой виртуальной сети или элементом, подключенным к ней. Адрес службы CIDR должен иметь размер меньше /12. |
| IP-адрес службы доменных имен (DNS) Kubernetes | IP-адрес в пределах диапазона адресов службы Kubernetes, который будет использоваться службой обнаружения кластеров (kube-dns). |
| Адрес моста Docker | IP-адрес (в нотации CIDR), используемый в качестве IP-адреса моста Docker на узлах. По умолчанию — 172.17.0.1/16. |

Как отмечено ранее, каждая виртуальная сеть, подготовленная для использования с подключаемым модулем Azure CNI, ограничена **4096 настроенными IP-адресами**. Каждый узел в кластере, настроенном для сетевого взаимодействия "Расширенный", может содержать не более **30 контейнеров pod**.

## <a name="deployment-parameters"></a>Параметры развертывания

При создании кластера AKS для сетевого взаимодействия уровня "Расширенный" можно настроить следующие параметры.

**Виртуальная сеть** — виртуальная сеть, в которую необходимо развернуть кластер Kubernetes. Если для кластера необходимо создать виртуальную сеть, выберите *Создать новый* и следуйте инструкциям раздела *Создание виртуальной сети*.

**Подсеть** — подсеть в виртуальной сети, в которую требуется развернуть кластер. Если для кластера необходимо создать подсеть в виртуальной сети, выберите *Создать новый* и следуйте инструкциям раздела *Создание подсети*.

**Диапазон адресов службы Kubernetes**. *Диапазон адресов службы Kubernetes* — это диапазон IP-адресов, адреса из которого назначаются службам Kubernetes в вашем кластере (чтобы получить дополнительные сведения о службах Kubernetes, ознакомьтесь с разделом [Services][services] (Службы) в документации по Kubernetes).

Диапазон IP-адресов службы Kubernetes:

* должен быть за пределами диапазона IP-адресов виртуальной сети вашего кластера;
* не должен перекрываться с какими-либо другими виртуальными сетями, с которыми у виртуальной сети кластера настроен пиринг;
* не должен перекрываться с какими-либо локальными IP-адресами.

Перекрывающиеся диапазоны IP-адресов могут привести к непредсказуемому поведению. Например, если pod пытается получить доступ к IP-адресу за пределами кластера, который также является IP-адресом службы, это может привести к непредсказуемому поведению и сбоям.

**IP-адрес службы DNS Kubernetes** — IP-адрес службы DNS кластера. Этот адрес должен быть в пределах *диапазона адресов службы Kubernetes*.

**Адрес моста Docker** — IP-адрес и маска сети, которую необходимо назначить мосту Docker. Этот IP-адрес должен быть за пределами диапазона IP-адресов виртуальной сети вашего кластера.

## <a name="configure-networking---cli"></a>Настройка сети с помощью интерфейса командной строки

При создании кластера AKS с помощью Azure CLI можно также настроить сетевое взаимодействие уровня "Расширенный". Используйте приведенные ниже команды для создания кластера AKS с включенными функциями сетевого взаимодействия уровня "Расширенный".

Сначала получите идентификатор ресурса существующей подсети, к которой будет присоединен кластер AKS.

```console
$ az network vnet subnet list --resource-group myVnet --vnet-name myVnet --query [].id --output tsv

/subscriptions/d5b9d4b7-6fc1-46c5-bafe-38effaed19b2/resourceGroups/myVnet/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/default
```

Используйте команду [az aks create][az-aks-create] с аргументом `--network-plugin azure` для создания кластера с функциями сетевого взаимодействия уровня "Расширенный". Измените значение `--vnet-subnet-id`, указав идентификатор подсети, полученный на предыдущем шаге.

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --network-plugin azure --vnet-subnet-id <subnet-id> --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.2.0.10 --service-cidr 10.2.0.0/24
```

## <a name="configure-networking---portal"></a>Настройка сети с помощью портала

На следующем снимке экрана на портале Azure показан пример настройки этих параметров во время создания кластера AKS.

![Конфигурация расширенного сетевого взаимодействия на портале Azure][portal-01-networking-advanced]

## <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

Следующие вопросы и ответы относятся к конфигурации сети **Расширенный**.

* *Можно ли настроить сетевое взаимодействие "Расширенный" с помощью Azure CLI?*

  Нет. Сетевое взаимодействие уровня "Расширенный" сейчас доступно только при развертывании кластеров AKS на портале Azure или с помощью шаблона Resource Manager.

* *Можно ли развернуть виртуальные машины в подсети моего кластера?*

  Нет. Развертывание виртуальных машин в подсети, используемой кластером Kubernetes, не поддерживается. Виртуальные машины можно развернуть в одной и той же виртуальной сети, но в разных подсетях.

* *Можно ли настроить политики сети для каждого контейнера pod?*

  Нет. Политики сети для каждого контейнера pod сейчас не поддерживаются.

* *Можно ли настроить максимальное число контейнеров pod, развертываемых на узел?*

  По умолчанию на каждом узле может размещаться максимум 30 контейнеров pod. Изменить максимальное значение можно только путем изменения свойства `maxPods` при развертывании кластера с помощью шаблона Resource Manager.

* *Как настроить дополнительные свойства для подсети, созданной во время создания кластера AKS? Например, конечные точки службы.*

  Полный список свойств для виртуальной сети и подсетей, создаваемых во время создания кластера AKS, можно настроить на странице стандартной конфигурации виртуальной сети на портале Azure.

## <a name="next-steps"></a>Дополнительная информация

### <a name="networking-in-aks"></a>Сетевое взаимодействие в AKS

Узнайте больше о сетевом взаимодействии в AKS из следующих статей:

[Использование статического IP-адреса с подсистемой балансировки нагрузки Службы контейнеров Azure (AKS)](static-ip.md)

[Входящий трафик HTTPS в Службе контейнеров Azure (AKS)](ingress.md)

[Использование внутренней подсистемы балансировки нагрузки со Службой контейнеров Azure (AKS)](internal-lb.md)

### <a name="acs-engine"></a>Обработчик ACS

[Обработчик Службы контейнера Azure (обработчик ACS)][acs-engine] — это проект с открытым исходным кодом, создающий шаблоны Azure Resource Manager, которые можно использовать для развертывания кластеров с поддержкой Docker в Azure. С помощью обработчика ACS можно развернуть Kubernetes, DC/OS, режим Swarm и оркестраторы Swarm.

Кластеры Kubernetes, созданные с помощью обработчика ACS, поддерживают подключаемые модули [kubenet][kubenet] и [Azure CNI][cni-networking]. Таким образом, обработчик ACS поддерживает сценарии как базового, так и расширенного сетевого взаимодействия.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[services]: https://kubernetes.io/docs/concepts/services-networking/service/
[portal]: https://portal.azure.com

<!-- LINKS - Internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-ssh]: aks-ssh.md
