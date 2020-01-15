# git基本使用

git 仓库创建后，会得到两个仓库地址，两个地址的区别是一个使用 https，另一个使用 ssh 协议。两者的区别是：

1. https 用443端口，可以对 repo 根据权限进行读写，只要有账号密码就可进行操作。

2. ssh 则用的是22端口，也可以对 repo 根据权限进行读写，但是需要 SSH Keys 授权，这个 key 是通过 ssh key 生成器生成的，然后放在 github 上，作为授权的证据，这样的话就不需要用户名和密码进行授权了。

3. 在使用git拉取或提交文件时，如果走http的方式，文件太大会造成提交报错，这时候必须走SSH的方式

## 免密操作
使用 https 地址上传和下载代码的时候，都会让输入 GitHub 的用户名及密码，通过以下配置，可以解决每次都要输入密码的麻烦。
1. 使用 ssh 协议的，可以在电脑设备和 GitHub 之间配置 SSH 秘钥对,公钥放在 Github 上，私钥放在电脑本地，这样使用有私钥的这台电脑，操作 GitHub 就不用输入密码了

上传key到github

> clip < ~/.ssh/id_rsa.pub

* 登录github
* 点击右上方的Accounting settings图标
* 选择 SSH key
* 点击 Add SSH key

2. 在当前用户目录下新建.gitconfig文件，写入如下配置保存后，随意打开一个项目，git pull 或者 git push 一次，下次就不需要输入密码了
```
[user]
name = 'git用户名'
email = 'git邮箱'

[credential]
helper = store
```

## 查看是否已经拥有密钥
默认情况下，用户的 SSH 密钥存储在其 ~/.ssh 目录下

    cd ~/.ssh
    ls
    
如果拥有密钥，就会看到 id_dsa 或 id_rsa 命名的文件，其中一个带有 .pub 扩展名。.pub 文件是你的公钥，另一个则是私钥。如果找不到这样的文件（或者根本没有 .ssh 目录），通过运行 ssh-keygen 程序来创建它们，在 Linux/Mac 系统中，ssh-keygen 随 SSH 软件包提供；在 Windows 上，该程序包含于 MSysGit 软件包中。

详细内容参考：https://git-scm.com/book/zh/v2/

## 如何管理本地多个SSH Key
有时候不仅Github需要使用ssh key，其它的项目或者平台也需要使用ssh key来认证，这时候就需要生成多个ssh key，并通过在~/.ssh目录下增加config文件来解决，具体做法如下：
1. 生成ssh key时，同时指定保存的文件名
> ssh-keygen -t rsa -f ~/.ssh/id_rsa.github -C "邮箱"

上面的id_rsa.github就是我们指定的文件名，这时~/.ssh目录下会多出id_rsa.github和id_rsa.github两个文件，id_rsa.github里保存的就是我们要使用的key。

2. 配置ssh config文件，如果文件不存在就创建

> vim ~/.ssh/config

```
Host github.com
    Hostname ssh.github.com
    Port 443
    User 用户名
    IdentityFile ~/.ssh/id_rsa.github
```

3.测试ssh 是否配置成功，以Github为例

> ssh -T git@github.com

## 如果使用了 https 的方式，怎么切换到 ssh
 
    步骤：
    1. 先查看当前的方式 git remote -v
    2. 把 https 方式改为 ssh 方式。先移除旧的 http 的 origin: git remote rm origin
    3. 再添加新的 ssh 方式的 origin: git remote add origin git@github.com:xxxxxxxxxxxx.git
    4. 查看有没有修改成功 git remote -v
  
## 直接执行 git pull 命令，就会有下面的提示：

      MacBook-Pro-5:web-crm wangxiao$ git push
      fatal: The current branch wangxiao has no upstream branch.
      To push the current branch and set the remote as upstream, use
          git push --set-upstream origin wangxiao

这个意思是：当前分支没有与远程分支关联，因此导致了提交代码失败

    解决方案有两种：
    1. 直接 git push origin master 推向指定的分支，最强暴的方法。
    2. 设置关联远程分支，执行 git push --set-upstream origin master，这样关联的好处是不用指定分支名

## 设置SSH密钥

    步骤：
    1. 输入 ssh 命令，看电脑是否安装 ssh 协议，如果没有，去网上搜索下载安装
    2. 首先利用本机安装的 Git 创建 SSH key，执行如下命令就可以 ssh-keygen -t rsa -C "your_email@example.com"，然后系统提示输入文件保存位置等信息，连续敲三次回车即可，生成的 SSH key 文件保存在中 ～/.ssh/id_rsa.pub
    3. 然后用”cat命令”打开该文件：cat ~/.ssh/id_rsa.pub
    4. 头像右边的箭头，点击后找到 settings，在左边的导航栏里选择 SSH key，点击 Add SSH key 
    5. 拷贝 .ssh/id_rsa.pub 文件内的内容，将它粘帖到 github 帐号管理中的添加 SSH key 界面中
    6. 在出现的界面中填写一个名称，填一个你自己喜欢的名称即可。然后将上面拷贝的~/.ssh/id_rsa.pub文件内容粘帖到key一栏，在点击“add key”按钮就可以了

    
