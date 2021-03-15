6. IK分词器原理

本质上是词典分词，在内存中初始化一个词典，然后在分词过程中逐个读取字符，和字典中的字符相匹配，把文档中的所有词语拆分出来的过程

7. Lucence 内部结构是什么
   索引(Index)： 在Lucene中一个索引是放在一个文件夹中的。 如上图，同一文件夹中的所有的文件构成一个Lucene索引。
   段(Segment)： 一个索引可以包含多个段，段与段之间是独立的，添加新文档可以生成新的段，不同的段可以合并。
   segments.gen和segments_X是段的元数据文件，也即它们保存了段的属性信息。
   文档(Document)： 文档是我们建索引的基本单位，不同的文档是保存在不同的段中的，一个段可以包含多篇文档。
   新添加的文档是单独保存在一个新生成的段中，随着段的合并，不同的文档合并到同一个段中。
   域(Field)： 一篇文档包含不同类型的信息，可以分开索引，比如标题，时间，正文，作者等，都可以保存在不同的域里。 不同域的索引方式可以不同，在真正解析域的存储的时候，我们会详细解读。
   词(Term)： 词是索引的最小单位，是经过词法分析和语言处理后的字符串。
8. Elasticsearch 了解多少，说说你们公司 es 的集群架构，索引数据大小，分片有多少，以及一些调优手段。elasticsearch 的倒排索引是什么。
   ElasticSearch（简称ES）是一个分布式、Restful的搜索及分析服务器，设计用于分布式计算；能够达到实时搜索，稳定，可靠，快速。和Apache Solr一样，它也是基于Lucence的索引服务器，而ElasticSearch对比Solr的优点在于：
   轻量级：安装启动方便，下载文件之后一条命令就可以启动。
   Schema free：可以向服务器提交任意结构的JSON对象，Solr中使用schema.xml指定了索引结构。
   多索引文件支持：使用不同的index参数就能创建另一个索引文件，Solr中需要另行配置。
   分布式：Solr Cloud的配置比较复杂。
9. Elasticsearch 与 Solr 的比较：

二者安装都很简单；
Solr 利用 Zookeeper 进行分布式管理，而 Elasticsearch 自身带有分布式协调管理功能;
Solr 支持更多格式的数据，而 Elasticsearch 仅支持json文件格式；
Solr 官方提供的功能更多，而 Elasticsearch 本身更注重于核心功能，高级功能多有第三方插件提供；
Solr 在传统的搜索应用中表现好于 Elasticsearch，但在处理实时搜索应用时效率明显低于 Elasticsearch。
Solr 是传统搜索应用的有力解决方案，但 Elasticsearch 更适用于新兴的实时搜索应用。
10、Elasticsearch 索引数据多了怎么办，如何调优，部署。

使用bulk API
初次索引的时候，把 replica 设置为 0
增大 threadpool.index.queue_size
增大 indices.memory.index_buffer_size
增大 index.translog.flush_threshold_ops
增大 index.translog.sync_interval
增大 index.engine.robin.refresh_interval
详细：http://www.jianshu.com/p/5eeeeb4375d4
11、Elasticsearch是如何实现Master选举的

Elasticsearch的选主是ZenDiscovery模块负责的，主要包含Ping（节点之间通过这个RPC来发现彼此）和Unicast（单播模块包含一个主机列表以控制哪些节点需要ping通）这两部分；
对所有可以成为master的节点（node.master: true）根据nodeId字典排序，每次选举每个节点都把自己所知道节点排一次序，然后选出第一个（第0位）节点，暂且认为它是master节点。
如果对某个节点的投票数达到一定的值（可以成为master节点数n/2+1）并且该节点自己也选举自己，那这个节点就是master。否则重新选举一直到满足上述条件。
12、Elasticsearch是如何避免脑裂现象的

当集群中master候选的个数不小于3个（node.master:
true）。可以通过discovery.zen.minimum_master_nodes
这个参数的设置来避免脑裂，设置为(N/2)+1。
假如集群master候选节点为2的时候，这种情况是不合理的，最好把另外一个node.master改成false。如果我们不改节点设置，还是套上面的(N/2)+1公式，此时discovery.zen.minimum_master_nodes应该设置为2。这就出现一个问题，两个master备选节点，只要有一个挂，就选不出master了。
13、客户端在和集群连接时，如何选择特定的节点执行请求的？
TransportClient利用transport模块远程连接一个elasticsearch集群。它并不加入到集群中，只是简单的获得一个或者多个初始化的transport地址，并以 轮询 的方式与这些地址进行通信。

14、在并发情况下，Elasticsearch如果保证读写一致？

可以通过版本号使用乐观并发控制，以确保新版本不会被旧版本覆盖，由应用层来处理具体的冲突；
另外对于写操作，一致性级别支持quorum/one/all，默认为quorum，即只有当大多数分片可用时才允许写操作。但即使大多数可用，也可能存在因为网络等原因导致写入副本失败，这样该副本被认为故障，分片将会在一个不同的节点上重建。
对于读操作，可以设置replication为sync(默认)，这使得操作在主分片和副本分片都完成后才会返回；如果设置replication为async时，也可以通过设置搜索请求参数_preference为primary来查询主分片，确保文档是最新版本。
15、Elasticsearch在部署时，对Linux的设置有哪些优化方法？

64 GB 内存的机器是非常理想的， 但是32 GB 和16 GB 机器也是很常见的。少于8 GB 会适得其反。
如果你要在更快的 CPUs 和更多的核心之间选择，选择更多的核心更好。多个内核提供的额外并发远胜过稍微快一点点的时钟频率。
如果你负担得起 SSD，它将远远超出任何旋转介质。 基于 SSD 的节点，查询和索引性能都有提升。如果你负担得起，SSD 是一个好的选择。
即使数据中心们近在咫尺，也要避免集群跨越多个数据中心。绝对要避免集群跨越大的地理距离。
请确保运行你应用程序的 JVM 和服务器的 JVM 是完全一样的。 在 Elasticsearch 的几个地方，使用 Java 的本地序列化。
通过设置gateway.recover_after_nodes、gateway.expected_nodes、gateway.recover_after_time可以在集群重启的时候避免过多的分片交换，这可能会让数据恢复从数个小时缩短为几秒钟。
Elasticsearch 默认被配置为使用单播发现，以防止节点无意中加入集群。只有在同一台机器上运行的节点才会自动组成集群。最好使用单播代替组播。
不要随意修改垃圾回收器（CMS）和各个线程池的大小。
把你的内存的（少于）一半给 Lucene（但不要超过 32 GB！），通过ES_HEAP_SIZE 环境变量设置。
内存交换到磁盘对服务器性能来说是致命的。如果内存交换到磁盘上，一个 100 微秒的操作可能变成 10 毫秒。 再想想那么多 10 微秒的操作时延累加起来。 不难看出 swapping 对于性能是多么可怕。
Lucene 使用了大量的文件。同时，Elasticsearch 在节点和 HTTP 客户端之间进行通信也使用了大量的套接字。 所有这一切都需要足够的文件描述符。你应该增加你的文件描述符，设置一个很大的值，如 64,000。
补充：索引阶段性能提升方法

使用批量请求并调整其大小：每次批量数据 5–15 MB 大是个不错的起始点。
段和段合并：Elasticsearch 默认值是 20 MB/s，对机械磁盘应该是个不错的设置。如果你用的是 SSD，可以考虑提高到 100–200 MB/s。如果你在做批量导入，完全不在意搜索，你可以彻底关掉合并限流。另外还可以增加 index.translog.flush_threshold_size 设置，从默认的 512 MB 到更大一些的值，比如 1 GB，这可以在一次清空触发的时候在事务日志里积累出更大的段。
如果你的搜索结果不需要近实时的准确度，考虑把每个索引的index.refresh_interval 改到30s。
如果你在做大批量导入，考虑通过设置index.number_of_replicas: 0 关闭副本。
16、对于GC方面，在使用Elasticsearch时要注意什么？

SEE：https://elasticsearch.cn/article/32
倒排词典的索引需要常驻内存，无法GC，需要监控data node上segment memory增长趋势。
各类缓存，field cache, filter cache, indexing cache, bulk queue等等，要设置合理的大小，并且要应该根据最坏的情况来看heap是否够用，也就是各类缓存全部占满的时候，还有heap空间可以分配给其他任务吗？避免采用clear cache等“自欺欺人”的方式来释放内存。
避免返回大量结果集的搜索与聚合。确实需要大量拉取数据的场景，可以采用scan & scroll api来实现。
cluster stats驻留内存并无法水平扩展，超大规模集群可以考虑分拆成多个集群通过tribe node连接。
想知道heap够不够，必须结合实际应用场景，并对集群的heap使用情况做持续的监控。
————————————————
版权声明：本文为CSDN博主「ThisLX」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/li521wang/article/details/89337282
