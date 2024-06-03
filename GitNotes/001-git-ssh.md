# 1. github正常配置ssh后`git clone`无法下载问题:

**解决方法**:
配置完ssh id_rsa后,在`~/.ssh/`文件夹新建`config`文件,内容如下:

```bash
# github
# ssh -T git@github.com
Port 443    
#Port 22 
Host github.com
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
```

