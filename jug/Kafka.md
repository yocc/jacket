APACHE KAFKA

在《财富》 100 强公司中，超过 80％的人信任并使用 Kafka。
Apache Kafka 是一个开放源代码的分布式事件流平台，成千上万的公司使用它来实现高性能数据管道，流分析，数据集成和关键任务应用程序。

核心能力

高通量
使用延迟低至 2ms 的计算机集群以网络受限的吞吐量传递消息。

可扩展
可以将生产集群规模扩展到多达一千个经纪人，每天数万亿条消息，数 PB 的数据以及数十万个分区。弹性扩展和收缩存储和处理。
production 生产
broker 经纪人
message 消息
partitions 分区
Elastically expand 弹性扩展
contract storage 收缩存储
durable 持久
streams of data 数据流
fault-tolerant 容错

永久储存
将数据流安全地存储在分布式，持久，容错的群集中。

高可用性
在可用区上有效地扩展群集，或跨地理区域连接单独的群集。

生态系统

内置流处理
使用事件时间和精确一次的处理来处理具有连接，聚集，过滤器，转换等事件的流。

几乎连接任何东西
Kafka 的现成的 Connect 接口与数百个事件源和事件接收器集成在一起，包括 Postgres，JMS，Elasticsearch，AWS S3 等。

客户资料库
使用多种编程语言读取，写入和处理事件流。

大型生态系统开源工具
大型的开源工具生态系统：利用大量社区驱动的工具。

信任和易于使用

关键任务
通过保证排序，零消息丢失和高效的一次处理来支持关键任务用例。

受到成千上万组织的信任
从互联网巨头到汽车制造商再到证券交易所，成千上万的组织使用 Kafka。超过 500 万次独特的终身下载。

广大的用户社区
Kafka 是 Apache Software Foundation 五个最活跃的项目之一，在世界各地有数百次聚会。

丰富的在线资源
丰富的文档，在线培训，指导教程，视频，示例项目，堆栈溢出等。

通过 O(1)的磁盘数据结构提供消息的持久化，这种结构对于即使数以 TB 的消息存储也能够保持长时间的稳定性能。

术语:
broker: Kafka 集群包含一个或多个服务器，这种服务器被称为 broker(逻辑概念); 一台物理机很可能有多个 broker (中间人)
topic: 消息的类别标识符; 一类 topic 会保存在一个或者多个 broker 上
partition: 分区, 纯物理概念, 每个 topic 存在于一个或者多个 分区上
producer: 生产者负责发布消息到 broker, 写到 broker (服务端)
consumer: 消费者负责从 broker 读取消息 (客户端)
consumer group:

什么是事件流?
事件流是人体中枢神经系统的数字等效形式。
它是“永远在线”世界的技术基础，在这个世界中，企业越来越多地由软件定义和自动化，而软件的用户则更多。
从技术上讲，事件流是一种以事件流的形式从事件源（例如数据库，传感器，移动设备，云服务和软件应用程序）实时捕获数据的实践。
持久存储这些事件流以供以后检索；实时以及回顾性地处理，处理和响应事件流；并根据需要将事件流路由到不同的目标技术。
因此，事件流确保了数据的连续流和解释，以便正确的信息在正确的时间，正确的位置。

我可以将事件流用于什么?
事件流适用于众多行业和组织的各种用例。它的许多示例包括:
实时处理付款和金融交易，例如在证券交易所，银行和保险中。
实时跟踪和监视汽车，卡车，车队和货运，例如在物流和汽车行业。
连续捕获和分析来自 IoT 设备或其他设备（例如工厂和风电场）中的传感器数据。
收集并立即响应客户的交互和订单，例如在零售，酒店和旅游行业以及移动应用程序中。
监测患者的医院护理情况并预测病情变化，以确保在紧急情况下及时得到治疗。
连接，存储和提供公司不同部门产生的数据。
用作数据平台，事件驱动的体系结构和微服务的基础。

ApacheKafka® 是事件流平台。那是什么意思？
Kafka 结合了三个关键功能，因此您可以使用一个经过战斗验证的解决方案来端到端实施事件流的用例:
发布（写入）和订阅（读取）事件流，包括从其他系统连续导入/导出数据。
根据需要持久而可靠地存储事件流。
处理事件流的发生或追溯。
并且以分布式，高度可伸缩，弹性，容错和安全的方式提供所有这些功能。
Kafka 可以部署在裸机硬件，虚拟机和容器，本地以及云中。
您可以在自我管理 Kafka 环境与使用各种供应商提供的完全托管服务之间进行选择。

卡夫卡如何概括地说？
Kafka 是一个分布式服务器，由通过高性能 TCP 网络协议进行通信的服务器和客户端组成。它可以部署在内部以及云环境中的裸机硬件，虚拟机和容器上。
服务器：Kafka 作为一台或多台服务器的集群运行，可以跨越多个数据中心或云区域。其中一些服务器构成了存储层，称为代理 broker。其他服务器运行 Kafka Connect 来连续导入和导出数据作为事件流，以将 Kafka 与现有系统集成在一起，例如关系数据库以及其他 Kafka 群集。为了实现关键任务用例，Kafka 群集具有高度可扩展性和容错能力：如果其任何服务器发生故障，其他服务器将接管其工作，以确保连续运行而不会丢失任何数据。
客户端：通过它们，您可以编写分布式应用程序和微服务，即使在网络问题或机器故障的情况下，它们也可以并行，大规模且以容错的方式读取，写入和处理事件流。 Kafka 附带了一些这样的客户端，由 Kafka 社区提供的数十个客户端进行了扩展：客户端可用于 Java 和 Scala，包括更高级别的 Kafka Streams 库，Go，Python，C / C ++和许多其他编程语言以及 REST API。

主要概念和术语
事件记录了世界或您的企业中“发生了某些事情”的事实。在文档中也称为记录或消息。当您向 Kafka 读取或写入数据时，您将以事件的形式进行操作。从概念上讲，事件具有键，值，时间戳和可选的元数据标题。这是一个示例事件：
事件键：“爱丽丝”
赛事价值：“向 Bob 支付了\$ 200”
活动时间戳记：“ 2020 年 6 月 25 日，下午 2:06”
生产者是那些向 Kafka 发布（写）事件的客户端应用程序，而消费者是那些订阅（读和处理）这些事件的客户端应用程序。在 Kafka 中，生产者和消费者之间完全脱钩并且彼此不可知，这是实现 Kafka 众所周知的高可伸缩性的关键设计元素。例如，生产者永远不需要等待消费者。 Kafka 提供各种保证，例如能够一次准确地处理事件。

活动被组织并持久地存储在主题 topic 中。非常简化，topic 主题类似于文件系统中的文件夹，事件是该文件夹中的文件。
示例主题 topic 名称可以是“付款”。 Kafka 中的主题始终是多生产者和多订阅者：一个主题可以有零个，一个或多个向其写入事件的生产者，以及零个，一个或多个订阅这些事件的使用者。
可以根据需要频繁读取主题中的事件-与传统的消息传递系统不同，使用后不会删除事件。
相反，您可以通过按主题的配置设置来定义 Kafka 将事件保留多长时间，之后旧的事件将被丢弃。
Kafka 的性能相对于数据大小实际上是恒定的，因此长时间存储数据是完全可以的。

主题 topic 已分区 partitioned，这意味着主题 topic 分布在位于不同 Kafka broker 经纪人上的多个“存储桶”中。
数据的这种分布式放置对于可伸缩性非常重要，因为它允许客户端应用程序同时从多个 broker 代理读取数据或向多个 broker 代理写入数据。
将新事件发布到主题 topic 时，实际上会将其附加到主题 topic 的一个分区 partition 中。
具有相同事件密钥（例如，客户或车辆 ID）的事件将写入同一分区，并且 Kafka 保证，给定主题分区的任何使用者都将始终以与写入时完全相同的顺序读取该分区的事件。

Kafka API
除了用于管理和管理任务的命令行工具外，Kafka 还具有用于 Java 和 Scala 的五个核心 API：

Admin API，用于管理和检查主题，代理和其他 Kafka 对象。
生产者 API，用于将事件流发布（写入）到一个或多个 Kafka 主题。
消费者 API 订阅（阅读）一个或多个主题并处理为其产生的事件流。
Kafka Streams API，用于实现流处理应用程序和微服务。它提供了更高级别的功能来处理事件流，包括转换，诸如聚合和联接之类的有状态操作，窗口，基于事件时间的处理等等。从一个或多个主题读取输入，以便生成一个或多个主题的输出，从而有效地将输入流转换为输出流。
Kafka Connect API 可以构建和运行可重用的数据导入/导出连接器，这些连接器从（到）外部系统和应用程序消耗（读取）或生成（写入）事件流，以便它们可以与 Kafka 集成。例如，与诸如 PostgreSQL 之类的关系数据库的连接器可能会捕获对一组表的所有更改。但是，实际上，您通常不需要实现自己的连接器，因为 Kafka 社区已经提供了数百个随时可用的连接器。
从这往哪儿走
要获得有关 Kafka 的动手经验，请遵循快速入门。
要更详细地了解 Kafka，请阅读文档。您还可以选择 Kafka 的书籍和学术论文。
浏览用例，了解我们全球社区中的其他用户如何从 Kafka 中获得价值。
加入当地的卡夫卡聚会小组，观看卡夫卡社区主要会议卡夫卡峰会的演讲。















通过 video_id 获取播放地址
http://i.api.ivideo.sina.com.cn/public/video/play?video_id=309048904&appver=V11219.0910.01&appname=sinaplayer_pc&applt=web&tags=sinaplayer_pc&player=all&jsonp=&plid=2019090301&prid=&uid=&tid=&pid=1&ran=0.11846234288967361&r=https%3A%2F%2Fnews.sina.com.cn%2Fs%2F2019-10-23%2Fdoc-iicezuev4398773.shtml%3Fcre%3Dtianyi%26mod%3Dpchp%26loc%3D1%26r%3D0%26rfunc%3D94%26tj%3Dnone%26tr%3D12&referrer=https%3A%2F%2Fwww.sina.com.cn%2F&ssid=gusr_pc_1571902679255&preload=0&uu=10.71.2.96_1551857951.449138&isAuto=0
通过 video_id 获取视频信息
http://i.api.ivideo.sina.com.cn/public/video/info?appname=sina_cn&appver=1.0.2&applt=web&video_id=308429377&tags=wap_comos
http://i.api.ivideo.sina.com.cn/public/video/info?tags=newsapp&appname=newsapp&appver=newsapp&applt=newsapp&player=app&video_id=309034417

http://i.api.ivideo.sina.com.cn/public/video/play/url?appname=newsapp&appver=newsapp&applt=other&tags=newsapp_Other&video_id=308880793&vid=30888079303&uu=d496c0e4e33c48243f4e1bac1ecad0982f8e0f0b&preload=0&ip=

http://i.api.ivideo.sina.com.cn/public/live/play?appname=sinalottery&appver=1&applt=webm&program_token=1728688
http://i.api.ivideo.sina.com.cn/public/live/info?appname=sinalottery&appver=1&applt=webm&program_token=1728688

通过 docid 获取播放地址
http://i.interface.sina.cn/sports/client/ty_miaopai.d.json?docid=ivhvpwy5960900%2Civhuipp3416236
通过 docid 获取正文页数据
fdoc: http://i.interface.sina.cn/sports/common/fdoc.d.json?app_key=2586208540&format=json&docID=ircuyvi1518374

线上获取视频信息的接口：
vms 视频用了两个接口
1、获取播放数，没有问题能拿到：http://count.video.sina.com.cn/getVideoView?video_ids=352356724%2C352356684%2C352355840%2C352355687%2C352352827%2C352354988%2C352353232%2C352353279%2C352353051%2C352352335%2C352352205%2C352351243%2C352351543%2C352350810%2C352349972%2C352349946%2C352349209&vids=&jsonp=
2、获取视频信息，没有问题，能拿到：http://i.s.video.sina.com.cn/video/info?video_id=352349209&player=app&appname=ivms
秒拍视频用了一个接口：
1、获取视频信息，拿不到播放数，其他没问题：http://i.interface.sina.cn/sports/client/ty_miaopai.d.json?docid=ivhvpwy5960900%2Civhuipp3416236
陈哥，秒拍获取视频的接口和你给的是一样的


yozr57













