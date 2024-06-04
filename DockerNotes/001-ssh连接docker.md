# docker下ubuntu的ssh访问

- 方法1: 容器与本机共享网络

1. **安装ssh**

```shell
apt-get update
# 安装ssh
apt-get install openssh-server openssh-client
# 重启ssh
service ssh restart
```

2. **修改sshd_config**

```shell
# 安装vim
apt-get install vim

cd /etc/ssh/
# 编辑sshd_config 将其
PermitRootLogin yes
Port 2222 # 端口号22与本地默认端口号重复
service ssh restart
```

3. **ssh 登录**

```shell
ssh -p 2222  root@127.0.0.1 
```

