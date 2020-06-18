## 1. 网页图标

- 代码

  ```html
  <link rel="shortcut icon" href="favicon.ico" type="image/x-icon"
  ```

- 制作网页图标
  1. 选取一张图片 
  2. http://www.bitbug.net/ 转换为ico格式即可

## 2. 网站SEO优化标签

- 代码

  ```html
  <title>网站名（产品名）- 网站的介绍</title>
  <meta name="description" content="“我们是…”“我们提供…”“×××网作为…”“电话：010…”">
  <meta name="keywords" content="6～8个关键词左右">
  ```

## 3. 使用字体图标

- 下载图标网站

  - [阿里巴巴矢量图标库](http://www.iconfont.cn/)
- [IcoMoon](http://icomoon.io)

- icomoon网站的使用

  - 下载字体包
    1. 进入首页点击 右上角IcoMoonApp进入app 
    2. 选择需要的字体图标， 点击右下角Generate Font
    3. 点击右下角Download 下载选中的图标到本地
    
  - 将字体引入到HTML中
  
    1. 首先将下载的字体包中的fonts文件夹复制到工程根目录下
  
    2. 进入下载的字体包点击demo.html打开，在需要的字体后面点击右侧的``并复制
  
    3. 在html中粘贴复制的内容（如下）
  
       ```html
       <span></span>
       ```
  
    4. 在样式中声明字体（fonts为自定义的字体路径，其它直接复制即可）
  
       ```css
       @font-face {
         font-family: 'icomoon';
         src:  url('fonts/icomoon.eot?7kkyc2');
         src:  url('fonts/icomoon.eot?7kkyc2#iefix') format('embedded-opentype'),
           url('fonts/icomoon.ttf?7kkyc2') format('truetype'),
           url('fonts/icomoon.woff?7kkyc2') format('woff'),
           url('fonts/icomoon.svg?7kkyc2#icomoon') format('svg');
         font-weight: normal;
         font-style: normal;
       }
       ```
  
    5. 给盒子添加字体样式
  
       ```css
       span {
           font-family: "icomoon"  /*与上面font-face中font-family定义的一致*/
       }
       ```
  
  - 追加字体图标到工程的图标库中
    1. 在icomoon的官网点击进入IconMoonApp再点击左上角的`Import Icons`按钮
    2. 选择下载字体包下的`selection.json`文件打开，选择需要的图标重新生成包，并覆盖原来的fonts目录即可

## 4. Logo优化

1. logo的html写法

   ```html
   <div class="logo">
       <h1><a href="index.html"><img src="img/logo.png" alt="品优购" title="品优购">品优购</a></h1>
   </div>
   ```

2. 取消链接中的文字

   - 方法一：文字超出盒子并隐藏

     ``` css
     text-index: -9999px;  /*首行缩进*/
     overflow: hidden  /*超出部分隐藏*/
     ```

   - 方法二：字体宽度设为0

     ```css
     font-size: 0;
     ```









