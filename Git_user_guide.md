# 一、安装Git #

## 1. Ubuntu ##

    sudo apt install git

## 2. Windows ##

（1）从Git官网上[下载安装程序](https://git-scm.com/download/win)  
（2）在开始菜单里找到“Git” -> “Git Bash”  
（3）在命令行输入：  

    $ git config --global user.name "your name"
    $ git config --global user.email "email@example.com"

# 二、创建版本库 #

<font color=#FF0000>注意事项：</font>  
（1）版本控制系统只能追踪文本文件的改动。  
（2）强烈推荐使用标准的UTF-8编码。  
（3）Windows系统下推荐使用Notepad++替代记事本，因为Microsoft记事本保存UTF-8编码的文件时，在每个文件开头添加了0xefbbbf字符。

step1：创建一个空目录

    $ mkdir learngit
    $ cd learngit
    $ pwd  % 显示当前目录(win下目录不能含有中文)

step2: 通过`git init`指令把当前目录变成Git可以管理的库

	$ git init

step3: 把文件添加到版本库  
（1）编写一个文本文件，如:readme.txt  
（2）用指令`git add <file1> <file2>...`告诉Git，把文件添加到Git库（一次可以添加多个文件）：

    $ git add readme.txt
（3）用指令`git commit -m <message>`告诉Git，把文件提交到Git库：

    $ git commit -m "wrote a readme file"


### 三、版本管理 ###

#### 1. 版本库操作 ####

    $ git status  # 查看版本库状态
    $ git diff    # 查看修改内容，在git add之前使用才有效。
    $ git log     # 版本历史记录
    $ git log --pretty=oneline

#### 2. 版本回退 ####
Git中，用`HEAD`表示当前版本，上一个版本是`HEAD^`，上上一个版本是`HEAD^^`，往上100个版本是`HEAD~100`。
  
    $ git reset --hard HEAD^

当返回了一个旧版本，又需要返回到最新的版本。分两种情况：    
（1）当命令行窗口没有关闭，使用如下指令：

    $ git reset --hard xxxxx  # xxxx填写需要返回的最新版本commit id。只需要填写前几位即可，Git会自动去找。  

（2）当命令行窗口已关闭，使用如下指令：  

    $ git reflog  # 查看历史指令，找到所需版本的commit id
    $ git reset --hard xxxx 

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的HEAD指针。

#### 3. 工作区和暂存区 ####
Workding Directory：电脑中能看到的目录，如Git文件夹。  
Repository：工作区有一个隐藏目录`.git`，是Git的版本库。版本库中主要文件有：（1）被称为stage的暂存区，master分支和指向master的指针叫HEAD。  

把文件往Git版本库添加的时候，分两步执行：  
step1: 用`git add`指令把文件添加进去，实际上是把文件修改添加到暂存区；  
step2: 用`git commit`指令提交更改，实际上就是把暂存区的所有内容提交到当前分支。

#### 4. 管理修改 ####
Git管理的是修改，而不是文件。`git commit`只负责把暂存区的修改提交，而不理会工作区的文件。<font color=#FF0000>因此，每次修改，如果不用`git add`到暂存区，`git commit`不对将其加到commit中</font>。

    $ git diff HEAD -- filename.txt  # 可查看工作区和版本库里最新版本的区别

#### 5. 撤销修改 ####
（1）当你改乱了工作区某个文件内容，想直接丢弃工作区的修改时，用指令`git checkout -- <file>`。  
（2）当你不但改乱了工作区某个文件内容，还添加到了暂存区时，想要丢掉修改，分两步，第一步用指令`git reset HEAD <file>`回到（1），第二步按照（1）操作。  
（3）已经提交了不合适的修改到版本库时，想要撤销本次修改，可参考“版本回退”一节。

#### 6. 删除文件 ####
通常直接在文件管理器中把没用的文件删除，或者用`rm`指令删除：

    $ rm test.txt
此时，Git知道删除了文件，因此工作区和版本库就不一致了，`git status`指令可查看哪些文件被删除了。
（1）从版本库中删除该文件

    $ git rm <file> 
    $ git commit -m "remove <file>

（2）误删，从版本库中将误删的文件恢复到最新版本，且只能恢复文件到最新版本，你会丢失<font color = #FF0000>最近一次提交后修改的内容，如何理解？</font>

    $ git checkout -- <file>
<font color = #FF0000> `git check`其实是用版本库中的版本替换工作区的版本。 </font>


### 四、远程库 ###

#### 1. 配置远程库 ####
step1：创建SSH Key。在用户主目录下查看有没有.ssh目录和id\_rsa（私钥）和id\_rsa.pub（公钥）文件，如果有跳过这一步，如果没有，需创建SSH Key：  

	$ ssh-keygen -t rsa -C "youremail@example.com"

然后采用默认设置。  
step2：登陆GitHub，添加SSH Key，填上任意Title，在Key文本框里粘贴id_rsa.pub文件内容。GitHub上可以添加多个SSH Key。

#### 2. 添加远程库并同步 ####
step1：在GitHub上创建一个新库，如：learngit  
step2：将本地Git库与远程库关联，用`git remote add origin`指令：

	$ git remote add origin git@github.com:fredx1618/learngit.git  # origin为Git默认远程库名

step3：将本地库所有内容推送到远程库上，用`git push`指令，<font color = #FF0000>实际上是把本地库master分支推送到远程库master分支</font>：

	$ git push -u origin master  # -u 将本地master分支与远程master分支关联，只需首次推送时使用

#### 3. 从远程库克隆 ####
step1：在GitHub创建一个新库，如：gitskills  
step2：将GitHub的gitskills克隆至本地Git库，用`git clone`指令：
	
	git clone git@github.com:fredx1618/gitskills.git

<font color = #FF0000> 注：</font>Git支持多种协议，包括https，但ssh速度最快。


### 五、分支管理 ###

分支作用：假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

#### 1. 创建与合并分支 ####
文件每次提交，Git都会把它们串成一条时间线，这条时间线就是一个分支。Git默认只有一条时间线，称为主分支，即：`master`分支。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的。所以，`HEAD`指向的是当前分支。 

![](https://i.imgur.com/JEhbXr5.png)

当我们创建新的分支，如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向dev，就表示当前分支在`dev`上：

![](https://i.imgur.com/BlQIqLH.png)

从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变。

![](https://i.imgur.com/RQIFWkH.png)

假如在`dev`上的工作完成了，就可以把`dev`合并到`master`上。直接把master指向`dev`的当前提交。

![](https://i.imgur.com/a7204F0.png)

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针删除，剩下一条`master`分支：

![](https://i.imgur.com/ziW2AYV.png)

&nbsp;

step1：创建`dev`分支，然后切换至`dev`分支

	$ git check -b dev  # -b 表示创建并切换，等价于下面两条指令
	$ git branch dev
	$ git checkout dev

step2：用`git branch`指令查看当前分支

	$ git branch  # 返回值中有 * 号，表示当前分支

step3：在`dev`分支上进行修改提交后，切换回`master`分支

	$ git checkout master

step4：把`dev`分支的修改合并到`master`分支上

	$ git merge dev

step5：删除`dev`分支

	$ git branch -d dev

<font color = #FF0000> 小结 </font>  
查看分支：`git branch`  
创建分支：`git branch <name>`  
切换分支：`git checkout <name>`  
创建+切换分支：`git checkout -b <name>`  
合并某分支到当前分支：`git merge <name>`  
删除分支：`git branch -d <name>`  

#### 2. 解决冲突 ####
当两个分支（master和feature1）各自都分别有新的提交，如下图所示：

![](https://i.imgur.com/nK8W1s5.png)

这种情况下，Git无法执行“快速合并”，只能试图把各自修改合并起来，但这种合并可能有冲突。

	$ git merge feature1  # 在master分支下，将master合并至feature1
	$ git status  # 查看冲突文件

解决办法：手动修改文件后在进行`add`、`commit`提交。解决后，`master`分支和`feature1`分支变成下图状态：

![](https://i.imgur.com/0d6Cu49.png)

可用`git log --graph`指令查看分支的合并情况

	$ git log --graph --pretty=oneline --abbrev-commit

最后删除`feature1`分支。

#### 3. 分支管理策略 ####
通常在合并分支时，如果可能，Git会用`Fast forward`模式，但在这种情况下，删除分支后，会丢掉分支信息。如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样从分支历史上就可以看出分支信息。
  
准备合并分支，如：`dev`

	$ git merge --no-ff -m "merge with no -ff" dev  # --no-ff表示禁用Fast forward

<font color = #FF0000>因为本次合并要创建一个新的commit，所以加上`-m`参数，把`commit`描述写进去。</font>

从下图可以看到，不使用`Fast forward`模式，merge后有历史分支，能看出曾经做过合并，注意与`Fast forward`模式区别。

![](https://i.imgur.com/73lsFJW.png)

在实际开发中，我们应该按照几个基本原则进行分支管理：首先，master分支应该是非常稳定的，即用来发布新版本，干活都在dev分支上，协同开发时都在dev分支上干活，只需要是不是地往dev分支上合并即可。

![](https://i.imgur.com/mdEfp71.png)

#### 4. Bug分支 ####
当接到一个代号101的bug任务时，但当前正在`dev`上进行的工作还没有提交，可以使用`stash`功能，将当前工作现场储藏起来，等以后恢复现场后继续工作

	$ git stash   # 将当前工作现场储藏起来
	$ git status  # 检查当前工作现场是否干净的

转到其他分支上修复bug，并提交

修复完成后，切回到`dev`分支继续干活

	$ git checkout dev
	$ git status  # 工作现场是干净的
	$ git stash list

工作现场恢复  
方法1：使用`git stash apply`恢复，再用`git stash drop`删除stash内容；  
方法2：使用`git stash pop`恢复，恢复的同时把stash内容也删了。  

可以进行多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用指令

	$ git stash apply stash@{0}

#### 5. Feature分支 ####
某个项目需要开发新功能，在新功能开发完毕后，在新建的分支`feature-vulcan`下开发完毕后，切回`dev`，准备合并，此时接到指令，新能共必须取消，需要删除新功能
	
	$ git branch -d feature-vulcan

删除失败，Git提醒，`feature-vulcan`分支还没有被合并，如果删除，将丢掉修改，如果要强行删除，需要使用

	$ git branch -D feature-vulcan

#### 6. 多人协作 ####
当从远程库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了。要查看远程库信息

	$ git remote
	$ git remote -v  # -v 显示更详细信息

推送分支

	$ git push origin master
	$ git push origin dev  # dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步

多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。  
多其他人从远程库clone时，默认情况下，其他人只能看到本地的master分支。  
如果其他人要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，用下面指令：

	$ git checkout -b dev origin/dev

<font color = #ff0000>当多人向`origin/dev`分支推送提交时，存在推送冲突</font>  
<font color = #ff0000>解决方法：</font>  用`git pull`把最新的提交从`origin/dev`抓下来，然后在本地合并，解决冲突，再推送。

	$ git pull

如果`git pull`失败了，原因时没有指定本地`dev`分支与远程`origin/dev`分支的连接

	$ git branch --set-upstream-to=origin/dev dev

再进行`git pull`操作，接着手动解决冲突，最后进行提交和push。

<font color = #ff0000>多人协作工作模式：</font>  
（1）首先，可以试图用`git push origin <branch-name>`推送自己的修改；          
（2）如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；      
（3）如果合并有冲突，则解决冲突，并在本地提交；        
（4）如果冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功。
    
如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的连接关系没有创建，用指令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

#### 7. Rebase ####


### 六、标签管理 ###

发布一个版本时，通常在版本库中打一个标签。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来，所以，标签也是版本库的一个快照。tag是一个让人容易记住的有意义名字，它跟某个commit绑在一起。

#### 1. 创建标签 ####

step1：切换到需要打标签的分支上

	$ git branch

step2：创建一个新标签

	$ git tag v1.0

查看所有标签

	$ git tag

<font color = #ff0000>默认标签是打在最新提交的commit上的。</font> 如果忘打标签，找到历史提交的commit id，然后打标签。

	$ git log --pretty=oneline --abbrev-cmmoit
	$ git tag <tag name> <commit id>

标签不是按时间顺序列出，而是按字母排序的，产看标签信息

	$ git show <tag name>

创建带说明的标签，用`-a`指定标签名，`-m`指定说明文字

	$ git tag -a v0.1 -m "version 0.1 released" 1094adb(commit id)

<font color = #ff0000>标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么这两个分支上都可以看到这个标签。</font>


#### 2. 操作标签 ####

（1）删除标签  

	$ git tag -d <tagname>

因为创建的标签只存储在本地，不会自动推送到远程。所以，打错的标签也可以在本地安全删除。  

（2）推送某个标签到远程

	$ git push origin <tagname>
	$ git push origin --tags  # 一次性推送全部尚未推送到远程的本地标签

（3）删除已推送至远程的标签，先从本地删除，然后从远程删除，可从GitHub上查看远程标签是否删除。
	
	$ git tag -d <tagname>
	$ git push origin :refs/tags/<tagname>


### 七、自定义Git ###

#### 1. 忽略特殊文件 ####
在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git会自动忽略这些文件。  
不需要从头写`.gitignore`文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：[https://github.com/github/gitignore](https://github.com/github/gitignore "https://github.com/github/gitignore")  

忽略文件原则：
（1）忽略操作系统自动生成的文件，比如缩略图等；  
（2）忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；  
（3）忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

step1：修改.gitignore配置文件  
step2：将.gitignore提交到Git，可以对.gitignore做版本管理  
step3：用`git status`指令检查工作区是否working directory clean  

	$ git add -f <filename>  # 强制添加到Git
	$ git check-ignore -v <filename>  # 检查那个规则写错了

#### 2. 配置别名 ####

常用别名，status->st，checkout->co，commit->ci，branch->br

	$ git config --global alias.st status  # --global参数是全局参数，表示指令在这台电脑所有Git库下都可以用
	$ git config --global alias.co checkout

配置一个`git last`，让其显示最后一次提交信息

	$ git config --global alias.last 'log -l'
	$ git last  # 显示最近一次提交

配置一个`git log`

	$ git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"


配置Git时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的库起作用。每个库的Git配置文件都放在`.git/config`文件中。配置别名也可以直接修改配置文件。


### 八、搭建Git服务器 ###

step1：安装Git

	$ sudo apt install git

step2：创建一个git用户，用来运行git服务

	$ sudo adduser git

step3：创建证书登录
收集所有需要登录用户的公钥，就是他们自己的`id_rsa.pub`文件，把所有公钥导入`/home/git/.ssh/authorized_keys`文件里，一行一个。

step4：初始化Git库
先选定一个目录作为Git库，假定是/srv/sample.git，在/srv目录下输入

	$ sudo git init --bare sample.git

Git就会创建一个裸库，裸库没有工作区，因为服务器上的Git库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git库通常以.git结尾。然后，把owner改为git

	$ sudo chown -R git:git sample.git

step5：禁用shell登录
出于安全考虑，第二步创建的git用户不允许登录shell，编辑/etc/passwd，找到：
	
	git:x:1001:1001:,,,:/home/git:/bin/bash

改为：
	
	git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell

step6：克隆远程库
在各自电脑上运行：

	$ git clone git@server:/srv/sample.git

管理公钥：可用Gitosis

管理权限：可用Gitolite


&nbsp;
### <font color=#FF0000>Common Problems and Solutions</font> ###

#### warning: LF will be replaced by CRLF in xxx ###
问题原因：不同操作系统所使用的换行符是不一样的,三大主流操作系统的换行符：  

- Unix/Linux 采用换行`LF`表示下一行（LineFeed，换行）
- Dos/Windows 采用回车+换行`CRLF`表示下一行（CarriageReturn LineFeed，回车换行）  
- Mac OS 采用回车`CR`表示下一行（CarriageReturn，回车）

在`Git`中，可以通过以下指令来显示Git采取那种对待换行符的方式

    $ git config core.autocrlf

返回值有三种，分别是：  

- true，Git会将增加的所有文件视为文本文件，将结尾的CRLF转换为LF，而checkout时会再将文件的LF格式转为CRLF格式。  
- false，line endings不做任何改变，本文文件保持为其原来的样子。  
- input，add时Git会把CRLF转换为LF格式，而checkout时仍为LF格式，因此Windows系统不推荐设置此值。  

解决办法：  
1. 将core.autocrlf设为false，如果只工作Windows或Linux平台没问题，如果存在跨平台，慎用。  
2. 当core.autocrlf设为true时，当上传一个二进制文件，Git可能会将二进制文件误认为是文本文件，会修改二进制文件，从而产生隐患。  

    $ git config --global core.autocrlf true  # true 位置存放你想要autocrlf输出结果，true，false或input。