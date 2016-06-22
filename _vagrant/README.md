# vgrant 

## 安装

1. 需要安装 virtualbox
2. <http://www.vagrantup.com/downloads.html> 下载相关版本的的软件
3. 检测是否安装成功 尝试命令`$ vagrant`


## 安装 <https://github.com/everpeace/vagrant-mesos>

1. Chef <https://downloads.chef.io/chef-dk>
2. 安装依赖插件 <https://github.com/everpeace/vagrant-mesos#prerequisites>
    - 需翻墙
3. 下载镜像
    - `aria2c "https://atlas.hashicorp.com/everpeace/boxes/mesos/versions/0.22.1.0/providers/virtualbox.box"`
    - aria2c 支持断点续传

## 参考资料
<https://github.com/astaxie/Go-in-Action/blob/master/ebook/zh/01.2.md>