2019-08-06

#### Git 命令

```
//克隆一个项目
git clone [url]

//列出所有本地分支
git branch 

//列出所有本地分支和远程分支
git branch -a

//从远程分支拉取 并切换到该分支
git checkout -b [local-branch-name] [remote-branch-name]

//从远程拉取
git pull

//显示有变更的文件
git status
//精简版显示
git status -s

//暂存
git add
//取消暂存
git reset

//删除文件并移除跟踪
git rm <filename>
//删除跟踪保留文件
git rm --cached <filename>

//删除本地分支
git branch -d <BranchName>
//强制删除本地分支
git branch -D <BranchName>

//删除远程分支
git push origin --delete <BranchName>

//查看本地具体文件改动的明细
git diff fileName

//查看远程仓库地址
git remote -v

//查看本地分支和追踪情况
git remote show origin

//本地同步已删除的远程分支
git remote prune origin
```