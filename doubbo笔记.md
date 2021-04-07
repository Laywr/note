# Doubbo

### 什么是分布式系统？

分布式系统是若干独立计算机的集合，这些计算机对于用户来说就像单个相关系统。

RPC（远程过程调用）----影响性能的两大要素，服务器之间的通信速度、序列化和反序列话的速度

灰度发布：服务升级更新时，先更新部分服务器，等这部分服务器运行没问题后，再慢慢更新剩下的服务器

### springBoot整合doubbo

1.导入doubbo-starter,在application.properties中配置属性,使用@DoubboService暴露服务,使用@Reference引用服务,在启动类上加@EnableDoubbo

2.保留doubbo.xml文件,导入doubbo-starter,启动类上加@ImportResource(locations = "classpath:doubbo.xml")

3.基于注解API的方式配置,将每一个组件手动放到容器中

### 负载均衡算法

1.基于权重的随机算法(Random LoadBalance)

2.基于权重的轮询算法(RoundRobin LoadBalance)

3.最少活跃数算法(LeastActive LoadBalance)

4.一致性hash算法(ConsistentHash LoadBalance)																																		相同参数的请求总是发到同一提供者。 当某一台提供者挂时，原本发往该提供者的请求，基于虚拟节点，平摊到其它提供者，不会引起 剧烈变动。

![image-20210405210848926](C:\Users\shadows\AppData\Roaming\Typora\typora-user-images\image-20210405210848926.png)