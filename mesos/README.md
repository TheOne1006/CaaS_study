## MeSOS

- 安装
- 启动
    - 启动 master
    - 启动 slave
- docker 启动


## 参考
<http://www.baidu.com/p/%E6%95%B0%E4%BA%BA%E7%A7%91%E6%8A%80?from=wenku>
<http://mesos.apache.org/documentation>
<http://blog.csdn.net/maitianshouwei/article/details/47392833>
<http://www.jianshu.com/p/b35b54aa364f>

## Error

`ulimit: open files: cannot modify limit:Operation not permitted`:
<http://blog.csdn.net/maitianshouwei/article/details/47392833>

### 构建

> 构建 安装

1. 按照 <http://mesos.apache.org/gettingstarted/> 安装依赖
2. 编译安装
3. ../configure --prefix=/home/userName/mesos_install // 默认 /user/local 权限警报
4. 或者 设置相关文件权限,或者使用 sudo
5. make && make install
6. 报错 `libiconv.so.2: cannot open shared object file`
  - 在 `/etc/ld.so.conf` 中加一行 `/usr/local/lib`
  - 执行 `$ sudo /sbin/ldconfig`
7. 为Mesos replicated log 创建目录,并赋予读写权限
  - `sudo mkdir -p /var/lib/mesos`
  - `sudo chown` &#96;whoami&#96; `/var/lib/mesos`

### 相关命令

| 命令       | 用法            |
| --------- | --------------- |
| mesos-local.sh | 在单进程里启动驻内存的集群 |
| mesos-tests.sh | 运行 Mesos 测试用例套件 |
| mesos.sh       | 启动 Mesos 命令的封装脚本, 不带任何参数运行时会显示所有可用参数 |

### 开启服务

方法1 单机启动:
`$ mesos-local` : 启动本地 Mesos 集群, 会在单进程里同时启动 master 和 slave 程序,并且可以快速检查 Mesos 是否安装正确

方法2:
```bash
# 启动一个 master 进程
mesos-master --work_dir=/var/lib/mesos

# 指定地址, 根据机器情况?? 什么情况??
# 启动一个 slave 进程
mesos-slave --master=[主机名|ip地址]:5000

## 执行测试
## C++ 测试
~/mesos/build/src $ ./test-framework --master=[主机名|ip地址]:5050
```

方法3:
```bash
# 配置文件,可能是0.28
#
#进入配置目录
$ cd /usr/local/etc/mesos

#添加masters和slaves文件，文件中每行是master和slave的节点主机名或IP地址
$ sudo vi /usr/local/etc/mesos/masters
# 输入 ubuntu

$ sudo vi /usr/local/etc/mesos/slaves
# 输入 ubuntu

#修改master配置
$ sudo cp mesos-master-env.sh.template mesos-master-env.sh
$ sudo vi mesos-master-env.sh

#添加下面几行
export MESOS_log_dir=/home/king/mesos_log/log
export MESOS_work_dir=/var/lib/mesos




# 启动 master 命令
$ mesos-start-masters.sh



```



- - -
