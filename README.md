# ansible
> 这是ansible 部署openstack，实现了部署controller单节点、computer单节点功能。目前仅适用于OpenEuler-sp3操作系统


## 功能介绍

### pre_work
    主要完成ansible部署节点前的一些准备工作，主要包括待部署节点的防火墙检查和关闭，selinux的检查和临时关闭，ansible控制节点和待部署节点之间的免密登录，以及离线源上传、解压、在线源备份、离线源配置等基础功能。



### controller
    完成对单节点controller节点部署

### computer
    完成对单节点computer节点部署

## 使用

### clone代码仓库
> git clone {{ repo_url }}

### 运行pre_work

将离线压缩包（命名为myyum.gz) 上传至pre_work/resources/目录下

修改hosts.ini文件中主机组下面的主机信息，不要修改主机组名称

~~检查pre_work/tasks/variables.yml文件中信息是否符合预期~~
    
进入pre_work目录

执行命令：ansible_playbook -i hosts.ini ./tasks/main.yml

### 运行controller
修改controller/hosts/hosts.ini中主机组controller_hosts下面的各个主机信息。

检查controller/variables.yml中变量值是否符合预期

进入controller目录

执行命令:

```
ansible-playbook -i ./hosts/hosts.ini install_openstack --extra-args "enable_mariadb= True enable_rabbitmq=True enable_keystone=True enable_glance=True enable_placement=True enable_neutron=True enable_cinder= True enable_nova=True"
```


### 运行computer
在computer/hosts/hosts.ini文件中computer_hosts下面填写各个主机信息。

检查computer/variables.yml中变量值是否符合预期

进入computer目录

执行命令：

```
ansible-playbook -i ./hosts/hosts.ini install_openstack_computer --extra-args "enable_neutron=True enable_nova=True"
```