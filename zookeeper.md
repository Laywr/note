# zookeeper

zookeeper-----分布式协调系统

**zab协议**---为分布式协调服务Zookeeper专门设计的一种支持崩溃恢复的原子广播协议，是Zookeeper保证数据一致性的核心算法。Zab借鉴了Paxos算法，但又不像Paxos那样，是一种通用的分布式一致性算法

**单点问题**---又称单点故障（Single Point Of Failure，SPOF）是指在系统中一旦失效就会让整个系统无法运作的部件。

### CAP理论

- 一致性：用户在任何时刻访问获取到的数据都是最新的数据
- 可用性：每次请求都能得到正确的响应
- 分区容错性：分布式系统遇到任何网络分区故障的时候，都能保证对外提供服务

### BASE 理论

BASE 是 Basically Available(基本可用)、Soft-state(软状态) 和 Eventually Consistent(最终一致性) 三个短语的缩写。

- **基本可用：**在分布式系统出现故障，允许损失部分可用性（服务降级、页面降级）。
- **软状态：**允许分布式系统出现中间状态。而且中间状态不影响系统的可用性。这里的中间状态是指不同的 data replication（数据备份节点）之间的数据更新可以出现延时的最终一致性。
- **最终一致性：**data replications 经过一段时间达到一致性。

BASE 理论是对 CAP 中的一致性和可用性进行一个权衡的结果，理论的核心思想就是：我们无法做到强一致，但每个应用都可以根据自身的业务特点，采用适当的方式来使系统达到最终一致性。