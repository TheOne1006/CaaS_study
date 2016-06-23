# 多个集群 使用

> 本机

mesos2 UI:<http://172.31.1.12:5050/#/>
zookeeper1 port: <http://172.31.0.11:2181>
slave1 ip: 172.31.2.11
marathon ip: http://172.31.3.11:8080

### 启动

启动marathon
```
nohup /opt/marathon/bin/start --master zk://172.31.0.11:2181/mesos --zk_hosts 172.31.0.11:2181 --event_subscriber http_callback > /var/log/marathon/nohup.log 2> /var/log/marathon/nohup.log < /dev/null &
```

启动master
```
nohup /usr/sbin/mesos-master --zk=zk://172.31.0.11:2181/mesos --port=5050 --log_dir=/var/log/mesos --cluster=MyCluster --ip=172.31.1.11 --quorum=1 --work_dir=/var/run/mesos
```


启动slave
```
/usr/sbin/mesos-slave --master=zk://172.31.0.11:2181/mesos --log_dir=/var/log/mesos --isolation=cgroups/cpu,cgroups/mem --containerizers=docker,mesos --executor_registration_timeout=5mins --ip=172.31.2.11 --work_dir=/tmp/mesos
```

启动zookeeper

```
/usr/bin/java -cp /etc/zookeeper/conf:/usr/share/java/jline.jar:/usr/share/java/log4j-1.2.jar:/usr/share/java/xercesImpl.jar:/usr/share/java/xmlParserAPIs.jar:/usr/share/java/netty.jar:/usr/share/java/slf4j-api.jar:/usr/share/java/slf4j-log4j12.jar:/usr/share/java/zookeeper.jar -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.local.only=false -Dzookeeper.log.dir=/var/log/zookeeper -Dzookeeper.root.logger=INFO,ROLLINGFILE org.apache.zookeeper.server.quorum.QuorumPeerMain /etc/zookeeper/conf/zoo.cfg
```
