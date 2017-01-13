## OnceDB

OnceDB基于Redis开发。将redis从一个键值对内存数据库，改造成支持查询和关系的NoSQL数据库。

- [OnceDB on Linux](https://github.com/OnceDoc/OnceDB)
- [OnceDB on Windows](https://github.com/OnceDoc/OnceDB.win)


## 安装

Linux 

    git clone https://github.com/OnceDoc/OnceDB.git
    cd OnceDB
    make

Windows

请到 [Release](https://github.com/OnceDoc/OnceDB.win/releases) 下载二进制可执行文件


## Redis参考

- [Redis指令参考](https://redis.io)
- [Redis on Linux](https://github.com/antirez/redis)
- [Redis on windows](https://github.com/MSOpenTech/Redis)



## 搜索指令说明

### search [key pattern] [operator] [value]

搜索字符串类型的健值

准备测试数据

    127.0.0.1:6379> set test1 'this is testing'
    OK
    127.0.0.1:6379> set test2 'this is article'
    OK
    127.0.0.1:6379> set test3 kris
    OK
    127.0.0.1:6379> set test4 10
    OK
    127.0.0.1:6379> set test5 100
    OK

= 完全匹配搜索

    127.0.0.1:6379> search test* = kris
    1) "test3"
    2) "kris"

~ 模糊搜索

    127.0.0.1:6379> search test* ~ is
    1) "test2"
    2) "this is article"
    3) "test1"
    4) "this is testing"

~| 包含并截取: 如果值过长则截取其中一段值打印，如果文字较短则会不截取

    127.0.0.1:6379> search test* ~| is
    1) "test2"
    2) "this is article"
    3) "test1"
    4) "this is testing"

<、>、<=、>= 比较搜索: 找到符合条件的数字

    127.0.0.1:6379> search test* > 20
    1) "test5"
    2) "100"



### hsearch [key pattern] [field] [operator] [value]

搜索hash类型的健值，operator同search

准备测试数据

    127.0.0.1:6379> hmset user:001 name kris email c2u@live.cn gender male
    OK
    127.0.0.1:6379> hmset user:002 name sunny age 24
    OK

搜索hash值age大于18，并打印出名字

    127.0.0.1:6379> hsearch user:* age > 18 name ~ ''
    1) "user:002"
    2) "24"
    3) "sunny"

### hselect [num of keys] key1 key2 key3 ... field1 field2 ...

批量打印出指定key的hash字段值

    127.0.0.1:6379> hselect 3 name email age user:001 user:002
    1) "user:001"
    2) "kris"
    3) "c2u@live.cn"
    4) (nil)
    5) "user:002"
    6) "sunny"
    7) (nil)
    8) "24"
