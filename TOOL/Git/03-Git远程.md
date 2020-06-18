## 4. 远程仓库

### 4.1 查看远程仓库

- 查看全部远程仓库

  ```bash
  git remote  # 查看已经配置的远程仓库的简写（origin）
  git remote -v  # 查看已经配置的远程仓库的简写及其URL
  ```

- 查看某个远程仓库

  ```bash
  git remote show origin
  ```

### 4.2 添加远程仓库

- 添加远程仓库

  ```bash
  git remote add <shortname> <url>  # shortname默认为origin
  ```

### 4.3 拉取远程仓库

- 拉取远程仓库的代码

  ```bash
  git fetch origin  # origin为远程仓库简写
  ```

### 4.4 推送到远程

- 推送到远程仓库

  ```bash
  git push orgin master
  ```

### 4.5 重命名远程仓库

- 移除或重命名远程仓库

  ```bash
  git remote rename pb paul  # 将远程仓库pb重命名为paul
  ```

### 4.6 移除远程仓库

- 移除

  ```bash
  git remote rm paul  # 移除某个远程仓库
  ```