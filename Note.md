# Git or Github

## 初始化
`git init` 将当前目录初始化为仓库，让目录具备版本管理的能力
```BASH
Initialized empty Git repository in D:/ttt/github_git/.git/
```

## 仓库状态
`git status` 查看仓库状态
```BASH
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Note.md
        README.md
        index.html

nothing added to commit but untracked files present (use "git add" to track)
```

## 将文件添加到暂存区
`git add index.html` 将文件添加到暂存区
```BASH
PS git status        
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   index.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Note.md
        README.md
```

## 设置提交人与邮箱
* 只需设置一次
`git config --global user.name "xxx"`
`git config --global user.email "xxx@outlook.com"`

## 提交代码
`git commit -m "add index.html file"`
```BASH
[master (root-commit) 108f014] add index.html file
 1 file changed, 268 insertions(+)
 create mode 100644 index.html
```

## 查看提交历史
`git log` 显示所有提交
`git log --oneline` 每个提交只占一行
```BASH
commit 108f01476eb9cfde3d91e65994c1a17da797cb7c (HEAD -> master)
Author: kerngzhang <kerngzhang@outlook.com>
Date:   Tue Oct 13 20:58:57 2026 +0800

    add index.html file

git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Note.md
        README.md

nothing added to commit but untracked files present (use "git add" to track)
```

## 查看变更差异
`git diff`
按`Q`退出差异预览

## 回滚
### 改动只在工作区
- staged 己经放进暂存区
- unstaged 未放进暂存区
```BASH
git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   index.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Note.md
```

`git restore <file>...` 可以撤消指定文件在工作区的改动
`git restore .` 可以撤消当前目录下的所有改动

### 改动在暂存区
```BASH
git status        
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   index.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Note.md
```
`git restore --staged <file>...` 回滚指定文件到工作区
`git restore --staged .` 回滚当前所有目录修改到工作区

### 改动在提交区
```BASH
git status                             
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Note.md

nothing added to commit but untracked files present (use "git add" to track)
```

`git reset --hard HEAD~1`

- `reset` 重置
- `--hard` 三个区域一起回滚
- `HEAD` 最新的提交对应`HEAD -> master`
- `HEAD~1` 上一次的提交

* 三种模式
* `--hard` 回滚工作、暂存与提交区
* `--mixed` 保留工作区，回滚暂存与提交区 **默认模式**
* `--soft` 保留工作区与暂存区，回滚提交区

### 回滚Commit提交
`git revert HEAD`
`:WQ`退出

## 忽略内容
`.gitignore`

## 分支与合并
### 分支
1. 创建分支
命令 `git branch feature/due-date` 其中`feature/due-date`是分支名

2. 列出分支
`git branch `显示所有分支
```BASH
feature/due-date
* master
```

3. 切换到新的分支
`git switch feature/due-date`
```BASH
* feature/due-date
  master
```

4. 创建并切换到新分支
`git switch -c feature/due-date` 
参数
- `-c` : 创建新分支Create

另一种老一点的方式
`git checkout -b feature/due-date`
作用
- 创建并切换分支`git checkout -b`

5. 合并分支
把别的分支合并到当前分支
- 如果要合并到`master`分支，需要先切换到`master`分支
- `git merge feature/due-date` 合并分支

6. 删除分支
`git branch -d feature/due-date`
`-d` : delete

7. 处理合并冲突
- 创建两个不同的支持修改同一个位置
`git branch -c feature/sort`
`git branch -c feature/all-done`
```BASH
git status                
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   index.html

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        Note.md

no changes added to commit (use "git add" and/or "git commit -a")
```
处理冲突后再执行`git commit`提交冲突合并

8. 提前处理冲突，将分支移到master的最前方 `rebase`
`git rebase master`
`git add index.html`
`git rebase --continue`
`:wq`即出rebase
再切换到主分支
合并分支


## Stash存储区
将改动暂存到Stash，把没有写完的代码先收起来，适用于临时打很快回来的情况
`git stash`将放在工作区或暂存区的还未提交的改动保存起来
`git stash pop`取出有改动但没有暂存的内容取出来

## worktree
所有的Worktree都有各自的工作区与暂存区，但是会共享提交区
`git worktree add -b feature/status ../kerngTodo master`
`git worktree list`
如果list有乱码，可执行`git config --global core.quotePath false`
`git worktree remove ../kerngTodo` 删除不用的WorkTree


## Github
### 配置 `SSH KEY`
`ssh-keygen -t ed25519 -c "xxx@some.com"`
- `-t ed25519` : 指定Key的类型
- `-C XXX` : 备注，一般为自己的邮箱

`ssh -T git@github.com` 尝试连线Github

### 创建Github仓库
在Web中创建仓库

### 推送到运程仓库
```BASH
git remote add origin https://github.com/KerngTS/git_demo.git
git branch -M main 将主分支的名字改为main
git push -u origin main
```

`-u origin main` 将本地与远端的main绑定，以后只需要`git push`就可以


### 拉取代码 
`git clone`