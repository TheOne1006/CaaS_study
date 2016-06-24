## 分布式实践

#### 专属物理机

![](/images/datacenter.png)
![](/images/image8.png)

给特定任务分配专属物理机，保证任务做到完全隔离,

缺点:  
1. 低利用率, 使用率不均匀
2. 较长的服务器启动时间

#### 虚拟机


![](/images/image9.png)

比物理机粒度更加精细, 增加了服务器利用率。

缺点:  
1. 管理更多的机器
2. 虚拟机带来的性能损耗

#### Mesos

![](/images/image11.png)
![](/images/image12.png)

1. 整合所有资源
2. 动态分配
