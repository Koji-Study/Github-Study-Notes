# 							                                GIT-STUDY

​																																									         By Koji



[TOC]

## CVS，SVN集中式版本控制系统特点

​		1.要先在服务器上下载最新的版本，修改后再更新到服务器上

​		2.必须联网才能工作





## GitHub分散式版本控制系统特点 

​		1.不存在“中央服务器”，每台电脑上都有个完整的版本库

​		2.不需要联网工作





## linux上安装git

```shell
git
#The program 'git' is currently not installed. You can install it by typing:
#sudo apt-get install git
sudo apt-get install git
```





## linux 创建版本库(Repository)                    

#### 1.选择合适的地方创建空目录

```shell
mkdir learngit       #创建空目录
cd learngit			 #进入创建的目录
pwd          		 #显示创建目录的路径
git init			 #在当前目录新建一个Git代码库，不可手动更改库里面的文件，否则可能造成混乱
ls -la 				 #列出当前目录下所有文件夹及文件细信息
```

#### 2.把文件添加到版本库

**##**文件最好使用`UTF-8`编码的纯文本文件，所有文字用同一种格式，既没有冲突，又能被所有平台支持

```shell
touch filename.txt	#创建一个空的.txt文件
vi filename.txt     #编辑文件
git add readme.txt  #添加文件到版本库
git commit -m "wrote a readme file" #把文件提交到版本库，-m表示本次提交的说明		
```

可以用`add`来添加不同的文件，最后一次性`commit`多个文件，`commit`时要在命令上加上`message`，若不加就会进入`message`的编辑，`ctr + x `退出



```shell
git status 			#掌握当前版本库的状态
git diff  filename.txt		#查看文件的不同
```





## 版本库文件版本回退

```shell
git reflog          #查看每次命令的历史log，以便到未来的某个版本
git log				#查看版本库提交的log,确定想要回到的版本是上第几个或者它的版本号
git reset --hard HEAD^   #回退到上一个版本
git reset --hard XXXXX   #XXXXX版本号的前面几位，即commit id
```

`HEAD`  表示当前版本

`HEAD^ `表示上一个版本

`HEAD~100` 表示上100个版本





## 工作区和暂存区

其中的关联：

**第一步**是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

**第二步**是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

![gitadd](gitadd.png)





## 管理修改

​		**1.第一次修改 -> `git add` -> 第二次修改 -> `git commit`**

​         `commit`提交的是暂存区的内容，如果文件修改后没有添加到暂存区，就不会被`commit`上去，如上，第二次修改的内容不会被`commit`上去

​		**2.第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`**

​		 这种情况下，俩个不同的修改的内容都被添加到暂存区里面了，`commit`时会被一次性都提交上去





## 撤销修改

`readme.txt `文件修改撤销的两种情况：

**第一种情况**：（ `git add `之前）

 readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

```shell
git checkout -- readme.txt   #撤销readme.txt在工作区的修改
```

**第二种情况**： （`git add `之后，`git commit`之前）

 readme.txt作了修改后被添加到暂存区后，现在，撤销修改就回到添加到暂存区前的状态。

```shell
git reset HEAD readme.txt    #撤销暂存区的修改，放回工作区
```

此时的情况就和第一种情况想同，即只做了修改但并未添加到暂存区，如果想回到未修改的状态，则需要

```shell
git checkout -- readme.txt   #撤销readme.txt在工作区的修改
```

*附加说明*     ：**第三种情况**（`git commit` 之后，推送到远程库之前）

```shell
git reset --hard HEAD^   #回退到上一个版本
```

或者

```shell
git reset --hard XXXXX   #XXXXX版本号的前面几位，即commit id
```

回退到上一个版本





## 删除文件

#### 1.从版本库中删除文件

```shell
git rm filename.txt    #从版本库中删除文件
git commit -m “message”            #提交到版本库中，确认删除
```

#### 2.删除文件后恢复文件的方法

**第一种情况**：误删除，从版本库中同步版本到工作区

```shell
rm fikename.txt    #从版本库中删除文件
					#与上一种情况相比较，未commit，且在工作区删除
git checkout -- filename.txt #从版本库中同步版本到工作区
```

**第二种情况**：真实删除，并且已经`commit`，此时只能回退到上一个版本

```shell
git reset --hard HEAD^   #回退到上一个版本
```

或者

```shell
git reset --hard XXXXX   #XXXXX版本号的前面几位，即commit id
```





## 远程库(origin)

创建的新的远程库,可以把已有的本地仓库与之关联，然后把本地仓库的数据推送到`GitHub`仓库

```shell
git remote add origin git@github.com:Koji-Study/gitskills.git  #关联远程库
```

将本地库的全部内容推送到远程库上

```shell
git push -u origin master     #首次推送master分支到远程
```

把本地库推送到远程，实际上是把当前分支推送到远程

​		由于远程库是空的，我们**第一次**推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容

推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取

时就可以简化命令。

从现在起，只要本地作了`commit`，就可以通过命令：

```shell
git push origin master       # 将分支的最新修改推送到GitHub
```

**注意：**将源码push到GitHub，会出现错误`error: failed to push some refs to 'git@github.com:Koji-Study/gitskills.git'`,尝试`git push -u origin master`也无效时，出现错误的主要原因是`github`中的`README.md`文件不在本地代码目录中,可以通过命令`git pull --rebase origin master`进行代码合并，执行以上命令后可以看到本地代码库中多了`README.md`文件，此时再执行语句` git push -u origin master`即可完成代码上传到`github`。





## 从远程库克隆

```shell
git clone git@github.com:Koji-Study/gitskills.git    #从远程库克隆
```

##GitHub给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地址，

```shell
git clone https://c5299169:827eade63186deddff7b85b2106f49298d626de2@github.tools.sap/MCC-CDC/RD_member_account_RAM_init.git
```

实际上，`Git`支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。使用`https`除了速度以

外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放`http`端口的公司内部就无法使用`ssh`协议而只能用`https`，通过`ssh`支持的原生`git`协议速度最快。





## 分支管理

#### 1.创建并切换到分支

```shell
git checkout -b dev       #创建并切换到dev分支，-b表示切换到该分支
```

或者

```shell
git branch dev		      #创建分支
git checkout dev		  #切换到分支
```

**##注意以上方法可能会引起撤销的命令混淆，则可以使用一下方法：(2.23版本以后可以使用)**

```shell
git switch -c dev		 #创建并切换到新的分支
```

或者

```shell
git branch dev		      #创建分支
git checkout dev		  #切换到分支
```

#### 2.查看创建的分支

```shell
git branch     			  #查看所有的分支
```

  当前分支前面会有一个*

```shell
wanghao@wanghao-VirtualBox:~/wanghao/gitskills$ git branch
  dev					#刚刚创建的分支
* master				#当前在master主分支上
```

#### 3.合并当前分支到master分支上

```shell
git merge dev 			#将制定的分支dev合并当前分支上
```

#### 4.合并之后删除无用的分支

```shell
git branch -d dev      #删除分支dev
```





## BUG分支

每个`bug`都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

#### 1.先保存当前的 工作现场

```shell
git stash      #保存当前的工作现场
```

#### 2.首先确定要在哪个分支上修复`bug`，假定需要在`master`分支上修复，就从`master`创建临时分支

```shell
git checkout master      # 切换到master分支上
git checkout -b issue101 #在当前分支上创建issuse临时分支
```

#### 3.修复bug，提交，合并

```shell
git add issuetxt.txt    							#将修改后的文件添加到暂存区
git commit -m "about the issue"   					#将已经添加到暂存区的文件提交
git checkout master    								#切换到出现问题的分支
git merge --no-ff -m "merged bug fix 101" issue101  #将修改好的临时bug分支合并到出现问题的分支
git branch -d issue101  							#将用于修改bug的临时分支删除
```

#### 4.回到原来的工作现场

```shell
git stash list     		#查看保存的工作现场
```

恢复方法一：

```shell
git stash apply       #恢复工作现场，但stash内容不删除
git stash drop        #删除stas
```

恢复方法二：

```shell
git stash pop         #恢复的同时并删除stash
```

####  5.复制相同的修复到其他分支上

在`master`分支上发现了问题并且在`mater`上进行了修复，而`dev`分支上也是有相同的问题，则可以将`master`上的修复复制到`dev`上，达到俩个分支同时都修复`bug`，而不需要逐个分支都一一修复处理的目的

```shell
git cherry-pick 4c805e2        #4c805e2是master分支在commit时的commitid
```





## 多人协作

​		**1.查看远程库信息**

```shell
git remote -v                  #查看远程库的详细信息
```

​		**2.分支的管控**

- `master`分支是主分支，因此要时刻与远程同步；

- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

- `bug`分支只用于在本地修复`bug`，就没必要推到远程了，除非老板要看看你每周到底修复了几个`bug`；

- `feature`分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

  **3.当你和别人多人协作时**

  首先，可以试图用`git push origin <branch-name>`推送自己的修改；

  如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；

  合并有冲突，则解决冲突，并在本地提交；

  没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功。

  如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。





## 标签管理

#### 1.创建标签

```shell
git checkout master					#切换到需要贴标签的分支
git tag t01	/git tag t01 fb3d915    #直接贴或者找到相应commitid来贴标签 
git tag      						#查看所有的标签
git show t01                 		#查看t01标签的详细信息
```

**注意：**标签总是和某个`commit`挂钩。如果这个c`ommit`既出现在`master`分支，又出现在`dev`分支，那么在这两个

分支上都可以看到这个标签。

#### 2.删除本地标签

```shell
git tag -d t01						#删除标签t01				
```

#### 3.推送标签到远程

```shell
git push origin t01                #推送t01标签到远程
git push origin --tags				#将所有未推送的标签全部推送到远程
```

#### 4.删除已经推送到远程的标签

```shell
git tag -d t01						#先将本地标签删除
git push origin :refs/tags/t01		#再将远程标签删除
```

