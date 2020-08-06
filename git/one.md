git是一种分布式版本控制系统，最初由Linus大神开发，用来辅助项目维护和开发。

**提交操作**

```shell
git add file

git add -u：将文件的修改、文件的删除，添加到暂存区。
git add .：将文件的修改，文件的新建，添加到暂存区。
git add -A：将文件的修改，文件的删除，文件的新建，添加到暂存区。

git commit -m 'xxx'
git commit -am "xxx" # 将上面两种操作合为一起
```


**拉取代码**

```shell
# 拉取所有分支
git fetch origin

# 创建一个工作区分支,--track 添加跟踪关系
git checkout --track origin/<branch-name> 或者
git chekout -b <branch-name> origin/<branch-name>

# 合并本地仓库的分支
$ git merge origin/master
# 或者
$ git rebase origin/master

# 拉取所有分支并合并对应分支
git pull origin

取回远程主机某个分支的更新，再与本地的指定分支合并。
git pull <远程库名> <远程分支名>:<本地分支名>
 
取回远程库中的master分支，与本地的master分支进行合并更新，要写成：
git pull origin master:master
 
如果是要与本地当前分支合并更新，则冒号后面的<本地分支名>可以不写
git pull origin master

git fetch origin master # 拉取远程master代码到本地仓库origin/master中
git fetch origin master:tmp #拉取远程master代码到本地仓库origin/master中，并创建新分支tmp或者合并到tmp分支中。

git branch -b branch-name: 创建并切到新分支,等同于
git checkout -b branch-name

# git push 会把当前的HEAD分支上所有新的提交上传到它所关联的远程分支上去。
git push

```

**撤销操作**

```shell
# 修改上一次提交的注释
git commit --amend -m "xxx"
# 漏了某个文件，需要添加进来 或者 某个文件需要重新修改下
git add xxx
git commit --amend -m "xxx"
# 法则：对于已经push到远程分支的提交，不要再试着用--amend修改

# 撤销add (场景：改动了代码，已经add操作)
git reset HEAD

# 撤销commit并删除改动代码（场景：改动了代码，已经commit，但改动的有些问题，需要把这次改动的代码全部删除）
git reset --hard HEAD^
或者git revert HEAD

# 撤销commit但不删除改动代码（场景：改动了代码，已经commit，但改动的有些问题，需要再之前的改动基础上继续修改）
git reset --soft HEAD^
git reset HEAD
或者
git reset --mixed HEAD^ # 默认模式

# 放弃工作副本中的所有改动（场景：改动了代码，但尚未add）
git reset --hard HEAD

# git
git reflog # 查看操作历史
git checkout <commit-id> # 此时会进入detached head状态
git checkout -b <branch-name> # 给detached分支命名

git checkout <commit-id> <filename> #把某个文件恢复到<commit-id>时的情况。
```



**更新分支**

```shell
git checkout XXX2.0
git fetch origin XXX2.0:temp #从远程的origin仓库的XXX2.0分支下载到本地并新建一个分支temp
git diff XXX2.0 temp #比较XXX2.0分支和temp分支的不同
git merge temp #确认可以合入后，合并temp分支到master分支
git branch -d temp #删除temp

或者

git fetch origin XXX2.0
git diff XXX2.0 origin/XXX2.0 或者 git log XXX2.0 origin/XXX2.0 # 比较不同
git merge origin/XXX2.0

或者

git pull origin XXX.2.0
```
**查看操作**

```shell
git log --oneline --graph --all # 查看分支的合并情况

git show <commit-id> --name-only #显示该次提交修改了哪些文件

git status -s
git diff --stat --name-only # 仅显示哪些文件有改动

git remote show origin # 显示远程仓库和本地仓库之前的关系

git branch -a # 查看所有分支
git branch -v
git branch -vv # 查看每个分支的跟踪情况。
```

**删除操作**

```shell
更新本地分支信息
加入服务器的某个分支删除了，但是本地通过git branch -a还是可以看得到，则可通过以下命令删除本地仓库中的分支
git remote prune origin
或者
git fetch origin --prune
然后通过以下命令删除工作区的分支
git branch -D xxx #删除本地分支即可

删除远程分支
git push origin --delete xxx

删除本地分支
git branch -d xxx
git branch -D xxx # 强制删除
# 删除本地仓库分支
git branch -dr origin/<branch-name>
```

**比较差异**

```shell
git diff # 工作区和暂存区
git diff HEAD origin/master # 工作区和本地仓库
git diff --cached #暂存区和本地仓库
git diff --stat # 仅显示哪些文件有改动
git diff <commit-id> <commit-id> # 比较两个commit之间的差异
```

**git设置**

```shell
列出当前配置：git config --list
列出repository配置：git config --local --list
列出全局配置：git config --global --list
列出系统配置：git config --system --list

配置用户名：git config --global user.name "your name"
配置用户邮箱：git config --global user.email "youremail@github.com"

配置解决冲突时使用哪种差异分析工具，比如要使用vimdiff：git config --global merge.tool vimdiff
配置git命令输出为彩色的：git config --global color.ui auto
配置git使用的文本编辑器：git config --global core.editor vi
```

**git工作区、暂存区、本地仓库、远程仓库交互命令**

```


拉取分支代码
git fetch origin xxx
git checkout -b xxx origin/xxx


```

## git fetch

一旦远程主机的版本库有了更新（Git术语叫做commit），需要将这些更新取回本地，这时就要用到`git fetch`命令。

> ```javascript
> $ git fetch <远程主机名>
> ```

上面命令将某个远程主机的更新，全部取回本地。

`git fetch`命令通常用来查看其他人的进程，因为它取回的代码对你本地的开发代码没有影响。

默认情况下，`git fetch`取回所有分支（branch）的更新。如果只想取回特定分支的更新，可以指定分支名。

> ```javascript
> $ git fetch <远程主机名> <分支名>
> ```

比如，取回`origin`主机的`master`分支。

> ```javascript
> $ git fetch origin master
> ```

所取回的更新，在本地主机上要用"远程主机名/分支名"的形式读取。比如`origin`主机的`master`，就要用`origin/master`读取。

`git branch`命令的`-r`选项，可以用来查看远程分支，`-a`选项查看所有分支。

> ```javascript
> $ git branch -r
> origin/master
> 
> $ git branch -a
> * master
>   remotes/origin/master
> ```

上面命令表示，本地主机的当前分支是`master`，远程分支是`origin/master`。

取回远程主机的更新以后，可以在它的基础上，使用`git checkout`命令创建一个新的分支。

> ```javascript
> $ git checkout -b newBrach origin/master
> ```

上面命令表示，在`origin/master`的基础上，创建一个新分支。

此外，也可以使用`git merge`命令或者`git rebase`命令，在本地分支上合并远程分支。

> ```javascript
> 
> ```

上面命令表示在当前分支上，合并`origin/master`。

参考：

https://www.ruanyifeng.com/blog/2014/06/git_remote.html



**合并分支**

从主分支新建了一个特性分支做修改，在修改的过程中，主分支又发生了更新，这个时候，如果要提交该特性分支，则应该先在本地合并最新的主分支之后再考虑push到远程分支。有两种方法：

```shell
git checkout xxx-feature # 先切换到你的特性分支
git fetch origin master:master # 更新master分支

git rebase master 然后，基于master来做变基操作
git push origin xxx-feature

或者
git checkout xxx-feature
git fetch origin master:master
git merge master
```

**detached分支**

HEAD 处于游离状态，通过git checkout可以很方便的在历史版本之间来回穿梭，但这也造成了一个问题，即它会在这个基础上开一个匿名分支，也就是说我们的提交是无法可见保存的，一旦切到别的分支，该分支的提交就不可追溯了。

```shell
可采取的方法为：
git checkout -b tmp # 即为该分支创建一个名字
git checkout master #切换到主分支
git merge tmp # 合并刚才的临时分支，有冲突就解决冲突
git branch -d tmp #删除临时分支
```

