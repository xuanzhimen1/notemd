## 1. Git环境

- 配置用户信息

  ```bash
  git config --global user.name "John Doe"
  git config --global user.email johndoe@example.com
  ```

- 修改默认的编辑器

  ```bash
  git config --global core.editor vim
  ```

- 查看配置信息

  ```bash
  git config --list
  ```

- 获取帮助

  ```bash
  git help [command]
  ```

  

## 2. Git仓库初始化

- 初始化git仓库

  ```bash
  git init
  ```

- clone一个过程仓库

  ```bash
  git clone https://github.com/libgit2/libgit2  # 克隆一个名libgit2与远程仓库一样的仓库到本地
  git clone https://github.com/libgit2/libgit2 myGitPrositoryname  # 重命名为myGitPrositoryname
  git clone https://github.com/libgit2/libgit2 -o booyah  # 默认克隆时别名为origin, -o 可手动指定仓库别名
  ```

## 3. Git本地操作命令

### 3.1 查看状态

- 查看状态

  ```bash
  git status  # 查看状态
  git status -s  # 查看状态简写
  ```

### 3.2 暂存与取消暂存

- 跟踪文件或对已跟踪文件进行暂存

  ```bash
  git add *  # 跟踪git仓库中全部示跟踪的文件
  git add README.md  # 跟踪一个文件
  ```

- 取消暂存

  ```bash
  git ss
  ```

  

### 3.3 忽略文件

- `.gitignore`文件中忽略文件的指定方式

  ```bash
  *.a
  !lib.a  # 排除除了lib.a文件的所有以.a为后缀的文件
  build/  # 排除build文件夹下的所有文件
  doc/*.txt  # 排除doc目录下的所有.txt为后缀的文件
  doc/**/*.txt  # 排除doc目录及子目录下的所有.txt为后缀的文件
  ```

  - `/`开头: 防止递归
  - `/`结尾: 指定目录
  - `!`: 取反
  - `[abc]`,`[1-4]`: 可取值: abc, 1234
  - `/**/`: 匹配任意中间目录

### 3.4 查看修改内容

- 查看修改内容

  ```bash
  git diff  # 查看被跟踪文件已暂存和未暂存的修改
  git diff --cached  # 查看被跟踪文件已暂存与仓库的修改
  ```

### 3.5 提交修改

- 提交

  ```bash
  git commit  # 打开git默认的编辑器编辑提交信息
  git commit -a  # 将已跟踪未暂存的文件一并暂存并提交
  git commit -m "commit message"  # 将已暂存的文件提交
  git commit -a -m "commit message"  # 将已跟踪未暂存的文件一并暂存提交
  ```

- 重新提交

  > 提交当前暂存区的内容到上一次提交并重新编写上次提交的提交信息

  ```bash
  git commit --amend
  ```

- git命令提交忽略部分文件

    ```bash
    # 忽略某个/某些文件的提交
    git update-index --assume-unchanged <要忽略的文件夹/文件夹下的文件>  # 示例如下
    git update-index --assume-unchanged .idea/*.xml
    # 重新恢复提交
    git update-index --no-assume-unchanged .idea/*.xml
    ```

### 3.6 移除文件

- 移除文件

  ```bash
  git rm README.md  # 从仓库和本地同时删除README.md
  # 用rm README.md时将在git status时出现changes not staged for commit
  git rm --cached README.md  # 从git仓库中删除,保留本地文件
  ```

### 3.7 移动文件(或重命名文件)

- 移动或重命名

  ```bash
  git mv file_from file_to
  ```

### 3.8 查看提交历史

- 提交历史

  ```bash
  git log  # 查看最近两次的提交
  git log -p  # 显示版本的提交内容
  git log -2  # 查看近两次的提交
  git log --stat  # 显示提交的简略信息
  git log --pretty=oneline  # 在一行显示提交信息
  git log --pretty=format:"%h - %qn, %ar : %s"  # 自定义git log的输出信息
  git log --pretty=format:"%h %s" --graph  # 显示分支的记录
  git log --since=2.weeks  # 2周内 其它格式如：2008-01-15  2 years 1
  day 3 minutes ago
  git log --until=2008-01-15  # 2008-01-15之前
  git log --author="xxx"  # 显示提交者为xxx的提交
  git log --grep="xxx"  # 显示提交信息中包含xxx的提交
  git log -SfunctionName  # 显示提交内容中包含functionName的提交
  ```

- 查看项目分叉历史

  ```bash
  git log --oneline --decorate --graph --all
  ```

### 3.9 撤消操作

- 取消暂存的文件

  ```bash
  git reset HEAD xxx.md
  ```

- 撤消对文件的修改

  > 不可恢复，相当于覆盖

  ```bash
  git checkout -- xxx.md
  ```

### 3.10 Git别名

- 别名

  ```bash
  git config --global alias.co checkout
  git config --global alias.br branch
  git config --global alias.ci commit
  git config --global alias.st status
  git config --global alias.unstage 'reset HEAD --'  # 取消全部的暂存文件
  git config --global alias.last 'log -1 HEAD'
  git config --global alias.visual '!gitk'
  ```

### 3.11 Git标签

- 列出标签

  ```bash
  git tag  # 列出所有的标签
  git tab -l 'v1.8.5*'  # 列出v1.8.5相关的标签
  ```

- 创建标签

  ```bash
  git tag -a v1.4 -m "my version 1.0"
  ```

- 查看标签信息与对应的提交信息

  ```bash
  git show v1.0
  ```

- 后期打标签(给特定的版本打标签)

  ```bash
  git tag -a v1.2 9fceb02
  ```

  > 9fceb02为提交的校验和(或部分校验和: 前几位)

- 推送标签到远程

  ```bash
  git push origin v1.5  # 向远程推送某个tag
  git push origin --tags  # 将本地的所有tag全部推送到远程
  ```

- 删除标签

  ```bash
  git tag -d v1.1  # 删除本地标签
  git push origin :refs/tags/v1.0
  ```