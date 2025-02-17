## Общие рекомендации по созданию отказоустойчивых приложений

- Четко разделяйте своё приложение на компоненты без сохранения состояния (stateless) и с сохранением состояния (stateful). Планируйте свое приложение так, чтобы любой из компонентов мог горизонтально масштабироваться. Избегайте наличия компонентов, которые возможно масштабировать только вертикально.
- Для организации взаимодействия различных компонентов приложения используйте балансировщики нагрузки (Load Balancers) и очереди сообщений / брокеры сообщений (Message Brokers).
- Управляйте своей виртуальной инфраструктурой через IaC (при помощи таких инструментов, как Terraform).
- Избегайте ручной правки конфигурации сервисов и ОС внутри виртуальной машины. Для конфигурирования виртуальных машин используйте системы управления конфигурациями (Ansible, AWX, Chef, Puppet). Таким образом вы сможете отслеживать все изменения конфигурации и эффективно ими управлять.
- Готовьте образы виртуальных машин для быстрого, безопасного и воспроизводимого развертывания виртуальной инфраструктуры (подход Immutable Infrastructure / Immutable Servers — больше информации можно узнать на страницах [Immutable Server](https://martinfowler.com/bliki/ImmutableServer.html), [What is mutable vs Immutable Infrastructure](https://www.hashicorp.com/resources/what-is-mutable-vs-immutable-infrastructure) и [Immutable Infrastructure](https://medium.com/the-cloud-architect/immutable-infrastructure-21f6613e7a23)).
- Повышайте Observability (наблюдаемость) своего приложения: настройте сквозной мониторинг приложения, начиная с уровня инфраструктуры и операционной системы и до уровня самого приложения для точного и быстрого выявления проблем и узких мест в приложении (больше информации можно узнать на странице [What is Observability](https://www.dynatrace.com/news/blog/what-is-observability-2/)).
- Отказоустойчивость экземпляров виртуальной инфраструктуры (виртуальные машины, диски, сети) уже реализована на уровне платформы. Нет никакого смысла самостоятельно повышать отказоустойчивость виртуальной инфраструктуры за счет методов, применяемых в физической инфраструктуре.
- Используйте доменные имена для общения узлов вашего приложения между собой. Таким образом, вы не будете привязаны к IP-адресам.
- Используйте S3-хранилище для долгосрочного хранения больших объемов информации или статического контента ([Cloud Storage](https://mcs.mail.ru/docs/ru/base/s3#) или сервисы [Cloud Data Platform](https://mcs.mail.ru/bigdata/)).
- Для компонентов вашего приложения, развернутых в [Kubernetes](https://mcs.mail.ru/help/kubernetes/scaling), настройте автомасштабирование (autoscaling). Это обеспечит предоставление необходимых вычислительных ресурсов в периоды увеличения нагрузки на приложение.
- При разработке приложений применяйте методологию The Twelve-Factor App (больше информации можно узнать на странице [12Factor](https://12factor.net/)).
- Изучите документацию на предмет функционала, возможностей и ограничений облачного провайдера.
- Напишите нам с помощью [формы обратной связи](https://mcs.mail.ru/help/contact-us), если у вас остались вопросы.

## Рекомендации к безопасности

- Используйте разные [приватные сети](/ru/networks/vnet/concepts/ips-and-inet) для разных приложений в рамках одного проекта или разворачивайте каждое приложение в собственном проекте.
- Используйте [группы безопасности](/ru/networks/vnet/concepts/traffic-limiting). Открывайте только те порты, которые необходимы для взаимодействия компонентов вашего приложения, а также порты, используемые для задач сопровождения и обслуживания вашей инфраструктуры и приложения.
- Выставляйте в интернет только эндпоинты (точки входа) своих приложений. Закройте доступ к инфраструктуре вашего сервиса из интернета.
- Используйте защищенный VPN-канал для организации сетевой связанности ваших локальных ресурсов с ресурсами в облаке. Больше информации можно узнать на странице [документации VPN сетей](/ru/networks/vnet/concepts/vpn).
- Используйте резервное копирование для критически важных компонентов вашего приложения. Проверяйте целостность резервных копий.
- Периодически проводите учения по восстановлению данных из резервных копий.
- Используйте MFA (мультифакторную аутентификацию).
- Подключайте сервисы AntiDDoS и WAF.

## Зоны доступности

Для создания отказоустойчивого географически распределенного приложения размещайте виртуальную инфраструктуру в разных зонах доступности. Зона доступности в VK Cloud соответствует отдельному ЦОДу уровня Tier III. Сами ЦОДы объединены оптическими каналами связи с высокой пропускной способностью. Использование зон доступности является полностью прозрачным для приложения: на уровне виртуальной инфраструктуры в рамках одного проекта не требуется никаких дополнительных настроек для организации взаимодействия между собой компонентов приложения.

Чтобы разместить виртуальную машину в определенной зоне доступности, достаточно задать соответствующий параметр при создании этой виртуальной машины. Размещайте компоненты вашего приложения на нескольких виртуальных машинах, распределенных по разным зонам доступности. Таким образом вы повышаете отказоустойчивость и доступность своего приложения в случае аварий.

## Блочные устройства (Диски)

В разделе «Диски» портала VK Cloud можно создать виртуальное блочное устройство с требуемыми значениями параметров «тип диска», «зона доступности» и «размер». Рекомендуем создавать диск в той же зоне доступности, что и виртуальная машина, к которой он будет подключен. Это поможет избежать дополнительных сетевых задержек (latency) при обращении к диску.

Размер блочного устройства может быть размером до 5 TB для типа дисков «сетевой HDD- / SSD-диск». Для типа дисков High-IOPS SSD ограничение размера составляет 2 TB. Если вам необходим бóльший размер блочного устройства на этом типе дисков, то следует создать несколько виртуальных блочных устройств High-IOPS SSD с необходимым суммарным объемом, подключить к виртуальной машине и создать на них один раздел с помощью LVM.

На платформе VK Cloud также есть тип дисков Low Latency NVME, характеризующийся минимальными задержками (latency) операций ввода-вывода. Этому типу соответствуют физические NVMe-диски, установленные непосредственно в гипервизор. За счет дополнительно примененного слоя абстрагирования работы с данными у дисков Low Latency NVME есть все те же преимущества, что и у остальных типов виртуальных дисков.

Типы дисков отличаются количеством IOPS и latency. При выборе типа отталкивайтесь от требований к IOPS и latency для конкретного приложения, которое будет использовать этот диск. Например, не стоит выбирать тип «сетевой HDD-диск» для нагруженных баз данных или выбирать High-IOPS SSD или Low Latency NVME для ненагруженных сред. Также не рекомендуем создавать диск значительно большим по сравнению с объемом хранимых данных. Иначе вы будете переплачивать за неиспользуемое пространство и производительность. При необходимости вы всегда можете изменить тип существующего диска и увеличить его размер.

Посмотреть доступные типы дисков в VK Cloud и SLA по ним можно в статье [Производительность дисков VK Cloud](/ru/base/iaas/concepts/volume-sla).

## Повышение отказоустойчивости СУБД

Для повышения отказоустойчивости инсталляции СУБД рекомендуем разворачивать ее в конфигурации реплика-сета или в кластерной конфигурации. Конфигурация с одним узлом (single node) подходит только для сред разработки и тестирования.

Распределите ноды кластера СУБД по разным зонам доступности. Так вы гарантируете сохранность данных и работоспособность приложения при аварии в одной из зон доступности. Обязательно настраивайте резервное копирование данных. Важно помнить, что репликация — это не резервное копирование. Для хранения резервных копий отличным выбором будет Cloud Storage — отказоустойчивое, геораспределенное объектное хранилище, работающее по протоколу S3.

На платформе VK Cloud для своих задач вы можете использовать популярные СУБД, которые мы предоставляем по модели [PaaS](https://mcs.mail.ru/databases/). Этот сервис имеет встроенные механизмы high-availability и инструменты создания консистентных резервных копий.

## Балансировщики нагрузки

Балансировщик нагрузки (Load balancer) — это инфраструктурный компонент, который принимает входящий трафик/запрос и распределяет его в соответствии с заданным методом балансировки по нескольким серверам. Серверы, между которыми распределяется трафик, образуют группу или пул балансировки. Применение балансировщиков нагрузки позволяет легко масштабировать используемые вычислительные ресурсы, обеспечить высокую доступность вашего приложения, а также применять различные схемы развертывания новых версий приложения без перерывов в обслуживании.

Распределяйте виртуальные машины в группе балансировки примерно в равном количестве между зонами доступности. Помимо этого, мы рекомендуем держать такое количество виртуальных машин в группе балансировки, которое позволит выдержать отказ одной-двух ВМ без деградации в обслуживании запросов к вашему приложению.

На платформе VK Cloud в качестве балансировщика нагрузки используйте наш отказоустойчивый сервис LBaaS. Руководство по настройке сервиса вы найдете [на странице Балансировщики нагрузки](https://mcs.mail.ru/help/network/balancers). После настройки LBaaS рекомендуется провести нагрузочное тестирование, а также пробное отключение виртуальных машин, входящих в группу балансировки.

## Очереди сообщений

Очередь сообщений (Message Queue) — это компонент, позволяющий различным частям приложения взаимодействовать между собой в асинхронном режиме. Их применение поможет увеличить надежность и масштабируемость вашего приложения. Очередь сообщений представляет собой буфер для временного хранения сообщений и конечные точки для отправки и получения сообщений компонентами приложения. Сообщение будет храниться в очереди, пока другой компонент не извлечет его и не выполнит с ним какую-то операцию. Очередь могут использовать многочисленные источники и получатели, но каждое сообщение обрабатывается одним получателем только один раз. Для более детального понимания, зачем нужны очереди сообщений, читайте [статью в нашем блоге](https://mcs.mail.ru/blog/zachem-nuzhny-ocheredi-soobshcheniy-v-mikroservisnoy-arkhitekture).

Платформа VK Cloud предоставляет сервис высокодоступных и отказоустойчивых [очередей сообщений](https://mcs.mail.ru/blog/zachem-nuzhny-ocheredi-soobshcheniy-v-mikroservisnoy-arkhitekture).
