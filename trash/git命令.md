	pwd  //显示当前目录
	ls -ah //显示所有文件
	cat <filename> //查看文件内容

//为你的机器(已经做为服务器)配置你的个人信息，方便别人找到你

	$ git config --global user.name "[name]"
	$ git config --global user.email "[email address]"

//cd 进入特定的目录下，创建创库(会生成.git隐藏文件)

	$ git init [project-name]
	$ git clone [url] //克隆网上的文件下来

//查看文件的状态(是否修改过)
	
	$ git status
//查看文件修改的内容

	$ git diff

//提交新建(修改的)文件到仓库,允许多次add,在一次commit提交

	$ git add [file]
	$ git add [file]
	......
	$ git commit -m "[descriptive message]"

//查看历史版本

	$ git log
	$ git log --pretty=oneline //简洁查看模式，数字串为版本号
	$ git log -2 //最近2次的提交

//查看输入过的命令行历史

	$ git reflog

//返回某个版本，commit可以为版本号的前几位数字或用“HEAD^”表示最近的上一个版本，“HEAD^^”上上个版本
	
	$ git reset --hard [commit]

//撤销修改

	           add(git status)             commit(git status)
	work(工作区)-------------->stage(暂存区)------------------------>master(版本库)---------------->服务器
	           git checkout -- <file>      git reset HEAD <file>//只要还没上传服务器，该命令还生效即还可撤销修改


使用：git status查看当前在哪个区，它会提示相应的撤销修改的命令

//删除文件(工作区，版本库)

	$ rm <file> //仅删除工作区的
	$ git checkout -- <file> //从版本库中恢复

//彻底删除文件

	$ git rm test.txt
	$ git commit -m "remove test.txt"

本地库与远程库关联并上传

(前提：在github服务器上添加本地git库的ssh公匙)
(公匙：在git-bash上tool菜单下创建钥匙对)
小结

要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；

关联后，使用命令git push -u origin master第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！


//创建，删除，合并分支

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>


- 解决分支合并冲突(原因：两个分支对同一个文件进行了修改)
**注意：**所谓的冲突，有时候合并操作并不会如此顺利。如果在不同的分支中都修改了同一个文件的同一部分，Git 就无法干净地把两者合到一起


	#查看冲突的文件
	git status 
	Unmerged paths:
    (use "git add <file>..." to mark resolution)

        both modified:   a.txt
	#调用界面化修改的工具，如下，放回按下回车即可进入解决冲突的界面，解决完后，在界面的上方选择已经解决冲突的按钮
	git mergetool
	#最后提交解决完的冲突文件
	git commit -a -m 'msg'
     


