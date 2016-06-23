# standalone 使用

Mesos web UI on: http://ip:5050  
Marathon web UI on: http://ip:8080  
Chronos web UI on: http://ip:8081  

## 登录
```bash
vagrant ssh
```


### test

nginx.json
```json
{
 "id":"nginx",
 "cpus":0.2,
 "mem":20.0,
 "instances": 1,
 "constraints": [["hostname", "UNIQUE",""]],
 "container": {
   "type":"DOCKER",
   "docker": {
     "image": "nginx",
     "network": "BRIDGE",
     "portMappings": [
        {"containerPort": 80, "hostPort": 0,"servicePort": 0, "protocol": "tcp" }
      ]
    }
  }
}
```

nginx-start.sh
```bash
curl -X POST http://127.0.0.1:8080/v2/apps -d @nginx.json -H "Content-type: application/json"
```

