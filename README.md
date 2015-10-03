# Git_Use
<a href="https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E4%BA%A4%E4%BA%92%E5%BC%8F%E6%9A%82%E5%AD%98"> git 交互式使用‘add 和 commit’</a> </br>

- 添加于撤销

``` java
  #添加所有
  git add --all
  #撤销所有
  git reset HEAD * ; git checkout -- <file>
```
## Git 的总结 ##
## 3大命令 ##
- 时刻清楚你的3个区间的状态:work(工作区间),stage/index(暂存区间),local repository(联网时remote repository).
	

	$ git status
	
- 时刻清楚你各个区间(版本，分支)的不同，变化

```java
	# work 与 stage 的不同 
	$ git diff 

	# stage 与 local repository 的不同
	$ git diff --cached

	# 当前HEAD(版本) 与 work 的不同
	$ git diff HEAD

	----下面的不是重点------
	# 两个提交版本的不同(用commit 对象的前5个字符串表示自己)
	$ git diff da985 b325c
	# 某个分支(dev) 与 当前 work的不同
	$ git diff dev
```

- 时刻清楚你提交过的版本

```java
	# 查看最近2次的提交版本，最上面的是最新的
	$ git log -2 --pretty=oneline
	# gitk图像化界面仅可查看当前分支的路线，或者合并后的路线
	$ gitk 
```
## 常见的命令提示 ##
- 工作目录下面的所有文件都不外乎这两种状态：已跟踪或未跟踪
 * Untracked(未被跟踪的)：从来没有add过，commit过的
 * tracked(已跟踪)：只要add过的就是
 * 未跟踪过的即从来没要add过的(没有加入到git的版本控制中):

```java
	$ git status
	Untracked files:
	  (use "git add <file>..." to include in what will be committed)
	
	        a.txt
	
	nothing added to commit but untracked files present (use "git add" to track)
 ```
 
-  benchmarks.rb 文件 add后的状态(说明work 和 stage 的一样，但是与repository不一样)
   需要从work 区间 提交到 repository

```java
	$ git status
	On branch master	
	Changes to be committed:
	(use "git reset HEAD <file>..." to unstage)
	modified: benchmarks.rb
```

-  benchmarks.rb 文件 add后又修改后的状态(说明work 的与stage 不同，stage与repository不同)
   1. 可以从stage 提交(commit)到 repository 去，但是只是将stage上文件的内容copy到了repository去   
   2. 可以从work 添加(add)到 stage 再提交(commit)到 repository去。是将work上的文件内容copy到了repository去

```java
	$ git status
	On branch master
	Changes to be committed:
	(use "git reset HEAD <file>..." to unstage)
	modified: benchmarks.rb
	Changes not staged for commit:
	(use "git add <file>..." to update what will be committed)
	(use "git checkout -- <file>..." to discard changes in working directory)
	modified: benchmarks.rb
```
	
## git 中常用的概念 ##

- 可参考progit.zh.pdf 的“git分支”章节

- **commit 对象(节点)：**
每一次的commit后都会产生一个commit 对象，该对象有自己的Id 号，同时它也是每个分支上的节点，由这些节点以单链表的形式连接起来。注意：第一个节点(即第一次commit时)是没有父节点的，而最后一个节点没有子节点，而其他的节点都有父节点和子节点。

- **tree 对象：**
  它在commit对象的内部，由多个blob 对象组成，它用来描述这次提交中包含了多少个文件

- **blob 对象：**
  它是文件对象，用来描述每个文件。

- **index(stage) ：** 
   index 也叫做stage,它是用来表示当前stage上的所有内容,即它记录了所有add过的文件
  
- **HEAD ：**
  它用来表示你当前所处的位置，它指向你当前用的commit的对象(节点)
  
- **master/其他分支 ： **
  你可以理解为指针，与HEAD 效果一样，用来指向某个commit对象(节点)

- **节点/版本 ： **
  commit对象可以叫做节点，也可以叫做版本，因为你每一次提交(commit)都是一个版本，而由此创建的 commit对象作为每个分支上的节点来使用。

- **文件冲突：**
   发生在真正意义上的合并(merge)即3方合并，文件冲突是指共同文件上的同一行上的内容出现了不同。

## 增删改交操作 ##

- 删除文件(仅工作区上进行）


		$ rm <file> //仅删除工作区的
		$ git checkout -- <file> //从版本库中恢复


- 彻底删除文件(从工作区和暂存区一起进行)
    

		$ git rm 不行的话可用"git rm -f test.txt"进行强制删除
		$ git rm test.txt
		$ git commit -m "remove test.txt"


- 删除文件(仅在暂存区[stage]进行，即本地[work]还存在的)


		$ git rm --cached test.txt
		$ git commit -m "remove test.txt"


- 撤销修改

```java
	           add(git status)             commit(git status)
	work(工作区)-------------->stage(暂存区)------------------------>master(版本库)---------------->服务器
	           git checkout -- <file>      git reset HEAD <file>//只要还没上传服务器，该命令还生效即还可撤销修改
```

- 移动文件或修改文件名


		$ git mv [file-original] [file-renamed]

- 具体的撤销自己的操作，请用"git help < command>"查看文档 

## 分支(相当指针)和HEAD的移动，合并 ##

- 具体请参考图解Git.html

- **Merge：**
  * fast-forward:指向只是简单的移动，并生成一个新的提交。将当前的分支(指针)指向要合并的分支所指向的节点。注意：只能往“后”移动，不能往前。
  **注意：这是不会出现文件冲突的，因为只是移动了分支的指针到后面位置**
  
  * 真正意义的merge:路线出现了分叉路口的两个分支的合并，默认把当前提交(ed489 如下所示)和另一个提交(33104)以及他们的共同祖父节点(b325c)进行一次三方合并。结果是先保存当前目录和索引，然后和父节点33104一起做一次新提交。
  **注意：这里就会有可能出现文件的冲突，冲突是指前面路线上某个共同的文件中的同一行上的内容出现了不同**

- **Cherry Pick :**cherry-pick命令"复制"一个提交节点并在当前分支做一次完全一样的新提交。


- **Rebase(同一条线上):**衍合是合并命令的另一种选择,衍合在当前分支上重演另一个分支的历史，提交历史是线性的


- 常用的命令


		#新建分支并转移到该分支
		$git checkout -b dev
		#转移到某个分支
		$git checkout master
		#当前分支合并dev分支
		$git merge dev
		#查看分支命令
		$git help branch

## 自己创建“.gitignore”文件与使用 ##
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


## 命令行式知识的学习心得 ##

- 要多敲命令。
- 要会看懂控制台的提示内容，不懂就查Google
- 要多使用自带的文档查看命令的使用，git help <command>
- 多看软件提供的官方文档
- 了解软件的设计与构造
- 抽象的地方，搜索找关于这方面的图解
- 不强求深入，根据需求学习，要用多少就学多少，不要看的太多，太深



## 小技巧  ##

	#命令补全：前面几个关键字后按 Tab 键，快速提示或补全
	git bran<Tab>
	#参数补全
	git commit --<Tab> 
	#使用内置的help文档
	git help <command>
