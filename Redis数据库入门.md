Redis数据库

## 1.NoSQL数据库

### 1.1 什么是NoSQL数据库

- **`not only sql:`**不仅是`sql`
- 非关系型数据库。

### 1.2 `NoSQL`的特性

- 不支持`SQL`语法
- 没有像关系型数据库一样的通用语法
- 每种`NoSQL`数据库都有自己的`api`和语法，都有擅长的业务场景。

### 1.3 NoSQL与SQL数据库的比较

- 适用场景的不同：`nosql`数据库适合用于关系特别负责的场景；而`sql`适用于关系相对简单的场景。
- 事务：一组`SQL`语句操作，要么都成功，要么都失败。
- 事务的支持：`sql`对事务支持十分完善，`NoSQL`基本不支持事务。

## 2.Redis数据库

### 2.1 什么是Redis

- `NoSQL`家庭中的一员，属于非关系型数据库。
- 由`C语言`编写、支持网络、可基于内存、可持久化日志型数据库。

### 2.2 Redis有什么特性

- 以`KEY-VALUE`形式存储数据。
- 支持数据的可持久化，可以将内存中的数据保存在磁盘，重启时候可以再次进行加载使用。
- 除了`KEY-VALUE`形式存储外，同时还提供`list`、`set`、`zset`、`hash`等数据结构存储数据。
- 支持数据的备份，使用`master-slave`模式备份数据。

### 2.3 Redis有什么优势

- 性能高：读写速度快
- 丰富数据类型
- 原子性：避免资源争夺
- 丰富特性：通知、`key`过期、`publish/subscribe`等

### 2.4 Redis用在哪

- 做缓存，Redis的所有数据存放在内存中（内存数据库）
- 适合于社交类的应用场景
- 大型项目中的特定场景

## 3.Redis的安装与配置

### 3.1 Redis安装

- 以下操作基于`UBUNTU`系统，到官网下载`redis`最新稳定版本

- 开始安装操作

  ```
  tar -zxvf redis-版本号.tar.gz # 解压
  sudo mv ./redis-版本号 /usr/local/redis/
  cd /usr/local/redis/ # 切换到redis目录
  sudo make # 开始生成
  sudo make test # 测试
  sudo make install # 安装,将redis的命令安装到/usr/local/bin/⽬录
  cd /usr/local/bin #安装完成后，我们进入目录/usr/local/bin中查看
  ```

- 执行`sudo make test`可能报错

  ```
  hadoop@stormspark:~/workspace/redis2.6.13/src$ make test
  You need tcl 8.5 or newer in order to run the Redis test
  make: *** [test] Error 1
  ```

- 解决方法

  ```
  wget http://downloads.sourceforge.net/tcl/tcl8.6.1-src.tar.gz  
  sudo tar xzvf tcl8.6.1-src.tar.gz  -C /usr/local/  
  cd  /usr/local/tcl8.6.1/unix/  
  sudo ./configure  
  sudo make  
  sudo make install 
  ```

### 3.2 配置

- Redis的配置信息在`/etc/redis/redis.conf`下。

- 核心配置选项

  - 绑定ip：如果需要远程访问，可将此⾏注释，或绑定⼀个真实ip

    > bind 127.0.0.1

  - 端⼝，默认为6379

    > port 6379

  - 是否以守护进程运⾏

    - 如果以守护进程运⾏，则不会在命令⾏阻塞，类似于服务
    - 如果以⾮守护进程运⾏，则当前终端被阻塞
    - 设置为yes表示守护进程，设置为no表示⾮守护进程
    - 推荐设置为yes

    > daemonize yes

  - 数据⽂件

    > dbfilename dump.rdb

  - 数据⽂件存储路径

    > dir /var/lib/redis

  - ⽇志⽂件

    > logfile /var/log/redis/redis-server.log

  - 数据库，默认有16个

    > database 16

  - 主从复制，类似于双机备份。

    > slaveof

- 在配置时，有些许文件夹需要我们手动创建。

### 3.3 启动服务端和客户端

- 服务端启动与关闭

  ```
  ps -aux|grep redis # 查看redis服务器进程
  sudo kill -9 pid # 杀死redis服务器
  sudo redis-server /etc/redis/redis.conf # 指定加载的配置文件
  ```

- 客户端操作

  ```
  redis-cli # 启动服务端
  redis-cli --help # 查看客户端帮助
  ping # 测试客户端与服务端的链接（链接成功返回pong）
  SELECT INDEX # 切换数据库 默认16个 索引为0-15
  ```

## 4.数据类型以及操作

### 4.1 string类型

- 字符串类型是Redis中最基础的数据存储类型
- Redis中是二进制安全的，这便意味着类型可以接受任何格式的数据（图片数据/json等）
- 值最多可以容纳数据长度是512M

### 4.2 string操作

- 设置键值(设置的键不存在则为添加，存在则修改)

  ```
  set key value
  ```

-  根据键查询值

  ```
  get key
  ```

- 设置过期时间

  ```
  setex key seconds value
  ```

- 设置多个键值

  ```
  mset key1 value1 key2 value2
  ```

- 查询多个值

  ```
  mget key1 key2
  ```

- 在值基础追加数据

  ```
  append key value
  ```

 ### 4.3 键命令

- 查找键，参数支持正则表达式

  > keys pattern

- 判断键是否存在，存在返回1，不存在返回0

  > exists key1

- 查看对应值得类型

  > type key1

- 删除对应的值

  > del key1 key2

- 删除键

  > del key1 key2

- 如果没有指定过期时间则，一直存在，直到使用del删除

- 给没有设置过期时间的key加上过期时间

  > expire key1 seconds

- 查看时效，以秒为单位

  > ttl key

### 4.4 hash类型

- 用于存储对象，对象的结构为属性值
- 值得类型为string

### 4.5 hash操作

- 设置单个属性

  > hset key file value

- 设置多个属性

  > hmset key file value key1 file1 value1

- 获取指定键所有的属性

  > hkeys key

- 获取一个属性的值

  > hget key filed

- 获取多个属性的值

  > hmget key file1 file2

- 获取所有属性的值

  > hvals key

- 删除属性

  > hdel key1 file1 file2

### 4.5 list类型

- 列表的元素类型为string
- 按照插入顺序排序

### 4.6 list操作

- 往左侧插入数据

  > lpush key value1 value2

- 往右侧插入数据

  > rpush key value1 value2

- 在指定元素的前或后插入新元素

  > linsert key before|after 现有元素 新元素

- 返回指定范围的元素

  - start、stop为元素的下标索引
  - 索引从左侧开始，第一个元素为0
  - 索引可以是负数，表示从尾部开始计数

  > lrange key start stop # 包含下标
  >
  > lrange key 0 -1 # 列出所有的元素

- 设置指定索引位置的值

  > lset key index value

- 删除指定元素

  - 将列表中前count次出现的值为value的元素移除
  - count >0: 从头到尾移除
  - count<0: 从尾到头移除
  - count=0: 移除所有

  > lrem key count value

### 4.7 set类型

- 无序的集合
- 元素为string类型
- 元素具有唯一性，不重复
- 说明：对于集合没有修改操作

### 4.8 set操作

- 添加元素

  > sadd key member1 member2

- 获取所有的元素

  > smembers key

- 删除指定元素

  > srem key value1 value2

### 4.9 zset类型

- sorted set，有序集合
- 元素为string类型
- 元素具有唯一性，不重复
- 每个元素都会关联一个double类型的score，表示权重，通过权重将元素从小到大排序
- 说明：没有修改操作

### 4.10 zset操作

- 添加

  > zadd key socre1 member1 score2 member2 ...

- 获取元素

  - 返回指定范围内的元素
  - 索引从左侧开始第一个为0；索引为负数，表示从尾部开始计数。
  - start、stop为元素的下标索引

  > zrange key start stop

- 返回socre值在min和max之间的成员

  > zrangebyscore key min max

- 返回成员member的score值

  > zscore key member

- 删除指定元素

  > zrem key member1 member2

- 删除权重在指定范围的元素

  > zremrangebyscore key min max

## 5.python与Redis进行交互

### 5.1 在python中安装Redis库

- 建议需要使用到redis的虚拟环境安装第三番分库

  > pip install redis
  >
  > easy_install redis

- 到网站下载

  >wget https://github.com/andymccurdy/redis-py/archive/master.zip
  >
  >unzip master.zip
  >
  >cd redis-py-master
  >
  >sudo python setup.py install

### 5.2 调用模块

- 引入模块

  ```python
  from redis import * # 导入redis库
  ```

- 这个模块中提供StrictRedis对象（Strict严格），用于连接redis服务器，并按照不同类型提供了不同方法，进行交互操作

### 5.3 StrictRedis对象方法

- 使用对象的init方法创建对象，指定参数host、post与指定的服务器和端口进行连接。
  - host默认值localhost
  - port默认值6379
  - db默认值0

```python
sr = StrictRedis(host='localhost', port=6379, db=0)
# 简写
sr =StrictRedis()
```

- 根据不同类型，拥有不同的实例方法。（详细参考前面数据类型的操作）

```python
from redis import *

if __name__ == '__main__':
    # 创建一个StrictRedis对象，连接数据库
    try:
        sr = StrictRedis()
        # 添加一个key：value name：raolong
        res = sr.set('name','raolong')
        # 返回一个布尔值表示是否存储成功
        print(res)
        # 获取name的值
        name = sr.get('name') # 如果取不到值返回none
        # 修改值
        sr.set('name', 'renwoxing')
        # 删除多个键值
        sr.delete('name')
        # 获取数据库中所有的键
        sr.keys() # 返回一个列表
        
    except Exception as e:
        print(e)
```

### 5.4 django存储session（使用redis）

- 安装包

  > pip install django-redis-sessions==0.5.6

- 到django项目中settings.py修改配置

  ```python
  # 设置redis存储session信息
  SESSION_ENGINE = 'redis_sessions.session'
  # redis服务的ip地址
  SESSION_REDIS_HOST = 'localhost'
  # redis服务的端口号
  SESSION_REDIS_PORT = 6379
  # redis中的那个数据库
  SESSION_REDIS_DB = 2
  # 连接redis的密码
  SESSION_REDIS_PASSWORD = ''
  # 前缀：唯一标识码
  SESSION_REDIS_PREFIX = 'session'
  ```

- 到应用views.py中创建视图函数

  ```python
  def set_session(request):
      request.session['username'] = 'smart'
      request.session['age'] = 18
      
      return HttpResponse('设置session')
  
  def get_session(request):
      username = request.session['username']
      age = request.session['age']
      
      return HttpResponse(username + ':' + age)
  ```

- 到应用urls.py中配置路由

  ```python
  url(r'^set_session/$', views.set_session)
  url(r'^get_session/$', views.get_session)
  ```

- 我们可以在redis客户端查看

  ```
  # 切换到指定数据库
  select 2
  # 查看所有键
  keys *
  ```

## 6.主从配置

