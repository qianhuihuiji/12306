这个文档是记录阅读此项目文档的一些思考。比如能够学习的两点，涉及到的知识点，关联的知识点，等等这些，想到的东西都记录下来。

- 想到的第一点：补充测试用例，整整tdd
- 20240107
从 `ticket-service/ticket/query` 接口开始阅读。v1实现方法看完了，基本逻辑捋顺了，但是还没感受到性能瓶颈在哪里。看完v2版本实现再对比。未完待续。。。

- 20240111
  阅读 login 接口，准备从功能、页面入手
- 20240112
 rocket mq安装有问题，无法下单
 按照文档所说，配置文件放在 /docker/software/rocketmq/conf/broker.conf 中：
```
brokerClusterName = DefaultCluster
brokerName = broker-a
brokerId = 0
deleteWhen = 04
fileReservedTime = 48
brokerRole = ASYNC_MASTER
flushDiskType = ASYNC_FLUSH
brokerIP1 = 127.0.0.1
```
启动容器命令为：
```
docker run -d \
-p 10911:10911 \
-p 10909:10909 \
--name rmqbroker \
--link rmqnamesrv:namesrv \
-v ${HOME}/docker/software/rocketmq/conf/broker.conf:/etc/rocketmq/broker.conf \
-e "NAMESRV_ADDR=namesrv:9876" \
-e "JAVA_OPTS=-Duser.home=/opt" \
-e "JAVA_OPT_EXT=-server -Xms512m -Xmx512m" \
foxiswho/rocketmq:broker-4.5.1
```
这样启动的容器，java程序连不上，因为 `brokerIP1`配置不生效，只能进入到容器，修改`/etc/rocketmq/broker.conf`，加上这行配置再重启：
```
brokerIP1 = 宿主机ipv4地址
```
用 `ipconfig`查看得到，本机是 `192.168.2.27`

- 20240114 
 阅读 `ticket-service/ticket/purchase/v2` 接口，未完待续

  - 20240115
   `rocket mq console` 要比 rocket mq 后启动，不然无法连接。
    - 阅读代码中提到了令牌桶算法
    - `ReentrantLock` 锁用法