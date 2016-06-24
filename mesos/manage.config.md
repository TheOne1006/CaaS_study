## 配置

所有的Mesos 配置参数，描述，默认值。最新列表维护在<http://mesos.apache.org/documentation/latest/configuration/>

### Mesos slave 和 Mesos master 共有配置项

| Flag  | Explanation   | default  |
| ----- | ------------- | -------- |
| `--ip`  | master/slave 所绑定的ip 参数, 当机器上多网卡时，推荐使用  | 默认接口ip |
| `--port` | 监听端口 | master:5050 slave:5051 |
| `--log_dir` | 日志文件路径 | 没有默认，不指定log_dir 不会打印日志 |
| `--logbufsecs` | log 刷新间隔 | 0 |
| `--loging_level` | 等于或者高于这个级别才会被打印, INFO, WARNING 和 ERROR | INFO |
| `--external_log_file` | 指定外部管理日志文件，能够被网页界面和 HTTP API 访问 | |
| `--hooks` | 由逗号分隔 hook 模块列表，这些模块将会被安装在 master 上 |
| `--modules` | ?? | |
| `--hostname` | master/slave 对外主机名 | 如果为设置则使用系统主机名 |
| `--quiet , --no-quiet` | 控制日志行为 --quiet 关闭向标准错误输出日志 | false
| `--version` | 显示 Mesos 版本 |  |


### Mesos master

可以通过 `mesos-master --hlep `查看详情

| Flag    | description            | default  |
| ------- | ---------------------- | ---------|
| `--quorum` | 使用基于 replicated log 的 registry时, 所需的 quorum 个数,需要 quorum 个 master 同意才能写入registry .这个数字应该大于 master/2 | |
| `--work_dir` | 所有框架的工作路径，以及 replication log 的存放路径 。必填配置参数 | |
| `--zk`  | 为集群提供选主服务的 zooKeeper url, 高可用必填 | |
| `--zk_session_timeout` | zookeeper 会话超时时间 | 10s |
| `--log_auto_initialize` | 检查是否自动化 replicated log .？？ | true |
| `--acl` | acl json 配置文件，后阶段值可以是json 字符串，也可以是含有json 的文件路径 | |
| `--allocation_interval` | 两次批量分配的间隔时间?? | 1s |
| `--authenticate*` | 鉴权相关 | |
| `--cluster` | Mesos 展示在网页接口的名字 | |
| `--framework_sorter` | 为每个框架分配资源的策略 | drf |
| `--user_sorter` | 为每个用户分配资源的策略 | drf |
| `--offer_timeout` | 在这个时间后，资源将从框架被回收， 这可以处理一些框架接受资源后一直占有不释放的问题 | |
| `--rate_limits` | 速率限制 | |
| `--slave_removal_rate` | master 移除slave 的最大速率？？| |
| `--registry` | registry 所使用的持久化策略, 可选值 replicated_log / in_memory | replicated_log |
| `--registry_fetch_timeout` 和 `--registry_store_timeout ` | ???? | |
| 等等 |  | |
| `--slave_reregister_timeout` | master 重新选举后 slave 重新注册的超时事件。如果slave 没有重新注册，则它将会被 registry 中删除 和 master 之间的通信业会被关闭 | 10min |
| `--webui_dir` | webUI 文件路径 | |
| `--whitelist` | 白名单配置文件，包含科技手资源 offer 的slave | 默认没有白名单，接受所有 slave 的资源 |
| `--resource_monitoring_interval` | 执行器资源使用监控间隔 | 1s |
| `--max_executors_per_slave` | 每个 slave 上所能运行的执行器最大个数，用于网络隔离器 | |



### Mesos slave

可以通过 `mesos-slave --hlep `查看详情

| Flag    | description            | default  |
| ------- | ---------------------- | ---------|
| `--attributes` | 节点所包含的属性列表，属性之间以逗号分隔，调度器可以利用属性作为调度器的限制条件，属性可以被当做格式为 "name:value"的标签如： `--attributes="rack:r1,attr1:val1"` | |
| `--master` | 指定要连接的 master 路径. 这个值可以是主机名、ip 地址、一个或者多个 zookeeper master uri。 也可以是两者之一的文件路径 必填项 | |
| `--checkpoint 或者 --no-checkpoint` | 检查是否将 slave 和框架的信息以检查点的形式保存到磁盘。检查点允许 slave 在重启之后恢复 | true |
| `--work_dir` | 框架工作路径 | /tmp/mesos |
| `--recover`  | 指明恢复策略，可以重连接以为 slave 会和之前仍在运行的执行器重新连接。 这只有在 `--checkpoint` 情况下发生。否则 slave 会以一个新 slave 身份向 master 注册，清理策略意味着关闭在slave 关闭前运行的执行器都将被杀死 | reconnect |
| `--recovery_timeout` | 等待 slave 恢复的超时时间，超过超时时间后，所有 失去的 slave连接器将会自杀 | 15min |
| `--strict` | ...?? | |
| `--isolation` | 使用隔离机制 ,可选值有 `posix/cpu,posix/mem`, 如果需要使用 `cgroups`隔离器,这里需要设置 `cgroups/cpu,cgroups/mem` | posix/cpus, posix/mem |
| `--[no-]cgroups_enable_cfs` | 开启 cfs 贷款限制的特定对cpu 进行硬限制 ? | false |
| `--cgroups_hierarchy` | 开启内存和交换区的限制 | false |
| `--cgroups_root` | 根 cgroups 的名字 | mesos |
| `--containerzers` | 逗号分隔的列表指明 Mesos 所使用的容器机的实现。目前支持的容器机有 Mesos, External, Docker . Mesos 会按照列表的顺序进行尝试 例如 `containerizers="mesos,docker"` | mesos |
| `--containerizer_path` | 指定外部容器机也可以执行的文件，该选项需要和 `--isolation=external` 一起发挥作用 | 
