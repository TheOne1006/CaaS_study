## Mesos slave

Mesos 管理的slave上的资源可以通过 _slave资源和slave属性_ 进行描述.  

> 负责:  

1. 利用已有的资源执行 框架(framework)下发的任务
2. slave 需要对运行中的任务进行合适的隔离, 隔离机制还需要保证每个任务都能公平享受承诺资源


- slave 资源
    1. 区别: 资源是slave任务运行所消耗的元素(如: CPU, 硬盘空间, 内存 等), 而 slave 的其他相关信息则通过属性描述
    2. 管理: slave 资源统一由Mesos master 统一管理分配给不同的框架
- slave 属性
    1. 作用: 标识每个slave的特殊信息(如: 特殊的操作系统, 特殊的软件版本, 特殊的网络环境, 或者当前 slave 的特殊硬件)
    2. 标识: 这些属性都是简单的 _键值对_
    3. 传递: 随着分给框架的资源 offer 进行传递。
    4. 非消耗: 由于这些属性并不会被任务消耗, 因此每次向框架提交 offer 都会传递这些属性.

每个Mesos 资源或者属性都可以归类为以下几种:  

- 标量(Scalar): 浮点数
- 范围值(Range): 一组标量只,通过 [min, max] 表示
- 集合(Set): 任意的字符串
- 文本值(Text): 属性中的任意字符串

资源命名与表示:  

1. 命名: 资源名称由 英文字母、数字、`-`,`/`,`.`,`_` 组成，
2. 特殊命名: Mesos master 会将特殊处理名称为 `cpus`,`mem`,`disk`,`ports` 的资源
    - 没有cpus 和  mem 资源的slave 节点 将不会为框架提供服务。
    - mem 和 disk 通过以 MB 为单位的标量值表示
    - ports 以范围值来表示
3. `--resources`: slave 给不同的框架 __资源清单__ 用 `--resource` 表示，每个资源和属性之间以分号分割

资源清单案例:  

```
--resources='cpus:30;mem:122880;disk:921600;ports:[21000-29000];bugs:{a,b,c}'
--attributes='rack:rack-2;datacenter:europe;os:ubuntuv14.4'
# 说明
1. 30个cpu， 120GB mem, 900 GB disk, 21000 - 29000 的ports
2. 以及 a、b、c 三个bugs 资源。
3. rack 值为 rack-2 的属性  //
4. datacenter 值为 europe // 数据中心为欧洲
5. os 值为 ubuntu14.4
```

> GPU 资源

目前不支持 GPU 资源的方法，但是支持用户自定义资源类型。  
如果我们制定 `gpu(*):8` 作为 `--resources` 的一部分，
那么它就会成为提供给框架的资源 offer 中的一部分,
然后框架就可以像其他资源一样使用GPU资源,

>> 给拥有 GPU 资源的 slave 节点设置一个属性如 `--attributes="hasGpu:true"`, Mesos 目前不支持 GPU 隔离,可以通过编写相关扩展来实现
