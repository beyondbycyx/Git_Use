## git的“add”,“commit”命令后的提示与状态 ##
- 工作目录下面的所有文件都不外乎这两种状态：已跟踪或未跟踪
 * Untracked(未被跟踪的)：从来没有add过，commit过的
 * tracked(已跟踪)：只要add过的就是

**还没add过a.txt文件(Untracked状态，其余的add后的都是tracked状态，不会出现“Untracked files”字眼了):**


	$ git status
	Untracked files:
	  (use "git add <file>..." to include in what will be committed)
	
	        a.txt
	
	nothing added to commit but untracked files present (use "git add" to track)


 **第一次"add"，a.txt**


	$ git status
	On branch master
	
	Initial commit
	
	Changes to be committed:
	  (use "git rm --cached <file>..." to unstage)//退出暂存区
	
	        new file:   a.txt

**commit后，a.txt：**

	hq@HQ-PC /g/git_test/a (master)
	$ git commit -m  add a.txt
	[master (root-commit) 47bb567] add
	 1 file changed, 1 insertion(+)
	 create mode 100644 a.txt
	
	hq@HQ-PC /g/git_test/a (master)
	$ git status
	On branch master
	nothing to commit, working directory clean
**删除a.txt文件(deleted状态):**

	$ git status
	On branch master
	Changes not staged for commit:
	  (use "git add/rm <file>..." to update what will be committed)
	  (use "git checkout -- <file>..." to discard changes in working directory)
	
	        deleted:    a.txt

**再创建一个新的a.txt(modifyed状态)：**

	$ git status
	On branch master
	Changes not staged for commit:
	  (use "git add <file>..." to update what will be committed)
	  (use "git checkout -- <file>..." to discard changes in working directory)
	
	        modified:   a.txt

**在暂存区(已经add,还没commit的提示)：**

	Changes to be committed:
		(use "git reset HEAD <file>..." to unstage)

**需要重新提交到暂存区(add后又修改的提示)：**

	Changes not staged for commit:
	(use "git add <file>..." to update what will be committed)
		(use "git checkout -- <file>..." to discard changes 


**add过后，又修改了(有两个版本的提示)：**

	Changes to be committed:
	(use "git reset HEAD <file>..." to unstage)
	modified: benchmarks.rb
	Changes not staged for commit:
	(use "git add <file>..." to update what will be committed)
	(use "git checkout -- <file>..." to discard changes in working directory)
	modified: benchmarks.rb
**3个区间都是一样的内容的提示：**

	$ git status
		On branch master
		nothing to commit, working directory clean

**一次提交所有的文件**

	git add  .

**注意：**运行了git add 之后又作了修改的文件，需要重新运行git add 把最新版本重新暂
存起来
**注意： **“git commit -a”跳过使用暂存区域，即跳过git add，一起commit(注意是已经tracked的文件)

## 创建“.gitignore”文件与使用 ##
- **.gitingore文件内容：**

		# 此为注释– 将被Git 忽略
		# 忽略所有.a 结尾的文件
		*.a
		# 但lib.a 除外
		!lib.a
		# 仅仅忽略项目根目录下的TODO 文件， 不包括subdir/TODO
		/TODO
		# 忽略build/ 目录下的所有文件
		build/
		# 会忽略doc/notes.txt 但不包括doc/server/arch.txt
		doc/*.txt
		# ignore all .txt files in the doc/ directory
		doc/**/*.txt
- **常用的正则：**

		[abc] 匹配任何一个列在方括号中的字符
		星号（*）匹配零个或多个任意字符
		问号（?）只匹配一个任意字符
		[0-9] 表示匹配所有0 到9 的数字，表示所有在这两个字符范围内的都可以匹配

## git diff --staged 和 git diff ##



- **注意：** 有3个区间即对应有3种状态，1是工作区间，2，是暂存区间，3是commit提交后的服务器。

- **diff比较有1和2比，2和3比**


- **git diff : 是 1 和 2 比**
- **git diff --staged ：是2和3比**

- **不同颜色代表不同，而内容不同才是真正的不同，对于commit后的内容默认是白色**




## rm  删除的操作 ##

- 删除工作目录的：git rm <file> ; rm <file>
- 删除暂存区的：git rm --cached <file>
- 强制删除所有的(特别是已经在暂存区，还没提交到仓库时)： git rm -f <file>

- 仅删除log/文件夹下当前所有*.log文件


	$ git rm log/*.log


-  递归删除log/文件夹下的所有*.log文件('\'的作用)


	$ git rm log/\*.log

## mv 移动文件 ##
-  mv < file> < file> //可用于改名字或移动文件可以“ * ”来代表所有的文件来进行移动，注意最后还是需要“commit”的。


	$ git mv *.*  to/



## log 查看提交历史 ##
- 可以设置查看显示的格式，查看最近多长时间内的历史，查看最近提交的多少次数
- 查看最近2次的提交


	git log -2 
	
## 修改最后一次提交 ##
- 上面的三条命令最终只是产生一个提交，第二个提交命令修正了第一个的提交内容。

	$ git commit -m 'initial commit'
	$ git add forgotten_file
	$ git commit --amend

## 创建版本库 ##
**1.创建文件夹**

	$ mkdir mydir
	$ cd mydir
	$ pwd 
      /users/hugo/mydir
**2.初始化仓库**

	$ git init

 **3.查看所有文件包括隐藏的**

	$ ls -ah

**4.添加(add)文件到git仓库**
	
	$ git add readme.txt

**5.确认提交(commit)文件到git仓库，并注释这次提交信息(commit可以一次提交所有add的文件)**

	$ git add file1.txt
	$ git add file2.txt file3.txt
	$ git commit -m "add 3 files."


**6.查看add或commit的状态**

	$ git status
## 从github下克隆文件到本地文件夹 ##

(可选)1.初始化本地的文件夹

	git init
2.从github网站下克隆文件下来
	
	git clone [url]


## 常见错误解决 ##
**git error: failed to push some refs to**

解决：

	git pull --rebase
	git push

完整语法：

	git pull --rebase origin master
	git push origin master

**如果 commit 提交不了**
用  git commit -am 'xxx' 强制提交