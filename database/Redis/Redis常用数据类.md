## 1. Redis 键-(key)

```
keys\*        //查看当前库所有 key(匹配:keys+1
exists key:   //判断某个 key 是否存在
type key:     //查看你的 key 是什么类型
del key:      //删除指定的 key 数据
unlink key:   //根据 value 选择非阻塞删除, 仅将 keys 从 keyspace 元数据中删除,真正的删除会在后续异步操作。
expire key10 秒钟: //为给定的 key 设置过期时间
ttl key:      //查看还有多少秒过期,-1 表示永不过期,-2 表示已过期
select:       //命令切换数据库
dbsize:       //查看当前数据库的 key 的数量
flush:        //清空当前库
flushall:     //通杀全部库
```

## 2. String

- String 是 Redis 最基本的类型,你可以理解成与 Memcached 一模一样的类型,一个 key 对应一个 value。
- String 类型是二进制安全的。意味着 Redis 的 string I 可以包含任何数据。比如 .jpg 图片/序列化的对象。
- String 类型是 Redis 最基本的数据类型, 一个 Redis 中字符串 value 最多可以是 512M

```
set <key> <value> //添加键值对，如果键已存在，会覆盖
get<key>          //查询对应键值
append <key> <value>     //将给定的< alue>追加到原值的未尾
strlen <key>      //获得值的长度
setnx <key> <value> //只有在key不存在时,设置key的值

incr <key>        //将key中储存的数字值増1, 只能对数字值操作,如果为空,新增值为1
decr <key>        //将key中储存的数字值减1, 只能对数字值操作,如果为空,新增值为-1
incrby/derby <key> <步长>  //将key中储存的数字值増减。自定义步长。
```
