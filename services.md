1. IaaS, PaaS, SaaS

**IaaS** — Infrastructure as a Service — _инфраструктура как услуга_, например, виртуальные серверы и виртуальная сеть; клиент может устанавливать любое программное обеспечение и приложения;

**PaaS** — Platform as a Service — _платформа как услуга_, например, веб-сервер или база данных; клиент управляет приложениями, операционной системой управляет провайдер;

**SaaS** — Software as a Service — _программное обеспечение как услуга_, например, электронная почта или иное офисное приложение; клиент пользуется приложением, базовыми настройками приложения управляет провайдер.

1. Vertical scaling vs horizontal scaling

**Horizontal scaling means that you scale by adding more machines** into your pool of resources whereas **Vertical scaling means that you scale by adding more power (CPU, RAM) to an existing machine**.

In the database world, horizontal-scaling is often based on the partitioning of the data i.e. each node contains only part of the data, in vertical-scaling the data resides on a single node and scaling is done through multi-core i.e. spreading the load between the CPU and RAM resources of that machine.

With horizontal-scaling it is often easier to scale dynamically by adding more machines into the existing pool - Vertical-scaling is often limited to the capacity of a single machine, scaling beyond that capacity often involves downtime and comes with an upper limit.

Good examples of horizontal scaling are Cassandra, MongoDB, [Google Cloud Spanner](https://cloud.google.com/spanner) .. and a good example of vertical scaling is MySQL - Amazon RDS (The cloud version of MySQL). It provides an easy way to scale vertically by switching from small to bigger machines. This process often involves downtime.

In-Memory Data Grids such as [GigaSpaces XAP](http://www.gigaspaces.com/datagrid), [Coherence](http://www.oracle.com/technetwork/middleware/coherence/overview/index.html) etc.. are often optimized for both horizontal and vertical scaling simply because they're not bound to disk. Horizontal-scaling through partitioning and vertical-scaling through multi-core support.