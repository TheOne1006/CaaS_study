# standalone 使用

Mesos web UI on: http://ip:5050  
Marathon web UI on: http://ip:8080  
Chronos web UI on: http://ip:8081  

## 登录
```bash
vagrant ssh
```

### Question

1. 设置 slave 的属性
2. 设置 指定端口
3. 选择指定机器


### test1 docker start

nginx-start.sh
```bash
#!/bin/bash
curl -X POST http://127.0.0.1:8080/v2/apps -H Content-Type: application/json -d '{
 "id":"nginx",
 "cpus":0.2,
 "mem":20.0,
 "instances": 1,
 "constraints": [["hostname", "UNIQUE",""]],
 "container": {
   "type":"DOCKER",
   "docker": {
     "image": "index.docker.io/library/nginx",
     "network": "BRIDGE",
     "portMappings": [
        {"containerPort": 80, "hostPort": 0,"servicePort": 0, "protocol": "tcp" }
      ]
    }
  }
}'
```


#### test2 new Framework

1. 各节点上安装netcat
2. 在Marathon页面，点击“Create Application”创建任务
    - command： while true; do ( echo "HTTP/1.0 200 Ok"; echo; echo "Hello World" ) | nc -l $PORT; done



#### 连接外部slave

test in slave1
```
sudo /usr/sbin/mesos-slave --master=zk://zoo1:2181/mesos --log_dir=/var/log/mesos --isolation=cgroups/cpu,cgroups/mem --containerizers=mesos --executor_registration_timeout=5mins --work_dir=/tmp/mesos
```