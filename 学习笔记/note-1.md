## 1.github上的工程clone到本地后，修改完代码后想要push到github，但一直会有提示输入用户名及密码
  原因：出现这种情况的原因是我们使用了http的方式clone代码到本地，相应的，也是使用http的方式将代码push到服务器
  解决：换成ssh方式即可
  
  步骤：
    a. 先查看当前的方式 git remote -v
    b. 把http方式改为ssh方式。先移除旧的http的origin: git remote rm origin
    c. 再添加新的ssh方式的origin: git remote add origin git@github.com:xxxxxxxxxxxx.git
    d. 查看有没有修改成功 git remote -v

  改动完之后直接执行git push是无法推送代码的，需要设置一下上游要跟踪的分支，与此同时会自动执行一次git push命令，此时已经不用要求输入用户名及密码啦！
  
  git push --set-upstream origin master

  git本地新建一个分支后，必须要做远程分支关联。如果没有关联，git会在下面的操作中提示你显式的添加关联。
  关联目的是如果在本地分支下操作： git pull, git push ，不需要指定在命令行指定远程的分支．直接推送到远程分支。
  
  如果没有关联分支也没有显式指定分支，而是直接执行git pull命令，就会有下面的提示：

      MacBook-Pro-5:web-crm wangxiao$ git push
      fatal: The current branch wangxiao has no upstream branch.
      To push the current branch and set the remote as upstream, use
          git push --set-upstream origin wangxiao

  这个意思是：当前分支没有与远程分支关联，因此导致了提交代码失败

  解决方案有两种：
    a.直接 git push origin master 推向指定的分支，最强暴的方法。
    b.正如上面所说关联远程分支，执行git push --set-upstream origin master，这样关联的好处是以后就不用每次git push 分支名

## 2.切换连接方式，设置关联，git没法push到github
  原因：使用ssh的方式链接git服务器，需要使用SSH密钥，这样无需每次都要输入用户名和密码
  解决：在本地创建SSH key，然后将生成的SSH key文件内容添加到github帐号上去

  步骤：
    --a.首先利用本机安装的Git创建SSH key，执行如下命令就可以ssh-keygen -t rsa -C "your_email@example.com"，然后系统提示输入文件保存位置等信息，连续敲三次回车即可，生成的SSH key文件保存在中～/.ssh/id_rsa.pub
    b.然后用”cat命令”打开该文件：cat ~/.ssh/id_rsa.pub
    c.拷贝.ssh/id_rsa.pub文件内的内容，将它粘帖到github帐号管理中的添加SSH key界面中
    d.头像右边的箭头，点击后找到settings，在左边的导航栏里选择 SSH key，点击 Add SSH key 
    e.在出现的界面中填写一个名称，填一个你自己喜欢的名称即可。然后将上面拷贝的~/.ssh/id_rsa.pub文件内容粘帖到key一栏，在点击“add key”按钮就可以了

  经过上面这些操作，就可以愉快的提交代码了。。。哈哈哈，git大法好！

  ### 划重点：HTTPS和SSH方式的区别：
    a.https用443端口，可以对repo根据权限进行读写，只要有账号密码就可进行操作。
    b.ssh则用的是22端口，也可以对repo根据权限进行读写，但是需要SSH Keys授权，这个key是通过ssh key生成器生成的，然后放在github上，作为授权的证据，这样的话就不需要用户名和密码进行授权了。
    
