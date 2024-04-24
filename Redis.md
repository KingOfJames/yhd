# Redis

* Redis：REmote Dictionary Server(远程字典服务器)；使用**C语言编写遵循BSD协议**，是一个高性能的**Key-Value内存数据库(NOSQL的一种)**提供了丰富的数据结构，例如：String、Hash、List、Set、SortedSet等等。数据是存在**内存**中的，同时Redis支持事务、持久化、LUA脚本、发布/订阅、缓存淘汰、流技术等多种功能特性提供了主从模式、Redis Sentinel和Redis Cluster集群架构方案；
* Redis与MySQL的区别：
  * Redis数据操作主要在内存，而MySQL主要存储在磁盘；
  * Redis在某一些场景使用中要明显优于MySQL，比如计数器、排行榜等方面；
  * Redis通常用于一些特定场景，需要与MySQL一起配合使用
  * 两者并不是相互替换和竞争关系，而是共用和配合使用；
* Redis的优势：
  * 性能极高，Redis能读的速度是110000次/秒，写的速度是81000次/秒；
  * Redis数据类型丰富，不仅仅支持简单的key-value类型的数据，同时还提供list、set、zset、hash等数据结构的存储；
  * Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用；
  * Redis支持数据的备份，即master-slave模式的数据备份；
* Redis中文文档：
  * http://www.redis.cn/
  * https://www.redis.com.cn/documentation.html
* Redis英文文档：
  * https://redis.io/commands/




# Redis的安装(Linux)

* 最好在Linux系统用Redis；

* 查看Linux系统的位数：`getconf LONG_BIT`；

* 检查是否有gcc的环境：`gcc -v`由于redis是由c语言编写的，它的运行需要C环境；

* 如果提示`gcc -v`不是内部命令，则需要下载C的环境：`yum -y install gcc gcc-c++ `；

* 查看Redis版本：`redis-server -v`；

* 如果没有，则需要下载Redis的Linux版本压缩包，并且使用XFTP将本地的Redis压缩包传输到Linux虚拟机中；

* Redis压缩包放在**/opt**目录下，并将Redis压缩包压缩：使用`tar -zxvf 压缩包名称`命令压缩；

* 进入Redis-7.0.14目录：![image-20240415135137145](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240415135137145.png)

  * Makefile：是一个编译的文件；
  * redis.conf：是redis的配置命令；
  * src：redis的源码；

* 在Redis-7.0.14目录下执行make命令：一步到位的命令：`make && make install`；

  * 出现该语句说明![image-20240415135502427](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240415135502427.png)成功；

  

* redis在linux下的默认安装路径为/usr/local/bin；

* ![image-20240415140540059](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240415140540059.png)

  * redis-benchmark：性能测试工具，服务启动后运行该命令；
  * redis-check-aof：修复有问题的AOF文件；
  * redis-check-dump：修复有问题的dump.rdb文件；
  * redis-cli：客户端，操作入口；
  * redis-sentinel：redis集群使用；
  * redis-server：Redis服务器启动命令；

  

* ![image-20240415142225501](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240415142225501.png)

  * 将默认的redis.conf拷贝到自己定义好的一个路径下，比如/myredis：使用`cp redis.conf /myredis/redis7.conf`命令拷贝；
  * 并且修改/myredis目录下redis.conf配置文件做初始化设置；

* 启动服务：使用`redis-server /myredis/redis7.conf`命令启动服务；

  * 查看redis的端口占用情况：`ps -ef|grep redis|grep -v grep`命令查看redis-server端口占用；

* 连接服务：使用`redis-cli -a redis密码 -p 6379`命令连接服务，`-a`代表授权登录；

  * 出现![image-20240415144831368](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240415144831368.png)代表连接成功；
  * 要redis不出现中文乱码，可以使用`redis-cli -a redis密码 --raw`来启动服务；

* 关闭服务：

  * 单实例关闭：`redis-cli -a redis密码 shutdown`命令；
  * 多实例关闭，指定端口关闭：`redis-cli -p 6379 shutdown`命令；
  
* 查看当前服务运行在那个文件夹：`config get dir`；



# Redis十大数据类型

## String（字符串）

* string是redis最基本的类型，一个key对应一个value；
* string类型是二进制安全的(即，string实现了序列化)，redis的string可以包含任何数据，比如jpg图片或者序列化对象；
* string类型是redis最基本的数据类型，一个redis中字符串value最多可以是512M；



* 单值单value；
* 常用：
  * set key value: `set key value [NX|XX] [GET] [EX seconds|PX milliseconds|EXAT unix-time-seconds|PXAT unix-time-milliseconds|KEEPTTL]`，set命令有EX、PX、NX、XX以及KEEPTTL五个可选参数，其中KEEPTTL为6.0版本添加的可选参数，其它为2.6.12版本添加的可选参数；
    * NX：键不存在的时候设置键值；
    * XX：键存在的时候设置键值；
    * EX seconds：以秒为单位设置过期时间；
    * PX milliseconds：以毫秒为单位设置过期时间；
    * EXAT timestamp：设置以秒为单位的UNIX时间戳所对应的时间为过期时间；
    * PXAT milliseconds-timestamp：设置以毫秒为单位的UNIX时间戳所对应的时间为过期时间；
    * KEEPTTL：保留设置前指定键的生存时间；
    * GET：返回指定键原本的值，若键不存在时返回nil；
* 同时设置/获取多个键值：
  * `mset key1 value1 [key2 value2 ...]`：同时设置多个键值；
  * `mget key1 [key2 ....]`：同时获取多个键值；
  * `msetnx key1 value1 [key2 value2 .....]`：同时设置多个键不存在时的键值，如果其中有一个存在，则整体都失败；
* 获取指定区间范围内的值：
  * `getrange key 开始索引 结束索引`：获取指定范围内的值，类似于Java的subString方法，如果是从0到-1，则是获取value的全部值；
  * `setrange key 开始索引 替换的值`：设置开始索引及以后的值（只会从开始索引开始替换长度为替换的值的长度的值），例如： k1 -> abcd1234 ，使用 setrange k1 1 xxyy后变为 k1 -> axxyy234；
* 数值增减：一定要全是数字才能进行加减；
  * 递增数字：`INCR key`，类似于数字自增；
  * 增加指定的整数：`INCRBY key increment`；
  * 递减数值：`DECR key`，类似于数字自减；
  * 减少指定的整数：`DECRBY key decrement`；
* 获取字符串长度和内容追加：
  * 获取字符串长度：`STRLEN key`；
  * 字符串内容追加：`APPEND key value`；

* 分布式锁：`set key value [EX seconds] [PX milliseconds] [NX|XX]`；
  * EX：key在多少秒之后过期；
  * PX：key在多少毫秒之后过期；
  * NX：当key不存在的时候，才创建key，效果等同于setnx；
  * XX：当key存在的时候，覆盖key；

* getset：将给定key的值设为value，并返回key的旧值，即先get然后set；



## List（列表）

* redis列表是简单的字符串列表，按照插入顺序排序。可以添加一个元素到列表的头部或者尾部；
* List的底层实际是个双端链表，最多可以包含2^32 - 1个元素（4294967295，美格列表超过40亿个元素），主要功能有push/pop等，一般用在栈、队列、消息队列等场景；
* left、right都可以插入或添加；
  * 如果键不存在，创建新的链表；
  * 如果键已存在，新增内容；
  * 如果值全移除，对应的键也就消失了；

* 它的底层实际是个双向链表，对两端的操作性能很高，通过索引下标的操作中间的节点性能会较差；
* ![image-20240416211624743](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240416211624743.png)



* 往列表中添加和遍历列表：
  * `LPUSH key value1 [value2 ...]`：向键名为key的列表中，从左到右的方向添加一个或多个值；
  * `RPUSH key value1 [value2 ...]`：向键名为key的列表中，从右到左的方向添加一个或多个值；
  * `LRANGE key start stop`：将key从左向右的，从start到stop的遍历这个列表；
* 从列表中移除左/右的第一个值：
  * `LPOP key`：移除列表key左边第一个值；
  * `RPOP key`：移除列表key右边第一个值；
* 通过索引下标获取列表中的元素：`LINDEX key index`；
* 获取列表中元素的个数：`LLEN key`；
* 删除N个值等于value的元素：`LREM key N value`；
* 截取指定索引区间的元素，然后重新赋值给列表key：`LTRIM key start stop`；
* 移除列表list1的右边第一个元素，并将该元素添加到另一个列表list2并返回：`RPOPLPUSH key1 key2`；
* 将列表key中index索引位置的值替换为value：`LSET key index value`；
* `LINSERT key before/after 已有的值 将插入的新值`：在列表key中，在已有值之前或之后插入新值；



## Hash（哈希表）

* Redis hash是一个string类型的field（字段）和value（值）的映射表，hash特别适合用于存储对象；
* Redis中每个hash可以存储2^32 - 1个键值对（40亿）；
* 类似于Java中的 ： `Map<String,Map<Object,Object>>`；
* 多值多value，且无重复，重复了的值的value会被新的值的value覆盖；



* 添加哈希表类型的元素：`HSET key filed1 value1 [filed2 value2 ...]`；
* 获取哈希表类型的某一个字段的值：`HGET key filed`;
* 添加哈希表类型的多个元素：`HMSET key filed1 value1 [filed2 value2 ...]`；
* 获取哈希表类型的多个键的值：`HMGET key filed1 [filed2 ...]`；
* `HGETALL key`：类似于Java中的HashMap的遍历，只不过是键值都不在同一行输出；
*  删除哈希表类型的多个键值对：`HDEL key filed1 [filed2 ...]`；
* 获取哈希表类型的key内的全部数量：`HLEN key`；
* 判断在哈希表类型的key中是否含有名字为filed的键：`HEXISTS key filed`；
* 获取出哈希表类型的key的所有键：`HKEYS key`；
* 获取出哈希表类型的key的所有键对应的值：`HVALS key`；
* 指定哈希表类型的key的键的值增加整数：`HINCRBY key filed 整数`；
* 指定哈希表类型的key的键的值增加浮点数：`HINCRBYFLOAT key filed 浮点数`；
* `HSETNX key filed1 value1 [filed2 value2 ...]`：不存在为哈希表类型的key时就赋值，否则就不赋值；



## Set（集合）

* Redis的Set是string类型的**无序集合**。集合成员是唯一的，这就意味着集合中不能出现重复的数据，集合对象的编码可以是intest或者hashtable；
* Redis中Set集合是通过**哈希表**实现的，所以添加，删除，查找的**时间复杂度都是O(1)**；
* 集合中最大的成员数为2^32 - 1（4294967295，美格集合都可存储40多亿个成员）；
* 单值多value，且无重复；



* 添加元素：`SADD key member1 [member2 ...]`；

* 遍历集合：`SMEMBERS key`；

* 判断集合中是否含有某一个元素：`SISMEMBER key member`；

* 删除集合中的某一个元素：`SREM key member`；

* 统计集合中有几个元素：`SCARD key`；

* 从集合中随机展现N个元素，但元素不删除：`SRANDMEMBER key N`，N取整数；

* 从集合中随机弹出N个元素，出N个删N个：`SPOP key N`，N取整数；

* 将集合key1里已存在的某个值赋给集合key2，并从集合key1中移除：`SMOVE key1 key2 member`；

* 集合运算：

  * A-B（属于A但不属于B的元素构成的集合）：`SDIFF key1 [key2 ...]`；
  * A∪B（属于A或者属于B的元素合并后的集合）：`SUNION key1 [key2 ...]`；
  * A∩B（属于A同时也属于B的共同拥有的元素构成的集合）：
    * `SINTER key1 [key2 ...]`；
    * `SINTERCARD numkeys key1 [key2 ...] [LIMIT count]`：numkeys表示集合的个数，`LIMIT count`表示需要输出的个数，该命令是Redis7的新命令。不返回结果集，只返回结果集里结果的个数；

  



## ZSet（有序集合）

* ZSet（sorted set：有序集合）；
* Redis ZSet和Set一样也是string类型元素的集合，且不允许重复的成员；
  * 不同的是每个元素都会关联一个double类型的分数，redis正是通过分数来为集合中的成员进行从小到大的排序；
  * ZSet的成员是唯一的，但分数(score)却可以重复；
  * ZSet集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。集合中最大的成员数为2^32 - 1；



* 添加元素：`ZADD key score1 member1 [score2 member2 ...]`；
* 按照元素分数从小到大的顺序返回索引从start到stop之间的所有元素：`ZRANGE key start stop [WITHSCORES]`；
* 按照元素分数从大到小的顺序返回索引从start到stop之间的所有元素：`ZREVRANGE key start stop [WITHSCORES]`；
* 获取指定分数范围的元素：`ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]`；
  * withscores：分数和值一起输出；
  * (：不包含的意思，例如( 60 90:表示区间(60,90]；
  * limit的作用是返回限制：例如 limit 0 1表示为：从下标为0的开始，取一个；
* 获取元素的分数：`ZSCORE key member`；
* 获取集合中元素的数量：`ZCARD key`；
* 删除元素：`ZREM key member1 [member2 ...]`；
* 增加某个元素的分数：`ZINCRBY key increment member`；
* 获取指定分数范围内的元素个数：`ZCOUNT key min max`；
* 从键名列表中的numkeys个非空排序集中弹出一个或多个元素，它们是成员分数对：`ZMPOP numkeys key1 [key2 ...] MIN|MAX [COUNT count]`；numkeys是集合的个数，min|max是取集合中最小或最大的分数对，count是弹出几个；
* 获得集合中某个元素的下标值：`ZRANK key memeber`；
* 获得集合中某个元素逆序的下标值：`ZREVRANK key member`；



## GEO（地理空间）

* Redis GEO主要用于存储地理位置信息，并对存储的信息进行操作，包括：
  * 添加地理位置的坐标；
  * 获取地理位置的坐标；
  * 计算两个位置之间的距离；
  * 根据用户给定的经纬度坐标来获取指定范围内的地理位置集合；
* 底层使用zset集合；
* 添加经纬度坐标：`GEOADD 经度1 纬度1 member1 [经度2 纬度2 member2 ...]`； 
* 从给定的key里返回所有指定member的位置(经度和纬度)，不存在的返回null：`GEOPOS key member1 [member2 ...]`；
* Redis GEO使用geohash来保存地理位置的坐标，geohash用于获取一个或多个位置元素的geohash值：`GEOHASH key member1 [member2 ...]`；
* 获取两个位置之间距离：`GEODIST key member1 member2 [m|km|ft|mi]`，m代表米，km代表千米，ft代表英尺，mi代表英里；
* 以给定的经纬度为中心，返回键包含的位置元素中，与中心的距离不超过给定最大距离的所有位置元素：`GEORADIUS key 经度 纬度 半径 [m|km|ft|mi] [WITHDIST] [WITHCOORD] [WITHHASH] [COUNT count] [DESC|ASC]`；
  * WITHDIST：在返回位置元素的同时，将位置元素与中心之间的距离也一并返回。距离的单位和用户给定的范围单位保持一致；
  * WITHCOORD：将位置元素的经度和纬度也一并返回；
  * WITHHASH：以52位有符号整数的形式，返回位置元素经过原始geohash编码的有序集合分值。主要用于底层应用或调试；
  * COUNT：限定返回的记录数；
  * `DESC|ASC`：desc是按距离从大到小排序展示，asc是按距离从小到大排序展示；

* 以给定的member为中心，返回键包含的位置元素中，与中心的距离不超过给定最大距离的所有位置元素：`GEORADIUSBYMEMBER key member 半径 [m|km|ft|mi] [WITHDIST] [WITHCOORD] [WITHHASH] [COUNT count] [DESC|ASC]`，与上一个相比输入要简单，并且键中含有该member时，返回结果中会含有该member，并且距离为0；



## HyperLogLog（基数统计）

* HyperLogLog是用来做基数统计的算法，HyperLogLog的优点是：在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定且是很小的；
* 在Redis里面，每个HyperLogLog键只需要花费12KB内存，就可以计算接近2^64个不同元素的基数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比；
* 由于HyperLogLog只会根据输入元素来计算基数，而不会存储输入元素本身，所以HyperLogLog不能像集合那样，返回输入的各个元素；
* 去重复统计功能的基数估计算法-》HyperLogLog；
* 基数：是一种数据集，去重复后的真实个数；
* 基数统计：用于统计一个集合中不重复的元素个数，就是对集合去重复后剩余元素的计算；



* 基本命令：

* ![image-20240422151850272](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240422151850272.png)

  

## bitmap（位图）

* Bit arrays（or simply bitmaps，我们可以称之为位图）。Bitmap支持的最大位数是2^32位；
* ![image-20240415154043426](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240415154043426.png)
* 由0和1状态表现的二进制位的bit数组；
* 用String类型作为底层数据结构实现的一种统计二值状态的数据类型。**位图本质是数组**，它是基于String数据类型的按位的操作。该数组由多个二进制位组成，每个二进制位都对应一个偏移量(称之为一个索引)；



* 基本命令：<img src="C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240422135740967.png" alt="image-20240422135740967"  />

* 统计某一个位图的字节数占用多少：`STRLEN key`；
  * 注：不是字符串长度而是占据几个字节，8位二进制为一个字节，超过8位后自己按照8位一组一字节再扩容；
* 统计某个键里面索引start到end中含有1的有多少个：`BITCOUNT key [start end]`；
* 对不同的二进制存储数据进行位运算(AND、OR、NOT、XOR)：`BITOP 位运算的英文 目标key key1 key2 [key3 ...]`；



## bitfield（位域）

* 通过bitfield命令可以一次性操作多个比特位域（指的是连续的多个比特位），它会执行一系列操作并返回一个响应数组，这个数组中的元素对应参数列表中的相应操作的执行结果；
* 就是通过bitfield命令我们可以一次性对多个比特位域进行操作；



* BITFIELD命令可以将一个Redis字符串看作是一个由二进制位组成的数组，并对这个数组中任意偏移进行访问。可以使用该命令对一个有符号的5位整型数的第1234位设置指定值，也可以对一个31位无符号整型数的第4567位进行取值。
* 返回指定的位域：`BITFIELD key [GET type offset]`；
  * type有两种：一个是`uN`，表示N位无符号整数；一个是`iN`，表示N位有符号整数；
  * offset表示位置；
* 设置指定位域的值：`BITFIELD key [SET type offset value]`；
* 自增：`BITFIELD key [INCRBY type offset increment]`，默认情况下，INCRBY使用WRAP参数；
* 溢出控制：`OVERFLOW [WRAP|SAT|FAIL]`；
  * WRAP：使用回绕方法处理有符号整数和无符号整数的溢出情况；
  * SAT：使用饱和计算方法处理溢出，下溢计算的结果为最小的整数值，而上溢计算的结果为最大的整数值；
  * FAIL：命令将拒绝执行那些会导致上溢或者下溢情况出现的计算，并向用户返回空值表示计算未被执行；





## Stream（流）

* Redis Stream是Redis5.0版本新增加的数据结构；
* Redis Stream主要用于消息队列（MQ，Message Queue），Redis本身是有一个Redis发布订阅（pub/sub）来实现消息队列的功能，但它有个缺点就是消息无法持久化，如果出现网络断开、Redis宕机等，消息就会被丢弃；
* 发布订阅（pub/sub）可以分发消息，但无法记录历史消息；
* 而Redis Stream提供了消息的持久化和主备复制功能，可以让任何客户端访问任何时刻的数据，并且能记住每一个客户端的访问位置，还能保证消息不丢失；
* ![image-20240422165805098](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240422165805098.png)



队列组相关指令：

* 向Stream队列中添加消息，如果指定的Stream队列不存在，则该命令执行时会新建一个Stream队列：`XADD key *|id filed1 value1 [filed2 value2 ...]`；
  * *表示服务器自动生成MessageID（类似mysql里面的主键自增）；
  * 会返回一个信息条目：即序列号（毫秒时间戳-该毫秒内产生的第n条消息减1），在相同的毫秒下序列号从0开始递增，序列号是64位长度；
* 获取消息列表（可以指定范围），忽略删除的消息：`XRANGE key start end [COUNT count]`；
  * start表示开始值，-代表最小值；
  * end表示结束值，+代表最大值；
  * count表示最多获取多少个值；
* 获取逆序的消息列表（可以指定范围），忽略删除的消息：`XREVRANGE key end start [COUNT count]`；
* 删除消息列表中的消息：`XDEL key id1 [id2 ...]`；
* 获取Stream队列的消息的长度：`XLEN key`；
* 允许的最大长度，对流进行修剪限制长度：`XTRIM key MAXLEN COUNT`；
* 允许的最小id，从某个id值开始比该id值小的将会被抛弃：`XTRIM key MINID id`；
* 获取消息（阻塞/非阻塞），只会返回大于指定ID的消息：`XREAD [COUNT count] [BLOCK milliseconds] STREAMS key1 [key2 ...] ID1 [ID2 ...]`；
  * count表示最多读取多少条消息；
  * BLOCK表示是否以阻塞的方式读取消息，默认不阻塞，如果milliseconds设置为0，表示永远阻塞；



* Stream的基础方法，使用xadd存入消息和xread循环阻塞读取消息的方式可以实现简易版的消息队列；
* ![image-20240422182022298](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240422182022298.png)



消费组相关指令：

* 创建消费者组：`XGROUP CREATE key 分组名称 $|0`；
  * $表示从Stream尾部开始消费；
  * 0表示从Stream头部开始消费；
* 按消费者组来读：`XREADGROUP GROUP 分组名称 消费者名称 [COUNT count] STREAMS key1 [key2 ...] ID1|> [ID2 ...]`；
  * ">"表示从一条尚未被消费的消息开始读取；
  * 消费组groupA内的消费者consumer1从mystream消息队列中读取所有消息，但是不同消费组的消费者可以消费同一消息，同一消费组的不可以消费同一消息；
  * 注：如果不写count，默认为消费该组的全部消息；
* 消费组的目的：让组内的多个消费者共同分担读取消息，所以，我们通常会让每个消费者读取部分消息，从而实现消息读取负载在多个消费者间是均衡分布的；
* 查询每个消费组内所有消费者（已读取、但尚未确认）的消息：`XPENDING key group [start end count [consumer]]`；
* 向消息队列确认消息处理已完成：`XACK key group id1 [id2 ...]`；

* 打印Stream\Consumer\Group的详细信息：`XINFO STREAM key`；



# Redis常用命令

* `keys *`：查看当前库所有的key；
* `exists key`：判断某个key是否存在，返回1表示存在，返回0表示不存在，返回值>1表示有n个key存在；
* `type key`：查看指定key是什么数据类型的；
* `del key`：删除指定的key数据，返回1表示删除成功，返回0表示删除失败；
* `unlink key`：非阻塞删除，仅仅将keys从keysapce元数据中删除，真正的删除会在后续异步中操作；
* `ttl key`：查看还有多少秒过期，-1表示永不过期，-2表示已过期；
* `expire key 秒钟`：为给定的key设置过期时间；
* `move key dbindex[0-15]`：将当前数据库的key移动到给定的数据库db当中，Redis默认有16个库；
* `select dbindex`：切换数据库【0-15】，默认为0；
* `dbsize`：查看当前数据库key的数量；
* `flushdb`：清空当前库；
* `flushall`：通杀全部库；
* `config get dir`：查看dump文件的保存路径；

**注**：

​	①：Redis的命令不区分大小写，但key是区分大小写的；

​	②：永远的帮助命令，`help @类型`；



# Redis的持久化

## RDB

* RDB（Redis数据库）：RDB持久性以指定的时间间隔执行数据集的时间点快照；
  * 实现类似照片记录效果的方式，就是把某一时刻的数据和状态以文件的形式写到**磁盘**上，也就是**快照**。这样一来即使故障宕机，快照文件也不会丢失，数据的可靠性也就得到了保证；
  * 这个快照文件就称为RDB文件（dump.rdb），其中，RDB就是Redis DataBase的缩写 ；

* 自动备份：在redis.conf文件里的**SNAPSHOTTING**下面第一个位置配置命令：`save <seconds1> <changes1> [<seconds2> <changes2> ...]`，并保存后，Redis将按照配置的时间间隔及执行次数将数据集保存到磁盘中；
  * 修改dump文件保存路径：在redis.conf文件中，还可在SNAPSHOTTING中找到dir 命令，将该单词后面的目录换成自己的dump文件路径；
  * 修改dump文件名称：在redis.conf文件中，还可以在SNAPSHOTTING中找到dbfilename命令，将后缀前的名字改为自己想改的名字；
  * 触发备份：当间隔时间及执行数据集的次数达到配置文件中的配置时，就会触发RDB备份；
  * 执行flushall/flushdb命令也会产生dump.rdb文件，但里面是空的，无意义；
  * 物理恢复：不可以把备份文件dump.rdb和生产Redis服务器放在同一台机器，必须分开各自存储，以防生产机物理损坏后备份文件也挂了；所以将备份文件移动到redis安装目录并启动服务即可恢复备份；
* 手动备份：Redis提供了两个命令来生产RDB文件，分别是save和bgsave（默认）；
  * save：在主程序中执行会**阻塞**当前Redis服务器，直到持久化工作完成执行save命令期间，Redis不能处理其他命令，一般禁止使用；
  * bgsave：Redis会在后台异步进行快照操作，**不阻塞**快照同时还可以响应客户端请求，该触发方式会fork一个子进程由子进程复制持久化过程；
  * fork是什么：在Linux程序中，fork()会产生一个和父进程完全相同的子进程，但子进程在此后多会exec系统调用，出于效率考虑，尽量避免膨胀；
  * LASTSAVE：可以通过该命令获取最后一次成功执行快照的时间；
    * 返回的是Linux时间戳，可以在Linux中使用`date -d @时间戳`命令来转为其他格式；
* 优势：
  * 适合大规模的数据恢复；
  * 按照业务定时备份；
  * 对数据完整性和一致性要求不高；
  * RDB文件在内存中的加载速度要比AOF快得多；
* 劣势：
  * 在一定的间隔时间做一次备份，如果Redis意外宕机，就会丢失从当前至最近一次快照期间的数据，快照之间的数据就会丢失；
  * 内存数据的全量同步，如果数据量太大会导致I/O严重影响服务器性能；
  * RDB依赖于主进程的fork，在更大的数据集中，这可能会导致服务器请求的瞬间延迟。fork的时候内存中的数据被克隆了一份；
* 禁用快照：在redis.conf文件中的SNAPSHOTTING中配置`save ""`就禁用掉了快照功能；



* RDB优化配置项：SNAPSHOTTING模块；
  * `save <seconds> <changes>`：设置快照规则；
  * `dbfilename`：设置快照后的文件名；
  * `dir`：设置快照文件的保存路径；
  * `stop-writes-on-bgsave-error`：默认为yes，如果配置为no，表示不在乎数据不一致或者有其他手段发现和控制这种不一致，那么在快照写入失败时，也能确保Redis继续接收新的写请求；
  * `rdbcompression`：默认yes，对于存储到磁盘中的快照，可以设置是否进行压缩存储。如果是的话，Redis会采用LZF算法进行压缩；
  * `rdbchecksum`：默认yes，在存储快照后，还可以让Redis使用CRC64算法来进行数据校验，但是这样做会增加大约10%的性能消耗，如果希望获取到最大的性能提升，可以关闭此功能；
  * `rdb-del-sync-files`：在没有持久性的情况下删除复制中使用的RDB文件启用。默认为no；
* ![image-20240423165848975](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240423165848975.png)



## AOF

* AOF（Append Only File）：以日志的形式来记录每个写操作，将Redis执行过的所有写指令记录下来（读操作不记录），只许追加文件但不可以改写文件，Redis启动之初会读取该文件重新构建数据，换言之，Redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作；
* 默认情况下，Redis没有开启AOF的。开启AOF功能需要设置配置：`appendonly yes`；



AOF持久化工作流程：![image-20240423210539640](C:\Users\风吹落叶红\AppData\Roaming\Typora\typora-user-images\image-20240423210539640.png)

①客户端作为命令的来源，会有多个源头以及源源不断的请求命令，这些将被写入AOF缓冲区的命令包括增、删、改命令，不包含查命令；

②这些命令到达Redis服务器后**并不是直接写入AOF文件**，而是将这些命令先放入AOF缓存区中进行保存。**这里的AOF缓冲区实际上是内存中的一片区域**，存在的目的是当这些命令达到一定量以后再写入磁盘，避免了频繁的磁盘I/O操作；

③AOF缓冲区会根据AOF缓冲区**同步文件的三种写回策略**将命令写入磁盘的AOF文件；

④随着写入AOF内容的增加为避免文件膨胀，会根据规则进行命令的合并（又称为**AOF重写**），从而起到AOF文件压缩的目的；

⑤当Redis服务器重启时会从AOF文件中载入数据；



AOF的三种写回策略：

* Always：同步写回，每个写命令执行完立刻同步的将日志写回磁盘，即上述②和③一起执行，但会有频繁的I/O操作；
* everysec（默认）：每秒写回，每个写命令执行完，只是先把日志写到AOF文件的内存缓冲区，每隔一秒把缓冲区中的内容写入磁盘；
* no：操作系统控制的写回，每个写命令执行完，只是先把日志写到AOF文件的内存缓冲区，由操作系统决定何时将缓冲区内容写回磁盘；

|  配置项  |     写回时机     |           优点           |                 缺点                 |
| :------: | :--------------: | :----------------------: | :----------------------------------: |
|  Always  |     同步写回     | 可靠性高，数据基本不丢失 | 每个写命令都要写入磁盘，性能影响较大 |
| Everysec |     每秒写回     |         性能适中         |        宕机时丢失1秒内的数据         |
|    No    | 操作系统控制写回 |          性能好          |          宕机时丢失数据较多          |

