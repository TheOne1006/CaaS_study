# ZooKeeper

分布式协同服务是构建分布式系统是必不可少的一环,
[Apache ZooKeeper](http://zookeeper.apache.org/) 正是分布式协同服务的一种实现.  
ZooKeeper 包含了一个名为 **ZooKeeper 原子性广播(Zookeepr Atomic Broadcast, ZAB) 的一致性算法.**  

ZAB 是一个高性能的广播算法,用来在主备系统中提供 _强一致性保证<http://dl.acm.org/citation.cfm?id=2056409>_. ZooKeeper 经过生茶环境的考验,许多项目都依赖 ZooKeeper 来保证高可用.  

## 安装

安装并运行一个 ZooKeeper 集群, 一个 N 个节点的 ZooKeeper 集群可以在 ceil(N/2) 个节点失效的情况下仍然保持正常工作. 
