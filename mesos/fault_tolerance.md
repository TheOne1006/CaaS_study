容错机制
----------------

Mesos
================

> 定义: 
    容错是数据中心操作系统十分重要的需求. 在发生故障的情况下仍然能保值功能的正常运行时大规模系统必不可少的能力。

Mesos 有三种容错情况需要考虑: 机器宕机、Mesos进程bug 和 升级。


### 当Mesos Slave 主机宕机

当 mesos slave 宕机时，mesos master 会发现宕机的情况，并给框架(framework) 发送一个 该slave 宕机事件。
框架可以将当前运行在该 slave 上的任务重新调度到其他健康的slave 上。  

当机器被修复，并且 slave 进程重新启动恢复到健康时，slave 会向master 注册并在此成为 mesos 集群中的一员。  
 
？？ 在发生 slave 进程错误或者 slave 升级时， slave进程可能在一段时间内不可用，但是在 slave 上的 执行器不会受到影响，
slave 进程在重启后会 __恢复__ 这些任务, 这一步也被称为 slave 恢复。  

### 当框架调度器 (framework Scheduler) 主机宕机

当框架调度器主机宕机时、程序错误或者升级导致错误时，框架下的已有任务将会继续执行。
如果框架实现了故障恢复，它将会向master注册并获取所有任务的状态信息


### Mesos master 宕机
Mesos 利用选主来保证 master 的高可用。这意味着当master 宕机、进程故障或者升级，导致master 不可用是，另一个守护的 master 会被选举为主， 所有的slave 和框架，包括执行器以及任务都会继续执行。  

slave 和框架都会像 新master 注册，因此master发生错误不会影响到整个集群的正常运行。  

Mesos 目前只支持 ZooKeeper 一种选举服务




ZooKeeper 故障检测及处理
========================

在zookeeper 的选举模型中有 _竞争者_ 和 _观察者_ 两种抽象模型，
竞争者试图成为master, 观察者向知道当前的master 是谁。  
每个Mesos master __即是竞争者也是观察者__(一方面想要知道当前master, 另一方面想要选举自己为新的 master ).  

__Mesos Slave和框架都是观察者__ ,寻找当前的master 并与之进行通信。  

ZooKeeper 群组领导候选者通过追踪下列ZooKeeper 会话时间来注册和注销进行处理，同时也利用这些事件来对 Mesos 组件进行监控。  

- 连接
- 重连
- 会话失效
- ZNode 创建、删除和更新

> Mesos 利用超时来检测各个组件是否可用。

Mesos 组件一旦和 __ZooKeeper__ 断开事件超过了配置的事件，Mesos 组件就会超时。
竞争者和观察者的会话分别会在 `MASTER_CONTENDER_ZK_SESSION_TIMEOUT` 和 `MASTER_DETECTOR_ZK_SESSION_TIMEOUT`指定的时间超时，默认(10s), 不同的 Mesos 和 ZooKeeper 断开后会采取不同的措施。


行为:  

- slave 和 zooKeeper 断开
    - 前提, mesos-slave 的master 地址由 zookeeper 管理
    - slave将不再响应接受的消息, 以确保不会执行从旧的 master 发来的信息,
    - 一旦slave 和 zooKeeper 连接, slave 会继续执行来自 mater 的消息, 此时 master 可能已经不是之前的 master
- 调度器驱动和ZooKeeper 断开连接
    - 它会通知调度器，并有调度器决定下一步如何处理。 ？？
- mesos 和 ZooKeeper 断开连接
    - 进入无主模式
    - 如果当前 master 是主,则它自己终止。需要管理人员手动启动并将其以守护master形式运行
    - 如果当前 master 是守护模式, 则它只需要等待和 ZooKeeper 重新连接
- 由于网络隔离导致 slave 无法和当前 master 通信, 那么 slave 的健康检查会失败,master 会将 slave 设置为为激活状态，未激活的slave 将不会像master 重新注册，并会被之后的消息要求主动关闭，该slave 上的任务都会被标记为 LOST , 框架将会对这些任务进行后续处理。






