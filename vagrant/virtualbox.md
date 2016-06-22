## 通过virtualbox 安装 Ubuntu 14.04 server 版本的虚拟机

1. virtualbox 无脑安装 ubuntu 14.04 后
2. 安装 ssh `sudo apt-get install openssh-server`
3. 测试是否启动 `ps -e |grep ssh`
4. ssh 启动命令 `sudo /etc/init.d/ssh start`
5. 设置端口转发 名称:ssh 协议TCP  主机:127.0.0.1  主机端口:54876 (自定义 > 2000) 子系统端口 22
6. ssh 登陆 `ssh -p 54876 userName@127.0.0.1`

### 设置网络

1. host-only
2. vboxnetN
3. PCnet-FAST III
4. 拒绝


### 设置免密登录

1. `[user@A ~]ssh-keygen -t rsa -P ''`, 在A机下生成公钥/私钥对
2. 用scp复制 把A机下的id_rsa.pub复制到B机下，在B机的.ssh/authorized_keys文件里
  - `[user@A ~]scp ~/.ssh/id_rsa.pub userName@target:/home/userName/.ssh/authorized_keys`
3. 权限设置,在 B 机器上
  - `[user@B ~]$ chmod 600 ~/.ssh/authorized_keys`
