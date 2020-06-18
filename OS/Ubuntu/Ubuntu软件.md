## 1. 软件汇总

### 1.1 截图软件

- 本机自带
  - PrtSc – 获取整个屏幕的截图并保存到 Pictures 目录。
  - Shift + PrtSc – 获取屏幕的某个区域截图并保存到 Pictures 目录。
  - Alt + PrtSc –获取当前窗口的截图并保存到 Pictures 目录。
  - Ctrl + PrtSc – 获取整个屏幕的截图并存放到剪贴板。
  - Ctrl + Shift + PrtSc – 获取屏幕的某个区域截图并存放到剪贴板。
  - Ctrl + Alt + PrtSc – 获取当前窗口的 截图并存放到剪贴板。

- shutter

  - 安装
  
    ```bash
  sudo apt install shutter
    ```
  
  - 配置快捷键
  
    - 进入系统设置中的“键盘设置”
    - 页面中会列出所有现有的键盘快捷键，拉到底部就会看见一个 “+” 按钮
    - 点击 “+” 按钮添加自定义快捷键并输入以下两个字段：
      - “名称”： 任意名称均可。
      - “命令”： shutter -s
  
  - 使用编辑功能
  
    ```bash
    # 下载安装必要的deb包
    https://launchpad.net/ubuntu/+archive/primary/+files/libgoocanvas-common_1.0.0-1_all.deb
    https://launchpad.net/ubuntu/+archive/primary/+files/libgoocanvas3_1.0.0-1_amd64.deb
    https://launchpad.net/ubuntu/+archive/primary/+files/libgoo-canvas-perl_0.06-2ubuntu3_amd64.deb
    # 安装包时，依赖错误时执行
    sudo apt -f install
    # 注销生效
    ```
  
- flameshot

  ```bash
  sudo apt install flameshot
  ```

  - 进入系统设置中的“键盘设置”
  - 页面中会列出所有现有的键盘快捷键，拉到底部就会看见一个 “+” 按钮
  - 点击 “+” 按钮添加自定义快捷键并输入以下两个字段：
    - “名称”： 任意名称均可。
    - “命令”： /usr/bin/flameshot gui
  - 最后将这个快捷操作绑定到 PrtSc 键上，可能会提示与系统的截图功能相冲突，但可以忽略掉这个警告。

