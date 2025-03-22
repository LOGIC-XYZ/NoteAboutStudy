## 添加修改到暂存区
```
git add 文件名     添加指定文件
git add .     添加所有已被Git管理的文件的更改，不包括未追踪的文件（包括忽略的文件）
git add -A     添加所有已被Git管理的文件的更改，包括未追踪的文件
```

## 提交修改到本地仓库
```
git commit -a     将所有已被Git管理的文件的修改添加到暂存区并提交
git commit     提交 git add 所添加的更改
git commit -m ' (commit message) '     附加信息（message）提交
```

## 储藏
存草稿，在切换分支时暂时保存未完成的工作，不必立即提交这些更改
stash@{n} - 相关描述信息（stash为标识符）
```
git stash     将当前工作目录中所有未提交的更改储藏起来
git stash list     查看草稿列表
git stash drop stash@{n}     删除指定草稿，不指定就是删除最新
git stash apply stash@{n}     将指定草稿应用到当前工作目录，但不删除
git stash pop stash@{n}     应用并删除这个草稿
```

## 撤销更改（慎用）
1. 撤销本地修改
```
git reset HEAD     将暂存区的所有文件移出
git reset --hard <commit>    永久删除未提交的更改
git reset --hard <remote/branch>     远程分支强制覆盖本地分支
git checkout HEAD <file>     将指定文件在工作目录中的修改还原为上一次提交时的状态
```
2. 删除添加到 .gitignore 文件前错误提交的文件
```
1. git rm -r --cached .
2. git add .
3. git commit -m 'remove file'
```
3. 撤销远程修改（创建一个新的提交，撤销指定提交的更改）
```
git revert <commit-hash>
```
4. 彻底删除指定版本（将指定提交后的提交彻底删除）
```
1. git reset --hard <commit-hash>
2. git push -f
```
## 更新与推送
```
git fetch <remote>     从远程仓库下载最新的提交和数据，但不会自动合并到本地分支（主要用于查看）
git pull origin 分支名     从远程仓库拉取最新的提交，并合并到本地分支
git pull --rebase <remote> <branch>     合并远程分支，用 rebase 方式，使提交历史更整洁
git push <remote> <branch>     将本地提交推送至远程仓库指定分支
git push <remote> --delete <branch>     删除远程仓库上的指定分支
git push --tags     将本地所有标签推送至远程仓库（使在远程仓库可访问）
```

## 查看信息
```

```

## 显示搜索内容
```

```

## 分支
```

```