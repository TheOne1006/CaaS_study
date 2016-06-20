## zooKeeper安装

> 1. 在Mesos 的 3rdparty 文件夹中包含了 ZooKeeper 的发行版.也可以从 <http://zookeeper.apache.org> 获取上游的发行版本

```bash
$ cd ~/mesos/3rdparty
```

> 2. 解压并进入目录

```bash
~/mesos/3rdparty$ tar -zxf zookeeper-*.tar.gz
~/mesos/3rdparty$ cd zookeeper-*
~/mesos/3rdparty$ mv zookeeper-* zookeeper
```

> 3. 在 king@ubuntu:~/mesos/3rdparty/zookeeper/conf 目录下创建配置文件 zoo.cfg . 可以利用发行版的默认配置阳历配置, 其中设置了

```bash
king@ubuntu:~/mesos/3rdparty/zookeeper/conf$ cp zoo_sample.cfg zoo.cfg
```

> 4. 配合 config 中的集群节点

```bash
king@ubuntu:~/mesos/3rdparty/zookeeper/conf$ vi zoo.cfg
```

加入一行
```
server.1=zoo1:2888:3888
```

在这里,我们使用 __伪集群模式__,   

所谓伪集群, 是指在单台机器中启动多个zookeeper进程, 并组成一个集群. 以启动3个zookeeper进程为例.


参考:  
<http://coolxing.iteye.com/blog/1871009>
<http://blog.csdn.net/huwei2003/article/details/49101269>
