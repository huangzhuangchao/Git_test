跟李立超老师学习Git的笔记：

配置:
name
	git config --global user.name "lilichao"
email
	git config --global user.email "27254174@qq.com"

使用git:
git status
	-查看当前仓库的状态
git init
	-初始化仓库

刚刚添加到项目中的文件处于未跟踪状态:
	未跟踪 ---> 暂存
		git add <filename> 将文件切换到暂存的状态
		git add * 将所有已修改（未跟踪）的文件暂存
	暂存 ---> 未修改
		git commit -m "xxxx" 将暂存的文件存储到仓库中
		git commit -a -m "xxx" 提交所有已修改的文件（未跟踪的文件不会提交）
	未修改 --> 修改
		修改代码后，文件会变为修改状态

4.常用的命令
	1.重置文件
		git restore <filename> # 恢复文件
		git restore --staged <filename> # 取消暂存状态
	2. 删除文件
		git rm <filename> # 删除文件
		git rm <filename>-f # 强制删除
	3. 移动文件
		git mv from to # 移动文件 重命名文件
		
分支
git在存储文件时，每一次代码代码的提交都会创建一个与之对应的节点，git就是通过一个一个的节点来记录代码的状态的。节点会构成一个树状结构，树状结
构就意味着这个树会存在分支，默认情况下仓库只有一个分支，命名为master。在使用git时，可以创建多个分支，分支与分支之间相互独立，在一个分支上修
改代码不会影响其他的分支。

git branch #查看当前分支
git branch <branch name> # 创建新的分支
git branch -d <branch name> # 删除分支
git switch <branch name> # 切换分支
git switch -c <branch name> # 创建并切换分支

在开发中，都是在自己的分支上编写代码，代码编写完成后，在将自己的分支合并到主分支中

变基(rebase)
在开发中除了通过merge来合并分支外，还可以通过变基来完成分支的合并
我们通过mere合并分支时，在提交记录中会将所有的分支创建和分支合并的过程全部都显示出来，这样当项目比较复杂，开发过程比较波折时，我必须要反
复的创建、合并、删除分支。这样一来将会使得我们代码的提交记录变得极为混乱。

原理(变基时发生了什么):
1.当我们发起变基时，git会首先找到两条分支的最近的共同祖先
2.对比当前分支相对于祖先的历史提交，并且将它们提取出来存储到一个临时文件中
3.将当前部分指向目标的基底
4.以当前基底开始，重新执行历史操作
变基和merge对于合并分支来说最终的结果是一样的! 但是变基会使得代码的提交记录更整洁更清晰. 大部分情况下，合并和变基是可以互换的，但是如果分支已经提交给远程仓库，
这时尽量不要使用变基

远程仓库(remote)
目前对于git所有操作都是在本地进行的。在开发中显然不能这样的，这时我们就需要一个远程的gi仓库。远程的git仓库和本地的本质没有什么区别，不同点
在于远程的仓库可以被多人同时访问使用，方便我们协同开发。在实际工作中，git的服务器通常由公司搭建内部使用或是购买一些公共的私有git服务器。我们
学习阶段，直接使用一些开放的公共git仓库。目前我们常用的库有两个:GitHub和Gitee(码云)

将本地库上传 git：
git remote add origin https://github.com/huangzhuangchao/Git_test.git
	# git remote add <remote name> <url>
git branch -M main
	# 修改分支的名字的为main
git push -u origin main
	# git push 将代码上传服务器上

将本地库上传 gitee：
git remote add gitee https://gitee.com/huangzhuangchao/git_demo.git
git push -u gitee main

远程库的操作的命令
git remote # 列出当前的关联的远程库
git remote add <远程库名> <url> # 关联远程仓库
git remote remove <远程库名>  # 删除远程库
git push -u <远程库名> <分支名> # 向远程库推送代码，并和当前分支关联
git push <远程库> <本地分支>:<远程分支>
git clone <url> # 从远程库下载代码

git push # 如果本地的版本低于远程库，push默认是推不上去
git fetch # 要想推送成功，必须先确保本地库和远程库的版本一致，fetch它会从远程仓库下载所有代码，但是它不会将代码和当前分支自动合并
		 # 使用fetch拉取代码后，必须要手动对代码进行合并
git pull  # 从服务器上拉取代码并自动合并 （fetch + merge）

注意：推送代码之前，一定要先从远程库中拉取最新的代码

tag标签
当头指针没有指向某个分支的头部时，这种状态我们称为分离头指针(HEAD detached)，分离头指针的状态下也可以操作代码，但是这些操作不会出
现在任何的分支上，所以注意不要在分离头指针的状态下来操作仓库