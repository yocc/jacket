# Elasticsearch



https://www.elastic.co/guide/en/elasticsearch/reference/current/targz.html#install-macos

https://www.kancloud.cn/yiyanan/elasticsearch_7_6/1670232





### FAQ

```shell
1.
./bin/elasticsearch -d -p ./logs/pid01

Question(1):
[chenchen@localhost elasticsearch-7.14.0]$ ./bin/elasticsearch -d -p ./logs/pid01
[chenchen@localhost elasticsearch-7.14.0]$ ERROR: [3] bootstrap checks failed. You must address the points described in the following [3] lines before starting Elasticsearch.
bootstrap check failure [1] of [3]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65535]
bootstrap check failure [2] of [3]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
bootstrap check failure [3] of [3]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured
ERROR: Elasticsearch did not exit normally - check the logs at /home/chenchen/tmp/elasticsearch-7.14.0/logs/yocc.log

Question(2):
[chenchen@localhost elasticsearch-7.14.0]$ ./bin/elasticsearch -d -p ./logs/pid01
[chenchen@localhost elasticsearch-7.14.0]$ ERROR: [2] bootstrap checks failed. You must address the points described in the following [2] lines before starting Elasticsearch.
bootstrap check failure [1] of [2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
bootstrap check failure [2] of [2]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured
ERROR: Elasticsearch did not exit normally - check the logs at /home/chenchen/tmp/elasticsearch-7.14.0/logs/yocc.log

Question(3):
[chenchen@localhost elasticsearch-7.14.0]$ ./bin/elasticsearch -d -p ./logs/pid01
[chenchen@localhost elasticsearch-7.14.0]$ ERROR: [1] bootstrap checks failed. You must address the points described in the following [1] lines before starting Elasticsearch.
bootstrap check failure [1] of [1]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured
ERROR: Elasticsearch did not exit normally - check the logs at /home/chenchen/tmp/elasticsearch-7.14.0/logs/yocc.log

Solution(1):
bootstrap check failure [1] of [3]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65535]
引导检查失败: 对于 elasticsearch 进程的最大文件描述符数量太少只有 4096, 至少要增加到 65535
通过 操作系统和每个进程 这两个配置同时修改为 1048576(2^20) 来解决:
操作系统: sudo vim /etc/sysctl.conf 文件, 增加 fs.file-max = 1020000, sudo sysctl -p /etc/sysctl.conf
每个进程: sudo vim /etc/security/limits.conf 文件, 增加 * soft nofile 1048576 和 * hard nofile 1048576

Solution(2):
bootstrap check failure [2] of [3]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
引导检查失败: 最大虚拟内存 vm.max_map_count 太少只有 65530(2^16), 至少要增加到 262144(2^18)
通过修改 sudo vim /etc/sysctl.conf 文件中, 增加 vm.max_map_count = 262144 设置 262144(2^18), sudo sysctl -p /etc/sysctl.conf

Solution(3):
bootstrap check failure [1] of [1]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured
引导检查失败: 默认 discovery 设置不适合生产环境使用; 必须至少配置 [discovery.seed_hosts、discovery.seed_providers、cluster.initial_master_nodes] 之一
通过修改 vim config/elasticsearch.yml 配置文件, 增加 discovery.seed_hosts: ["10.222.69.183:9300"] 和 cluster.initial_master_nodes: ["n01"] 配置, 保存退出自动生效
```





### 标准动作

```
# 操作系统
1. Elasticsearch 的 java 进程启动需要特定专用账号, 不能root
2. 对于 .zip 和 .tar.gz 包, 在启动 Elasticsearch 之前将 ulimit -n 65535 设置为 root, 或者在 /etc/security/limits.conf 中将 nofile 设置为 65535.

# Elasticsearch 配置文件

```







```shell
Elasticsearch 是什么?

角色:
. Elastic 栈的核心是 Elasticsearch.
. Elasticsearch 是索引, 搜索, 分析
. Kibana 交互方式探索, 可视化, 共享对数据的洞察，并管理和监控堆栈
. Logstash 和 Beats 有助于收集、聚合和丰富您的数据并将其存储在 Elasticsearch 中

实际应用举例:
向应用或网站添加搜索框
存储和分析日志、指标和安全事件数据
使用机器学习实时自动建模数据的行为
作为存储引擎自动化业务工作流
作为地理信息系统 (GIS) 管理、集成和分析空间信息
作为生物信息学研究工具存储和处理遗传数据
```

```shell
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.14.0-linux-x86_64.tar.gz
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.14.0-linux-x86_64.tar.gz.sha512
sudo yum install perl-Digest-SHA.x86_64 -y
shasum -a 512 -c elasticsearch-7.14.0-linux-x86_64.tar.gz.sha512        # Compares the SHA of the downloaded .tar.gz archive and the published checksum, which should output elasticsearch-{version}-linux-x86_64.tar.gz: OK.
tar -xzf elasticsearch-7.14.0-linux-x86_64.tar.gz
cd elasticsearch-7.14.0/        # This directory is known as $ES_HOME.
```

```shell
action.auto_create_index: .monitoring*,.watches,.triggered_watches,.watcher-history*,.ml*

PUT _cluster/settings
{
  "persistent": {
  	# 允许自动创建名为 my-index-000001 或 index10 的索引, 阻止创建与模式 index1* 匹配的索引, 并允许创建与 ind* 模式匹配的任何其他索引. 模式按指定的顺序匹配.
    "action.auto_create_index": "my-index-000001,index10,-index1*,+ind*" 
  }
}

PUT _cluster/settings
{
  "persistent": {
  	# 完全禁用自动索引创建
    "action.auto_create_index": "false" 
  }
}

PUT _cluster/settings
{
  "persistent": {
  	# 允许自动创建任何索引. 这是默认设置.
    "action.auto_create_index": "true" 
  }
}
```

```shell
./bin/elasticsearch				# 启动 Elasticsearch, stdout, Ctrl-C, # -q 或者 --quiet 关闭在 stdout 上打印 log
./bin/elasticsearch -d -p pid				# -d 守护进程daemon; -p pid记录文件路径;
./bin/elasticsearch -d -p ./logs/pid
./bin/elasticsearch -d -p ./logs/pid -E cluster.name=my_cluster -E node.name=node1
pkill -F pid				# pid路径字符串, 进程ID所在的pid文件路径
pkill -F ./logs/pid

curl -X GET "localhost:9200/?pretty"				# 向 9200 端口发送 HTTP 请求, 验证 Elasticsearch 是否运行中
# 以下为响应
{
  "name" : "Cp8oag6",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "AT69_T_DTp-1qgIJlatQqA",
  "version" : {
    "number" : "7.14.0",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "f27399d",
    "build_date" : "2016-03-30T09:51:41.449Z",
    "build_snapshot" : false,
    "lucene_version" : "8.9.0",
    "minimum_wire_compatibility_version" : "1.2.3",
    "minimum_index_compatibility_version" : "1.2.3"
  },
  "tagline" : "You Know, for Search"
}
[chenchen@localhost elasticsearch-7.14.0]$ curl -X GET "10.222.69.183:9200/?pretty"
{
  "name" : "n01",
  "cluster_name" : "yocc",
  "cluster_uuid" : "fkpY4upDSbehCTAe11zM3A",
  "version" : {
    "number" : "7.14.0",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "dd5a0a2acaa2045ff9624f3729fc8a6f40835aa1",
    "build_date" : "2021-07-29T20:49:32.864135063Z",
    "build_snapshot" : false,
    "lucene_version" : "8.9.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

```shell
1. 默认情况下, Elasticsearch 从 $ES_HOME/config/elasticsearch.yml 文件中加载其配置
2. 在命令行上使用以下 -E 语法在配置文件中指定的任何设置
	 ./bin/elasticsearch -d -Ecluster.name=my_cluster -Enode.name=node_1
```

```shell
目录结构:
默认情况下, 所有文件和目录都包含在 $ES_HOME 解压缩归档文件时创建的目录中.
卸载 Elasticsearch 就是删除 $ES_HOME 目录.
建议更改 config 目录, data 目录和 logs 目录的默认位置, 以便以后不再删除重要数据.

类型        说明                                               默认位置                        设置
home        $ES_HOME                                          解包时创建的目录
bin         二进制脚本位置, 包括启动一个node 和 安装插件            $ES_HOME/bin
conf        elasticsearch.yml 配置文件位置                      $ES_HOME/config                ES_PATH_CONF
data        节点上分配的每个索引/分片的数据文件的位置                $ES_HOME/data                  path.data
logs        日志文件位置                                        $ES_HOME/logs                  path.logs
plugins     插件文件位置. 每个插件都将包含在一个子目录中.            $ES_HOME/plugins
repo        共享文件系统仓库位置. 可以容纳多个位置.                 Not configured                 path.repo
            文件系统存储库可以放置在此处指定的任何目录的任何子目录中.
```

```shell
配置文件位置:
Elasticsearch 有三个配置文件:
. elasticsearch.yml 用于配置 Elasticsearch
. jvm.options 用于配置 Elasticsearch JVM 设置
. log4j2.properties 用于配置 Elasticsearch 日志记录

# 存档分发版, 
	## $ES_HOME/config, 可以通过 ES_PATH_CONF 环境变量修改
  ## ES_PATH_CONF=/path/to/my/config ./bin/elasticsearch
  ## 或者通过 .bashrc 文件 export ES_PATH_CONF 环境变量
# 软件包分发版(RPM包, Debian包)
```

```shell
配置文件格式为 YAML.
1. 
path:
    data: /var/lib/elasticsearch
    logs: /var/log/elasticsearch
    
2. 拍平
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch

3. 有序非标量
discovery.seed_hosts:
   - 192.168.1.10:9300
   - 192.168.1.11
   - seeds.mydomain.com

4. 不常见, 数组非标量
discovery.seed_hosts: ["192.168.1.10:9300", "192.168.1.11", "seeds.mydomain.com"]
```

```shell
# 环境变量替换
node.name:    ${HOSTNAME}
network.host: ${ES_NETWORK_HOST}
# 环境变量的值必须是简单的字符串.
# 使用逗号分隔的字符串提供 Elasticsearch 将解析为列表的值. 
# 例如，Elasticsearch 会将以下字符串拆分为 ${HOSTNAME} 环境变量的值列表:
export HOSTNAME="host1,host2"
```

```shell
# 集群和节点设置类型, 根据它们的配置方式进行分类:
1. 动态, Dynamic
	 生效优先顺序:
	 	1). Transient setting, 临时设置, 重启后会被重置
		2). Persistent setting, 持久设置, 重启也没问题
		3). elasticsearch.yml setting, 常规文件配置
		4). Default setting value, 默认值配置
2. 静态, Static
	 静态设置只能在 未启动 或 关闭 的节点上使用 elasticsearch.yml 进行配置.
	 必须在集群中的每个相关节点上设置静态设置.
```

```shell
# 重要配置, 集群生产环境必须考虑的配置项:
# 默认情况下您应该让您的集群随时做好生产准备.

1. 路径设置
path:
  data: /var/data/elasticsearch
  logs: /var/log/elasticsearch
	# 连通前文共有3个目录, 官方建议放到 $ES_HOME 之外, 以防止删除. 同时强调为了安全 path.data 目录只能由 elasticsearch 系统账号访问. 多路径设置在 7.13 中被废弃.
	# 如果还需要多路径, 您可以在 path.data 中指定多个路径. Elasticsearch 跨所有提供的路径存储节点的数据, 但将每个分片的数据保存在同一路径上. Elasticsearch 不会跨节点的数据路径平衡分片. 单个路径中的高磁盘使用率会触发整个节点的高磁盘使用率水印. 如果触发, 即使节点的其他路径有可用磁盘空间, Elasticsearch 也不会向节点添加分片. 
	# 如果您需要额外的磁盘空间, 我们建议您添加新节点而不是额外的数据路径.
	# 以下是多路径, 但非常不建议.
path:
  data:
    - /mnt/elasticsearch_1
    - /mnt/elasticsearch_2
    - /mnt/elasticsearch_3

2. 集群名设置
	 # 一个节点只有在与集群中的所有其他节点共享其 cluster.name 时才能加入集群. 默认名称是 elasticsearch
cluster.name: logging-prod

3. 节点名称设置
	 # Elasticsearch 使用 node.name 作为 Elasticsearch 特定实例的人类可读标识符. 节点名称在 Elasticsearch 启动时默认为机器的主机名, 但可以在 elasticsearch.yml 中明确配置: 
node.name: prod-data-2

4. 网络主机设置
	 # 默认情况下，Elasticsearch 只绑定回环地址, 例如 127.0.0.1 和 [::1]. 这足以在单个服务器上运行一个或多个节点的集群进行开发和测试, 但弹性生产集群必须涉及其他服务器上的节点. 有许多网络设置，但通常您只需要配置 network.host:
network.host: 192.168.1.10
	 # 当您为 network.host 提供值时, Elasticsearch 假定您正在从开发模式转移到生产模式, 并将许多系统启动检查从警告升级到异常. 查看开发模式和生产模式之间的差异。

5. 发现和集群形成设置
	 # 在进入生产之前配置两个重要的 发现 和 集群形成 设置, 以便集群中的节点可以相互发现并选举一个主节点.
discovery.seed_hosts
	 # 开箱即用, 无需任何网络配置, Elasticsearch 将绑定到可用的环回地址并扫描本地端口 9300 到 9305 以连接运行在同一服务器上的其他节点. 此行为提供了无需进行任何配置的自动集群体验.
	 # 当您想与其他主机上的节点形成集群时, 请使用静态 discovery.seed_hosts 设置. 此设置提供集群中其他节点的列表, 这些节点符合主节点条件, 并且可能处于活动状态且可联系以播种发现过程. 此设置接受集群中所有符合主节点条件的节点的 YAML 序列或地址数组. 每个地址可以是 IP 地址或通过 DNS 解析为一个或多个 IP 地址的主机名. 
discovery.seed_hosts:
   - 192.168.1.10:9300
   - 192.168.1.11 
   - seeds.mydomain.com 
   - [0:0:0:0:0:ffff:c0a8:10c]:9301 
	 ## . 端口是可选的, 默认为 9300, 但可以覆盖.
	 ## . 如果一个主机名解析为多个 IP 地址, 该节点将尝试在所有解析地址处发现其他节点.
	 ## . IPv6 地址必须用方括号括起来.
	 # 如果您的符合主节点的节点没有固定的名称或地址, 请使用替代主机提供程序来动态查找它们的地址.
cluster.initial_master_nodesedit
	 # 当您第一次启动 Elasticsearch 集群时, 集群引导步骤会确定在第一次选举中计票的 主合格节点 集. 在开发模式下, 未配置发现设置, 此步骤由节点本身自动执行.
	 # 由于自动引导本质上是不安全的, 因此在生产模式下启动新集群时, 您必须明确列出应在第一次选举中计算其选票的 主合格节点. 您可以使用 cluster.initial_master_nodes 设置来设置此列表.
	 # 第一次成功形成集群后, 从每个节点的配置中删除 cluster.initial_master_nodes 设置. 重新启动集群或向现有集群添加新节点时, 请勿使用此设置.
discovery.seed_hosts：
   - 192.168.1.10:9300
   - 192.168.1.11
   -seeds.mydomain.com
   - [0:0:0:0:0:ffff:c0a8:10c]:9301
cluster.initial_master_nodes:
   - 主节点-a
   - 主节点-b
   - 主节点-c
	 # 通过 node.name 标识初始主节点, 默认为它们的主机名. 确保 cluster.initial_master_nodes 中的值与 node.name 完全匹配. 如果您使用完全限定域名 (FQDN), 例如 master-node-a.example.com 作为您的节点名称, 则您必须使用此列表中的 FQDN. 相反, 如果 node.name 是没有任何尾随限定符的裸主机名, 则还必须省略 cluster.initial_master_nodes 中的尾随限定符. 请参阅引导集群以及发现和集群形成设置.

6. 堆大小设置
	 # 默认情况下, Elasticsearch 会根据节点的角色和总内存自动设置 JVM 堆大小. 我们建议为大多数生产环境使用默认大小.
	 # 自动堆大小调整需要捆绑的 JDK, 或者如果使用自定义 JRE 位置, 则需要 Java 14 或更高版本的 JRE.
	 # 如果需要, 您可以通过手动设置 JVM 堆大小来覆盖默认大小.

7. JVM 堆转储路径设置
	 # 默认情况下, Elasticsearch 将 JVM 配置为将内存不足异常时的堆转储到默认数据目录($ES_HOME/data). 
	 # 如果此路径不适合接收堆转储, 请修改 jvm.options 中的 -XX:HeapDumpPath=... 条目:
	 	 ## 如果指定目录, JVM 将根据正在运行的实例的 PID 为堆转储生成文件名.
		 ## 如果指定固定文件名而不是目录, 则当 JVM 需要对内存不足异常执行堆转储时, 该文件不得存在. 否则, 堆转储将失败.

8. GC 日志设置
	 # 默认情况下, Elasticsearch 启用垃圾收集 (GC) 日志. 这些在 jvm.options 中配置并输出到与 Elasticsearch 日志相同的默认位置($ES_HOME/logs). 默认配置每 64 MB 轮换一次日志, 最多可消耗 2 GB 的磁盘空间.
	 # 您可以使用 JEP 158：统一 JVM 日志记录中描述的命令行选项重新配置 JVM 日志记录. 除非您直接更改默认的 jvm.options 文件 ,否则除了您自己的设置之外, 还会应用 Elasticsearch 默认配置. 要禁用默认配置, 首先通过提供 -Xlog:disable 选项禁用日志记录, 然后提供您自己的命令行选项. 这将禁用所有 JVM 日志记录, 因此请务必查看可用选项并启用您需要的所有内容.
	 # 要查看原始 JEP 中未包含的更多选项, 请参阅使用 JVM 统一日志记录框架启用日志记录.
	 # 例子
	 # 通过使用一些示例选项创建 $ES_HOME/config/jvm.options.d/gc.options, 将默认 GC 日志输出位置更改为 /opt/my-app/gc.log:
# 关闭所有以前的日志配置
-Xlog：禁用
# JEP 158 中的默认设置, 但使用 `utctime` 而不是 `uptime` 来匹配下一行
-Xlog:all=warning:stderr:utctime,level,tags
# 使用多种选项启用 GC 日志记录到自定义位置
-Xlog:gc*,gc+age=trace,safepoint:file=/opt/my-app/gc.log:utctime,pid,tags:filecount=32,filesize=64m
	 # 配置 Elasticsearch Docker 容器以将 GC 调试日志发送到标准错误 (stderr). 这让容器编排器处理输出. 如果使用 ES_JAVA_OPTS 环境变量, 请指定:
MY_OPTS="-Xlog:disable -Xlog:all=warning:stderr:utctime,level,tags -Xlog:gc=debug:stderr:utctime"
docker run -e ES_JAVA_OPTS="$MY_OPTS" # etc

9. 临时目录设置
	 # 默认情况下, Elasticsearch 使用启动脚本在系统临时目录 /tmp 正下方创建的私有临时目录.
	 # 在 .bashrc 配置文件中 export $ES_TMPDIR 环境变量设置临时目录. 同时此目录应设置权限, 以便只有运行 Elasticsearch 的用户才能访问它

10. JVM 致命错误日志设置
		# 默认情况下, Elasticsearch 将 JVM 配置为将致命错误日志写入默认日志目录($ES_HOME/logs). 
		# 这些是 JVM 在遇到致命错误（例如分段错误）时生成的日志. 如果此路径不适合接收日志, 请修改 jvm.options 中的 -XX:ErrorFile=... 条目.

11. 集群备份
		# 在灾难中, 快照可以防止永久性数据丢失. 快照生命周期管理是对集群进行定期备份的最简单方法. 有关更多信息, 请参阅备份集群.
		# 备份集群的唯一可靠且受支持的方法是拍摄快照. 
		# 您不能通过复制其节点的数据目录来备份 Elasticsearch 集群. 不支持从文件系统级备份恢复任何数据的方法. 如果您尝试从这样的备份中恢复集群, 它可能会失败并报告损坏或丢失文件或其他数据不一致, 或者它可能会成功地丢失一些数据.
```

```shell
重要系统配置

理想情况下, Elasticsearch 应该在服务器上单独运行并使用所有可用资源. 
为此, 您需要将操作系统配置为允许运行 Elasticsearch 的用户访问比默认允许更多的资源. 
在投入生产之前, 必须考虑以下设置: (7)
. Disable swapping, 禁用交换
. Increase file descriptors, 增加文件描述符
. Ensure sufficient virtual memeory, 确保足够的虚拟内存
. Ensure sufficient threads, 确保足够的线程
. JVM DNS cache settings, JVM DNS 缓存设置
. Temporary directory not mounted with noexec, 没有用 noexec 挂载的临时目录
. TCP retransmission timeout, TCP重传超时

开发模式 vs 生产模式
默认情况下，Elasticsearch 假定您在开发模式下工作. 如果上述任何设置未正确配置, 则会在日志文件中写入警告, 但您将能够启动和运行 Elasticsearch 节点.
一旦您配置了诸如 network.host 之类的网络设置, Elasticsearch 就会假定您正在转向生产并将上述警告升级为异常. 这些异常将阻止您的 Elasticsearch 节点启动. 这是一项重要的安全措施, 可确保您不会因为服务器配置错误而丢失数据.
```

```shell
Disable swapping, 禁用交换

禁用交换
大多数操作系统尝试将尽可能多的内存用于文件系统缓存, 并急切地换出未使用的应用程序内存. 
这可能会导致部分 JVM 堆甚至其可执行页面被换出到磁盘.
交换对性能和节点稳定性非常不利, 应该不惜一切代价避免.
它可能导致垃圾收集持续几分钟而不是几毫秒, 并可能导致节点响应缓慢甚至与集群断开连接. 
在弹性分布式系统中, 让操作系统杀死节点更有效.
有三种方法可以禁用交换. 首选选项是完全禁用交换. 如果这不是一个选项, 则是否更喜欢最小化交换与内存锁定取决于您的环境.

(第一种)禁用所有交换文件
通常 Elasticsearch 是唯一运行在机器上的服务, 它的内存使用由 JVM 选项控制. 应该不需要启用交换.
在 Linux 系统上, 您可以通过运行以下命令暂时禁用交换:
sudo swapoff -a
这不需要重启 Elasticsearch
要永久禁用它, 您需要编辑 /etc/fstab 文件并注释掉所有包含单词 swap 的行.

(第二种)配置交换性
Linux 系统上的另一个可用选项是确保将 sysctl 值 vm.swappiness 设置为 1. 
这减少了内核交换的倾向, 并且在正常情况下不应导致交换, 同时仍然允许整个系统在紧急情况下进行交换.

(第三种)启用 bootstrap.memory_lock
另一种选择是在 Linux/Unix 系统上使用 mlockall, 或在 Windows 上使用 VirtualLock, 尝试将进程地址空间锁定到 RAM 中, 防止任何 Elasticsearch 堆内存被换出.
一些平台在使用内存锁时仍然交换堆外内存. 要防止堆外内存交换, 请改为禁用所有交换文件.
要启用内存锁, 请在 elasticsearch.yml 中将 bootstrap.memory_lock 设置为 true:
bootstrap.memory_lock: true
如果 mlockall 尝试分配的内存超过可用内存，它可能会导致 JVM 或 shell 会话退出!
启动 Elasticsearch 后, 您可以通过检查此请求输出中 mlockall 的值来查看此设置是否成功应用:
curl -X GET "localhost:9200/_nodes?filter_path=**.mlockall&pretty"
[chenchen@localhost elasticsearch-7.14.0]$ curl -X GET "10.222.69.183:9200/_nodes?filter_path=**.mlockall&pretty"
{
  "nodes" : {
    "K1Km7KXSQzOmwwtEK5Ve3Q" : {
      "process" : {
        "mlockall" : false
      }
    }
  }
}
如果您看到 mlockall 为 false, 则表示 mlockall 请求失败. 您还将在日志中看到一行包含无法锁定 JVM 内存的更多信息.
在 Linux/Unix 系统上, 最可能的原因是运行 Elasticsearch 的用户没有锁定内存的权限. 这可以授予如下:
.zip 和 .tar.gz
在启动 Elasticsearch 之前将 ulimit -l unlimited 设置为 root. 或者, 在 /etc/security/limits.conf 中将 memlock 设置为无限制:
# allow user 'elasticsearch' mlockall
elasticsearch soft memlock unlimited
elasticsearch hard memlock unlimited
使用 systemd 的系统
在 systemd 配置中将 LimitMEMLOCK 设置为无穷大.
mlockall 失败的另一个可能原因是 JNA 临时目录（通常是 /tmp 的子目录）使用 noexec 选项挂载. 这可以通过使用 ES_JAVA_OPTS 环境变量为 JNA 指定一个新的临时目录来解决:
export ES_JAVA_OPTS="$ES_JAVA_OPTS -Djna.tmpdir=<path>"
./bin/elasticsearch
或者在 jvm.options 配置文件中设置这个 JVM 标志.
```

```shell
Increase file descriptors, 增加文件描述符

文件描述符
Elasticsearch 使用了很多文件描述符或文件句柄. 耗尽文件描述符可能是灾难性的, 并且很可能会导致数据丢失.
确保将运行 Elasticsearch 的用户的打开文件描述符数量限制增加到 65536 或更高.

对于 .zip 和 .tar.gz 包, 在启动 Elasticsearch 之前将 ulimit -n 65535 设置为 root, 或者在 /etc/security/limits.conf 中将 nofile 设置为 65535.
在 macOS 上, 您还必须将 JVM 选项 -XX:-MaxFDLimit 传递给 Elasticsearch, 以便它利用更高的文件描述符限制.
RPM 和 Debian 软件包已将文件描述符的最大数量默认为 65535, 不需要进一步配置. 65536(2^16)
您可以使用 Nodes stats API 检查为每个节点配置的 max_file_descriptors, 包括:
curl -X GET "localhost:9200/_nodes/stats/process?filter_path=**.max_file_descriptors&pretty"
```

```shell
Ensure sufficient virtual memeory, 确保足够的虚拟内存

虚拟内存
Elasticsearch 默认使用 mmapfs 目录来存储其索引. 操作系统对 mmap 计数的默认限制可能太低, 这可能会导致内存不足异常.
在 Linux 上，您可以通过以 root 身份运行以下命令来增加限制:
sysctl -w vm.max_map_count = 262144
要永久设置此值, 请更新 /etc/sysctl.conf 中的 vm.max_map_count 设置. 
要在重新启动后进行验证, 请运行 sysctl vm.max_map_count.
RPM 和 Debian 软件包将自动配置此设置. 无需进一步配置.
```

```shell
Ensure sufficient threads, 确保足够的线程

线程数
Elasticsearch 使用多个线程池来进行不同类型的操作. 重要的是它能够在需要时创建新线程. 确保 Elasticsearch 用户可以创建的线程数至少为 4096.
这可以通过在启动 Elasticsearch 之前将 ulimit -u 4096 作为 root 来完成, 或者通过在 /etc/security/limits.conf 中将 nproc 设置为 4096 来完成.
在 systemd 下作为服务运行时的包分发将自动配置 Elasticsearch 进程的线程数. 不需要额外的配置. 4096(2^12)
```

```shell
JVM DNS cache settings, JVM DNS 缓存设置

DNS 缓存设置
Elasticsearch 运行时有一个安全管理器. 有了安全管理器, JVM 默认无限期地缓存正主机名解析, 默认缓存负主机名解析十秒钟.
Elasticsearch 使用默认值覆盖此行为, 将正查找缓存 60 秒, 并将负查找缓存 10 秒. 这些值应该适用于大多数环境, 包括 DNS 解析随时间变化的环境. 
如果没有, 您可以在 JVM 选项中编辑值 es.networkaddress.cache.ttl 和 es.networkaddress.cache.negative.ttl. 请注意, Java 安全策略中的值 networkaddress.cache.ttl=<timeout> 和 networkaddress.cache.negative.ttl=<timeout> 将被 Elasticsearch 忽略, 除非您删除 es.networkaddress.cache.ttl 和 es 的设置. networkaddress.cache.negative.ttl.
```

```shell
Temporary directory not mounted with noexec, 没有用 noexec 挂载的临时目录

未使用 noexecedit 挂载 JNA 临时目录
这仅与 Linux 相关
Elasticsearch 使用 Java Native Access (JNA) 库来执行一些与平台相关的本机代码.
在 Linux 上, 支持此库的本机代码在运行时从 JNA 存档中提取. 
此代码被提取到 Elasticsearch 临时目录, 该目录默认为 /tmp 的子目录, 并且可以使用 ES_TMPDIR 变量进行配置. 
或者, 可以使用 JVM 标志 -Djna.tmpdir=<path> 控制此位置.
由于本机库作为可执行文件映射到 JVM 虚拟地址空间, 因此不得使用 noexec 挂载此代码提取位置的底层挂载点, 因为这会阻止 JVM 进程将此代码映射为可执行文件. 
在某些强化的 Linux 安装中, 这是 /tmp 的默认挂载选项. 底层挂载是使用 noexec 挂载的一个迹象是. 在启动时 JNA 将无法加载, 并出现 java.lang.UnsatisfiedLinkerError 异常, 并显示一条消息, 无法从共享对象映射段.
请注意, 异常消息可能因 JVM 版本而异. 此外, 依赖于通过 JNA 执行本机代码的 Elasticsearch 组件将失败并显示消息, 表明这是因为 JNA 不可用. 如果您看到此类错误消息, 则必须重新挂载用于 JNA 的临时目录, 以免使用 noexec 挂载.
```

```shell
TCP retransmission timeout, TCP重传超时

TCP重传超时
每对 Elasticsearch 节点通过多个 TCP 连接进行通信, 这些 TCP 连接保持打开状态, 直到其中一个节点关闭或节点之间的通信因底层基础设施故障而中断.
TCP 通过隐藏通信应用程序的临时网络中断, 在偶尔不可靠的网络上提供可靠的通信. 
在通知发件人任何问题之前, 您的操作系统会多次重新传输任何丢失的消息. 
Elasticsearch 必须等待重传发生, 并且只有在操作系统决定放弃时才能做出反应. 因此, 用户还必须等待一系列重传完成.

大多数 Linux 发行版默认重新传输任何丢失的数据包 15 次. 重传会呈指数衰减, 因此这 15 次重传需要 900 多秒才能完成. 
这意味着 Linux 需要花费数分钟才能使用此方法检测网络分区或故障节点.

Linux 默认允许通过网络进行通信, 这些网络可能会经历很长时间的丢包, 但这种默认设置对于大多数 Elasticsearch 安装使用的高质量网络来说是过度的, 甚至是有害的.
当集群检测到节点故障时 它会通过重新分配丢失的分片、重新路由搜索以及可能选择新的主节点来做出反应. 
高可用集群必须能够及时发现节点故障, 这可以通过减少允许的重传次数来实现.
与远程集群的连接也应该比 Linux 默认允许的更快地检测故障. 因此 Linux 用户应该减少 TCP 重传的最大次数.

您可以通过以 root 身份运行以下命令将 TCP 重传的最大数量减少到 5. 五次重传对应于大约六秒的超时.
sysctl -w net.ipv4.tcp_retries2=5
要永久设置此值, 请更新 /etc/sysctl.conf 中的 net.ipv4.tcp_retries2 设置. 要在重新启动后进行验证, 请运行 sysctl net.ipv4.tcp_retries2.

此设置适用于所有 TCP 连接, 也会影响与 Elasticsearch 集群以外的系统通信的可靠性. 如果您的集群通过低质量网络与外部系统通信, 那么您可能需要为 net.ipv4.tcp_retries2 选择更高的值. 因此, Elasticsearch 不会自动调整此设置.

相关配置
Elasticsearch 还实现了自己的内部健康检查, 其超时时间比 Linux 上的默认重传超时要短得多. 由于这些是应用程序级别的健康检查, 因此它们的超时必须考虑到应用程序级别的影响, 例如垃圾收集暂停. 您不应减少与这些应用程序级健康检查相关的任何超时.
您还必须确保您的网络基础设施不会干扰节点之间的长期连接, 即使这些连接看起来是空闲的. 在达到一定年龄时断开连接的设备是 Elasticsearch 集群问题的常见来源, 不得使用.
```

```shell
配置操作系统设置
. 临时保存用 ulimit
. 永久保存在 /etc/security/limits.conf 中

1. ulimitedit (临时)
在 Linux 系统上, ulimit 可用于临时更改资源限制.
在切换到将运行 Elasticsearch 的用户之前, 通常需要将限制设置为 root. 
例如, 要将打开的文件句柄数(ulimit -n)设置为 65,536, 您可以执行以下操作:
sudo su  
ulimit -n 65535 
su elasticsearch 
. 变成 root
. 更改最大打开文件数
. 成为 elasticsearch 用户为了启动 Elasticsearch
新限制仅在当前会话期间应用.
您可以使用 ulimit -a 查询所有当前应用的限制.

2. /etc/security/limits.conf
在 Linux 系统上, 可以通过编辑 /etc/security/limits.conf 文件为特定用户设置永久限制.
要将 elasticsearch 用户的最大打开文件数设置为 65,535, 请将以下行添加到 limits.conf 文件中:
elasticsearch  -  nofile  65535
此更改只会在 elasticsearch 用户下次打开新会话时生效.
```

```shell
数据输入: 文档和索引
Elasticsearch 是一个分布式文档存储, 不是将信息存储为列状数据的行(类似MySQL), 而是存储已序列化为 JSON 文档的复杂数据结构.
当集群中有多个 Elasticsearch 节点时, 存储的文档分布在整个集群中, 并且可以从任何节点立即访问.

存储文档后, 它会被编入索引, 并且可以近乎实时地 near real-time （在 1 秒内）完全搜索(其实是非实时搜索).
使用倒排索引 inverted index 的数据结构, 支持非常快速的全文搜索 full-text searches.
倒排索引列出出现在任何文档中的每个唯一单词, 并标识每个单词出现在的所有文档.

索引是文档的优化集合, 这里每个文档都是字段的集合, 这些字段是包含数据的键值对.
默认情况下, Elasticsearch 索引每个字段中的所有数据, 每个索引字段都有一个专用的、优化的数据结构.
例如, 文本字段存储在倒排索引中, 数值和地理字段存储在 BKD 树中. 
使用每个字段的数据结构来组合和返回搜索结果的能力使 Elasticsearch 如此之快.

启用动态映射后, Elasticsearch 会自动给字段建索引.(具有无模式的能力)

但是, 最终, 您比 Elasticsearch 更了解您的数据以及您希望如何使用它. 您可以定义规则来控制动态映射并显式定义映射以完全控制字段的存储和索引方式.

您能自定义映射:
区分全文字符串字段和精确值字符串字段
执行特定于语言的文本分析
优化部分匹配的字段
使用自定义日期格式
使用无法自动检测的数据类型，例如 geo_point 和 geo_shape

为了不同的目的以不同的方式索引相同的字段通常很有用.
例如, 您可能希望将字符串字段索引为用于全文搜索的文本字段和用于排序或聚合数据的关键字字段.
或者, 您可以选择使用多个语言分析器来处理包含用户输入的字符串字段的内容.

在索引期间应用于全文字段的分析链也在搜索时使用. 
当您查询全文字段时, 在索引中查找术语之前, 查询文本会经过相同的分析.
```

```shell
信息输出: 搜索和分析
虽然您可以将 Elasticsearch 用作文档存储并检索文档及其元数据, 但真正的强大之处在于能够轻松访问构建在 Apache Lucene 搜索引擎库上的全套搜索功能.

Elasticsearch 提供了一个简单、一致的 REST API 来管理您的集群以及索引和搜索您的数据. 
出于测试目的, 您可以直接从命令行或通过 Kibana 中的开发人员控制台轻松提交请求.
在您的应用程序中, 您可以将 Elasticsearch 客户端用于您选择的语言: Java、JavaScript、Go、.NET、PHP、Perl、Python 或 Ruby.

搜索您的数据
Elasticsearch REST API 支持结构化查询、全文查询和将两者结合的复杂查询.
结构化查询类似于您可以在 SQL 中构造的查询类型.
例如, 您可以搜索员工索引中的性别和年龄字段, 并按租用日期字段对匹配项进行排序.
全文查询查找与查询字符串匹配的所有文档, 并按相关性排序返回它们——它们与您的搜索词的匹配程度.

除了搜索单个术语外, 您还可以执行短语搜索、相似性搜索和前缀搜索, 并获得自动完成建议.

有要搜索的地理空间数据或其他数字数据吗?
Elasticsearch 在支持高性能地理和数值查询的优化数据结构中索引非文本数据.

您可以使用 Elasticsearch 的综合 JSON 样式查询语言 (Query DSL) 访问所有这些搜索功能.
您还可以构建 SQL 样式的查询以在 Elasticsearch 内部搜索和聚合本地数据, 而 JDBC 和 ODBC 驱动程序使广泛的第三方应用程序能够通过 SQL 与 Elasticsearch 交互.

分析你的数据
Elasticsearch 聚合使您能够构建复杂的数据摘要并深入了解关键指标、模式和趋势. 
聚合不仅可以找到众所周知的 "大海捞针", 还可以回答以下问题:

大海捞针有多少针?
针的平均长度是多少?
按制造商划分的针的中位数长度是多少?
在过去的六个月中, 每一年都向大海捞针添加了多少针?
您还可以使用聚合来回答更微妙的问题, 例如:

您最受欢迎的针头制造商是哪些?
是否有任何异常或异常的针团?
因为聚合利用了用于搜索的相同数据结构, 所以它们也非常快.
这使您能够实时分析和可视化数据. 
您的报告和仪表板会随着数据的变化而更新, 以便您可以根据最新信息采取行动.

更重要的是, 聚合与搜索请求一起运行.
您可以在单个请求中对相同数据同时搜索文档、过滤结果和执行分析.
并且由于聚合是在特定搜索的上下文中计算的, 因此您不仅会显示所有 70 号针的计数, 还显示了与用户搜索条件匹配的 70 号针的计数 -- 例如, 所有尺寸 70 不粘绣花针.

但是等等, 还有更多
想要自动化时间序列数据的分析吗? 您可以使用机器学习功能创建数据中正常行为的准确基线并识别异常模式.
通过机器学习, 您可以检测:
. 与值、计数或频率的时间偏差相关的异常
. 统计稀有度
. 某个群体成员的异常行为
最好的部分是? 您无需指定算法、模型或其他与数据科学相关的配置即可执行此操作.
```

```shell
可扩展性和弹性: 集群clusters、节点nodes和分片shards

Elasticsearch 旨在始终可用并根据您的需求进行扩展. 
它通过自然分布来做到这一点. 
您可以将服务器（节点）添加到集群以增加容量, Elasticsearch 会自动在所有可用节点之间分配您的数据和查询负载.
无需大修您的应用程序, Elasticsearch 知道如何平衡多节点集群以提供可扩展性和高可用性. 节点越多越好.

这是如何运作的?
在幕后, Elasticsearch 索引实际上只是一个或多个物理分片的逻辑分组, 其中每个分片实际上是一个独立的索引.
通过将索引中的文档分布在多个分片中, 并将这些分片分布在多个节点上, Elasticsearch 可以确保冗余, 这既可以防止硬件故障, 又可以在将节点添加到集群时增加查询容量.
随着集群的增长(或缩小), Elasticsearch 会自动迁移分片以重新平衡集群.

有两种类型的分片: 主分片和副本. 
索引中的每个文档都属于一个主分片.
副本分片是主分片的副本.
副本提供数据的冗余副本, 以防止硬件故障并增加处理读取请求(如搜索或检索文档)的容量.

索引中的主分片数量在创建索引时是固定的, 但副本分片的数量可以随时更改, 而不会中断索引或查询操作.

这取决于...
关于分片大小和为索引配置的主分片数量, 有许多性能考虑和权衡.
分片越多, 维护这些索引的开销就越大.
分片大小越大, 当 Elasticsearch 需要重新平衡集群时, 移动分片所需的时间就越长.

查询大量小分片会使每个分片的处理速度更快, 但更多查询意味着更多开销, 因此查询较少数量的较大分片可能会更快. 简而言之……视情况而定.

作为起点:
. 旨在将平均分片大小保持在几 GB 到几十 GB 之间. 对于具有基于时间的数据的用例, 通常会看到 20GB 到 40GB 范围内的分片.
. 避免大量碎片问题. 一个节点可以容纳的分片数量与可用的堆空间成正比. 作为一般规则, 每 GB 堆空间的分片数应小于 20.
为您的用例确定最佳配置的最佳方法是使用您自己的数据和查询进行测试.

发生灾害时
出于性能原因, 集群中的节点需要位于同一网络上. 
跨不同数据中心的节点平衡集群中的分片只需要太长时间.
但是高可用性架构要求您避免将所有鸡蛋放在一个篮子里.
在一个位置发生重大中断的情况下, 另一个位置的服务器需要能够接管. 无缝. 
答案? 跨集群复制 (CCR).

CCR 提供了一种将索引从主集群自动同步到可用作热备份的辅助远程集群的方法.
如果主集群出现故障，辅助集群可以接管.
您还可以使用 CCR 创建辅助集群, 以便为您的用户提供地理位置邻近的读取请求.

跨集群复制是主动-被动的. 
主集群上的索引是活动的领导者索引, 处理所有写请求.
复制到辅助集群的索引是只读的跟随者.

照顾和喂养
与任何企业系统一样, 您需要工具来保护、管理和监控您的 Elasticsearch 集群.
集成到 Elasticsearch 中的安全、监控和管理功能使您能够将 Kibana 用作管理集群的控制中心.
数据汇总和索引生命周期管理等功能可帮助您随着时间的推移智能地管理数据
```

```shell
引导检查
总的来说，我们有很多用户遇到意外问题的经验，因为他们没有配置重要的设置。在以前版本的 Elasticsearch 中，其中一些设置的错误配置被记录为警告。可以理解，用户有时会错过这些日志消息。为了确保这些设置得到应有的关注，Elasticsearch 在启动时进行了引导检查。

这些引导程序检查会检查各种 Elasticsearch 和系统设置，并将它们与 Elasticsearch 操作安全的值进行比较。如果 Elasticsearch 处于开发模式，则任何失败的引导程序检查都会在 Elasticsearch 日志中显示为警告。如果 Elasticsearch 处于生产模式，任何失败的引导程序检查都会导致 Elasticsearch 拒绝启动。

有一些引导程序检查总是被强制执行，以防止 Elasticsearch 在不兼容的设置下运行。这些检查是单独记录的。

开发与生产模式
默认情况下，Elasticsearch 绑定到用于 HTTP 和传输（内部）通信的环回地址。这对于下载和使用 Elasticsearch 以及日常开发来说都很好，但对于生产系统来说却毫无用处。要加入集群，必须可以通过传输通信访问 Elasticsearch 节点。要通过非环回地址加入集群，节点必须将传输绑定到非环回地址并且不使用单节点发现。因此，如果一个 Elasticsearch 节点不能通过非环回地址与另一台机器形成集群，我们认为它处于开发模式，否则如果它可以通过非环回地址加入集群，则认为它处于生产模式。

注意HTTP和transport可以通过http.host和transport.host独立配置；这对于将单个节点配置为可通过 HTTP 访问以用于测试目的而无需触发生产模式非常有用。

单节点发现
我们认识到一些用户需要将传输绑定到外部接口以测试他们对传输客户端的使用。对于这种情况，我们提供了发现类型单节点（通过将discovery.type设置为单节点来配置）；在这种情况下，一个节点将选举自己成为主节点，并且不会与任何其他节点一起加入集群。

强制引导检查
如果您在生产中运行单个节点，则可以逃避引导程序检查（通过不将传输绑定到外部接口，或通过将传输绑定到外部接口并将发现类型设置为单节点）。对于这种情况，您可以通过在 JVM 选项中将系统属性 es.enforce.bootstrap.checks 设置为 true 来强制执行引导程序检查。如果您处于这种特定情况，我们强烈建议您这样做。此系统属性可用于独立于节点配置强制执行引导程序检查。
```

```shell
堆大小检查
默认情况下，Elasticsearch 根据节点的角色和总内存自动调整 JVM 堆的大小。如果您手动覆盖默认大小并以不同的初始和最大堆大小启动 JVM，则 JVM 可能会在系统使用期间调整堆大小时暂停。如果启用 bootstrap.memory_lock，JVM 会在启动时锁定初始堆大小。如果初始堆大小不等于最大堆大小，则某些 JVM 堆在调整大小后可能不会被锁定。为避免这些问题，请使用等于最大堆大小的初始堆大小启动 JVM。
```

```shell
文件描述符检查
文件描述符是用于跟踪打开的“文件”的 Unix 构造。但在 Unix 中，一切都是文件。例如，“文件”可以是物理文件、虚拟文件（例如 /proc/loadavg）或网络套接字。 Elasticsearch 需要大量文件描述符（例如，每个分片由多个段和其他文件组成，以及与其他节点的连接等）。此引导程序检查在 OS X 和 Linux 上强制执行。要通过文件描述符检查，您可能必须配置文件描述符。
```

```shell
内存锁检查
当 JVM 进行主要的垃圾收集时，它会接触堆的每一页。如果这些页面中的任何一个被换出到磁盘，它们将不得不被换回内存。这会导致大量磁盘抖动，而 Elasticsearch 更愿意将其用于服务请求。有多种方法可以将系统配置为禁止交换。一种方法是通过 mlockall (Unix) 或虚拟锁 (Windows) 请求 JVM 将堆锁定在内存中。这是通过 Elasticsearch 设置 bootstrap.memory_lock 完成的。但是，在某些情况下，此设置可以传递给 Elasticsearch，但 Elasticsearch 无法锁定堆（例如，如果 elasticsearch 用户没有 memlock unlimited）。内存锁定检查验证是否启用了 bootstrap.memory_lock 设置，JVM 是否能够成功锁定堆。要通过内存锁检查，您可能需要配置 bootstrap.memory_lock。
```

```shell
检查的最大线程数
Elasticsearch 通过将请求分解为多个阶段并将这些阶段交给不同的线程池执行程序来执行请求。 Elasticsearch 中有针对各种任务的不同线程池执行器。因此，Elasticsearch 需要能够创建大量线程。最大线程数检查确保Elasticsearch进程在正常使用情况下有权创建足够多的线程。此检查仅在 Linux 上强制执行。如果您在 Linux 上，要通过最大线程数检查，您必须配置您的系统以允许 Elasticsearch 进程创建至少 4096 个线程的能力。这可以通过 /etc/security/limits.conf 使用 nproc 设置来完成（请注意，您可能还必须增加 root 用户的限制）。
```

```shell
最大文件大小检查
作为单个分片组件的段文件和作为 translog 组件的 translog 生成可能会变得很大（超过数 GB）。在 Elasticsearch 进程可以创建的文件的最大大小受到限制的系统上，这可能会导致写入失败。因此，这里最安全的选项是最大文件大小不受限制，这就是最大文件大小引导程序检查强制执行的内容。要通过最大文件检查，您必须将系统配置为允许 Elasticsearch 进程写入无限大小的文件。这可以通过 /etc/security/limits.conf 使用 fsize 设置为无限制来完成（请注意，您可能还必须增加 root 用户的限制）。
```

```shell
最大大小虚拟内存检查
Elasticsearch 和 Lucene 使用 mmap 非常有效地将索引的部分映射到 Elasticsearch 地址空间。这将某些索引数据保留在 JVM 堆之外，但保留在内存中以便快速访问。为了使其有效，Elasticsearch 应该有无限的地址空间。最大大小虚拟内存检查强制 Elasticsearch 进程具有无限的地址空间，并且仅在 Linux 上强制执行。要通过最大大小虚拟内存检查，您必须配置您的系统以允许 Elasticsearch 进程拥有无限地址空间的能力。这可以通过在 /etc/security/limits.conf 中添加 <user> - 来实现。这可能还需要您增加 root 用户的限制。
```

```shell
最大地图计数检查
继续上一点，为了有效地使用 mmap，Elasticsearch 还需要能够创建许多内存映射区域。最大映射计数检查检查内核是否允许进程具有至少 262,144 个内存映射区域，并且仅在 Linux 上强制执行。要通过最大映射计数检查，您必须通过 sysctl 将 vm.max_map_count 配置为至少 262144。

或者，仅当您使用 mmapfs 或 hybridfs 作为索引的存储类型时，才需要最大映射计数检查。如果您不允许使用 mmap，则不会强制执行此引导程序检查。
```

```shell
客户端 JVM 检查
OpenJDK 派生的 JVM 提供了两种不同的 JVM：客户端 JVM 和服务器 JVM。这些 JVM 使用不同的编译器从 Java 字节码生成可执行的机器代码。客户端 JVM 针对启动时间和内存占用进行了调整，而服务器 JVM 则针对最大化性能进行了调整。两个 VM 之间的性能差异可能很大。客户端 JVM 检查确保 Elasticsearch 不在客户端 JVM 内运行。要通过客户端 JVM 检查，您必须使用服务器 VM 启动 Elasticsearch。在现代系统和操作系统上，服务器 VM 是默认设置。
```

```shell
使用串行收集器检查
针对不同工作负载的 OpenJDK 派生 JVM 有各种垃圾收集器。特别是串行收集器最适合单逻辑 CPU 机器或极小的堆，这两种机器都不适合运行 Elasticsearch。将串行收集器与 Elasticsearch 结合使用可能会对性能造成破坏性影响。串行收集器检查确保 Elasticsearch 未配置为与串行收集器一起运行。要通过串行收集器检查，您不能使用串行收集器启动 Elasticsearch（无论它来自您正在使用的 JVM 的默认设置，还是您已使用 -XX:+UseSerialGC 明确指定它）。请注意，Elasticsearch 附带的默认 JVM 配置将 Elasticsearch 配置为在 JDK14 及更高版本中使用 G1GC 垃圾收集器。对于较早的 JDK 版本，配置默认为 CMS 收集器。
```

```shell
系统调用过滤器检查
Elasticsearch 根据操作系统（例如 Linux 上的 seccomp）安装各种风格的系统调用过滤器。安装这些系统调用过滤器是为了防止执行与分叉相关的系统调用的能力，作为抵御对 Elasticsearch 的任意代码执行攻击的防御机制。系统调用过滤器检查确保如果启用了系统调用过滤器，则它们已成功安装。要通过系统调用过滤器检查，您必须修复系统上阻止系统调用过滤器安装的任何配置错误（检查您的日志），或者通过将 bootstrap.system_call_filter 设置为 false 来禁用系统调用过滤器，风险自负。
```

```shell
OnError 和 OnOutOfMemoryError 检查
JVM 选项 OnError 和 OnOutOfMemoryError 允许在 JVM 遇到致命错误 (OnError) 或 OutOfMemoryError (OnOutOfMemoryError) 时执行任意命令。但是，默认情况下，Elasticsearch 系统调用过滤器 (seccomp) 处于启用状态，这些过滤器可防止分叉。因此，使用 OnError 或 OnOutOfMemoryError 和系统调用过滤器是不兼容的。 OnError 和 OnOutOfMemoryError 检查会在使用这些 JVM 选项之一并启用系统调用过滤器时阻止 Elasticsearch 启动。始终执行此检查。要通过此检查，请不要启用 OnError 或 OnOutOfMemoryError；相反，升级到 Java 8u92 并使用 JVM 标志 ExitOnOutOfMemoryError。虽然这不具有 OnError 和 OnOutOfMemoryError 的全部功能，但启用 seccomp 将不支持任意分叉。
```

```shell
抢先体验
OpenJDK 项目提供了即将发布的版本的抢先体验快照。这些版本不适合生产。抢先体验检查检测这些抢先体验快照。要通过此检查，您必须在 JVM 的发布版本上启动 Elasticsearch。
```

```shell
G1GC检查
众所周知，JDK 8 附带的 HotSpot JVM 的早期版本存在一些问题，当启用 G1GC 收集器时，这些问题可能会导致索引损坏。受影响的版本早于 JDK 8u40 附带的 HotSpot 版本。 G1GC 检查检测这些 HotSpot JVM 的早期版本。
```

```shell
所有权限检查
all 权限检查确保引导期间使用的安全策略不会将 java.security.AllPermission 授予 Elasticsearch。使用授予的所有权限运行等效于禁用安全管理器。
```

```
发现配置检查编辑
默认情况下，当 Elasticsearch 首次启动时，它会尝试发现在同一主机上运行的其他节点。如果在几秒钟内没有发现任何选定的主节点，那么 Elasticsearch 将形成一个集群，其中包含发现的任何其他节点。在开发模式下无需任何额外配置即可形成此集群很有用，但这不适用于生产，因为可能形成多个集群并因此丢失数据。

此引导程序检查可确保发现未使用默认配置运行。可以通过设置至少以下属性之一来满足它：

discovery.seed_hosts
discovery.seed_providers
cluster.initial_master_nodes
```

```shell
X-Packedit 的引导程序检查
除了 Elasticsearch 引导程序检查之外，还有特定于 X-Pack 功能的检查。

加密敏感数据检查
如果您使用 Watcher 并选择加密敏感数据（通过将 xpack.watcher.encrypt_sensitive_data 设置为 true），您还必须在安全设置存储中放置一个密钥。

要通过此引导程序检查，您必须在集群中的每个节点上设置 xpack.watcher.encryption_key。有关更多信息，请参阅在 Watcher 中加密敏感数据。

PKI领域检查
如果您使用 Elasticsearch 安全功能和公钥基础设施 (PKI) 领域，则必须在集群上配置传输层安全性 (TLS) 并在网络层（传输或 http）上启用客户端身份验证。有关详细信息，请参阅 PKI 用户身份验证和设置基本安全性和 HTTPS。

要通过此引导程序检查，如果启用了 PKI 领域，则必须配置 TLS 并在至少一个网络通信层上启用客户端身份验证。

角色映射检查
如果您使用本机或文件领域以外的领域对用户进行身份验证，则必须创建角色映射。这些角色映射定义了分配给每个用户的角色。

如果使用文件来管理角色映射，则必须配置一个 YAML 文件并将其复制到集群中的每个节点。默认情况下，角色映射存储在 ES_PATH_CONF/role_mapping.yml 中。或者，您可以为每种类型的领域指定不同的角色映射文件，并在 elasticsearch.yml 文件中指定其位置。有关更多信息，请参阅使用角色映射文件。

要通过此引导程序检查，角色映射文件必须存在且必须有效。角色映射文件中列出的专有名称 (DN) 也必须有效。

SSL/TLS 检查
如果您启用 Elasticsearch 安全功能，除非您有试用许可证，否则您必须为节点间通信配置 SSL/TLS。

使用环回接口的单节点集群没有此要求。有关更多信息，请参阅配置安全性。

要通过此引导程序检查，您必须在集群中设置 SSL/TLS。

令牌 SSL 检查
如果您使用 Elasticsearch 安全功能并且启用了内置令牌服务，则必须将集群配置为对 HTTP 接口使用 SSL/TLS。需要 HTTPS 才能使用令牌服务。

特别是，如果在elasticsearch.yml 文件中将xpack.security.authc.token.enabled 设置为true，则还必须将xpack.security.http.ssl.enabled 设置为true。有关这些设置的更多信息，请参阅安全设置和高级 HTTP 设置。

要通过此引导程序检查，您必须启用 HTTPS 或禁用内置令牌服务。
```

```shell

MVVM与MVC的区别有：1、mvvm各部分的通信是双向的，而mvc各部分通信是单向的；2、mvvm是真正将页面与数据逻辑分离放到js里去实现，而mvc里面未分离.

https://blog.csdn.net/weixin_38118016/article/details/90605126
https://www.jianshu.com/p/297e13045605


Elasticsearch: 高性能, 高可用, 易扩展, 的分布式搜索引擎.
. 高性能: 
. 高可用: 高可用就是冗余; 1. 服务可用性, 多个节点(即多个java实例); 2. 数据可用性, 多个节点分布冗余;
. 易扩展: 水平扩容
. 分布式: 1. 支持PB容量; 2. 个别节点坏了, 整个集群也能正常工作;
. 搜索引擎



集群:
集群名称为集群唯一标识
两种配置方式:
. 通过配置文件(永久), cluster.name: my-application
. cli(命令行单次), -E cluster.name = test
一个分布式集群可以有一个或多个节点.
Cluster State: es 集群相关的数据称为 cluster state, 主要记录如下信息:
	. 节点信息, 比如节点名称, 链接地址等
	. 索引信息, 比如索引名称, 配置等


节点:
一个节点就是一个 ElasticSearch 的实例, 本质上就是一个Java进程.
每一个节点在启动之后, 会分配一个UID, 保存在data目录下.
每个节点都有完备的数据.
引入 replica 副本.
启动一个节点, 或者是新增一个节点: 
	./bin/elasticsearch -E cluster.name=cluster_xxx -E node.name=node_xxx
两种配置方式:
. 通过配置文件(永久), node.name: node-1
. cli(命令行单次), -E node.name = node1
节点类型:
Master Node:
	.  一个集群只有一个 Master Node, 谁是 Master Node 可以通过修改 cluster state 来修改
	. cluster state 存储在每个节点 node 上, master node 维护最新版本, 并同步给其他节点 node
	. master node 是通过集群内的所有节点选举产生的, 可以被选举的节点称为 master-eligible 节点, node.master:true
Master eligible Node
Date Node
	. 存储数据的节点, 默认所有节点都是, node.data:true
Ingest Node
Coordinating Node
	. 负责处理请求的节点, 所有节点的默认角色, 不能取消.
Hot & Warm Node
Machine Learning Node
Tribe Node


索引:
一组文档的集合

分片:
分片是PB级数据的基石.
分片是集群分发数据的单元.
分布式的搜索引擎, 所以索引通常都会分解成不同部分, 而这些分布在不同节点的数据就是分片.
分片分布在任意节点上, 分片的数量在索引创建时指定, 默认5个, 之后不能再修改.
分片不能太大, 影响性能, 一般50G以内, 虽未要求.
分片在 es 运行时不能修改调整, 只能重新创建新的索引来替代 reindex
分片中尽可能使用基于时间的索引来管理数据保留期.
ES 自动管理和组织分片, 也会在必要时对分片进行再平衡.
分片分为两种类型:
. 主分片, Primary Shard, 索引创建时指定, 不能修改, 除非 reindex; 一个分片是一个运行的 Lucene 实例
. 副本分片, Replica Shard, 是主分片的副本拷贝, 由主分片同步, 可以有多个, 提高读能力, 副本分片可以随时动态调整.

./bin/elasticsearch -d -p ./logs/pid01 -E cluster.name=yocc -E node.name=n01
./bin/elasticsearch -d -p ./logs/pid02 -E cluster.name=yocc -E node.name=n02
pkill -F ./logs/pid01
pkill -F ./logs/pid02

副本
ES 默认为一个索引创建 5 个主分片, 并分别为其创建一个副本分片.
也就是说每个索引都由 5 个主分片成本, 而每个主分片都相应的有一个 copy
分片及副本的分配将是高可用及快速搜索响应的设计核心.主分片与副本都能处理查询请求, 它们的唯一区别在于只有主分片才能处理索引请求.
副本对搜索性能非常重要，同时用户也可在任何时候添加或删除副本。额外的副本能给带来更大的容量, 更高的呑吐能力及更强的故障恢复能力

如上图，有集群两个节点，并使用了默认的分片配置. ES自动把这5个主分片分配到2个节点上, 而它们分别对应的副本则在完全不同的节点上。其中 node1 有某个索引的分片1、2、3和副本分片4、5，node2 有该索引的分片4、5和副本分片1、2、3。

    当数据被写入分片时，它会定期发布到磁盘上的不可变的 Lucene 分段中用于查询。随着分段数量的增长，这些分段会定期合并为更大的分段。 此过程称为合并。 由于所有分段都是不可变的，这意味着所使用的磁盘空间通常会在索引期间波动，因为需要在删除替换分段之前创建新的合并分段。 合并可能非常耗费资源，特别是在磁盘I / O方面。

    分片是 Elasticsearch 集群分发数据的单元。 Elasticsearch 在重新平衡数据时可以移动分片的速度，例如发生故障后，将取决于分片的大小和数量以及网络和磁盘性能。

    注1：避免使用非常大的分片，因为这会对群集从故障中恢复的能力产生负面影响。 对分片的大小没有固定的限制，但是通常情况下很多场景限制在 50GB 的分片大小以内。
    注2：当在ElasticSearch集群中配置好你的索引后, 你要明白在集群运行中你无法调整分片设置. 既便以后你发现需要调整分片数量, 你也只能新建创建并对数据进行重新索引(reindex)(虽然reindex会比较耗时, 但至少能保证你不会停机).
    主分片的配置与硬盘分区很类似, 在对一块空的硬盘空间进行分区时, 会要求用户先进行数据备份, 然后配置新的分区, 最后把数据写到新的分区上。

    注3：尽可能使用基于时间的索引来管理数据保留期。 根据保留期将数据分组到索引中。 基于时间的索引还可以轻松地随时间改变主分片和副本的数量，因为可以更改下一个要生成的索引。


```









```shell
https://www.elastic.co/guide/cn/elasticsearch/guide/current/data-in-data-out.html
面向对象编程语言如此流行的原因之一是对象帮我们表示和处理现实世界具有潜在的复杂的数据结构的实体, 到目前为止, 一切都很完美!
但是当我们需要存储这些实体时问题来了, 传统上, 我们以行和列的形式存储数据到关系型数据库中, 相当于使用电子表格. 正因为我们使用了这种不灵活的存储媒介导致所有我们使用对象的灵活性都丢失了.
但是否我们可以将我们的对象按对象的方式来存储？这样我们就能更加专注于 "使用" 数据, 而不是在电子表格的局限性下对我们的应用建模. 我们可以重新利用对象的灵活性.
一个 "对象" 是基于特定语言的内存的数据结构. 为了通过网络发送或者存储它, 我们需要将它表示成某种标准的格式. JSON 是一种以人可读的文本表示对象的方法. 它已经变成 NoSQL 世界交换数据的事实标准. 当一个对象被序列化成为 JSON, 它被称为一个 "JSON 文档".
Elastcisearch 是分布式的 "文档" 存储. 它能存储和检索复杂的数据结构 -- 序列化成为JSON文档 -- 以 "实时" 的方式. 换句话说, 一旦一个文档被存储在 Elasticsearch 中, 它就是可以被集群中的任意节点检索到.
```

```shell
三个必须的元数据元素如下:
_index: 文档在哪存放. 我们的数据是被存储和索引在 "分片" 中，而一个索引仅仅是逻辑上的命名空间, 这个命名空间由一个或者多个分片组合在一起.
_type: 文档表示的对象类别, string, 在索引中对数据进行逻辑分区.
_id: 文档唯一标识, string. 一个文档的 _index 、 _type 和 _id 唯一标识一个文档. 自动生成的 ID 是 URL-safe、 基于 Base64 编码且长度为20个字符的 GUID 字符串. 这些 GUID 字符串由可修改的 FlakeID 模式生成, 这种模式允许多个节点并行生成唯一 ID, 且互相之间的冲突概率几乎为零.

1. 文档加入索引, 文档可以被 "索引" —— 存储和使文档可被搜索
PUT /{index}/{type}/{id}
{
  "field": "value",
  ...
}

例子, 如果我们的索引称为 website, 类型称为 blog, 并且选择 123 作为 ID, 那么索引请求应该是下面这样:
curl -X PUT "localhost:9200/website/blog/123?pretty" -H 'Content-Type: application/json' -d'
{
  "title": "My first blog entry",
  "text":  "Just trying this out...",
  "date":  "2014/01/01"
}
'
Elasticsearch 响应体如下所示:
{
   "_index":    "website",
   "_type":     "blog",
   "_id":       "123",
   "_version":  1,
   "created":   true
}

无 _id:
curl -X POST "localhost:9200/website/blog/?pretty" -H 'Content-Type: application/json' -d'
{
  "title": "My second blog entry",
  "text":  "Still trying this out...",
  "date":  "2014/01/01"
}
'

2. 取回一个文档, _index, _type, 和 _id
curl -X GET "localhost:9200/website/blog/123?pretty&pretty"
响应:
{
  "_index" :   "website",
  "_type" :    "blog",
  "_id" :      "123",
  "_version" : 1,
  "found" :    true,
  "_source" :  {
      "title": "My first blog entry",
      "text":  "Just trying this out...",
      "date":  "2014/01/01"
  }
}
_source 字段不会被格式化, 和之前传入时的格式一样. 
2.1. 如何判断是否查询到, {"found": true/false}, 通过 -i 参数获取相应头来判断.
curl -i -XGET http://localhost:9200/website/blog/124?pretty
响应:
HTTP/1.1 404 Not Found
Content-Type: application/json; charset=UTF-8
Content-Length: 83

{
  "_index" : "website",
  "_type" :  "blog",
  "_id" :    "124",
  "found" :  false
}
2.2. 返回文档的一部分字段
curl -X GET "localhost:9200/website/blog/123?_source=title,text&pretty"
响应:
{
  "_index" :   "website",
  "_type" :    "blog",
  "_id" :      "123",
  "_version" : 1,
  "found" :   true,
  "_source" : {
      "title": "My first blog entry" ,
      "text":  "Just trying this out..."
  }
}
2.3. 只返回 _source
curl -X GET "localhost:9200/website/blog/123/_source?pretty"
响应:
{
   "title": "My first blog entry",
   "text":  "Just trying this out...",
   "date":  "2014/01/01"
}

3. 检查文档是否存在
如果只想检查一个文档是否存在 -- 根本不想关心内容 -- 那么用 HEAD 方法来代替 GET 方法. HEAD 请求没有返回体, 只返回一个 HTTP 请求报头:
curl -i -XHEAD http://localhost:9200/website/blog/123
响应:
HTTP/1.1 200 OK
Content-Type: text/plain; charset=UTF-8
Content-Length: 0
或者:
HTTP/1.1 404 Not Found
Content-Type: text/plain; charset=UTF-8
Content-Length: 0

4. 更新索引中的文档
在 Elasticsearch 中文档是 不可改变 的, 不能修改它们. 相反, 如果想要更新现有的文档, 需要 重建索引 或者进行替换.
curl -X PUT "localhost:9200/website/blog/123?pretty" -H 'Content-Type: application/json' -d'
{
  "title": "My first blog entry",
  "text":  "I am starting to get the hang of this...",
  "date":  "2014/01/02"
}
'
响应:
{
  "_index" :   "website",
  "_type" :    "blog",
  "_id" :      "123",
  "_version" : 2,
  "created":   false 
}
created 标志设置成 false, 是因为相同的索引、类型和 ID 的文档已经存在.

. 从旧文档构建 JSON
. 更改该 JSON
. 删除旧文档
. 索引一个新文档
唯一的区别在于, update API 仅仅通过一个客户端请求来实现这些步骤, 而不需要单独的 get 和 index 请求.

4. 创建新文档
_id 不写, 由系统自动创建, 那么 _index, _type, _id组合肯定不同.
4.1. PUT /website/blog/123?op_type=create
{ ... }
4.2. PUT /website/blog/123/_create
{ ... }
如果创建新文档的请求 "成功执行", Elasticsearch 会返回元数据和一个 201 Created 的 HTTP 响应码.
如果具有相同的 _index 、 _type 和 _id 的文档 "已经存在", Elasticsearch 将会返回 409 Conflict 响应码, 以及如下的错误信息:
{
   "error": {
      "root_cause": [
         {
            "type": "document_already_exists_exception",
            "reason": "[blog][123]: document already exists",
            "shard": "0",
            "index": "website"
         }
      ],
      "type": "document_already_exists_exception",
      "reason": "[blog][123]: document already exists",
      "shard": "0",
      "index": "website"
   },
   "status": 409
}

6. 删除文档
curl -X DELETE "localhost:9200/website/blog/123?pretty"
如果找到该文档, Elasticsearch 将要返回一个 200 ok 的 HTTP 响应码, 和一个类似以下结构的响应体. 注意, 字段 _version 值已经增加:
{
  "found" :    true,
  "_index" :   "website",
  "_type" :    "blog",
  "_id" :      "123",
  "_version" : 3
}
如果文档没有找到, 我们将得到 404 Not Found 的响应码和类似这样的响应体:
{
  "found" :    false,
  "_index" :   "website",
  "_type" :    "blog",
  "_id" :      "123",
  "_version" : 4
}
即使文档不存在 ( Found 是 false ), _version 值仍然会增加.
正如已经在更新整个文档中提到的, 删除文档不会立即将文档从磁盘中删除, 只是将文档标记为已删除状态. 随着你不断的索引更多的数据, Elasticsearch 将会在后台清理标记为已删除的文档.

7. 冲突
在数据库领域中, 有两种方法通常被用来确保并发更新时变更不会丢失:
悲观并发控制: 这种方法被关系型数据库广泛使用, 它假定有变更冲突可能发生, 因此阻塞访问资源以防止冲突. 一个典型的例子是读取一行数据之前先将其锁住, 确保只有放置锁的线程能够对这行数据进行修改.
乐观并发控制: Elasticsearch 中使用的这种方法假定冲突是不可能发生的, 并且不会阻塞正在尝试的操作. 然而, 如果源数据在读写当中被修改, 更新将会失败. 应用程序接下来将决定该如何解决冲突. 例如, 可以重试更新、使用新的数据、或者将相关情况报告给用户.

8. 乐观并发控制
Elasticsearch 是分布式的. 当文档创建、更新或删除时, 新版本的文档必须复制到集群中的其他节点. Elasticsearch 也是异步和并发的, 这意味着这些复制请求被并行发送, 并且到达目的地时也许 "顺序是乱的".
Elasticsearch 使用这个 _version 号来确保变更以正确顺序得到执行. 如果旧版本的文档在新版本之后到达, 它可以被简单的忽略.

[chenchen@localhost elasticsearch-7.14.0]$ curl -X PUT "localhost:9200/website/blog/1/_create?pretty" -H 'Content-Type: application/json' -d'
> {
>   "title": "My first blog entry",
>   "text":  "Just trying this out..."
> }
> '
{
  "_index" : "website",
  "_type" : "blog",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
[chenchen@localhost elasticsearch-7.14.0]$ curl -X GET "localhost:9200/website/blog/1?pretty"
{
  "_index" : "website",
  "_type" : "blog",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 0,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "title" : "My first blog entry",
    "text" : "Just trying this out..."
  }
}
[chenchen@localhost elasticsearch-7.14.0]$ curl -X PUT "localhost:9200/website/blog/1?version=1&pretty" -H 'Content-Type: application/json' -d'
> {
>   "title": "My first blog entry",
>   "text":  "Starting to get the hang of this..."
> }
> '
{
  "error" : {
    "root_cause" : [
      {
        "type" : "action_request_validation_exception",
        "reason" : "Validation Failed: 1: internal versioning can not be used for optimistic concurrency control. Please use `if_seq_no` and `if_primary_term` instead;"
      }
    ],
    "type" : "action_request_validation_exception",
    "reason" : "Validation Failed: 1: internal versioning can not be used for optimistic concurrency control. Please use `if_seq_no` and `if_primary_term` instead;"
  },
  "status" : 400
}
[chenchen@localhost elasticsearch-7.14.0]$ curl -X PUT "localhost:9200/website/blog/1?pretty" -H 'Content-Type: application/json' -d'
{
  "title": "My first blog entry",
  "text":  "Starting to get the hang of this..."
}
'
{
  "_index" : "website",
  "_type" : "blog",
  "_id" : "1",
  "_version" : 2,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 1,
  "_primary_term" : 1
}
[chenchen@localhost elasticsearch-7.14.0]$ curl -X PUT "localhost:9200/website/blog/1?version=1&pretty" -H 'Content-Type: application/json' -d'
{
  "title": "My first blog entry",
  "text":  "Starting to get the hang of this......"
}
'
{
  "error" : {
    "root_cause" : [
      {
        "type" : "action_request_validation_exception",
        "reason" : "Validation Failed: 1: internal versioning can not be used for optimistic concurrency control. Please use `if_seq_no` and `if_primary_term` instead;"
      }
    ],
    "type" : "action_request_validation_exception",
    "reason" : "Validation Failed: 1: internal versioning can not be used for optimistic concurrency control. Please use `if_seq_no` and `if_primary_term` instead;"
  },
  "status" : 400
}
[chenchen@localhost elasticsearch-7.14.0]$ curl -X PUT "localhost:9200/website/blog/2?version=5&version_type=external&pretty" -H 'Content-Type: application/json' -d'
> {
>   "title": "My first external blog entry",
>   "text":  "Starting to get the hang of this..."
> }
> '
{
  "_index" : "website",
  "_type" : "blog",
  "_id" : "2",
  "_version" : 5,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 2,
  "_primary_term" : 1
}
[chenchen@localhost elasticsearch-7.14.0]$ curl -X PUT "localhost:9200/website/blog/2?version=10&version_type=external&pretty" -H 'Content-Type: application/json' -d'
> {
>   "title": "My first external blog entry",
>   "text":  "This is a piece of cake..."
> }
> '
{
  "_index" : "website",
  "_type" : "blog",
  "_id" : "2",
  "_version" : 10,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 3,
  "_primary_term" : 1
}

9. 文档的部分更新
更新一个文档的方法是检索并修改它，然后重新索引整个文档.
文档是不可变的：他们不能被修改，只能被替换.
检索-修改-重建索引 的处理过程.
区别在于这个过程发生在分片内部，这样就避免了多次请求的网络开销。通过减少检索和重建索引步骤之间的时间，我们也减少了其他进程的变更带来冲突的可能性。
增加字段 tags 和 views 到我们的博客文章，如下所示:
[chenchen@localhost ~]$ curl -X POST "localhost:9200/website/blog/1/_update?pretty" -H 'Content-Type: application/json' -d'
> {
>    "doc" : {
>       "tags" : [ "testing" ],
>       "views": 0
>    }
> }
> '
{
  "_index" : "website",
  "_type" : "blog",
  "_id" : "1",
  "_version" : 3,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 4,
  "_primary_term" : 1
}
[chenchen@localhost ~]$ curl -X GET "localhost:9200/website/blog/1?pretty"
{
  "_index" : "website",
  "_type" : "blog",
  "_id" : "1",
  "_version" : 3,
  "_seq_no" : 4,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "title" : "My first blog entry",
    "text" : "Starting to get the hang of this...",
    "views" : 0,
    "tags" : [
      "testing"
    ]
  }
}
[chenchen@localhost ~]$ curl -X POST "localhost:9200/website/blog/1/_update?pretty" -H 'Content-Type: application/json' -d'
> {
>    "script" : "ctx._source.views+=1"
> }
> '
{
  "_index" : "website",
  "_type" : "blog",
  "_id" : "1",
  "_version" : 4,
  "result" : "updated",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 5,
  "_primary_term" : 1
}
[chenchen@localhost ~]$ curl -X GET "localhost:9200/website/blog/1?pretty"
{
  "_index" : "website",
  "_type" : "blog",
  "_id" : "1",
  "_version" : 4,
  "_seq_no" : 5,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "title" : "My first blog entry",
    "text" : "Starting to get the hang of this...",
    "views" : 1,
    "tags" : [
      "testing"
    ]
  }
}

10. 取回多条文档
[chenchen@localhost ~]$ curl -X GET "localhost:9200/_mget?pretty" -H 'Content-Type: application/json' -d'
{
   "docs" : [
      {
         "_index" : "website",
         "_type" :  "blog",
         "_id" :    2
      },
      {
         "_index" : "website",
         "_type" :  "blog",
         "_id" :    1
      }
   ]
}
'
{
  "docs" : [
    {
      "_index" : "website",
      "_type" : "blog",
      "_id" : "2",
      "_version" : 10,
      "_seq_no" : 3,
      "_primary_term" : 1,
      "found" : true,
      "_source" : {
        "title" : "My first external blog entry",
        "text" : "This is a piece of cake..."
      }
    },
    {
      "_index" : "website",
      "_type" : "blog",
      "_id" : "1",
      "_version" : 4,
      "_seq_no" : 5,
      "_primary_term" : 1,
      "found" : true,
      "_source" : {
        "title" : "My first blog entry",
        "text" : "Starting to get the hang of this...",
        "views" : 1,
        "tags" : [
          "testing"
        ]
      }
    }
  ]
}
[chenchen@localhost ~]$ curl -X GET "localhost:9200/website/blog/_mget?pretty" -H 'Content-Type: application/json' -d'
{
   "docs" : [
      { "_id" : 2 },
      { "_id" :   1 }
   ]
}
'
{
  "docs" : [
    {
      "_index" : "website",
      "_type" : "blog",
      "_id" : "2",
      "_version" : 10,
      "_seq_no" : 3,
      "_primary_term" : 1,
      "found" : true,
      "_source" : {
        "title" : "My first external blog entry",
        "text" : "This is a piece of cake..."
      }
    },
    {
      "_index" : "website",
      "_type" : "blog",
      "_id" : "1",
      "_version" : 4,
      "_seq_no" : 5,
      "_primary_term" : 1,
      "found" : true,
      "_source" : {
        "title" : "My first blog entry",
        "text" : "Starting to get the hang of this...",
        "views" : 1,
        "tags" : [
          "testing"
        ]
      }
    }
  ]
}

11.
action/metadata 行指定 哪一个文档 做 什么操作 。

action 必须是以下选项之一:

create
如果文档不存在，那么就创建它。详情请见 创建新文档。
index
创建一个新文档或者替换一个现有的文档。详情请见 索引文档 和 更新整个文档。
update
部分更新一个文档。详情请见 文档的部分更新。
delete
删除一个文档。详情请见 删除文档。
metadata 应该指定被索引、创建、更新或者删除的文档的 _index 、 _type 和 _id 。


```



```
路由一个文档到一个分片中
当索引一个文档的时候，文档会被存储到一个主分片中。
实际上，这个过程是根据下面这个公式决定的: shard = hash(routing) % number_of_primary_shards
routing 是一个可变值，默认是文档的 _id ，也可以设置成一个自定义的值。 routing 通过 hash 函数生成一个数字，然后这个数字再除以 number_of_primary_shards （主分片的数量）后得到 余数 。这个分布在 0 到 number_of_primary_shards-1 之间的余数，就是我们所寻求的文档所在分片的位置。
这就解释了为什么我们要在创建索引的时候就确定好主分片的数量 并且永远不会改变这个数量：因为如果数量变化了，那么所有之前路由的值都会无效，文档也再也找不到了。
```



































