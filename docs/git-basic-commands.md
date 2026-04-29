# Git 常用指令指南

> 目标：记录 Git 日常高频命令。重点是“能查、能用、能理解基本流程”。

## 1. 基本概念

```text
workspace     工作区：你正在编辑的文件
staging area  暂存区：git add 后的文件
repository    本地仓库：git commit 后的版本历史
remote         远程仓库：GitHub / GitLab 上的仓库
branch         分支：一条独立开发线
commit         提交：一次版本快照
merge          合并：把一个分支的改动合到另一个分支
```

常见流程：

```text
修改文件 -> git add -> git commit -> git push
```

## 2. 配置 Git

查看配置：

```bash
git config --list
```

设置用户名和邮箱：

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

查看用户名和邮箱：

```bash
git config --global user.name
git config --global user.email
```

## 3. 初始化仓库

在当前目录初始化：

```bash
git init
```

设置主分支为 `main`：

```bash
git branch -M main
```

添加远程仓库：

```bash
git remote add origin https://github.com/username/repo.git
```

首次提交并推送：

```bash
git add .
git commit -m "init project"
git push -u origin main
```

从 GitHub 克隆仓库：

```bash
git clone https://github.com/username/repo.git
```

## 4. 查看状态和历史

查看当前状态：

```bash
git status
```

简洁查看状态：

```bash
git status -sb
```

查看提交历史：

```bash
git log
```

单行历史：

```bash
git log --oneline
```

查看最近 5 次提交：

```bash
git log --oneline -5
```

查看某次提交详情：

```bash
git show <commit-id>
```

## 5. 添加和提交

添加所有改动：

```bash
git add .
```

添加指定文件：

```bash
git add README.md
```

添加指定目录：

```bash
git add docs/
```

提交：

```bash
git commit -m "add git guide"
```

修改最近一次提交信息：

```bash
git commit --amend -m "new commit message"
```

注意：已经 push 到远程的 commit 不要随便 amend，除非你明确知道影响。

## 6. 查看文件差异

查看工作区未暂存改动：

```bash
git diff
```

查看已暂存改动：

```bash
git diff --staged
```

查看两个分支差异：

```bash
git diff main feature-branch
```

查看某个文件差异：

```bash
git diff README.md
```

## 7. 远程仓库

查看远程仓库：

```bash
git remote -v
```

添加远程仓库：

```bash
git remote add origin https://github.com/username/repo.git
```

修改远程仓库地址：

```bash
git remote set-url origin https://github.com/username/repo.git
```

删除远程仓库：

```bash
git remote remove origin
```

从远程拉取：

```bash
git pull
```

指定分支拉取：

```bash
git pull origin main
```

推送：

```bash
git push
```

首次推送并建立 upstream：

```bash
git push -u origin main
```

获取远程信息但不合并：

```bash
git fetch
```

## 8. 分支 Branch

查看本地分支：

```bash
git branch
```

查看本地和远程分支：

```bash
git branch -a
```

创建分支：

```bash
git branch feature-rag
```

切换分支：

```bash
git switch feature-rag
```

创建并切换分支：

```bash
git switch -c feature-rag
```

旧写法：

```bash
git checkout -b feature-rag
git checkout main
```

重命名当前分支：

```bash
git branch -m new-branch-name
```

删除本地分支：

```bash
git branch -d feature-rag
```

强制删除本地分支：

```bash
git branch -D feature-rag
```

删除远程分支：

```bash
git push origin --delete feature-rag
```

## 9. 分支合并 Merge

把 `feature-rag` 合并到 `main`：

```bash
git switch main
git pull
git merge feature-rag
git push
```

常见开发流程：

```bash
git switch main
git pull
git switch -c feature-rag

# 修改文件
git add .
git commit -m "add rag feature"

git switch main
git pull
git merge feature-rag
git push
```

如果合并冲突：

```bash
git status
```

打开冲突文件，删除冲突标记：

```text
<<<<<<< HEAD
当前分支内容
=======
要合并进来的分支内容
>>>>>>> feature-rag
```

解决后：

```bash
git add .
git commit -m "resolve merge conflict"
git push
```

取消一次正在进行的 merge：

```bash
git merge --abort
```

## 10. Rebase

把当前分支基于最新 `main` 重放：

```bash
git switch feature-rag
git fetch origin
git rebase origin/main
```

rebase 适合让提交历史更线性。

如果 rebase 冲突：

```bash
git status
```

解决冲突后：

```bash
git add .
git rebase --continue
```

取消 rebase：

```bash
git rebase --abort
```

注意：

```text
不要随便 rebase 已经推送并且别人也在使用的公共分支。
```
rebase 暂时了解即可。单人学习项目优先使用 merge，不确定时不要 rebase。

## 11. Stash 临时保存

临时保存当前改动：

```bash
git stash
```

带说明保存：

```bash
git stash push -m "work in progress"
```

查看 stash：

```bash
git stash list
```

恢复最近一次 stash：

```bash
git stash pop
```

恢复但不删除 stash：

```bash
git stash apply
```

删除最近一次 stash：

```bash
git stash drop
```

清空所有 stash：

```bash
git stash clear
```

## 12. 撤销和恢复

撤销工作区某个文件的修改：

```bash
git restore README.md
```

撤销所有未暂存修改：

```bash
git restore .
```

取消暂存某个文件：

```bash
git restore --staged README.md
```

取消暂存所有文件：

```bash
git restore --staged .
```

把某个文件恢复到指定提交：

```bash
git restore --source <commit-id> README.md
```

回退最近一次 commit，但保留改动：

```bash
git reset --soft HEAD~1
```

回退最近一次 commit，并取消暂存，但保留工作区改动：

```bash
git reset --mixed HEAD~1
```

危险操作：

```bash
git reset --hard HEAD~1
```

`--hard` 会丢弃改动，使用前必须确认。

## 13. 标签 Tag

创建标签：

```bash
git tag v0.1.0
```

查看标签：

```bash
git tag
```

推送标签：

```bash
git push origin v0.1.0
```

删除本地标签：

```bash
git tag -d v0.1.0
```

删除远程标签：

```bash
git push origin --delete v0.1.0
```

## 14. .gitignore

`.gitignore` 用来忽略不应该提交的文件。

常见内容：

```gitignore
__pycache__/
*.pyc
.env
.venv/
venv/
node_modules/
.DS_Store
dist/
build/
```

如果文件已经被 Git 跟踪，再加入 `.gitignore` 不会自动取消跟踪。

取消跟踪但保留本地文件：

```bash
git rm --cached .env
```

## 15. 代理 Proxy

查看代理：

```bash
git config --global --get http.proxy
git config --global --get https.proxy
```

设置全局代理：

```bash
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

取消全局代理：

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

只给当前仓库设置代理：

```bash
git config http.proxy http://127.0.0.1:7890
git config https.proxy http://127.0.0.1:7890
```

测试 GitHub 连接：

```powershell
Test-NetConnection github.com -Port 443
```

## 16. 常见报错

### `fatal: not a git repository`

当前目录不是 Git 仓库。

处理：

```bash
git init
```

或进入正确的仓库目录。

### `nothing to commit, working tree clean`

没有可提交改动。

检查：

```bash
git status
```

### `src refspec main does not match any`

通常表示还没有任何 commit。

处理：

```bash
git add .
git commit -m "init project"
git push -u origin main
```

### `rejected: fetch first`

远程有本地没有的提交。

处理：

```bash
git pull origin main
git push
```

如果本地和远程是分别初始化的：

```bash
git pull origin main --allow-unrelated-histories
git push
```

### `Failed to connect to github.com port 443`

网络或代理问题。

处理：

```bash
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

## 17. 推荐工作流

### 单人项目

```bash
git pull
git status
git add .
git commit -m "update docs"
git push
```

### 功能分支开发

```bash
git switch main
git pull
git switch -c feature-name

# 修改文件
git add .
git commit -m "add feature"
git push -u origin feature-name
```

然后在 GitHub 上开 Pull Request。

合并后本地更新：

```bash
git switch main
git pull
git branch -d feature-name
```

## 18. 最小必记命令

```bash
git status
git add .
git commit -m "message"
git pull
git push
git branch
git switch -c branch-name
git merge branch-name
git log --oneline
git diff
```

