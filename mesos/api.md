# Mesos 接口

测试 docker <http://www.jianshu.com/p/eef9803d3b19>


## Mesos REST 接口

可以通过 浏览器或者命令工具如 curl 来访问 REST API.  
以下为 REST 服务接入点:  

| 命令                | 用途                 |
| ------------------ | -------------------- |
| /__processes__     | 列出集群中的进程        |
| /files/[browse debug read download] | MESOS 文件REST |
| /logging/toggle      | 在一段时间开启日志模式 |
| /master/health       | 以http 码返回 master 健康状态, 200 表示正常 |
| /master/redirect     | 利用 HTTP 307 返回码跳转到当前 master |
| /master/observer     | ? 主机健康信息  |
| /master/roles  | 列出当前所有已复制的角色 |
| /master/state  | 当前集群使用情况总结报告 |
| /master/tasks  | 当前所有活跃框架下的所有任务 |
