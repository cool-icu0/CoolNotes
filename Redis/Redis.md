# NoSQL
> NoSQL(Not Only SQL )，意即“不仅仅是SQL”，泛指**非关系型的数据库。 **
> NoSQL 不依赖业务逻辑方式存储，而以简单的**key-value模式存储**。
> ⦁	不遵循SQL标准。
> ⦁	不支持ACID。
> ⦁	远超于SQL的性能。
> NoSQL适用场景 
> ⦁	对数据高并发的读写
> ⦁	海量数据的读写
> ⦁	对数据高可扩展性的
> **Redis是NoSQL类型的数据库**

**为什么会使用到NoSQL数据库？**



## 技术的分类
1、解决功能性的问题：Java、Jsp、RDBMS、Tomcat、HTML、Linux、JDBC、SVN
2、解决扩展性的问题：Struts、Spring、SpringMVC、Hibernate、Mybatis
3、解决性能的问题：**NoSQL**、Java线程、Hadoop、Nginx、MQ、ElasticSearch
## 发展
随着Web2.0的时代的到来，用户访问量大幅度提升，同时产生了大量的用户数据。加上后来的智能移动设备的普及，所有的互联网平台都面临了巨大的性能挑战。

用户的访问量过大会造成服务器的cup及内存的压力，而服务器去读取数据会造成数据库的IO压力。
![image-20220726231129462](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220726231129462.png)
解决CPU及内存压力，就有负载均衡Nginx之类的技术。
![](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/shayu931/RedisBook01.png)
**解决数据库的IO压力，就有了缓存数据库Redis**
![](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/shayu931/RedisBook02.png)
:::

# Redis
:::info
### Redis是一个开源的key-value存储模式的非关系型数据库。
⦁	五大数据类型，包括string(字符串)、list(链表)、set(集合)、zset(sorted set --有序集合)和hash（哈希类型）。
⦁	这些数据类型都支持push/pop、add/remove及取交集并集和差集及更丰富的操作，而且这些操作都是原子性的。
⦁	在此基础上，Redis支持各种不同方式的排序。
⦁	数据都是缓存在内存中。
⦁	Redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件。
⦁	在此基础上实现了master-slave(主从)同步。
:::
# Redis的安装
> Redis官方网站	Redis中文官方网站
> [http://redis.io](http://redis.io)	[http://redis.cn/](http://redis.cn/)
> [Redis安装运行（linux系统）](https://blog.csdn.net/qq_45721579/article/details/125296341?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166116110616781667890159%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=166116110616781667890159&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~times_rank-1-125296341-null-null.142^v42^pc_rank_34,185^v2^control&utm_term=redis%E5%AE%89%E8%A3%85%E8%BF%90%E8%A1%8C&spm=1018.2226.3001.4187)
> [Redis安装（Windows环境下Redis安装 不推荐）](https://blog.csdn.net/weixin_52799373/article/details/124251235?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166114721416782248538065%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=166114721416782248538065&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~times_rank-2-124251235-null-null.142^v42^pc_rank_34,185^v2^control&utm_term=redis%E5%AE%89%E8%A3%85&spm=1018.2226.3001.4187)
> 官方推荐使用linux系统，Windows的版本停止更新了。

# Redis相关知识
:::success

- 端口6379
- 默认16个数据库，类似数组下标从0开始，初始默认使用0号库
   - 使用命令select   <dbid>来切换数据库。如: select 8 
   - 统一密码管理，所有库同样密码。
- Redis是_**单线程**_+多路IO复用技术  （版本更新之后有多线程了，redis6）
   - 多路复用是指使用一个线程来检查多个文件描述符（Socket）的就绪状态，比如调用select和poll函数，传入多个文件描述符，如果有一个文件描述符就绪，则返回，否则阻塞直到超时。得到就绪状态后进行真正的操作可以在同一个线程里执行，也可以启动线程执行（比如使用线程池）（基于操作系统的介绍）
   - 我的理解就是，多个客人找redis黄牛买票，redis黄牛去联系cup车站，有票就卖给redis黄牛，没票cup车站可以做其他的事情，redis黄牛和客人继续等待（描述的不是很准确大概是这样）
   :::
# 常用五大数据类型
:::warning
## redis键（key）操作

-  keys _ 查看当前库所有key（匹配： keys _1） 
-  exists key 判断某个key是否存在 
-  type key 查看你的key是什么类型 
-  del key 删除指定的key数据 
-  unlink key 根据value选择非阻塞删除（仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作） 
-  expire key 设置过期时间 
-  ttl key查看key还有多少时间过期（-2代表已经过期，-1表示永不过期） 
- dbsize查看当前数据库的key的数量
- flushdb清空当前库
- flushall通杀全部库
:::
# String（Redis字符串）
> string是redis最基本的类型，一个key对应一个value
> string类型是二进制安全的。意味着redis的string可以包含任何数据，比如jpg图片或者序列化的对象
> string类型是redis最基本的数据类型，一个redis中字符串最多可以是512M
> ## 常用命令
> - set <key><value>添加键值对
> - get <key> 获取值
> - append <key><value> 来给当前键值对追加一个值，追加到原值的末尾
> - stelen<key> 获得值的长度
> - setnx <key><value>只有在key不存在时 设置key的值
> - incr <key> 将key中储存的数字值增1，只能对数字值操作，如果为空，新增值为1
> - decr <key> 将key中储存的数字值减1，只能对数字值操作，如果为空，新增值为-1
> - incrby/decrby <key><步长>将key中储存的值增减，自定义步长就是加减多少。
> - mset <key1><value1><key2><value2>同时设置一个或多个key-value对
> - mget 同时获取一个或多个value
> - msetnx <key1><value1><key2><value2> 同时设置一个或多个键值对，当且仅当所有给定key都不存在
> - getrange <key><起始位置><结束位置> 获得值的范围，类似Java中的substring，前包，后包
> - setrange <key><起始位置><value> 用value覆写key所存储的字符串值，从<起始位置>开始（索引从0开始）
> - setex <key><过期时间><value> 设置键值的用时，设置过期时间，单位秒
> - getset <key><value> 以新换旧，设置了新值同时获得旧值
> #### 原子性：
> 所谓原子操作是指不会被线程调度机制打断的操作，这种操作一旦开始，就一直运行到结束，中间不会有任何context switch（切换到另一个进程）
> 1、在单线程中，能够在单条指令中完成的操作都可以认为是“原子操作”（注意这里有引号），因为中断只能发生于指令之间
> 2、在多线程中，不能被其他进程打断的操作就叫原子操作
> Redis单命令的原子性主要得益于Redis的单线程
> ## 数据结构
> 	string的数据结构为简单动态字符串，是可以修改的字符串，内部结构实现上类似于Java的ArrayList，采用预分配冗余空间的方式来减少内存的频繁分配。
> ![image-20220727123548075](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220727123548075.png)
> 如图中所示，内部为当前字符串实际分配的空间capacity 一般要高于实际字符串长度len，当字符串长度小于1M时，扩容都是加倍现有的空间，如果超过1M，扩容一次只会多扩1M的空间，需要注意的是，字符串最大长度为512M

# List（Redis列表）
:::danger
redis列表是简单的字符串列表，按照插入顺序排序，你可以添加一个元素到列表的头部（左边）或者尾部（右边）
![image-20220727123852045](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220727123852045.png)
他的底层实际是个双向链表，对两端的操作性能很高，通过索引下标的操作中间节点的性能会较差。

## 常用命令

- lpush/rpush <key><value1><value2><value3> 从左边/右边插入一个或多个值
- lrange <key><start><stop> 按照索引下标获得元素（从左到右）
- lrange <key> 0 -1  看查该key下的所有元素
- lpop/rpop <key> 从左边/右边吐出一个值 。 **值在健在，值光健亡**
- rpoplpush <key1><key2> 从key1列表右边吐出一个值，插到key2列表左边
- lindex <key><index> 按照索引下标获得元素（从左到右）
- llen <key> 获得列表长度
- linsert <key> before <value><newvalue> 在value的后面插入<newvalue>插入值
- lrem <key><n><value> 从左边删除n个value
- lset<key><index><value> 将列表key下标为index的值替换为value
## 数据结构
list的数据结构为快速链表quickList
首先在列表元素较少的情况下会使用一块连续的内存存储，这个结构是ziplist，也即是压缩列表
他将所有的元素紧挨着一起存储，分配的是一块连续的内存
当数据量比较多的时候才会改成quicklist
因为普通的链表需要的附加指针空间太大，会比较浪费空间。比如这个列表里寸的只是int类型的数据，结构上还需要两个额外的指针prev和next
![image-20220727135259344](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220727135259344.png)
Redis将链表和ziplist结合起来组成了quicklist。也就是将多个ziplist使用双向指针串起来使用。这样既满足了快速的插入删除性能，又不会出现太大的空间冗余。
:::

# Sit（Redis集合）
> redis set 对外提供的功能于list类似是一个列表的功能，特殊之处在于set是可以**自动排重**的，当你需要存储一个列表数据，又不希望出现重复数据时。set是一个很好的选择，并且set提供了判断某个成员是否在一个set集合内的重要接口，这个也是list所不能提供的
> redis的set是string类型的无序集合，他底层其实是一个value为null的哈希表，所以添加，删除，查找的**复杂度都是O（1）**
> ## 常用命令
> - sadd <key><value1><value2>... 将一个或多个member元素加入到集合key中，已经存在的menmber元素将被忽略
> - smenmbers<key> 取出该集合的所有值
> - sismember <key><value>判断集合key是否含有该value值，有1，无0
> - scard <key> 返回该集合的元素个数
> - srem <key><value1><value2>...删除集合中的某些元素
> - spop<key>随机从该集合中吐出一个值
> - srandmember <key><n> 随机从该集合中取出n个值，不会从集合中删除
> - smove <source><destination> value把集合从一个值从一个集合移动到另一个集合
> - sinter <key1><key2>返回两个集合的交集元素
> - sunion <key1><key2>返回两个集合的并集元素
> - sdiff <key1><key2>返回两个集合的差集元素，1有2没有
> ## 数据结构
> Set数据结构是dict字典，字典是用哈希表实现的。
> Java中HashSet的内部实现使用的是HashMap，只不过所有的value都指向同一个对象。Redis的set结构也是一样，它的内部也使用hash结构，所有的value都指向同一个内部值。

# Hash（Redis哈希）
> 	redis hash是一个键值对集合
> 	redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象
> 类似Java里面的map<String,Object>
> ## 常用命令
> - hset <ket><field><value> 给key集合中的field键赋值value
> - hget<key1><field>从<key1>集合<field>取出value
> - hmset <key1><field1><value1><field2><value2>..批量设置hash的值
> - hexists<key1><field>查看哈希表key中，给定域是否存在
> - hkeys<key> 列出该hash集合的所有field
> - hvals<key>列出该hash集合的所有value
> - hincrby <key><field><increment>为哈希表key中的域field的值加上增量
> - hsetnx <key><field><value>将哈希表key中的域field的值设置为value，当且仅当域field不存在
> ## 数据结构
> Hash类型对应的数据结构是两种：ziplist（压缩列表），hashtable（哈希表）。当field-value长度较短且个数较少时，使用ziplist，否则hashtable

# ZSet(Redis有序集合Sorted Set)
:::info
redis有序集合zset与普通集合set非常相似，是一个**没有重复元素**的字符串集合，不同之处是有序集合的每个成员都关联了一个score（评分、权重），这个score被用来按照从最低分到最高分的方式排序集合中的成员。集合的成员是唯一的，但是score可以是重复的。
因为元素是有序的，所以你也可以很快的根据score或者position来获取一个范围的元素。
访问有序集合的中间元素也是非常快的，因此你能够使用有序集合作为一个没有重复成员的智能列表。
## 常用命令

- zadd <key><score1><value1><score2><value2>将一个或多个member元素及其score加入到有序集key中
- zrange<key><start><stop>[WITHSCORES]返回有序集key中，下标在start和stop之间的元素带WITHSCORES，可以让分数一起和值返回到结果集
- zrangebyscore key minmax [withscore] [limit offset count]返回有序集key中，所有score值介于min和max之间（包括等于min或max）的成员。有序集成员按score值递增（从小到大）次序排列
- zrevrangebyscore key maxmin [withscores] [limit offset count] 同上，改为从大到小
- zincrby <key><increment><value> 为元素的score加上增量
- zrem <key><value> 删除该集合下，指定值的元素
- zount<key><min><max> 统计该集合，分数区间内的元素个数
- zrank <key><value> 返回该值在集合中的排名，从0开始
## 数据结构
	Sorted Set（zset）是redis提供的一个非常特别的数据结构，一方面它等价于java的数据结构Map<String,Double>，可以给每一个元素value赋予一个权值score，另一方面又类似于TreeSet，内部的元素会按照权重score进行排序，可以得到每个元素的名次，还可以通过score的范围来获取元素的列表。
zset底层使用了两个数据结构
1、hash，hash的作用就是关联元素value和权重score，保障元素value的唯一性，可以通过元素value找到相应的score值。
2、跳跃表，跳跃表的目的在于给元素value排序，根据score的范围获取元素列表。
:::
**跳跃表	**

有序集合在生活中比较常见，例如根据成绩对学生排名，根据得分对玩家排名等。对于有序集合的底层实现，可以用数组、平衡树、链表等。数组不便元素的插入、删除；平衡树或红黑树虽然效率高但结构复杂；链表查询需要遍历所有效率低。
Redis采用的是跳跃表。跳跃表效率堪比红黑树，实现远比红黑树简单。
2、实例
	对比有序链表和跳跃表，从链表中查询出51
（1） 有序链表
![image-20220727150205067](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220727150205067.png)
要查找值为51的元素，需要从第一个元素开始依次查找、比较才能找到。共需要6次比较。
（2） 跳跃表
![image-20220727150256931](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220727150256931.png)
从第2层开始，1节点比51节点小，向后比较。
21节点比51节点小，继续向后比较，后面就是NULL了，所以从21节点向下到第1层
在第1层，41节点比51节点小，继续向后，61节点比51节点大，所以从41向下
在第0层，51节点为要查找的节点，节点被找到，共查找4次。
（我自己的理解就是跳一个进行比较，比目标值小就又跳一个比较，比目标值大就比较前一个值。)
从此可以看出跳跃表比有序链表效率要高

# Redis文件配置
> 找到文件redis.conf并打开
> （想详细了解的，可以自己去读一下redis.conf文件注解）
> #### ###Units单位###
> 配置大小单位，开头定义了一些基本的度量单位，只支持bytes，不支持bit，大小写不敏感
> #### ###include###
> 一个页面包含另一个页面的内容，类似jsp中的include，多实例的情况可以把公用的配置文件提取出来
> #### ###network###
> - bind	配置默认情况bind=127.0.0.1只能接收本机的访问请求；不写的情况下，无限制接收任何ip地址的访问，如果开启了protected-mode，那么在没有设定bind ip且没有设密码的情况下，redis只允许接收本机的响应
> - protected-mode	开启他的保护模式，开启之后就只能本机访问，而远程无法访问，默认为yes
> - port	端口号，默认为6379
> - tcp-backlog 511 	设置tcp的backlog，backlog其实是一个连接队列，backlog队列总和 = 未完成三次握手队列+已完成三次握手队列。在高并发环境下你需要一个高backlog值来避免慢客户端连接问题注意linux内核会将减小到/proc/sys/net/core/somaxconn的值（128），所以需要确认增大/proc/sys/net/core/somaxconn和/proc/sys/net/ipv4/tcp_max_syn_backlog（128）两个值来达到想要的效果
> - timeout 0	一个空闲的客户端维持多少秒会关闭，0表示永不关闭
> - tcp-keepalive 300	检查心跳时间，检查redis活连接数，单位为秒，如果设置为0，则不会进行Keepalive检测，建议设置成60 
> #### ###GENERAL###
> - daemonize 		是否为后台进程，设置为yes守护进程，后台启动
> - pidfile		存放pid文件的位置，每个实例会产生一个不同的pid文件
> - loglevel	notice	 日志的输出级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为**notice**
> - logfile  ""	日志文件名称
> - databases 16	默认有16个库，默认使用数据库为0
> #### ###SECURITY安全###
> - 密码设置，打开注释 requirepass 
>
> 访问密码的查看、设置和取消
> 在命令中设置密码，只是临时的。重启redis服务器，密码就还原了。
> 永久设置，需要再配置文件中进行设置。
> ![image-20220730144237188](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220730144237188.png)
>
> #### ###LIMITS限制###
> - maxclients	设置redis同时可以与多少个客户端进行连接，默认情况下为10000个客户端如果达到了限制，redis则会拒绝新的连接请求，并且会向这些连接请求方发出“max number of clients reacher“以作回应
> - maxmemory	建议必须设置，否则，将内存占满，造成服务器宕机。设置redis可以使用的内存量，一旦到达内存使用上限，redis将会试图移除内部数据，一处规则可以通过maxmemory-policy来指定。	 如果redis无法根据移除规则来移除内存中的数据，或者设置了“不允许移除”，那么redis则会针对那些需要申请内存的指令返回错误信息，比如SET、LPUSH等。	 但是对于无内存申请的指令，仍然会正常响应，比如GET等。如果你的redis是主redis（说明你的redis有从redis），那么在设置内存使用上限时，需要在系统中留出一些内存空间给同步队列缓存，只有在你设置的是“不移除”的情况下，才不用考虑这个因素。
> - maxmemory-polocy
>    - volatile-lru ：使用lru算法移除key，只对设置了过期时间的键；（最近最少使用）
>    - allkeys-lru： 在所有集合key中，使用lru算法移除key
>    - volatile-random 在过期集合中移除随机的key，只对设置了过期时间的键
>    - allkeys-random：在所有集合key中，移除随机的key
>    - volatile-tll：移除那写tll值最小的key，即那些最近要过期的key
>    - noeviction： 不进行移除，针对写操作，只是返回错误信息
> - maxmemory-samples	设置样本数量，LRU算法和最小TTL算法都并非是精确的算法，而是估算值，所以你可以设置样本的大小，redis默认会检查这么多个key并选择其中LRU的那个。 一般设置3到7的数字，数值越小样本越不准确，但性能消耗越小。

# 消息的发布与订阅
> 消息的发布与订阅是redis带的一个功能
> redis发布订阅（pub/sub）是一种消息通信模式：发送者（pub）发送消息，订阅者（sub）接收消息
> redis客户端可以订阅任意数量的频道
> 1、客户端可以订阅频道如下图
> ![image-20220730145408989](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220730145408989.png)
> 2、当给这个频道发布消息后，消息就会发送给订阅的客户端
> ![image-20220730145420014](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220730145420014.png)
>
> ### 发布订阅命令行实现
> 1、 打开一个客户端订阅channel1
> SUBSCRIBE channel1
> ![image-20220730145530661](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220730145530661.png)
> 2、打开另一个客户端，给channel1发布消息hello
> publish channel1 hello
> ![image-20220730145549967](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220730145549967.png)
> 返回的1是订阅者数量
> 3、打开第一个客户端可以看到发送的消息
> ![image-20220730145629320](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220730145629320.png)
> 注：发布的消息没有持久化，如果在订阅的客户端收不到hello，只能收到订阅后发布的消息

# Bitmap（Redis新数据类型）
:::success
## 简介
现代计算机用二进制（位）作为信息的基础单位，1个字节等于8位， 例如“abc”字符串是由3个字节组成， 但实际在计算机存储时将其用二进制表示， “abc”分别对应的ASCII码分别是97、98、99，对应的二进制分别是01100001、01100010和01100011，合理地使用操作位能够有效地提高内存使用率和开发效率。
Redis提供了Bitmaps这个“数据类型”可以实现对位的操作：
（1） Bitmaps本身不是一种数据类型，实际上它就是字符串（key-value） ， 但是它可以对字符串的位进行操作。
（2） Bitmaps单独提供了一套命令， 所以在Redis中使用Bitmaps和使用字符串的方法不太相同。 可以把Bitmaps想象成一个以位为单位的数组， 数组的每个单元**只能存储0和1**， 数组的下标在Bitmaps中叫做偏移量。（不太清楚的可以继续往下看，实例说明）
![image-20220730145718081](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220730145718081.png)

## 常用命令
setbit<key><offset><value> 设置bitmaps中某个<key>偏移量<offset>的值<value>（0或1）
*offset:偏移量从0开始
实例:	<key>unique:users:20201106代表2020-11-06这天的独立访问用户的Bitmaps
![image-20220730150000867](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220730150000867.png)
每个独立用户是否访问过网站存放在Bitmaps中， 将访问的用户记做1， 没有访问的用户记做0， 用偏移量作为用户的id。
设置键的第offset个位的值（从0算起） ， 假设现在有20个用户，userid=1， 6， 11， 15， 19的用户对网站进行了访问， 那么当前Bitmaps初始化结果如图
![](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/shayu931/RedisBook03.png)
第一次初始化bitmaps时，假如偏移量(数组下标)非常大，那么整个初始化过程执行会比较慢，可能会造成redis阻塞。
getbit<key><offset> 获取bitmaps中某个偏移量的值
bitcount	(这里我就不详细介绍了)
统计字符串被设置成为1的bit数<value>。一般情况下，给定的整个字符串都会被进行计数，通过指定额外的start或end参数，可以让计数只在特定的位上进行。start和end参数的设置，都可以使用负数值：比如-1表示最后一个位，而-2表示倒数第二个位，start、end是指bit组字节的下标数，二者皆包含
bitcount<key>[start end] 统计字符串从start字节到end字节比特值为1的数量
bitop and(or/not/xor) <destkey> [key..]
bitop是一个复合操作，它可以做多个bitmaps的and（交），or（并），not（非），xor（异或)操作并将结果保存在destkey中。
:::

# HyperLogLog（Redis新数据类型）
:::warning
## 简介
为解决基数问题（求集合中不重复元素个数的问题），又不占用非常大的空间。
（数据存储在mysql表中使用distinct count计算不重复个数，或使用Redis提供的hash、set、bitmaps等数据结构来处理，以上的方案结果精确，但随着数据不断增加，导致占用空间越来越大）

redis提出一种降低一定精度来平衡存储空间的HyperLogLog，redis HyperLogLog是用来做基数统计的算法，HyperLogLog的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定的，并且是很小的
zairedis里面，每个HyperLogLog键只需要花费12kb内存，就可以计算接近2^64个不同元素的基数，这和计算基数时，元素越多越耗费内存的集合形成鲜明对比
但是，因为HyperLogLog只会根据输入元素来计算基数，而不会储存输入元素本身，所以HyperLogLog不能像集合那样，返回输入的各个元素
## 常用命令
pfadd <key><element> [element...]添加指定元素到HyperLogLog
pfvount<key>[key...]计算HLL的近似基数，可以计算多个HLL。
pfmerge<destkey><sourcekey>[sourcekey...] 将一个或多个HLL合并后的结果存储在另一个HLL中
:::
# Geospatial（Redis新数据类型）
> ## 简介
> Redis 3.2 中增加了对GEO类型的支持。GEO，Geographic，地理信息的缩写。该类型，就是元素的2维坐标，在地图上就是经纬度。redis基于该类型，提供了经纬度设置，查询，范围查询，距离查询，经纬度Hash等常见操作。
> ## 常用命令
> geoadd<key><longitude><latitude><member> [longitude latitude member...]添加地理位置（经度，纬度，名称）
> 两级无法直接添加，有效的经度从-180度到180度有效的维度从-85.05112878度到85.05112878度
> 已经添加的数据，是无法再次往里面添加的
> geopos <key><member>[member...]获取坐标值
> geodist<key><member1><member2> [m|km|ft|mi]获取两个位置之间的直线距离（米”默认值“，千米，英里，英尺）
> georadius<key><longitude><latitude> radius m|km|ft|mi 以给定的经纬度为中心，找出某一半径内的元素

# Redis Jedis测试
:::danger
Java 使用 Redis | 菜鸟教程
[https://www.runoob.com/redis/redis-java.html](https://www.runoob.com/redis/redis-java.html)
:::
```java
<!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
	<dependency>
	<groupId>redis.clients</groupId>
	<artifactId>jedis</artifactId>
	<version>4.2.3</version>
	</dependency>

```
:::danger
包括了安装，连接，使用实例等。（连接本地时记得改改config文件里面有个属性是控制本地连接的。）
（还有防火墙这一层，需要把6379端口放行，以防链接超时）
## 完成一个手机验证码功能
要求：
1、输入手机号，点击发送后随机生成6位数字码，2分钟有效
2、输入验证码，点击验证，返回成功或失败
3、每个手机号每天只能输入3次
:::
```java
package com.deng.util;


import redis.clients.jedis.Jedis;

import java.util.Random;

/**
 * @Author shayu
 * @DateTime 2022/8/23 14:17
 */


public class PhoneCode {
    public static void main(String[] args) {

        //模拟验证码发送
        verifyCode("123456789101");
        //校验验证码是否相同
        getRedisCode("123456789101","234311");
    }

    //3.验证码的校验
    public static void getRedisCode(String phone,String code){
        //从redis中获取验证码
        Jedis jedis = new Jedis("localhost",6379);
        //验证码key
        String codeKey = "VerifyCode" + phone + ":code";
        String redisCode  = jedis.get(codeKey);
        //判断
        if (redisCode.equals(code)){
            System.out.println("成功");
        }else {
            System.out.println("失败");
        }
        jedis.close();
    }



    //2.每个手机每天只能发送三次，验证码放到redis中，设置过期时间
    public static void verifyCode(String phone){
        //连接本地的 Redis 服务
        Jedis jedis = new Jedis("localhost",6379);
        // 如果 Redis 服务设置了密码，需要下面这行，没有就不需要
        // jedis.auth("123456");
        System.out.println("连接成功");
        //查看服务是否运行
        System.out.println("服务正在运行: "+jedis.ping());

        //拼接key
        //手机发送key的次数
        String countKey = "VerifyCode" + phone + ":count";
        //验证码key
        String codeKey = "VerifyCode" + phone + ":code";

        //每个手机每天发送三次
        String count = jedis.get(countKey);
        if (count == null){
            //没有发送过，第一次发送
            //设置发送次数为1
            jedis.setex(countKey,24*60*60,"1");
        }else if (Integer.parseInt(count) <= 2){
            //发送次数+1
            jedis.incr(countKey);
        }else if (Integer.parseInt(count) > 2){
            System.out.println("今天的发送次数超过三次");
            jedis.close();
            return;
        }

        //将生成的验证码放到redis里面去
        String vcode = getCode();
        jedis.setex(codeKey,120,vcode);
        jedis.close();


    }

    //1.生成6位数字的验证码
    public static String getCode(){
        Random random = new Random();
        String code = "";
        for (int i = 0; i < 6; i++) {
           int  rand = random.nextInt(10);
           code += rand ;
        }
        return code;
    }
}
/**
 * 有一个小问题，方法过期，也就是会不会有人卡着过期时间去要验证码，
 * 是不是就可以一直要了。（但过期时间有是一天，不会有人这么无聊吧。
 * 可以延伸一下，具体这么做我也不太清楚。）
 *  这也只是一个小荔枝，具体实际业务会更加复杂。
 *
 */
```
:::danger
## Springboot整合Redis
SpringBoot之Redis访问(spring-boot-starter-data-redis) - 码农教程
[http://www.manongjc.com/detail/25-fidimhvkzoxchrg.html](http://www.manongjc.com/detail/25-fidimhvkzoxchrg.html)
（我偷懒不想写具体教程，需要的可以看看跟着写写）
:::
# Redis 事务操作
> ## 定义
> Redis事务是一个单独的隔离操作：事务中的所有命令都会**序列化、按顺序地执行**。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。
> Redis事务的主要作用就是串联多个命令防止别的命令插队。
> ## Multi、Exec、discard
> 从输入Multi命令开始（事务开始），输入的命令都会依次进入命令队列中，但不会执行，直到输入Exec（执行）后，Redis会将之前的命令队列中的命令依次执行。
> 组队的过程中可以通过discard来放弃组队。 
> ![image-20220731165752989](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220731165752989.png)
> ![image-20220731165812368](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220731165812368.png)
> 组队成功，提交成功
> ![image-20220731165825351](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220731165825351.png)
> 组队阶段报错，提交失败，组队中某个命令出现了报告错误，执行时整个的所有队列都会被取消。
> ![image-20220731165909910](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220731165909910.png)
> 组队成功，提交有成功有失败情况，如果执行阶段某个命令报出了错误，则只有报错的命令不会被执行，而其他的命令都会执行，不会回滚。

## 事务冲突
> 想想一个场景：有很多人有你的银行卡,同时去参加双十一抢购
> 一个请求想给金额减8000
> 一个请求想给金额减5000
> 一个请求想给金额减1000
> 最终会导致账号金额出现负数，是不合理的。（禁止贷款）
> ![image-20220731170212554](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220731170212554.png)
> 解决以上问题
>
> ## 悲观锁
> 悲观锁，顾名思义，就是很悲观，每次去拿数据的时候都认为别人会**修改**，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会block直到他拿到锁。在操作之前先上锁。
> ## 乐观锁
> 乐观锁，顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会**修改**，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。**乐观锁适用于多读的应用类型，这样可以提高吞吐量**。redis就是利用这种check-and-set机制实现事务的。
> ## watch key[key...]
> 在执行multi之前，先执行watch key1[key2]。可以监视一个或多个key，如果在事务执行之前这个key被其他命令所改动，那么事务将被打断。
> set balance 100,执行watch就行监视，操作balance+10，可以完成。
> ![](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/shayu931/RedisBook04.png)
> 同时开启监视，开启事务，balance+20，但前面已经进行了对key的监视，发生了变动，事务执行就会打断。
> ![](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/shayu931/RedisBook05.png)
>
> ## Redis事务的三大特性
> - 单独的隔离操作
>    - 事务中的所有命令都会序列化，按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断
> - 没有隔离级别的概念
>    - 队列中的命令没有提交之前都不会实际被执行，因为事务提交之前任何指令都不会被实际执行
> - 不保证原子性
>    - 事务中如果有一条命令执行失败，其后的命令仍然会被执行，没有回滚。

# Redis持久化
> [为什么要有持久化？](https://blog.csdn.net/JavaTeachers/article/details/108998121?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166159333816782395337767%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166159333816782395337767&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-108998121-null-null.142^v42^pc_rank_34,185^v2^control&utm_term=redis%E6%8C%81%E4%B9%85%E5%8C%96&spm=1018.2226.3001.4187)（当我看到持久化时，我第一反应就是为什么要持久化，然后去搜了一下，这位博主讲的特别有意思，简单来说就是怕redis突然宕机数据丢失做的一种保护化措施。）
> redis提供了两个不同形式的持久化方式
> - RDB(Redis DataBase)
> - AOF(Append Of File)
> 

## RDB
> 在指定的时间间隔内将**内存**中的数据集快照写入**磁盘(硬盘)**，也就是行话讲的Snapshot快照，他恢复时是将快照文件直接读到内存里
> 是redis默认持久化的方式
> ### 执行
> redis会单独创建一个子进程（fork）来进行持久化，会先将数据写入到一个临时文件中，待持久化过程结束了，再用这个临时文件替换上次持久化好的文件。整个过程中，**主进程是不进行任何IO操作的**，这就确保了极高的性能。 如果需要进行大规模的数据恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加高效。RDB的缺点是最后一次持久化后的数据可能丢失。（这句我当时也不太清楚，继续往下看。）

**fork延伸**

（说实话我也没弄太明白这是啥，浅显理解是个进程吧）> - Fork的作用是复制一个与当前进程一样的进程。新进程的所有数据（变量、环境变量、程序计数器等）数值都和原进程一致，但是是一个全新的进程，并作为原进程的子进程

> -  在Linux程序中，fork()会产生一个和父进程完全相同的子进程，但子进程在此后多会exec系统调用，出于效率考虑，Linux中引入了“**写时复制技术**”
> - **一般情况父进程和子进程会共用同一段物理内存**，只有进程空间的各段的内容要发生变化时，才会将父进程的内容复制一份给子进程。

> ## 流程
> ![](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/shayu931/RedisBook06.png)
> dump.rdb也就是备份文件，redis可以根据这个文件来恢复数据。
>
> ### 备份配置
> redis.conf可以对rdb文件进行配置
> - dbfilename dump.rdb  	文件名配置
> - dir  ./		文件位置配置，默认为Redis启动时命令行所在的目录下
> - stop-writes-on-bgsave-error  yes  当Redis无法写入磁盘的话，直接关掉Redis的写操作。yes打开
> - rdbcompression  yes 对于存储到磁盘中的快照，可以设置是否进行压缩存储。如果是的话，redis会采用LZF算法进行压缩。如果你不想消耗CPU来进行压缩的话，可以设置为关闭此功能。推荐yes.
> - rdbchecksum  yes 在存储快照后，还可以让redis使用CRC64算法来进行数据校验，但是这样做会增加大约10%的性能消耗，如果希望获取到最大的性能提升，可以关闭此功能,推荐yes.
>
> **最重要的命令 save **
> ![image-20220731174621350](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220731174621350.png)
>
> - save 900 1 # 3600秒（60分钟）内有1个key写入就保存（备份）
> - save 300 10 # 300秒（5分钟）内有10个key写入就保存（备份）
> - save 60 10000 # 60秒（1分钟）内有10000个key写入就保存（备份）可以同时打开多个。
>
> 而上面的数据问题也就是因为这个原因，比如 sava  20 10 在20秒内有10个key写入就保存，但当在18秒时宕机了，这时就会导致数据丢失。问题虽然有解决，但没完全解决。
> ### rdb文件的使用
> 先通过config get dir  查询rdb文件的目录
> 将*.rdb的文件拷贝到别的地方
> rdb的恢复
> -  关闭Redis
> - 先把备份的文件拷贝到工作目录下cp dump2.rdb dump.rdb
> -  启动Redis, 备份数据会直接加载
> ### 优点：
> 适合大规模数据恢复节省空间对数据完整性和一致性要求不高恢复速度快
> ### 缺点：
> 在一定间隔时间做一次备份，所以如果redis意外down掉的话，就会丢失最后一次快照后的所有修改fork的时候，内存中的数据被克隆了一份，大致2倍的膨胀性需要考虑，数据量大的时候，耗性能
>

## AOF
> 以日志的形式来记录每个**写**操作（增量保存），将redis执行过的所有写指令记录下来(读操作不记录)，只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。
> ## 配置
> - aof默认不开启。
> - 在redis.confg中配置文件名称，默认为appendonly.aofaof文件的保存路径，同rdb的路径一致
> - 两种持久化同时开启，系统默认取aof的数据（数据不会存在丢失）
> - aof的备份机制和性能虽然和rdb不同，但备份和操作同rdb一样
>    - 正常启动
>       - 修改默认的appendonly no，改为yes
>       - 将有数据的aof文件复制一份保存到对应目录（查看目录：config get dir）
>       - 恢复：重启redis然后重新加载
>    - 异常恢复
>       - 修改默认的appendonly no，改为yes
>       - 如遇到aof文件损坏，通过/usr/local/bin/redis-check-aof --fix appendonly.aof进行恢复
>       - 备份被写坏的aof文件
>       - 恢复：重启redis，然后重新加载
> ## AOF同步频率（备份频率）设置
> - appendfsync always始终同步，每次redis的写入都会立刻记入日志；性能较差但数据完整性比较好
> - appendfsync everysec每秒同步，每秒计入日志一次，如果宕机，本秒的数据可能丢失
> - appendfsync noredis不主动进行同步，把同步时机交给操作系统。
> ## rewrite 压缩(重写)
> AOF采用文件追加方式，**文件会越来越大为避免出现此种情况**，新增了重写机制, 当AOF文件的大小超过所设定的阈值时，Redis就会启动AOF文件的内容压缩， 只保留可以恢复数据的最小指令集.可以使用命令bgrewriteaof （bg  rewrite  aof）
> #### 重写原理，如何实现重写
> AOF文件持续增长而过大时，会fork出一条新进程来将文件重写(也是先写临时文件最后再rename，这里不懂看下面的流程)，redis4.0版本后的重写，是指上就是把rdb 的快照，以二级制的形式附在新的aof头部，作为已有的历史数据，替换掉原来的流水账操作。也就是简写，栗如：set v1 v2  set  b1 b2 会变成set v1 v2 b1  b2。

配置no-appendfsync-on-rewrite：如果no-appendfsync-on-rewrite=yes ,不写入aof文件只写入缓存，用户请求不会阻塞，但是在这段时间如果宕机会丢失这段时间的缓存数据。（降低数据安全性，提高性能）
如果no-appendfsync-on-rewrite=no,  还是会把数据往磁盘里刷，但是遇到重写操作，可能会发生阻塞。（数据安全，但是性能降低）
> #### 触发机制，何时重写
> Redis会记录上次重写时的AOF大小，默认配置（可配置）是当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发（重写虽然可以节约大量磁盘空间，减少恢复时间。但是每次重写还是有一定的负担的，因此设定Redis要满足一定条件才会进行重写。）

文件大小触发重写配置auto-aof-rewrite-percentage：设置重写的基准值，文件达到100%时开始重写（文件是原来重写后文件的2倍时触发）
auto-aof-rewrite-min-size：设置重写的基准值，最小文件64MB。达到这个值开始重写。
例如：文件达到70MB开始重写，降到50MB，下次什么时候开始重写？100MB
系统载入时或者上次重写完毕时，Redis会记录此时AOF大小，设为base_size,
如果Redis的AOF当前大小>= base_size +base_size*100% (默认)且当前大小>=64mb(默认)的情况下，Redis会对AOF进行重写。 
> ## 重写流程
> （1）bgrewriteaof触发重写，先判断是否当前有bgsave或bgrewriteaof在运行，如果有，则等待该命令结束后再继续执行。
> （2）主进程fork出子进程执行重写操作，保证主进程不会阻塞。
> （3）子进程遍历redis内存中数据到临时文件，客户端的写请求同时写入aof_buf缓冲区和aof_rewrite_buf重写缓冲区保证原AOF文件完整以及新AOF文件生成期间的新的数据修改动作不会丢失。
> （4）1).子进程写完新的AOF文件后，向主进程发信号，父进程更新统计信息。2).主进程把aof_rewrite_buf中的数据写入到新的AOF文件。
> （5）使用新的AOF文件覆盖旧的AOF文件，完成AOF重写。
> ![image-20220731213215703](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220731213215703.png)
>
> ## 优势
> - 备份机制更稳健，丢失数据概率更低
> - 可读的日志文件，通过操作aof稳健，可以处理误操作
> ## 劣势
> - 比起rdb占用更多的磁盘空间
> - 恢复备份速度更慢
> - 每次读写都同步的话，有一定性能压力
> - 存在一定bug，造成恢复不能
>
> # 总结
> ## 用哪个好
> 官方推荐两个都启用。
> 如果对数据不敏感，可以选单独用RDB。
> 不建议单独用AOF，因为可能会出现Bug。
> 如果只是做纯内存缓存，可以都不用。

官方建议（没事的可以晃俩眼）
-  RDB持久化方式能够在指定的时间间隔能对你的数据进行快照存储
-  AOF持久化方式记录每次对服务器写的操作,当服务器重启的时候会重新执行这些命令来恢复原始的数据,AOF命令以redis协议追加保存每次写的操作到文件末尾. 
-  Redis还能对AOF文件进行后台重写,使得AOF文件的体积不至于过大
- 只做缓存：如果你只希望你的数据在服务器运行的时候存在,你也可以不使用任何持久化方式.
-  同时开启两种持久化方式
-  在这种情况下,当redis重启的时候会优先载入AOF文件来恢复原始的数据, 因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整.
-  RDB的数据不实时，同时使用两者时服务器重启也只会找AOF文件。那要不要只使用AOF呢？ 
-  建议不要，因为RDB更适合用于备份数据库(AOF在不断变化不好备份)，快速重启，而且不会有AOF可能潜在的bug，留着作为一个万一的手段。
#### 性能建议
因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留save 900 1这条规则。
如果使用AOF，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了。
代价,一是带来了持续的IO，二是AOF rewrite的最后将rewrite过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。
只要硬盘许可，应该尽量减少AOF rewrite的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上。
默认超过原大小100%大小时重写可以改到适当的数值。
# Redis的主从复制
:::info
# 是什么，能干嘛
主机数据更新后根据配置和策略，自动同步到备机的master(主)/slaver(从)机制，**Master以写为主，Slave以读为主 **

-  读写分离，性能扩展
-  容灾快速恢复 （也就是解决宕机问题，一个slaver(从)死了可以用其他的slaver(从)，而主死了后面说。）

![image-20220731213848099](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220731213848099.png)
## 操作
操作之前有几个配置注意下

1. 开启daemonize yes  (后台启动)
1. dump.rdb名字dbfilename  （之前有修改过的注意一下）
1. appendonly  AOP的生成  关掉或者换名字
#### 新建多个config文件，并拷贝redis.conf里的内容，include命令去拷贝。
![](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/shayu931/RedisBook07.png)
#### 编辑新建的redis6379.conf，:wq!保存退出
![](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/shayu931/RedisBook08.png)
一样的操作新建编辑redis6380.conf和redis6381.conf，注意里面的名字和端口不一样

- Pid文件名字pidfile
- 指定端口port	
- slave-priority 10 	设置从机的优先级，值越小，优先级越高，用于选举主机时使用。默认100
- Log文件名字
#### 启动三台redis服务器
![image-20220731233845060](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220731233845060.png)
#### 看查系统进程，三台服务器是否启动
![image-20220731233859352](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220731233859352.png)
#### 看查三台主机运行情况，role属性角色
![image-20220731233920372](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220731233920372.png)
#### 配置主从
命令：slaveof <ip><port>  	成为某个实例的从服务器
1、在6380和6381上执行 : slaveof 127.0.0.1 6379
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661657920631-b7b186e2-82bb-4d64-9612-dc6c10116eb8.png#clientId=u1bfebe78-debc-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ubcb69635&margin=%5Bobject%20Object%5D&originHeight=174&originWidth=556&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u1a2edf2a-0b25-4318-ba2b-680ea7a036e&title=)
2、在主机上写，在从机上可以读取数据
在从机上写数据报错
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661657920620-c7e0c713-2711-4d41-a9a7-4d93db92a2bb.png#clientId=u1bfebe78-debc-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ud1a1be35&margin=%5Bobject%20Object%5D&originHeight=38&originWidth=513&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u2ee819c6-09cf-4158-8655-35bb25f3849&title=)
3、主机挂掉，重启就行，一切如初
4、从机重启需重设：slaveof 127.0.0.1 6379
可以将配置增加到文件中。永久生效。
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661657920624-99acf192-9b00-4cee-a681-82ccddb2f217.png#clientId=u1bfebe78-debc-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=uc8ab5301&margin=%5Bobject%20Object%5D&originHeight=253&originWidth=549&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u0899f2ad-52c3-4d21-ad5a-98e0ee81667&title=)
### 几个常见的问题
问：切入点问题？slave1、slave2是从头开始复制还是从切入点开始复制?比如从k4进来，那之前的k1,k2,k3是否也可以复制？
答：当slave1挂掉之后，再次连接，他会去和主master同步数据，从头复制数据。
问：从机是否可以写？set可否？ 
答：从机只负责读取，主机负责写入。
问：主机shutdown后情况如何？从机是上位还是原地待命？
答：一般情况下当主机挂掉之后，从机是会原地待命的，不会上位（要上位的话，怎么上，这也就是下面要讲的哨兵机制）
问：主机又回来了后，主机新增记录，从机还能否顺利复制？ 
其中一台从机down后情况如何？依照原有它能跟上大部队吗？
答：主机回来后新增的数据记录，会同步到从机，从机联机主机时都会去比较数据然后同步。
#### 复制原理

- Slave启动成功连接到master后会发送一个sync命令

- Master接到命令启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令， 在后台进程执行完毕之后，master将传送整个数据文件到slave,以完成一次完全同步

- 全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。

- 增量复制：Master继续将新的所有收集到的修改命令依次传给slave,完成同步

- 但是只要是重新连接master,一次完全同步（全量复制)将被自动执行

  ![image-20220801105844173](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220801105844173.png)
### 性质

- 一主俩从，一般情况下都是配置的一主俩从。
- 薪火相传
   - 上一个Slave可以是下一个slave的Master，Slave同样可以接收其他slaves的连接和同步请求，那么该slave作为了链条中下一个的master, 可以有效减轻master的写压力,去中心化降低风险。

用slaveof  <ip><port>   
中途变更转向:会清除之前的数据，重新建立拷贝最新的
风险是一旦某个slave宕机，后面的slave都没法备份
主机挂了，从机还是从机，无法写数据了
![image-20220801105607960](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220801105607960.png)

- 反客为主
   - 当一个master宕机后，后面的slave可以立刻升为master，其后面的slave不用做任何修改。

用slaveof  no one  将从机变为主机。

:::
## 哨兵模式(sentinel)
:::info
**反客为主的自动版**，能够后台监控主机是否故障，如果故障了根据投票数自动将从库转换为主库
![image-20220801110018170](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220801110018170.png)

### 怎么玩(使用步骤)
**调整为一主二仆模式，6379带着6380、6381**
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661676158143-26c09190-5301-4ab5-aded-deb870825a32.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ubaf3599f&margin=%5Bobject%20Object%5D&originHeight=173&originWidth=556&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u05a229e4-33b4-476f-bf83-1ec708f8ff3&title=)
**自定义的/myredis目录下新建sentinel.conf文件，名字绝不能错**
**配置哨兵,填写内容**
sentinel monitor mymaster 127.0.0.1 6379 1
其中mymaster为监控对象起的服务器名称， 1 为至少有多少个哨兵同意迁移的数量。 
**启动哨兵**
/usr/local/bin
redis做压测可以用自带的**redis-benchmark**工具
执行**redis-sentinel /myredis/sentinel.conf**
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661676158123-bf710ee1-3079-4458-b9f8-931bd65a9c42.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u0e6e8b0f&margin=%5Bobject%20Object%5D&originHeight=499&originWidth=556&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u9d8c49fe-4c48-4552-b0b7-fb0f2f49cd4&title=)
**当主机挂掉，从机选举中产生新的主机**
(大概10秒左右可以看到哨兵窗口日志，切换了新的主机)
哪个从机会被选举为主机呢？根据优先级别：replica-priority
**原主机重启后会变为从机。**
### 小缺点：复制延时
由于所有的**写操作**都是先在Master上操作，然后同步更新到Slave上，所以从Master同步到Slave机器有一定的延迟，当系统很繁忙的时候，延迟问题会更加严重，Slave机器数量的增加也会使这个问题更加严重。（也就是RBD和AOF导致的）
### 流程
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661676472730-70397d21-b1a8-4f22-911d-bfbbebda130c.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u8e80e703&margin=%5Bobject%20Object%5D&originHeight=301&originWidth=553&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ua273f55f-9099-484c-9dab-5caacf40b52&title=)
优先级在redis.conf中默认：replica-priority 100，值越小优先级越高
偏移量是指获得原主机数据最全的
每个redis实例启动后都会随机生成一个40位的runid
:::
# Redis集群
> # 问题
> 容量不够，redis如何进行扩容？
> 并发写操作， redis如何分摊？
> **另外，主从模式，薪火相传模式，主机宕机，导致ip地址发生变化，应用程序中配置需要修改对应的主机地址、端口等信息。**
> 之前通过代理主机来解决，但是redis3.0中提供了解决方案。就是无中心化集群配置。
> ![](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/shayu931/RedisBook09.png)
>
> # 什么是集群
> Redis 集群实现了对Redis的水平扩容，即启动N个redis节点，将整个数据库分布存储在这N个节点中，每个节点存储总数据的1/N。（也就是上面的用户，订单，商品）
> Redis 集群通过分区（partition）来提供一定程度的可用性（availability）： 即使集群中有一部分节点失效或者无法进行通讯， 集群也可以继续处理命令请求。
> ## 操作实例
> 首先吧我们之前操作的数据删除，也就是删除rdb文件
> 命令：rm -rf dump63* 	删除带dump63的所有文件
> ### 建6个实例，6379,6380,6381,6389,6390,6391
> ![image-20220801113056666](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220801113056666.png)
>
> 和之前一样的，6个conf文件，不过里面新增了一些东西
>
> #### 集群conf配置
> 1. cluster-enabled yes 打开集群模式
> 1. cluster-config-file nodes-6379.conf 设定节点配置文件名
> 1. cluster-node-timeout 15000 设定节点失联时间，超过该时间（毫秒），集群自动进行主从切换。
> 1. 注意开启daemonize yes（RDB）和Appendonly (AOF)关掉或者换名字
>
> vi中使用查找替换修改另外5个文件的快速操作
> 例如：:%s/6379/6380	全局替换6379替换6380

```java
include /home/bigdata/redis.conf
port 6379
pidfile "/var/run/redis_6379.pid"
dbfilename "dump6379.rdb"
dir "/home/bigdata/redis_cluster"
logfile "/home/bigdata/redis_cluster/redis_err_6379.log"
clu - ster-enabled yes
cluster-config-file nodes-6379.conf
cluster-node-timeout 15000
```
> 完成之后就可以启动6个redis服务
> ![image-20220801113121076](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/4496/image-20220801113121076.png)
>
> ###  将六个节点合成一个集群(无中心集群)
> 组合之前，请确保所有redis实例启动后，nodes-xxxx.conf文件都生成正常。在目录中ll查看，前面的文件就是之前conf配置的节点文件（cluster-config-file nodes-6379.conf）
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661686597966-f34caa93-bc88-4504-bd71-5d1b4953c625.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ua8336e3d&margin=%5Bobject%20Object%5D&originHeight=325&originWidth=551&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u65945c01-303e-4c44-8575-01c36f12144&title=)
> #### 合成集群：
> 在redis6中已经封装好了合成集群的命令操作
> 先打开redis本地目录  cd /opt/redis-6.2.1/src，需要在这个目录下执行下面的命令
>  redis-cli --cluster create --cluster-replicas 1 192.168.11.101:6379 192.168.11.101:6380 192.168.11.101:6381 192.168.11.101:6389 192.168.11.101:6390 192.168.11.101:6391
> 此处不要用127.0.0.1， 请用真实IP地址--replicas 1 采用最简单的方式配置集群，一台主机，一台从机，正好三组。
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661686597961-6981329c-275e-47b9-ac8b-612b8f8ce339.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ufccacf03&margin=%5Bobject%20Object%5D&originHeight=208&originWidth=556&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ub7de300a-f716-427f-b1dc-816eeb05cad&title=)
> 下面是redis给你提供的分配方案
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661686598083-d1d8a325-3b09-43ad-8f34-c5c32dbb34bd.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u55f22c7e&margin=%5Bobject%20Object%5D&originHeight=400&originWidth=453&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u847d0e89-d180-4f2f-9369-268a76305ce&title=)
> ### 登录
> 普通方式登录可能直接进入读主机，存储数据时，会出现MOVED重定向操作。所以，应该以集群方式登录。
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661686598008-fb6425da-ac66-42c2-8a8b-543e6db4eb47.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u1181ade8&margin=%5Bobject%20Object%5D&originHeight=96&originWidth=352&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u641840ba-e164-4135-8aec-4effe04ca01&title=)
> ###  -c 登录，采用集群策略连接，设置数据会自动切换到相应的写主机
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661686597930-851d3ae0-4d7e-4190-885f-ca0afc2f0795.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u4784084c&margin=%5Bobject%20Object%5D&originHeight=136&originWidth=545&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ucc7c4600-d49d-4f2b-9ea6-8aa1037c719&title=)
> ### 通过 cluster nodes 命令查看集群信息
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661686598511-8471f8ad-a489-4bfb-9dab-a305b2c39eb3.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u0be7de4c&margin=%5Bobject%20Object%5D&originHeight=62&originWidth=549&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u70baba52-51e0-4a3c-9b92-1221695d0fc&title=)
> ### redis cluster 如何分配这六个节点?
> 上面合成集群命令解析
> 一个集群至少要有三个主节点。
> 选项--cluster-replicas 1 表示我们希望为集群中的每个主节点创建一个从节点。
> 分配原则尽量保证每个主数据库运行在不同的IP地址，每个从库和主库不在一个IP地址上。
> ### 什么是slots
> （新概念，下面会用到的，读一下就好）
> [OK] All nodes agree about slots configuration.
> >>> Check for open slots...
> >>> Check slots coverage...
> [OK] All 16384 slots covered.
> 一个 Redis 集群包含16384个插槽（hash slot）， 数据库中的每个键都属于这 16384 个插槽的其中一个， 集群使用公式 CRC16(key) % 16384 来计算键 key 属于哪个槽， 其中 CRC16(key) 语句用于计算键 key 的 CRC16 校验和 。
> 集群中的每个节点负责处理一部分插槽。 举个例子， 如果一个集群可以有主节点， 其中：
> 节点 A 负责处理 0 号至 5460 号插槽。节点 B 负责处理 5461 号至 10922 号插槽。节点 C 负责处理 10923 号至 16383 号插槽。
> 这个东西是确保在无中心化集群中读写是能找到正确的数据的关键。
> ###  在集群中录入值
> 在redis-cli每次录入、查询键值，redis都会计算出该key应该送往的插槽slot，如果不是该客户端对应服务器的插槽，redis会报错，并告知应前往的redis实例地址和端口。
> redis-cli客户端提供了 –c 参数实现自动重定向。
> 如 redis-cli -c –p 6379 登入后，再录入、查询键值对可以自动重定向。
> 不在一个slot下的键值，是不能使用mget,mset等多键操作。
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661687650200-ef8a8d3c-9be8-47e9-8868-d34f7c3911e6.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ubaabc2d9&margin=%5Bobject%20Object%5D&originHeight=38&originWidth=477&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u81cdcc52-e87e-45b1-b70f-71e91626fc6&title=)
> 可以通过{}来定义组的概念，从而使key中{}内相同内容的键值对放到一个slot中去。
> mset k1{cust} v1 k2{cust} v2     	解释：cust对象里的k1键v1值。
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661687650087-04781f8c-10f8-4021-992b-543374a1b188.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u47dff9d5&margin=%5Bobject%20Object%5D&originHeight=51&originWidth=477&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u944d87ca-7a48-48ff-b31d-0c87cfb69e9&title=)
> ###  查询集群中的值
> cluster keyslot cust		看查cust的插槽位置
> cluster countkeysinslot	4847	查看插槽4847中有多少值
> **CLUSTER GETKEYSINSLOT <slot><count>** 返回 count 个 slot 槽中的键。（在后面加数据可以看出第几个键的值）
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661687650099-4ddb3091-4977-4888-a2f4-3687279ed2bc.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ud852581c&margin=%5Bobject%20Object%5D&originHeight=159&originWidth=458&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u74bdba92-af33-4a1e-9830-c2e244bdcb0&title=)
> ## 故障恢复
> 如果主节点下线？从节点能否自动升为主节点？注意：**15秒超时**
> 例如挂掉6379。
> ![](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/shayu931/RedisBook10.png)
> 主节点恢复后，主从关系会如何？**主节点回来变成从机**。
>
> 问：如果所有某一段插槽的主从节点都宕掉，redis服务是否还能继续?
> 答：如果某一段插槽的主从都挂掉，而cluster-require-full-coverage 为yes ，那么 ，整个集群都挂掉
> 如果某一段插槽的主从都挂掉，而cluster-require-full-coverage 为no ，那么，该插槽数据全都不能使用，也无法存储。
> redis.conf中的参数 cluster-require-full-coverage
>
> ##  集群的Jedis开发
> 即使连接的不是主机，集群会自动切换主机存储。主机写，从机读。
> 无中心化主从集群。无论从哪台主机写的数据，其他主机上都能读到数据。
>  public class JedisClusterTest {
>    public static void main(String[] args) { 
>       Set<HostAndPort>set =new HashSet<HostAndPort>();
>       set.add(new HostAndPort("192.168.31.211",6379));
>       JedisCluster jedisCluster=new JedisCluster(set);
>       jedisCluster.set("k1", "v1");
>       System.out.println(jedisCluster.get("k1"));
>    }
>  }
> ![](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/shayu931/RedisBook11.png)
>
> ## Redis 集群提供了以下好处
> 1. 实现扩容
> 1. 分摊压力
> 1. 无中心配置相对简单
> ## Redis 集群的不足
> 1. 多键操作是不被支持的 
> 1. 多键的Redis事务是不被支持的。lua脚本不被支持
> 1. 由于集群方案出现较晚，很多公司已经采用了其他的集群方案，而代理或者客户端分片的方案想要迁移至redis cluster，需要整体迁移而不是逐步过渡，复杂度较大。

# Redis的应用问题解决
## 缓存穿透
> 穿过redis缓存直接访问数据库
> ### 问题描述
> key对应的数据在数据源（数据库）并不存在，每次针对此key的请求从缓存获取不到，请求都会压到数据源，从而可能压垮数据源。比如用一个不存在的用户id获取用户信息，不论缓存还是数据库都没有，若黑客利用此漏洞进行攻击可能压垮数据库。
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661690849209-92d28cfb-4e2e-47da-85eb-55d2b946903c.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u0cc76925&margin=%5Bobject%20Object%5D&originHeight=269&originWidth=554&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u9d6b2260-bbf1-4384-86cc-bd8f670ec36&title=)
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661690849424-0c87d6c2-f76d-4c31-8cfc-63d16628792b.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u91c85d84&margin=%5Bobject%20Object%5D&originHeight=508&originWidth=1363&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u7ff533b9-8172-4c93-8002-62b2b2e4bf6&title=)
> ### 16.1.2.	解决方案
> 一个一定不存在缓存及查询不到的数据，由于缓存是不命中时被动写的，并且出于容错考虑，如果从存储层查不到数据则不写入缓存，这将导致这个不存在的数据每次请求都要到存储层去查询，失去了缓存的意义。
> **解决方案：**
> （1）	**对空值缓存：**如果一个查询返回的数据为空（不管是数据是否不存在），我们仍然把这个空结果（null）进行缓存，设置空结果的过期时间会很短，最长不超过五分钟
> （2）	**设置可访问的名单（白名单）**：使用bitmaps类型定义一个可以访问的名单，名单id作为bitmaps的偏移量，每次访问和bitmap里面的id进行比较，如果访问id不在bitmaps里面，进行拦截，不允许访问。就是白名单。
> （3）	**采用布隆过滤器：**(布隆过滤器（Bloom Filter）是1970年由布隆提出的。它实际上是一个很长的二进制向量(位图)和一系列随机映射函数（哈希函数）。我也不清楚。
> 布隆过滤器可以用于**检索一个元素是否在一个集合中**。它的**优点**是空间效率和查询时间都远远超过一般的算法，**缺点**是有一定的误识别率和删除困难。)
> 将所有可能存在的数据哈希到一个足够大的bitmaps中，一个一定不存在的数据会被 这个bitmaps拦截掉，从而避免了对底层存储系统的查询压力。
> （4）	**进行实时监控**：当发现Redis的命中率开始急速降低，需要排查访问对象和访问的数据，和运维人员配合，可以设置黑名单限制服务
> 

## 缓存击穿
> 缓存击穿是指缓存中没有但数据库中有的数据（一般是缓存时间到期），这时由于并发用户特别多，同时读缓存没读到数据，又同时去数据库去取数据，引起数据库压力瞬间增大，造成过大压力
> ### 问题描述
> key对应的数据存在，但在redis中过期，此时若有大量并发请求过来，这些请求发现缓存过期一般都会从后端DB加载数据并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮。
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661691524790-c5773419-cc73-4736-82c8-5c97841d1d5a.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u97d14846&margin=%5Bobject%20Object%5D&originHeight=263&originWidth=554&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ud541794e-3bdf-421a-86d9-b218b147179&title=)
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661691524545-9143cfe4-a7f1-47ed-9f3b-4b70d47e46c8.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=uea06937b&margin=%5Bobject%20Object%5D&originHeight=491&originWidth=1425&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uc852dd70-044f-4b4c-8496-d33d6c5ef28&title=)
> ### 16.2.2.	解决方案
> key可能会在某些时间点被超高并发地访问，是一种非常“热点”的数据。这个时候，需要考虑一个问题：缓存被“击穿”的问题。
> **解决问题：**
> （1）**预先设置热门数据**：在redis高峰访问之前，把一些热门数据提前存入到redis里面，加大这些热门数据key的时长
> （2）**实时调整**：现场监控哪些数据热门，实时调整key的过期时长
> （3）**使用锁**：（不想看，看不懂）
> 1. 就是在缓存失效的时候（判断拿出来的值为空），不是立即去load db。
> 1. 先使用缓存工具的某些带成功操作返回值的操作（比如Redis的SETNX）去set一个mutex key
> 1. 当操作返回成功时，再进行load db的操作，并回设缓存,最后删除mutex key；
> 1. 当操作返回失败，证明有线程在load db，当前线程睡眠一段时间再重试整个get缓存的方法。
> 
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661691524791-55d6a42f-b796-465f-98ff-ddef54771b5a.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ud021073f&margin=%5Bobject%20Object%5D&originHeight=411&originWidth=448&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uef6b25e1-386b-424c-9810-27fe62919ad&title=)
> 

## 缓存雪崩
>   缓存雪崩是指缓存中数据大批量到过期时间，而查询数据量巨大，引起数据库压力过大甚至down机。和缓存击穿不同的，缓存击穿指并发查同一条数据，缓存雪崩是不同数据都过期了，很多数据都查不到从而查数据库。（下面是尚硅谷的解释，我没看太明白）
> ### 问题描述
> key对应的数据存在，但在redis中过期，此时若有大量并发请求过来，这些请求发现缓存过期一般都会从后端DB加载数据并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮。
> 缓存雪崩与缓存击穿的区别在于**这里针对很多key缓存，前者则是某一个key**
> 正常访问⬇
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661692457643-73b2e186-98de-46bd-a9dc-a11e1df217fb.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ub7ada21e&margin=%5Bobject%20Object%5D&originHeight=324&originWidth=554&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uf52abd26-6374-4bc8-936a-6354a96a2a2&title=)
> 缓存失效瞬间⬇
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661692457513-e99625a5-5df3-436f-b3b5-3a5b62c560c2.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u77941c93&margin=%5Bobject%20Object%5D&originHeight=317&originWidth=554&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u986e9a2e-6f92-4df1-9469-3ce96958b6e&title=)
> ![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661692457588-fe086591-b234-4971-8299-df4661c9d00e.png#clientId=u0dc0f196-419b-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u8a5e4c42&margin=%5Bobject%20Object%5D&originHeight=536&originWidth=1470&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ue3a6974f-5b40-4079-8525-585905cb4a8&title=)
> ### 16.3.2.	解决方案
> 缓存失效时的雪崩效应对底层系统的冲击非常可怕！
> **解决方案：**
> （1）	**构建多级缓存架构：**nginx缓存 + redis缓存 +其他缓存（ehcache等）
> （2）	**使用锁或队列：**用加锁或者队列的方式保证来保证不会有大量的线程对数据库一次性进行读写，从而避免失效时大量的并发请求落到底层存储系统上。不适用高并发情况
> （3）	**设置过期标志更新缓存**：记录缓存数据是否过期（设置提前量），如果过期会触发通知另外的线程在后台去更新实际key的缓存。
> （4）	**将缓存失效时间分散开**：比如我们可以在原有的失效时间基础上增加一个随机值，比如1-5分钟随机，这样每一个缓存的过期时间的重复率就会降低，就很难引发集体失效的事件。

# 分布式锁
:::success
### 问题描述
随着业务发展的需要，**原单体单机部署**的系统被演化成**分布式集群系统**后，由于分布式系统多线程、多进程并且分布在不同机器上，这将使原单机部署情况下的并发控制锁策略失效，单纯的Java API并不能提供分布式锁的能力。为了解决这个问题就**需要一种跨JVM的互斥机制来控制共享资源的访问**，这就是分布式锁要解决的问题！
**分布式锁主流的实现方案**：

1. 基于数据库实现分布式锁
1. 基于缓存（Redis等）
1. 基于Zookeeper每一种分布式锁解决方案都有各自的优缺点：
1. 性能：redis最高
1. 可靠性：zookeeper最高这里，我们就基于redis实现分布式锁。
#### 我自己的理解
就是之前我们不是开始使用无中心化集群了吗？用户可以从任意个集群访问到数据，但我们要确保数据的正确性，对他的修改操作的正确性，slots 插槽感觉只能确保读取数据时数据的正确性，而需要对数据进行操作时，确保他的准确和完整，就需要上锁，锁住之后就只能我对他进行操作。我自己理解不到位，下面的两个链接大家可以看看，自己也可以去搜搜。
[分布式锁1](https://blog.csdn.net/x275920/article/details/125409441?ops_request_misc=&request_id=&biz_id=102&utm_term=%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-125409441.nonecase&spm=1018.2226.3001.4187)
[分布式锁2](https://blog.csdn.net/jkwanga/article/details/124087372?ops_request_misc=&request_id=&biz_id=102&utm_term=%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-124087372.nonecase&spm=1018.2226.3001.4187)
### 操作
![](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/shayu931/RedisBook12.png)
## 使用redis实现分布式锁 （可以试着做做，我没做）
redis命令 ： 
set sku:1:info “OK” NX PX 10000

EX second：设置键的过期时间为 second 秒。 SET key value EX second 效果等同于 SETEX key second value

PX millisecond：设置键的过期时间为 millisecond 毫秒。 SET key value PX millisecond 效果等同于 PSETEX key millisecond value 

NX ：只在键不存在时，才对键进行设置操作。 SET key value NX 效果等同于 SETNX key value 

XX ：只在键已经存在时，才对键进行设置操作。
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661764601077-3edfdd42-0423-44e1-a815-d9623d295712.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u45875969&margin=%5Bobject%20Object%5D&originHeight=413&originWidth=516&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u4076be8c-5a84-4598-bfa6-eba86adaf56&title=)
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661764600940-b806a8e1-8317-461c-82eb-0dddd6ae5bfc.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ud8c6a6d4&margin=%5Bobject%20Object%5D&originHeight=569&originWidth=1178&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u95c0345a-687d-432e-80b6-6a827fc54d6&title=)

1. 多个客户端同时获取锁（setnx）
1. 获取成功，执行业务逻辑{从db获取数据，放入缓存}，执行完成释放锁（del）
1. 其他客户端等待重试
### 16.4.3.	编写代码
Redis: set num 0
@GetMapping("testLock")
public void testLock(){
    //1获取锁，setne
    Boolean lock = redisTemplate.opsForValue().setIfAbsent("lock", "111");
    //2获取锁成功、查询num的值
    if(lock){
        Object value = redisTemplate.opsForValue().get("num");
        //2.1判断num为空return
        if(StringUtils.isEmpty(value)){
            return;
        }
        //2.2有值就转成成int
        int num = Integer.parseInt(value+"");
        //2.3把redis的num加1
        redisTemplate.opsForValue().set("num", ++num);
        //2.4释放锁，del
        redisTemplate.delete("lock");
    }else{
        //3获取锁失败、每隔0.1秒再获取
        try {
            Thread.sleep(100);
            testLock();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
重启，服务集群，通过网关压力测试：ab -n 1000 -c 100 [http://192.168.140.1:8080/test/testLock](http://192.168.140.1:8080/test/testLock)
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661764600981-323e1b8b-8467-404c-8822-423a55faa08b.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u4855bc4a&margin=%5Bobject%20Object%5D&originHeight=278&originWidth=554&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u2936eec0-c752-4418-b3ae-025988340d4&title=)
查看redis中num的值 ：
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661764600968-7a5a85d7-3555-4335-a96d-31d96ff30581.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u3e8c32ed&margin=%5Bobject%20Object%5D&originHeight=79&originWidth=254&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u64113b54-b466-4a01-b06c-57b4761d7fc&title=)
但是还会有一些问题，见下，问题及相关解决方案。
### 16.4.4.	优化之设置锁的过期时间
**问题：**setnx刚好获取到锁，业务逻辑出现异常，导致锁无法释放
**解决：**设置过期时间，自动释放锁。
设置过期时间有**两种方式：**

1. 首先想到通过expire设置过期时间（缺乏原子性：如果在setnx和expire之间出现异常，锁也无法释放）
1. 在set时指定过期时间（推荐）

![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661764600993-91507cd0-7d9f-49a5-b84d-a34cf1886916.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ue20dfdf3&margin=%5Bobject%20Object%5D&originHeight=469&originWidth=535&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ub67a2cd0-98dd-44fc-97c5-a953fa57531&title=)
设置过期时间：
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661764601643-085b740c-ec61-4031-b3ea-e14e6c2831f0.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ub7c40e22&margin=%5Bobject%20Object%5D&originHeight=216&originWidth=553&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uc61a7824-febc-4778-8de6-957570dd8f9&title=)
压力测试肯定也没有问题。自行测试
### 16.4.5.	优化之UUID防误删
**问题：**可能会释放其他服务器的锁。
**场景：**如果业务逻辑的执行时间是7s。执行流程如下

1. index1 业务逻辑没执行完，3秒后锁被自动释放。
1. index2 获取到锁，执行业务逻辑，3秒后锁被自动释放。
1. index3 获取到锁，执行业务逻辑
1. index1 业务逻辑执行完成，开始调用del释放锁，这时释放的是index3 的锁，导致index3 的业务只执行1s就被别人释放。最终等于没锁的情况。

**解决：**setnx获取锁时，设置一个指定的唯一值（例如：uuid）；释放前获取这个值，判断是否自己的锁
**图片分析：**
![](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/shayu931/RedisBook13.png)
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661764601789-ed83bf4b-119b-409a-bfb7-b670163d8818.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ue50fad5c&margin=%5Bobject%20Object%5D&originHeight=477&originWidth=535&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uaf3a6b3e-3745-410b-8223-bc1c4d03bc0&title=)
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661764601914-05608d8f-af7d-4e70-9be8-57c64bd75894.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ua2fe31ab&margin=%5Bobject%20Object%5D&originHeight=376&originWidth=553&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uf655bad7-bd96-4d5d-ac89-6614e7735dc&title=)

### 16.4.6.	优化之LUA脚本保证删除的原子性
**问题：**删除操作缺乏原子性。
**图片分析：**
![](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/shayu931/RedisBook14.png)
**场景：**

1. index1执行删除时，查询到的lock值确实和uuid相等

uuid=v1
set(lock,uuid)；
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661764602340-3b5987e8-fffc-4160-ba0a-d6f196dcf0fd.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u2a14195d&margin=%5Bobject%20Object%5D&originHeight=26&originWidth=553&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u4196d6f7-dbe4-4a08-9126-63db796efdd&title=)

1. index1执行删除前，lock刚好过期时间已到，被redis自动释放在redis中没有了lock，没有了锁。

![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661764602309-e8f3899c-6b8b-45b5-898b-d1edee4d5983.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u027a1401&margin=%5Bobject%20Object%5D&originHeight=24&originWidth=379&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u30513b45-e4b1-4643-95be-2f75ebd1be7&title=)

1. index2 获取了lockindex2 线程获取到了cpu的资源，开始执行方法

uuid=v2
set(lock,uuid)；

1. index1执行删除，此时会把index2的lock删除index1 因为已经在方法中了，所以不需要重新上锁。index1有执行的权限。index1已经比较完成了，这个时候，开始执行

![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661764602455-1c87c577-c2a7-49e9-a52f-d5966cd6b67a.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=uc4d7171d&margin=%5Bobject%20Object%5D&originHeight=24&originWidth=379&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=udbaaa804-c111-40a8-9f54-17db58dc3fa&title=)
删除的index2的锁！
@GetMapping("testLockLua")
public void testLockLua() {
    //1 声明一个uuid ,将做为一个value 放入我们的key所对应的值中
    String uuid = UUID.randomUUID().toString();
    //2 定义一个锁：lua 脚本可以使用同一把锁，来实现删除！
    String skuId = "25"; // 访问skuId 为25号的商品 100008348542
    String locKey = "lock:" + skuId; // 锁住的是每个商品的数据
    // 3 获取锁
    Boolean lock = redisTemplate.opsForValue().setIfAbsent(locKey, uuid, 3, TimeUnit.SECONDS);

    // 第一种： lock 与过期时间中间不写任何的代码。
    // redisTemplate.expire("lock",10, TimeUnit.SECONDS);//设置过期时间
    // 如果true
    if (lock) {
        // 执行的业务逻辑开始
        // 获取缓存中的num 数据
        Object value = redisTemplate.opsForValue().get("num");
        // 如果是空直接返回
        if (StringUtils.isEmpty(value)) {
            return;
        }
        // 不是空 如果说在这出现了异常！ 那么delete 就删除失败！ 也就是说锁永远存在！
        int num = Integer.parseInt(value + "");
        // 使num 每次+1 放入缓存
        redisTemplate.opsForValue().set("num", String.valueOf(++num));
        /*使用lua脚本来锁*/
        // 定义lua 脚本
        String script = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
        // 使用redis执行lua执行
        DefaultRedisScript<Long> redisScript = new DefaultRedisScript<>();
        redisScript.setScriptText(script);
        // 设置一下返回值类型 为Long
        // 因为删除判断的时候，返回的0,给其封装为数据类型。如果不封装那么默认返回String 类型，
        // 那么返回字符串与0 会有发生错误。
        redisScript.setResultType(Long.class);
        // 第一个要是script 脚本 ，第二个需要判断的key，第三个就是key所对应的值。
        redisTemplate.execute(redisScript, Arrays.asList(locKey), uuid);
    } else {
        // 其他线程等待
        try {
            // 睡眠
            Thread.sleep(1000);
            // 睡醒了之后，调用方法。
            testLockLua();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
Lua 脚本详解：
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661764602400-6a6fe81c-4bbf-4ebd-89dd-ebaa755c39f6.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ue8328f38&margin=%5Bobject%20Object%5D&originHeight=258&originWidth=553&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u5364a68f-784a-4887-a9e8-fa3ac166b17&title=)
项目中正确使用：
定义key，key应该是为每个sku定义的，也就是每个sku有一把锁。
String locKey ="lock:"+skuId; // 锁住的是每个商品的数据
Boolean lock = redisTemplate.opsForValue().setIfAbsent(locKey, uuid,3,TimeUnit.SECONDS);
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661764602583-fae6cdd9-f0b0-4ea0-a14a-c9041f95564d.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u2501300d&margin=%5Bobject%20Object%5D&originHeight=181&originWidth=553&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u91b2131a-10a0-455c-889c-a56a9d82b1e&title=)
### 16.4.7.	总结
1、加锁
// 1. 从redis中获取锁,set k1 v1 px 20000 nx
String uuid = UUID.randomUUID().toString();
Boolean lock = this.redisTemplate.opsForValue()
      .setIfAbsent("lock", uuid, 2, TimeUnit.SECONDS);
2、使用lua释放锁
// 2. 释放锁 del
String script = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
// 设置lua脚本返回的数据类型
DefaultRedisScript<Long> redisScript = new DefaultRedisScript<>();
// 设置lua脚本返回类型为Long
redisScript.setResultType(Long.class);
redisScript.setScriptText(script);
redisTemplate.execute(redisScript, Arrays.asList("lock"),uuid);
3、重试
Thread.sleep(500);
testLock();
为了确保分布式锁可用，我们至少要确保锁的实现同时**满足以下四个条件**：

- 互斥性。在任意时刻，只有一个客户端能持有锁。
- 不会发生死锁。即使有一个客户端在持有锁的期间崩溃而没有主动解锁，也能保证后续其他客户端能加锁。
- 解铃还须系铃人。加锁和解锁必须是同一个客户端，客户端自己不能把别人加的锁给解了。
- 加锁和解锁必须具有原子性。

:::
# Redis6.0新功能
:::info
（这后面逐渐暴躁，没有去多写，就看了看）
##  ACL
### 简介
Redis ACL是Access Control List（访问控制列表）的缩写，该功能允许根据**可以执行的命令**和**可以访问的键**来限制某些连接。
在Redis 5版本之前，Redis 安全规则只有密码控制 还有通过rename 来调整高危命令比如 flushdb ， KEYS* ， shutdown 等。Redis 6 则提供ACL的功能对用户进行更细粒度的权限控制 ：
（1）接入权限:用户名和密码 （2）可以执行的命令 （3）可以操作的 KEY
参考官网：[https://redis.io/topics/acl](https://redis.io/topics/acl)
### 命令
命令：
    （1）查看权限列表：acl list
    （2）查看权限类别：acl cat
    （3）查看当前用户：acl whoami
    （4）创建和编辑用户：acl setuser
具体使用见下：
1、使用acl list命令展现用户权限列表（1）数据说明
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661765267313-39ae69fd-2885-45e6-a70d-a664cffa5e4c.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u1f39a8cb&margin=%5Bobject%20Object%5D&originHeight=179&originWidth=554&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=udced35c6-3d59-466d-aba7-0a7d2e2e4ce&title=)
2、使用acl cat命令（1）查看添加权限指令类别
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661765267190-891233b4-4287-4119-b7c2-3403801ea4e7.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u56497593&margin=%5Bobject%20Object%5D&originHeight=353&originWidth=201&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=udf416665-4b46-4387-aa93-fdc556a697f&title=)
（2）加参数类型名可以查看类型下具体命令
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661765267196-16508df9-32a6-4f75-b758-c5e0cbc34af1.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u353d08a5&margin=%5Bobject%20Object%5D&originHeight=336&originWidth=257&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=uef994e40-96be-4355-a49d-03020ddaecf&title=)
3、使用acl whoami命令查看当前用户
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661765267291-b082cbb1-c71c-4266-bd3e-44ad96a8978b.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u34bee10e&margin=%5Bobject%20Object%5D&originHeight=34&originWidth=212&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u4b9e8203-711f-4948-9925-e8be3bd0fa9&title=)
4、使用aclsetuser命令创建和编辑用户ACL
（1）ACL规则		下面是有效ACL规则的列表。某些规则只是用于激活或删除标志，或对用户ACL执行给定更改的单个单词。其他规则是字符前缀，它们与命令或类别名称、键模式等连接在一起。
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661765267558-af1385ab-d70b-4f79-a032-23f7277f3321.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ue3563bef&margin=%5Bobject%20Object%5D&originHeight=535&originWidth=748&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ufde4f5ef-09d2-4ea1-a0f9-8213d1e55a1&title=)
（2）通过命令创建新用户默认权限acl setuser user1
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661765267876-8d1a139d-2b08-469e-a0b5-438a6a17f1a6.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u1463afe0&margin=%5Bobject%20Object%5D&originHeight=82&originWidth=295&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u36af4245-7822-4926-88dc-3c8360bba21&title=)
在上面的示例中，我根本没有指定任何规则。如果用户不存在，这将使用just created的默认属性来创建用户。如果用户已经存在，则上面的命令将不执行任何操作。
（3）设置有用户名、密码、ACL权限、并启用的用户acl setuser user2 on >password ~cached:* +get
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661765267939-94d5e32f-90f3-4155-83ab-7ba701632016.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u8fcf5cd8&margin=%5Bobject%20Object%5D&originHeight=105&originWidth=554&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ub04490a1-fec3-4413-9e16-931b7eb32e3&title=)
(4)切换用户，验证权限
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661765268036-63adc135-0591-4eab-9ce5-5272e22e7f0f.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u467ab856&margin=%5Bobject%20Object%5D&originHeight=199&originWidth=554&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u59804b3d-a9a0-4153-b5a1-865f307f943&title=)
## IO多线程
### 简介
Redis6终于支撑多线程了，告别单线程了吗？
IO多线程其实指**客户端交互部分的网络IO交互处理模块多线程**，而非执行命令多线程。Redis6执行命令依然是单线程。
### 原理架构
Redis 6 加入多线程,但跟 Memcached 这种从 IO处理到数据访问多线程的实现模式有些差异。Redis 的多线程部分只是用来**处理网络数据的读写和协议解析**，执行命令仍然是单线程。之所以这么设计是不想因为多线程而变得复杂，需要去控制 key、lua、事务，LPUSH/LPOP 等等的并发问题。整体的设计大体如下:
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661765268040-1a6df778-d7c2-4990-939f-fe2f46819545.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=u41886b6d&margin=%5Bobject%20Object%5D&originHeight=347&originWidth=554&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=ud8220f11-d0ac-492e-bb2d-6383a4f6bc4&title=)
另外，多线程IO默认也是不开启的，需要再配置文件中配置
io-threads-do-reads  yes 
io-threads 4
##  工具支持 Cluster
之前老版Redis想要搭集群需要单独安装ruby环境，Redis 5 将 redis-trib.rb 的功能集成到 redis-cli 。
另外官方 redis-benchmark 工具开始支持 cluster 模式了，通过多线程的方式对多个分片进行压测。
![](https://cdn.nlark.com/yuque/0/2022/png/26770555/1661765268235-9b1920ec-080a-40a5-9b75-4fb5c84cbb8e.png#clientId=ucba575ac-63a8-4&crop=0&crop=0&crop=1&crop=1&from=paste&id=ue417a959&margin=%5Bobject%20Object%5D&originHeight=239&originWidth=554&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u78d14fb8-5bc1-4ead-8cb6-7e69109de94&title=)
##  Redis新功能持续关注
Redis6新功能还有：
1、RESP3新的 Redis 通信协议：优化服务端与客户端之间通信
2、Client side caching客户端缓存：基于 RESP3 协议实现的客户端缓存功能。为了进一步提升缓存的性能，将客户端经常访问的数据cache到客户端。减少TCP网络交互。
3、Proxy集群代理模式：Proxy 功能，让 Cluster 拥有像单实例一样的接入方式，降低大家使用cluster的门槛。不过需要注意的是代理不改变 Cluster 的功能限制，不支持的命令还是不会支持，比如跨 slot 的多Key操作。
4、Modules APIRedis 6中模块API开发进展非常大，因为Redis Labs为了开发复杂的功能，从一开始就用上Redis模块。Redis可以变成一个框架，利用Modules来构建不同系统，而不需要从头开始写然后还要BSD许可。Redis一开始就是一个向编写各种系统开放的平台。
:::
