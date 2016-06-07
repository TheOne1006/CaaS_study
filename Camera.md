## 属于解释

1. Mesos-master: Mesos master，主要负责管理各个framework和slave，并将slave上的资源分配给各个framework
2. Mesos-slave: Mesos slave，负责管理本节点上的各个mesos-task，比如: 为各个executor分配资源
3. Framework: 计算框架，如: Hadoop，Spark等，通过MesosSchedulerDiver接入Mesos
4. Executor: 执行器，安装到mesos-slave上，用于启动计算框架中的task。
5. Framework schedule : framework 调度器
6. clusters : 集群(计算机集群),是一种计算机系统， 它通过一组松散集成的计算机软件或硬件连接起来高度紧密地协作完成计算工作。在某种意义上，他们可以被看作是一台计算机。
7. node : 节点, 集群系统中的单个计算机通常称为节点
8. 热备份: 热备份是系统处于正常运转状态下的备份
