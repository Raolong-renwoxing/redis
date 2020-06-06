# Redis数据库

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

  