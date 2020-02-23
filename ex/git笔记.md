- 代码回退
git log # 得到你需要回退一次提交的commit id
git reset --hard <commit_id>  # 回到其中你想要的某个版
或者
git reset --hard HEAD^  # 回到最新的一次提交
或者
git reset HEAD^  # 此时代码保留，回到 git add 之前

git push origin HEAD --force # 强制提交一次，之前错误的提交就从远程仓库删除

- 单独提交某个commit
git cherry-pick 4db0729d