# git-command（[git官网](https://git-scm.com/book/zh/v2 "git官网")）
*注：使用 [mdeditor](http://www.mdeditor.com/ "mdeditor") markdown在线编辑器*

master：默认分支    
origin：默认远程版本库
head：默认开发分支
head^(head~1): head的 最新commit版本

------------


## 一：创建版本库
1. 克隆远程项目到本地（自己创建git-command文件夹）
>git clone https://github.com/zhouffan/git-command.git

2. 初始化本地版本库
> git init

## 二：修改和提交
1. 查看状态
> git status

2. 查看变更内容
> git diff

3. 跟踪所有文件( 产生关联即为**跟踪** )
> git add .

4. 跟踪单个文件
> git add xxx.txt

5. 文件改名
> git mv xxx.txt yyy.txt

6. 删除文件（也停止跟踪了） (输入git rm xxx.txt会有提示)
> git rm** -f** xxx.txt

7. **停止跟踪文件**（不删除）
> git rm **--cache** xxx.txt

8. 提交更新的文件
> git commit -m 'add xxx.txt'

9. 修改最后一次提交（**修改注释**）
> **git commit --amend**


注1: 提交可累加多次（by 2 commits），最后一起 push 到远端
```shell
zhouffandeMacBook-Pro:git-command zhouffan$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
zhouffandeMacBook-Pro:git-command zhouffan$ git push
```
## 三：撤销

1.   **撤回添加** (有提示)----与 git add对立
 > git restore --staged  xxx.txt

2. 撤回commit
	HEAD^(HEAD~1或者HEAD~2/3/4...)
	**--soft**  （--mixed ）：仅删除commit， 不撤销修改代码，不撤销add .
	**--hard**: 删除commit，删除改动代码，撤销 add.， 恢复到上次commit状态(**慎用，修改都没有了**)
 > git **reset** --soft HEAD^
    git reset --hard HEAD~2     #回退到上上版本，且**删除所有修改**

3. 撤销 所有修改（在commit前操作前， 之后需要先执行reset）
> git checkout head xxx.txt

4. 撤销已经提交的指定版本 ([git revert 的操作](https://www.jianshu.com/p/5e7ee87241e4 "git revert 的操作"))
> git revert 'commit'

## 四：查看提交历史
1. 查看提交历史 (commit id)
> git log

2. 查看简单的提交历史 (https://www.cnblogs.com/ZnCl/p/11000403.html)
> git log  --oneline
> git log --pretty=oneline

3. **查看提交线**
> **git log --graph --pretty=oneline --abbrev-commit**

4. 查看**所有**分支的所有操作记录（包括已经**被删除**的 commit 记录和 reset 的操作）
> git reflog
## 五：分支与标签

1. 显示所有本地分支/标签
> git branch/tag

2. 创建本地分支/标签
> git branch/tag xxx

3. 删除本地分支/标签
> git branch/tag -d xxx

4.  切换分支（标签）
> git checkout 'branch/tag'
> git checkout xxx

## 六：合并/衍合
 1. 合并指定分支到当前分支
 > git merge 'branch'

 2. 衍合指定分支到当前分支([使用](https://www.liaoxuefeng.com/wiki/896043488029600/1216289527823648 "使用"))  ：“变基”
 > git rebase 'branch'
- 使用场景：本地多次commit（未push），当远程已有提交。首先pull，执行（git log --graph --pretty=oneline --abbrev-commit）会发现，本地commit还是基于之前，并没有基于其他人已经提交到远程的（并且有分叉）。此时执行git rebase，会发现本地commit是基于其他人已经提交的（且没有分叉）。最后push。
- rebase：可以把本地未push的分叉提交历史整理成直线；
- rebase：使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

## 七：远程操作

1. 查看远程版本库地址
 > git remote -v

2. 查看指定远程版本库信息
> git remote show xxx

3. 添加远程版本库
> git remote add 'remote' 'url'

4. 从远程库获取代码
> git fetch 'remote'

5. 下载代码及快速合并
> git pull 'remote' 'branch'

6. 上传代码及快速合并
> git push 'remote' 'branch'

7. 删除远程分支/标签
> git push 'remote' 'branch/tag'

8. 上传所有标签
> git push --tags

9. **拉去远程分支到本地**

> git checkout -b test /origin/test      (git branch -a)


------------


------------


## 使用总结

1. 在分支branch下自由切换后，需要打tag，直接执行git tag xxx。得到一系列tag本地版本，最后进行git push --tags， 上传到远端。(分支切换时，需要上传完当前修改)

2.  --global: 本地所有的Git仓库配置, 也可以指定某个仓库
	> git config --global user.name "Your Name"
	> git config --global user.email "email@example.com"
	
 - git config --system --list : 查看系统config
 - git config --global  --list  :**查看当前用户配置**
 - git config --local  --list    :查看当前仓库配置信息

3. 上传**本地已有项目**到github（同时远端也新建好了项目）
	- cd xxxx
	- git init
	- git add .
	- git commit -m ‘xxx’
	//关联到远程库
	- git remote add origin https://xxxxx
	//获取远程库与本地同步合并（如果远程库不为空必须做这一步，否则后面的提交会失败）
	- git pull —rebase origin master
	//把本地库的内容推送到远端，实际上把当前分支master推送到远程。
	- git push -u origin master 







