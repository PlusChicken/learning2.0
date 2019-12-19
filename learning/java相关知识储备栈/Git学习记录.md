# Git学习记录

#### gi

工作区(写代码用的)-->git add-->暂存区(临时存储)-->git commit-->本地库(历史版本)

#### Git和代码托管中心

代码托管中心的任务：维护远程库

局域网环境下

----GitLab服务器

外网环境下

----GitHub

----码云

#### 本地库和远程库

团队内部协作

跨团队写作

### Linux 和git一些执行代码

ll  ------显示当前所有文件

ls -lA   -------显示隐藏文件

mkdir   -------创建新文件

pwd  --------显示路径

rm [file name]-------删除操作

删除操作已经提交到本地库：找回就只需要转回之前版本就好了

删除操作尚未提交到本地库：指针位置使用HEAD

git init  ------创建git新库

cat ------读取文件的内容

注意： .git 目录中存放的是本地库相关的子目录和文件，不要删除，也不要胡乱修改。

git专属的命令都是由git开始的

git status -------查看状态

git add [file name]--------从本地库提交到暂存区

git commit --------提交

-------使用  ：nu ------就可以调出VIM的数字栏

git commit -m "My first commit,modify goof.text" good.text   ---------快捷方法

git diff [file name]

​	将工作区中的文件和暂存区进行比较

git diff [本地库中历史版本] [file name]

​	将工作区中的文件和本地库历史记录比较

git diff HEAD^ [file name]

不指定文件名的时候会可以比较当前工作中的所有文件的差异

git remote -v ------查询所有别名

git remote add origin [网上仓库地址]

git push origin master 传输分支

git log --------查找版本索引(历史纪录)

-------git log --pretty=oneline

-------git log  oneline   (只能显示现在和之前的版本，不能显示之后的版本)

-------gir reflog

多屏显示时的控制方式：

------空格向下翻页

------b向上翻页

------q退出

HEAD@{移动到当前版本需要多少步}

#### 回退/前进

[推荐]

git reset --hard [局部索引值]

git reset --hard HEAD^    (^只能后退一个^退一步，两个^^退两步)

git reset --hard HEAD~3   (~[数字] 表示后退数字步)

#### reset命令的三个参数对比

--soft 参数

​		仅仅在本地库移动HEAD指针

--mixed 参数

​		在本地库移动HEAD指针

​		重置暂存区

--hard参数

​		在本地库移动HEAD指针

​		重置暂存区

​		重置工作区

#### 设置签名

形式

用户名：tom

Email地址：goodMorning@atguigu.com

作用：区分不同开发人员的身份

辨析：这里设置的签名和登陆远程库(代码托管中心)的账号、密码没有任何关系。

命令

----项目级别/仓库级别：仅在当前本地库范围内有效

-------- **git config** user.name tom_pro

-------- **git config** user.email goodMorning_pro@atguigu.com

----系统用户级别：登陆当前操作系统的用户范围

-------- **git config --global** user.name tom_glb

-------- **git config --global** user.email goodMorning_glb@atguigu.com

----级别优先级

--------就近原则：项目级别优先于系统用户级别，二者都有时采用项目级别的签名

--------如果只有系统用户级别的签名，就以系统用户级别的签名为准

--------二者都没有不允许

#### 分支

分支：在版本控制过程中，使用多条线同时推进多个任务。

![111](C:\Users\周俞吉\Desktop\111.jpg)

hot_fix 热修复

master 主干

feature_blue 分支

feature_game 分支

分支的好处

​	同时并行推进多个功能开发，提高开发效率

​	各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可。

#### 分支代码

git checkout hot_fix -----切换分支

git branch -v  -----查询所有的分支名

git branch hot_fix  ------创建hot_fix分支

合并分支

第一步：切换到接受修改的分支(被合并，增加新内容)上

第二步：执行merge命令就可以了

git merge [被合并的分支名]

分支冲突

是将主干合并到分支(可以在Vim修改器中修改该内容)

冲突解决：

1. 编辑文件，删除特殊符号

2. 把文件修改到满意的程度，保存退出

3. git add[文件名]

4. git commit -m"日志信息"

   ---注意：此时commit一定不能带具体文件名

#### 克隆

命令：

​	git origin[远程地址]

效果：

​	完整的把远程库下载到本地

​	创建origin远程地址别名

​	初始化本地库 

#### 拉取

pull=fetch+merge

git fetch[远程库地址别名] [远程分支名]  ----先看看有没有问题

git merge[远程库地址别名/远程分支名]  ----之后再合并

#### 解决冲突

要点

​	如果不是基于GitHub远程库的最新版所做的修改，不能推送，必须先拉取。

​	拉取下来后如果进入冲突状态，则按照“分支冲突解决”操作解决即可。

类比

​	债权人：老王

​	债务人：小刘

​	老王说：10天后归还。小刘接受，双方达成一致。

​	老王媳妇说：5天后归还。小刘不能接受。老王媳妇需要老王确认后再执行。

#### 跨团队协助操作模式

外人 先fork clone 到本地库 push回远程库 pullrequest[分为new pull request 和 Create pull request ] 去 大佬远程库 然后大佬允许之后merge

#### 使用SSH登陆[以前是使用Http登陆的]

ssh-keygen -t rsa -C [邮件名]

cat id_rsa.pub  (复制里面的码去公共库的SSH and GPG keys [new sshkey])

新建远程地址的别名 输入ssh的地址

#### Eclipse中忽略文件

##### 概念：Eclipse特定文件

​	这些都是Eclipse为了管理我们创建的工程而维护的文件，和开发的代码没有直接关系。最好不要在Git中进行追踪，也就是把它们忽略。

​	.classpath 文件

​	.project 文件

​	.setting 目录下所有文件

##### 为什么要忽略Eclipse 特定文件呢？

​	同一个团队中很难保证大家使用相同的IDE工具，而IDE工具不同时，相关工程特定文件就有可能不同，如果这些文件加入版本控制，那么开发时很有可能需要为了这些文件解决冲突。

##### 配置Java.gitignore

可以在github中先找到初始配置

在加入.classpath  .project  .settings  target

之后在.gitconfig 中加入

[core]

​		excludesfile = C:/[自己的路径]/Java.gitignore

[注意：一定要使用/不能使用\\]

克隆单个分支： git clone -b template https://github.com/iview/iview-admin.git  //clone template分支

