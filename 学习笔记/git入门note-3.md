# merge 和 rebase 的区别

1.切换到 experiment 分支

    git checkout experiment

2.以 master 为基底分支，改变 experiment 的基准

  git rebase master

# 在 experiment 变基后回到master分支开始合并

    $ git checkout master
    $ git merge experiment

【变基原理：】
1.首先找到这两个分支（即当前分支 experiment、变基操作的目标基底分支 master）的最近共同祖先 C2；
2.然后对比当前分支相对于该祖先的历次提交，提取相应的修改并存为临时文件；
3.然后将当前分支指向目标基底 C3；
4.最后以此将之前另存为临时文件的修改依序应用到master分支
