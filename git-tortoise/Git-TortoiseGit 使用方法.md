# Git-TortoiseGit 使用方法

## 一、 以 ssh 方式推送远端

### 1. 获得ssh-key 并添加到git

1. 在任意文件夹下右键打开GitBush 命令行；
2. 使用 `ssh-keygen -t rsa ` 命令生成ssh-key；
3. 生成的ssh 会保存在当前用户的.ssh文件夹下 `C:\Users\ASUS\.ssh`;
4. 该文件夹有三个文件，其中`id_rsa `是本地的私钥, `id_rsa.pub`是需要保存到git的公钥;
5. 将`d_rsa.pub` 文件中的内容复制下来；
6. 打开github，点击头像，选择Settings选项，在选项中有`SSH and GPG keys`选项，将复制的公钥添加到git,保存。



### 2. 设置推送远端

1. 在工作区中右键选择Tortoise的`Git-同步`(工作区是指与`.git`同级的目录,本地仓库在`.git`文件夹下)
2. 选择管理远端URL
3. 在管理窗口中选择左侧的 `Git-远端`选项
4. 设置远端名称，在GitHub中获得仓库的ssh地址，并设置到`URL`一栏中(**非推送URL**)
5. 点击左侧选项的`网络`设置，设置其中的ssh客户端，我这里是`D:\Git\usr\bin\ssh.exe`
6. 回到远端设置Puttty密钥，在 `C:\Users\ASUS\.ssh`中选择私钥`id_rsa`，如果发现没有要将右下加选择所有文件类型
7. 点击`添加/保存` 保存远程链接
8. 在Git-同步界面选择推送即可



