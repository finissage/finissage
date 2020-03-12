
# Git 教程
git 的三种状态  
`commited`:已提交，表示该文件已经被安全保存在本地数据库中。  
`modified`:已修改，表示改了某个文件  
`staged`:已暂存，表示已经修改的文件放在下次提交要保存的清单中  

## 配置文件
> 配置文件分三种,对应不同的文件  
> 系统 `--system` 指的是 `/etc/gitconfig`  
> 全局 `--global` 指的是 `~/gitconfig`  
> 本项目 `--local` 指的是 `.git/config`  

## 查看配置文件

> 本项目所有  
`git config --local --list`

> 单个查看 查看本项目配置的用户名   
`git config --local user.name `

## 添加及修改配置
> 第一次设置是添加，重复添加是修改 

> 设置用户名  
`git config --local user.name 'username'`

> 设置邮箱  
`git config --local user.email 'email@gmail.com'`

> 设置文本编辑器(需提前安装 vim 工具)  
`git config --local core.editor vim`

> 设置对比差异工具(需提前安装 vimdiff 工具)  
`git config --local merge.tool vimdiff`

> 设置记住密码  
`git config credential.helper store`

> 设置别名(git br 代替 git branch)  
`git config alias.br branch`

## 开发常用操作


> 克隆代码  
`git clone url 文件夹名称  `

> 创建本地分支用于跟踪远程仓库中的`master`分支。  
`git pull`  
*目的是从原始克隆的远端仓库中抓取数据后，并合并到工作目录当前分支。*  

> 查看文件修改状态  
`git diff filename `

> 查看本地仓库状态  
`git status `

> 工作区添加到暂存区  
`git add filename  `

> 跳过 add 直接提交工作区的文件  
`git commit -a -m '提交描述'`  

> 撤销（已修改 -> 修改前）  
`git reset HEAD fileName`

## 分支管理  
> 查看所有分支  
`git branch -a`

> 新建分支  
`git branch newBranhName`

> 切换分支  
`git checkout brancName`

> 创建并切换分支  
`git checkout -b newBranchName`

> 合并分支(主分支操作, 将开发分支合并到主分支)  
`git merge 主分支 开发分支`

> 创建远程分支 (将本地分支推送到远程)
`git push origin branchName`

> 关联远程分支（本地和远程分支都存在）
`git branch --set-upstream-to=originBeanchName`
`git branch -u originBeanckName` 

> 删除分支  
`git branch -d branchName`

> 强制删除本地分支
`git branch -D branchName`

> 删除远程分支
`git push origin --delete originBranchName`