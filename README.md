# ansible
> 这是ansible 部署openstack，实现了部署controller单节点、computer单节点功能。目前仅适用于OpenEuler-sp3操作系统


## 功能介绍

### pre_work
    主要完成ansible部署节点前的一些准备工作，主要包括待部署节点的防火墙检查和关闭，selinux的检查和临时关闭，ansible控制节点和待部署节点之间的免密登录，以及离线源上传、解压、在线源备份、离线源配置等基础功能。


### controller
    完成对单节点controller节点部署。
    目前包含mariadb,rabbitmq,keystone,glance,placement,neutron,cinder,nova

### computer
    完成对单节点computer节点部署。
    包含nova-compute,neutron 相关包安装

## 使用

### clone代码仓库
> git clone {{ repo_url }}

### 安装ansible
#### 方式1：直接安装ansible
    yum install ansible
#### 方式2：虚拟环境安装ansible
    # 安装python3-venv包
    sudo apt install python3-venv
    # 创建一个虚拟环境并激活
    python3 -m venv /path/to/venv
    # 更新pip
    pip install -U pip
    # 安装ansible
    pip install 'ansible-core>=2.14,<2.16'

    # 等待安装完成

    # 运行命令查看ansible 详细信息
    ansible --version

### 运行pre_work

将离线压缩包（命名为myyum.gz) 上传至pre_work/resources/目录下

修改hosts.ini文件中主机组下面的主机信息，不要修改主机组名称

~~修改pre_work/tasks/variables.yml文件中ansible_host为待部署节点ip~~
    
进入pre_work目录

执行命令：ansible_playbook -i hosts.ini ./tasks/main.yml

### 运行controller
修改controller/hosts/hosts.ini中主机组controller_hosts下面的各个主机信息。

检查controller/variables.yml中变量值是否符合预期

进入controller目录

执行命令:

```
ansible-playbook -i ./hosts/hosts.ini install_openstack.yml --extra-vars "enable_mariadb=True enable_rabbitmq=True enable_keystone=True enable_glance=True enable_placement=True enable_neutron=True enable_cinder=True enable_nova=True"
```


### 运行computer
在computer/hosts/hosts.ini文件中computer_hosts下面填写各个主机信息。

检查computer/variables.yml中变量值是否符合预期

进入computer目录

执行命令：

```
ansible-playbook -i ./hosts/hosts.ini install_openstack_computer.yml --extra-vars "enable_neutron=True enable_nova=True"
```

### 一些
1. 部署节点hostname不要有特殊字符如下划线_, @等符号
2. glance validate中会在线下载cirros镜像去验证，大多数生产环境是断网的，是否采用其他验证方式替代？
3. 部署节点/etc/hosts中nameserver 配置域名有问题，需要检查和配置

### 需要解决的问题
2. 现网环境基本不允许关闭防火墙，目前安装前都需要关闭防火墙，如何解决？