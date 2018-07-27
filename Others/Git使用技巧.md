# Git使用技巧

## 编辑器设置

git config --global core.editor "'C:\Program Files (x86)\Notepad++\notepad++.exe' -multiInst -notabbar -nosession -noPlugin '$*'"

## 修改最后一次commit的信息

* `git commit --amend` 弹出Notepad++编辑器
* 在编辑器中修改commit信息
* 关闭编辑器，git自动将原来的commit信息替换为新的commit信息。注意：此时的commitId不是原来的了。

## 合并多个commit

### [参考链接](https://www.jianshu.com/p/964de879904a)

### 需求

假如有3个commit(commitId1/commitId2/commitId3)，现在需要将后两个合并成一个commit

### 步骤

1. `git rebase -i <commitId1>`
2. Git会自动打开Notepad++，将`<commitId2>`和`<commitId3>`合并为一个commit
   * `pick <commitId2> <message2>` 保留commit2
   * `squash <commitId3> <message3>` 将此commit合并到前一个commit。
   * 保存并关闭Notepad++
3. Git会自动再次打开Notepad++，显示message2和message3信息。
   * 将commit信息更改为你需要的commit信息。
   * 保存并关闭Notepad++
4. `git log` 会看到后两个commit合并为了一个

## 新功能完成后在主分支只有一次提交

### 需求

* 基于master完成一个feature
* 在完成feature的过程中，master也有更新
* 在feature做完后，已经将所有feature的commit合并为一个
* 现在需要将新的feature merge到更新了的master上。

### 步骤

1. 在*feature分支*上执行：
    * `git rebase master feature` 由于master其他人有可能进行了更改，所以需要将feature rebase到master上
2. 可以看到现在的分支为*feature|rebase* 。如果有冲突，执行：
    * 修改冲突文件
    * 执行`git add xx.js`
    * 执行 `git rebase --continue`
3. 可以看到又回到了*feature分支*。该分支包含的内容次序为：`初始master提交点`←`master新的提交点`←`feature的提交点`
4. 切换到*master分支*： `git checkout master`
5. 将*feature分支*merge到*master分支*：`git merge feature`
6. `git log`查看到master的提交记录为：`初始master提交点`←`master新的提交点`←`feature的提交点`
