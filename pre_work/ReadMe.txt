第一次执行pre_work之前，先手动ssh连接登陆每个节点一次，将fingerprint信息添加到ansible控制节点的~/.ssh/konw_hosts文件中。仅限第一次执行！！！
pre_work:部署节点环境准备
    1.防火墙检测和关闭
    2.selinux临时关闭（可能重启）
    3.免密登录
    3.离线源（上传、解压、在线源备份、离线源构建、清理）


免密登录
    检查ansible控制节点是否安装sshpass,进行明文密码登录和免密配置
    当然，要检测ansible控制节点和部署节点之间是否已经免密。若是，跳过。否则配置免密




computer：
完成对计算节点的部署


controller：
完成对控制节点的部署


