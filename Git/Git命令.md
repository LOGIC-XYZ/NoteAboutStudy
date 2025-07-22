![[../attachments/Snipaste_2025-07-22_14-34-26.png]]

## 添加修改到暂存区
```sh
git add <file-name>     添加指定文件
git add .     添加所有已被Git管理的文件的更改，不包括未追踪的文件（包括忽略的文件）
git add -A     添加所有已被Git管理的文件的更改，包括未追踪的文件
```

## 提交修改到本地仓库
```sh
git commit -a     将所有已被Git管理的文件的修改添加到暂存区并提交
git commit     提交 git add 所添加的更改
git commit -m ' <commit-message> '     附加信息（message）提交
```

## 更新与推送
```sh
git fetch <remote>     从远程仓库下载最新的提交和数据，但不会自动合并到本地分支（主要用于查看）
git pull origin <branch>     从远程仓库拉取最新的提交，并合并到本地分支
git pull --rebase <remote> <branch>     合并远程分支，用 rebase 方式，使提交历史更整洁
git push <remote> <branch>     将本地提交推送至远程仓库指定分支
git push <remote> --delete <branch>     删除远程仓库上的指定分支
git push --tags     将本地所有标签推送至远程仓库（使在远程仓库可访问）
```

## 查看信息
```sh
git status     显示当前工作目录下已修改的文件，包括已暂存与未暂存
git diff     显示当前工作目录与上次提交版本中不同的地方
git log     显示当前分支的所有提交历史
git log --author=“username”     显示某个用户的所有提交历史
git log -p <file>     显示特定文件的所有修改历史
```

## 显示搜索内容
```sh
git grep “<文本>”     从当前目录的所有文件中查找文本内容
git grep “<文本>” V<版本号>     在指定版本中查找
```

## 分支
1. 增删查
```sh
git branch     列出所有分支
git branch -r     列出所有远端分支
git branch <new-branch>     基于当前分支创建新分支
git branch --track <new-branch><remote-branch>     基于远程分支创建一个新本地分支，并使其与远程分支保持追踪关系
git branch -d <branch>     删除本地分支（未合并则不会）
git branch -D <branch>     删除本地分支（未合并也会）
```
2. 切换分支
```sh
git checkout <branch>     切换到指定分支
git checkout -b <branch>     创建新分支并切换
```
3. 标签
```sh
git tag <tag-name>     为当前提交打标签
git tag -a <tag-name>     为当前提交打标签并附加消息
```

## 储藏
存草稿，在切换分支时暂时保存未完成的工作，不必立即提交这些更改
stash@{n} - 相关描述信息（stash为标识符）
```sh
git stash     将当前工作目录中所有未提交的更改储藏起来
git stash list     查看草稿列表
git stash drop stash@{n}     删除指定草稿，不指定就是删除最新
git stash apply stash@{n}     将指定草稿应用到当前工作目录，但不删除
git stash pop stash@{n}     应用并删除这个草稿
```

## 撤销更改（慎用）
1. 撤销本地修改
```sh
git reset HEAD     将暂存区的所有文件移出
git reset --hard <commit>    永久删除未提交的更改
git reset --hard <remote/branch>     远程分支强制覆盖本地分支
git checkout HEAD <file>     将指定文件在工作目录中的修改还原为上一次提交时的状态
```
2. 删除添加到 .gitignore 文件前错误提交的文件
```sh
1. git rm -r --cached .
2. git add .
3. git commit -m 'remove file'
```
3. 撤销远程修改（创建一个新的提交，撤销指定提交的更改）
```sh
git revert <commit-hash>
```
4. 彻底删除指定版本（将指定提交后的提交彻底删除）
```sh
1. git reset --hard <commit-hash>
2. git push -f
```

## 关于配置（config）
1. 配置 pull 的时候默认 rebase （把远程的提交 pull 下来，把本地的提交接在远程提交后，处理本地提交与远程提交有冲突的时候）
```sh
git config pull.rebase true
```


ps:应该还有一些，以后用到再添加（似乎还需要一份关于配置ssh连接的总结，下次搞虚拟机的时候再总结一下吧，摆！）