git 常见命令
----

# 列出所有本地分支
    $ git branch

# 列出所有远程分支
    $ git branch -r

# 列出所有本地分支和远程分支
    $ git branch -a

# 新建一个分支，但依然停留在当前分支
    $ git branch [branch-name]

# 新建一个分支，并切换到该分支
    $ git checkout -b [branch]

# 新建一个分支，指向指定commit
    $ git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
    $ git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
    $ git checkout [branch-name]

# 切换到上一个分支
    $ git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
    $ git branch --set-upstream [branch] [remote-branch]

# 合并指定分支到当前分支
    $ git merge [branch]

# 选择一个commit，合并进当前分支
    $ git cherry-pick [commit]

# 删除分支
    $ git branch -d [branch-name]

# 删除远程分支
    $ git push origin --delete [branch-name]
    $ git branch -dr [remote/branch]


撤销
___

# 恢复暂存区的指定文件到工作区
    $ git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
    $ git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
    $ git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
    $ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
    $ git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
    $ git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
    $ git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
    $ git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
    $ git revert [commit]

# 暂时将未提交的变化移除，稍后再移入
    $ git stash
    $ git stash pop