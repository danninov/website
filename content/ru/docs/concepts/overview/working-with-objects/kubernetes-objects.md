---
title: Изучение объектов Kubernetes
content_type: concept
weight: 10
card:
  name: concepts
  weight: 40
---

<!-- overview -->

На этой странице объясняется, как объекты Kubernetes представлены в API Kubernetes, и как их можно определить в формате `.yaml`.



<!-- body -->

## Изучение объектов Kubernetes {#kubernetes-objects}

*Объекты Kubernetes* — сущности, которые хранятся в Kubernetes. Kubernetes использует их для представления состояния кластера. В частности, они описывают следующую информацию:

* Какие контейнеризированные приложения запущены (и на каких узлах).
* Доступные ресурсы для этих приложений.
* Стратегии управления приложения, которые относятся, например, к перезапуску, обновлению или отказоустойчивости.

После создания объекта Kubernetes будет следить за существованием объекта. Создавая объект, вы таким образом указываете системе Kubernetes, какой должна быть рабочая нагрузка кластера; это *требуемое состояние* кластера.

Для работы с объектами Kubernetes – будь то создание, изменение или удаление — нужно использовать [API Kubernetes](/docs/concepts/overview/kubernetes-api/). Например, при использовании CLI-инструмента `kubectl`, он обращается к API Kubernetes. С помощью одной из [клиентской библиотеки](/docs/reference/using-api/client-libraries/) вы также можете использовать API Kubernetes в собственных программах.

### Спецификация и статус объекта

Почти в каждом объекте Kubernetes есть два вложенных поля-объекта, которые управляют конфигурацией объекта: *`spec`* и *`status`*.
При создании объекта в поле `spec` указывается _требуемое состояние_ (описание характеристик, которые должны быть у объекта).

Поле `status` описывает _текущее состояние_ объекта, которое создаётся и обновляется самим Kubernetes и его компонентами. {{< glossary_tooltip text="Плоскость управления" term_id="control-plane" >}} Kubernetes непрерывно управляет фактическим состоянием каждого объекта, чтобы оно соответствовало требуемому состоянию, которое было задано пользователем.

Например: Deployment — это объект Kubernetes, представляющий работающее приложение в кластере. При создании объекта Deployment вы можете указать в его поле `spec`, что хотите иметь три реплики приложения. Система Kubernetes получит спецификацию объекта Deployment и запустит три экземпляра приложения, таким образом обновит статус (состояние) объекта, чтобы он соответствовал заданной спецификации. В случае сбоя одного из экземпляров (это влечет за собой изменение состояние), Kubernetes обнаружит несоответствие между спецификацией и статусом и исправит его, т.е. активирует новый экземпляр вместо того, который вышел из строя.

Для получения дополнительной информации о спецификации объекта, статусе и метаданных смотрите документ с [соглашениями API Kubernetes](https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md).

### Описание объекта Kubernetes

При создании объекта в Kubernetes нужно передать спецификацию объекта, которая содержит требуемое состояние, а также основную информацию об объекте (например, его имя). Когда вы используете API Kubernetes для создания объекта (напрямую либо через `kubectl`), соответствующий API-запрос должен включать в теле запроса всю указанную информацию в JSON-формате. **В большинстве случаев вы будете передавать `kubectl` эти данные, записанные в файле .yaml**. Тогда инструмент `kubectl` преобразует их в формат JSON при выполнении запроса к API.

Ниже представлен пример `.yaml`-файла, в котором заданы обязательные поля и спецификация объекта, необходимая для объекта Deployment в Kubernetes:

{{< codenew file="application/deployment.yaml" >}}

Один из способов создания объекта Deployment с помощью файла `.yaml`, показанного выше — использовать команду [`kubectl apply`](/docs/reference/generated/kubectl/kubectl-commands#apply), которая принимает в качестве аргумента файл в формате `.yaml`. Например:

```shell
kubectl apply -f https://k8s.io/examples/application/deployment.yaml --record
```

Вывод будет примерно таким:

```
deployment.apps/nginx-deployment created
```

### Обязательные поля

В файле `.yaml` создаваемого объекта Kubernetes необходимо указать значения для следующих полей:

* `apiVersion` — используемая для создания объекта версия API Kubernetes
* `kind` — тип создаваемого объекта
* `metadata` — данные, позволяющие идентифицировать объект (`name`, `UID` и необязательное поле `namespace`)
* `spec` — требуемое состояние объекта

Конкретный формат поля-объекта `spec` зависит от типа объекта Kubernetes и содержит вложенные поля, предназначенные только для используемого объекта. В [справочнике API Kubernetes](/docs/reference/generated/kubernetes-api/{{< param "version" >}}/) можно найти формат спецификации любого объекта Kubernetes.
Например, формат  `spec` для объекта Pod находится в [ядре PodSpec v1](/docs/reference/generated/kubernetes-api/{{< param "version" >}}/#podspec-v1-core), а формат `spec` для Deployment — в [DeploymentSpec v1 apps](/docs/reference/generated/kubernetes-api/{{< param "version" >}}/#deploymentspec-v1-apps).



## {{% heading "whatsnext" %}}


* [Обзор API Kubernetes](/docs/reference/using-api/api-overview/) более подробно объясняет некоторые из API-концепций
* Познакомиться с наиболее важными и основными объектами в Kubernetes, например, с [подами](/docs/concepts/workloads/pods/pod-overview/).
* Узнать подробнее про [контролеры](/docs/concepts/architecture/controller/) в Kubernetes
