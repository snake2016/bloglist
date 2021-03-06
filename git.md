## Git 简介及常用操作说明

## git 与 svn 区别
svn是集成式开发，依赖于企业的中央服务器，速度慢，而且必须联网才能使用，git是分布式的；

集中式版本控制系统 | 分布式版本控制系统
---|---
版本库集中放置在中央服务器，必须先从中央服务器下载最新的版本，然后修改后再上传至中央服务器；必须联网才能工作 | 几乎没有中央服务器的概念，每台电脑都有一个完整的版本库，无需联网使用，多人协作，只需相互交换各自修改的部分，但为了交换方便，也会有一台电脑充当类似中央服务器的角色，非必须


## git 的诞生
Linus在1991年创建了开源的Linux,在2002年以前，世界各地的志愿者把源代码文件通过diff的方式发给Linus，然后由Linus本人通过手工方式合并代码；

到了2002年，Linux系统已经发展了十年了，代码库之大让Linus很难继续通过手工方式管理了，社区的弟兄们也对这种方式表达了强烈不满，于是Linus选择了一个商业的版本控制系统BitKeeper，BitKeeper的东家BitMover公司出于人道主义精神，授权Linux社区免费使用这个版本控制系统。

2005年就被打破了，原因是Linux社区牛人聚集，不免沾染了一些梁山好汉的江湖习气。开发Samba的Andrew试图破解BitKeeper的协议（这么干的其实也不只他一个），被BitMover公司发现，于是BitMover公司怒了，要收回Linux社区的免费使用权。

Linus可以向BitMover公司道个歉,花了两周时间自己用C写了一个分布式版本控制系统，这就是Git！一个月之内，Linux系统的源码已经由Git管理了；
## 版本库的创建

第一步，创建一个空目录
```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```
第二步，通过`git init`使其变成一个git可以管理的仓库
```
$ git init
```
往git版本库里添加文件，可以通过使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；

然后用命令`git commit`告诉Git，把文件提交到仓库,`-m`后面输入的是本次提交的说明;

```
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```
### 文件的修改与状态查看
`git status`命令可以让我们时刻掌握仓库当前的状态

```
$ git status
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#    modified:   readme.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
```
如果`git status`告诉你有文件被修改过`git diff`可以查看修改内容

```
$ git diff readme.txt 
```
## 版本间的穿梭
我们用`git log`命令查看提交历史，
如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：

在Git中，用**HEAD**表示当前版本，也就是最新的提交的ID，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

```
$ git reset --hard 3628164
HEAD is now at 3628164 append GPL
```
版本号没必要写全，前几位就可以了，Git会自动去找;


要重返未来，不知道最新版的id时，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本;

## 工作区，暂存区和版本区
![一张图看懂git暂存区](http://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

![add](http://www.liaoxuefeng.com/files/attachments/001384907720458e56751df1c474485b697575073c40ae9000/0)

![commit](http://www.liaoxuefeng.com/files/attachments/0013849077337835a877df2d26742b88dd7f56a6ace3ecf000/0)

### 撤销修改

---
#### 工作区的撤销：
`git checkout -- file`可以丢弃工作区的修改,`git checkout`其实是用版本库里的版本替换工作区的版本;

`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令;
```
$ git checkout -- readme.txt
```
#### 暂存区的撤销:
`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区;

`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。

### 删除文件

---
命令`git rm <file>`用于删除一个文件

情况一：确实要删除版本库中的某个文件

```
$ git rm test.txt
rm 'test.txt'
$ git commit -m "remove test.txt"
```
情况二：删错了文件

```
$ git checkout -- test.txt
```
参考上一小节工作区的撤销，该命令可以让工作区与版本区同步；

## 远程仓库
GitHub是提供Git仓库托管服务的网站；本地Git仓库和GitHub仓库之间的传输是通过SSH加密；

##### 第一步，创建SSH Key
在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开终端，创建SSH Key：

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```
换成自己的邮件，一路回车，使用默认值即可

##### 第二步，登陆GitHub设置
打开“Account settings”，“SSH Keys”页面，然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容；

####  本地仓库-->远程仓库（推送）
1. 登陆GitHub，“Create a new repo”按钮，创建一个新的仓库;
2. 把一个已有的本地仓库与之关联，需要用到命令`git remote add origin git@server-name:path/repo-name.git`;

```
 git remote add origin git@github.com:michaelliao/learngit.git
```
`michaelliao`替换成自己的GitHub账户名;
添加后，远程库的名字就是`origin`，这是Git默认的叫法;
3. 把本地库的所有内容推送到远程库上；

```
$ git push -u origin master
```
第一次推送master分支时，`-u`参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令;
4. 后续只要本地作了提交，就可以通过命令把本地master分支的最新修改推送至GitHub；

```
$ git push origin master
```


#### 远程仓库-->本地仓库（克隆）
要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆;

```
$ git clone git@github.com:michaelliao/gitskills.git
Cloning into 'gitskills'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.

$ cd gitskills
$ ls
README.md
```

Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。
`https://github.com/michaelliao/gitskills.git`这个地址也是可以的；


####  通常使用方法
1. 远程创造git仓库
1. 本地mkdir创建目录，git init
1. 本地远程建立关联，git remote add origin git@github.com:snake2016/rep-name.git
1. git add . git commit  -m
1. git fetch origin 将远程主机的更新拉取到本地
1. git pull origin master 拉取远程分支代码到本地
1. git push -u origin master 
1. 后续本地有修改时直接git push origin master:master





----
## 分支管理
#### 分支的创建与合并
1. 创建分支`dev`并切换到该分支
```
$ git checkout -b dev
```
`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

```
$ git branch dev
$ git checkout dev
```
在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

2. 查看分支是`git branch`命令，所有的分支都会被列出，当前分支前加`*`号；

```
$ git branch
* dev
  master
```
3. `dev`分支工作完成，切回`master`分支时
```
$ git checkout master
```
4. `git merge`命令用于合并指定分支到当前分支，现在时再`master`分支上，只需

```
$ git merge dev
```
请注意有时会加上`--no-ff`参数，表示禁用`Fast forward`，因为通常合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

```
$ git merge --no-ff -m "merge with no-ff" dev
```
因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去;
5. 合并完成以后，可以放心的删除`dev`分支，此时再通过`git branch`命令查看就只有`master`一个分支了；

```
$ git branch -d dev
```
##### 正常企业团队内部合作分支如下：
![image](http://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)

master分支应该是非常稳定的，仅用来发布新版本，每个人都在dev分支上干活，有自己的分支，时不时地往dev分支上合并就可以了。
#### 分支冲突问题

当分支和主干都有提交，git无法快速自动合并时，须先手动解决冲突后，再提交，合并完成。

用`git log --graph`命令可以看到分支合并图;

#### bug 分支管理
工作进行一半，需紧急处理其他事情时，可先将当前工作现场“储藏”起来，等以后恢复现场后继续工作；

```
$ git stash
```
需要恢复时，先用`git stash list`查看，然后恢复，有两个办法：
1. 用`git stash apply`恢复，但是恢复后，`stash`内容并不删除，你需要用`git stash drop`来删除；

2. 用`git stash pop`，恢复的同时把`stash`内容也删了：

#### 新功能Feature分支管理
开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

#### 多人协作工作模式
1. 首先，可以试图用`git push origin branch-name`推送自己的修改；
1. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
1. 如果合并有冲突，则解决冲突，并在本地提交；
1. 没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！
1. 如果`git pull`提示`“no tracking information”`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。



