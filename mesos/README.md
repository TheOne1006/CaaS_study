# Apache Mesos

Mesos是Apache下的开源 __分布式资源管理框架__ ，它被称为是分布式系统的内核.  


内部机制:

- Mesos 架构
- 资源分配
- 资源隔离
- 容错


### mesos 概述与使用

1. [x] 开源集群管理工具
2. [x] 分布式系统内核  
    - 使用了与 Linux 内核相同的原则构建
    - 与 Linux 内核相比，提供了不同层面的抽象
    - mesos 内核运行在 __每一台机器__ 上,为应用提供api
3. [x] 有效的资源隔离和共享
    - 隔离
    - 共享
4. [x] 可扩展到 10,000 个节点
5. [x] 基于 zookeeper 的容错机制
    - 使得 mesos 是一个高可用高容错的机制
6. [ ] 支持 Docker 容器
7. [ ] 任务隔离
    - mesos 本身基于 linux container
8. [ ] 多种资源调度（CPU,内存,磁盘以及端口)


```
MESOS:  

+--------------+ +------------+ +--------+ +--------+ +------------+
| Hadoop       | | Hadoop     | | MPI    | | Spark  | | other      |
| (production) | | (ad-hoc)   | |        | |        | | aplication |
+--------------+ +------------+ +--------+ +--------+ +------------+

+------------------------------------------------------------------+
|                              MESOS                               |
+------------------------------------------------------------------+

+--------------+ +--------------+ +--------------+ +---------------+
| NODE         | | NODE         | | NODE         | | NODE          |
+--------------+ +--------------+ +--------------+ +---------------+
```



## Mesos 调度架构

![](images/mesos_framework.jpg)

mesos 实现了两级调度架构

- Master 守护进程: 管理集群中所有节点上运行的Slave守护进程(蓝色)
  - 集群由物理(虚拟)服务组成,用于运行应用程序的任务.如 hadoop
  - Master 任务1: 协调 __全部__ 的 Slave,
  - Master 任务2: 确定每个节点的可用资源,聚合计算跨节点所有可用资源报告,
  - Master 任务3: 并且向注册到 Master 的 Framework (这里应该是指 scheduler??) 发出资源邀约. framework 可以根据应用的需求 选择/拒绝 master 的资源邀约.
  - Master 任务4: 一旦(framewoke scheduler) 接收邀约, Master 即协调 Slave 和 Framework 调度参与节点上任务,并在容器中执行, 以使多中类型的任务,可以在同一节点运行
- Framework 包括调度和执行器（红色虚线) 第二级
  - 每个节点上都会运行执行器
  - mesos 能和不同类型的 framework 通信(调度 schedule /执行 execute).
  - 每种 Framework 有响应的应用集群管理


## Mesos 工作流程

![mesos_work_flow](/images/mesos_work_flow.jpg)

- 第一步,Slave1 向 Master 汇报可用资源(如4cpu, 4gb 内存).
- 然后, Master 触发分配策略模块
- 第二步, Master 向Framework1 发送 __资源邀约__,描述可用资源(如:s1 上的可用资源信息)
- 第三步, Framework1 调度器 响应了 Master,需要在Slave上运行两个任务,(task1 2cpu 1gb , task2 1cpu 2gb)
- 第四部, Master 向 Slave1 发布任务,分配资源给 __任务执行器__
- 接下来,由这两个执行器执行这些任务.
- 剩余的资源分配给其他的 framework 需要的时候使用。

## Mesos 数据持久化

当任务被调度的时候，分配的节点可以访问到所需要的数据  
mesos提供了一下几种方式实现数据持久化:  

- 分布式文件系统
    - 如: Hadoop
    - 保证文件可以被 mesos 的每个节点所访问
    - 缺点: 网络延迟
- 使用数据存储复制的本地文件系统
    - 利用了应用程序级别的复制,来确保被多个节点所访问的
    - 优点: 不必考虑网络延迟
    - 缺点: 必须要配置 mesos 使特定的任务在特定的节点执行
- 不使用复制的本地文件系统
    - 将数据存储在特定节点
    - 缺点: 只能预留单个节点
- 动态预留
    - 为默认任务提前预留某个资源,
- 持久化卷
    - mesos 向 framework 发送两种资源邀约，1计算邀约, 2持久化邀约

## MeSOS 容错

MeSOS结构的容错方案:  

- Master
    - 是整个 Mesos 集群的大脑,
    - 采用热备份
    - 一般为1个master节点 和 多个备用节点,由 zookeeper监控
    - 当一个 master 出现故障时, zookeeper 会根据领导者选举算法,选择一个新的 master 节点,
    - 当一个新的 master 当选后, zookeeper 会通知 Framework 和 slave 节点到新的 master 上注册
- Framework
    - 调度器的容错: framework 调度器 通过 framework 将调度器注册多份 Master 上
    - 当一个调度器故障的使用, master 会通知另一个调度器来接管。
    - 而 Framework 自身需要负责实现，调取器共享状态的机制
- Slave
    - 定期将自己的资源和健康状况上报给 Master
    - 当 Master 在一定时间内,没有收到一个 Slave 的资源信息 或者 健康报告时,会认为这个 Slave 节点是不可用的
    - 然后会通知 Framework 将运行在故障 Slave 节点的任务，重新调度高可用的节点上
- 执行器/任务( Executor/Job )
    - 与 Slave 机制相似
    - Master 会向分配任务的 framework 调度器汇报 __执行器__ 任务的失败,  
    - 并允许调度器根据 __其配置策略__, 在任务执行失败后做出相应的处理,  
    - 通常情况下，framework 在接受 master 相应的资源邀约后, 会在新的 Slave 节点上重新执行任务


## 使用 - 案例

mesos 提供了一个 UI 的界面,   
可以非常方便的通过这个UI界面管理 mesos 集群,
并且 __监控和管理__ slave 的状况  
