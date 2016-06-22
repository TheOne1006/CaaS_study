## 通信

Mesos 目前利用一个和 HTTP 类似的协议在各个组件之间进行通信.  
Mesos 利用 libprocess 实现通信, 相关代码在 3rdparty/libprocess 中.  
libprocess 实现了进程之间的异步通信, libprocess 消息是不可变的,也更易于内部消息的并发. (??)  

下列 API 都使用了 Mesos 的通信机制

- 调度器 API: 框架调度器和 Mesos master 进行通信. 该组内部通信目前仅由SchedulerDriver APi 使用
- 执行器 API: 执行器和 Mesos slave 之间进行通信.
- 内部 API: Mesos master 和 slave 之间通信
- 运维 API: 这组 API 主要是为运维人员准备,并被 WEB UI 等类似应用中使用. 和其他大多数 Mesos API 不同,这是一组 同步 API

角色消息:
1. 路径: 每个角色都需要通过 HTTP POST 请求来发送消息, 消息的路径由 __角色名__ 和 __消息名__ 组成.  
2. 区别: 为了和普通 HTTP 区分, 请求头部的 User-Agent 域被设为 "libprocess/..."
3. 数据: 消息的数据存储在 HTTP 请求的 body 中
3. Mesos 使用 protocol buffers (src/messages/message.proto) 对消息进行序列化，消息的解析由消息的接受方式实现
