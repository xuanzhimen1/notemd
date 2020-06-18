## 5. 分支

### 5.1 分支创建切换合并删除

- 创建新分支

  ```bash
  git branch testing
  ```

- 切换分支

  ```bash
  git checkout testing
  ```

- 创建并切换分支

  ```bash
  git checkout -b testing
  ```

- 分支的合并

  ```bash
  # 将testing分支合并到master分支
  git checkout master
  git merge testing master
  # 三方合并冲突解决方法
  # 1. 手动合并有冲突的文件并执行如下操作
  git add  # 对每个冲突文件执行该操作
  # 1. 或使用tool解决冲突,冲突解决git自动询问是不解决了冲突
  git mergetool  
  # 2. 提交merge信息
  git commit
  ```

- 删除分支

  ```bash
  git branch -d testing
  git branch -D testing  # 强制删除分支并丢掉分支上的所有工作
  ```

### 5.2 远程分支

- 查看远程分支的信息

  ```bash
  git remote -v
  git ls-remote  # 查看远程仓库的完整列表
  git remote show (remote)  # 获取远程分支的详细信息
  ```

- 推送分支到远程(如果分支已存在则将本地最新内容推送到远程)

  ```bash
  git push origin hotfix  # 推送本地hotfix为远程的hotfix分支
  git push origin hotfix:fix  # 推送本地分支到远程重命名为fix
  ```

- 从远程拉取分支(并检出代码到本地,并设置为跟踪分支)

  ```bash
  git checkout -b [branch] [remotename]/[branch]  # 本地branch与远程branch可不同名
  # 快捷方式
  git checkout --track origin/serverfix
  ```

- 拉取

  ```bash
  git fetch origin  # origin为服务器别名
  git pull  # 相当于git getch + git merge (不推荐)
  ```

- 跟踪分支

  ```bash
  # 查看跟踪分支
  git branch -vv
  ```

- 删除远程分支

  ```bash
  git push origin --delete serverfix
  ```