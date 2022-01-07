# Manjaro安装配置

## 安装

- 制作USB安装盘

- USB启动并安装

## 配置

- 安装git
  
  - 略（系统默认已安装）

- 输入法
  
  - fcitx
    
    ```shell
    # 安装输入法框架
    sudo pacman -S fcitx5-im
    # 安装中文输入法
    sudo pacman -S fcitx5-chinese-addons fcitx5-rime
    # 配置
    vi /etc/environment
    # 详情如下
    ###
    GTK_IM_MODULE=fcitx
    QT_IM_MODULE=fcitx
    XMODIFIERS=@im=fcitx
    SDL_IM_MODULE=fcitx
    ###
    ```

- zsh
  
  ```shell
  # 安装zsh
  sudo pacman -S zsh
  # 设置zsh为默认shell
  chsh -s /bin/zsh # 注：重启后才生效
  # 查看系统中存在的shell
  chsh -l
  ```

- oh-my-zsh
  
  - 根据官网命令安装（需要提前安装好git）
  
  - 安装`zsh-syntax-highlighting`和`zsh-autosuggestions`插件
  
  - 编辑`.zshrc`文件
    
    - 修改主题
    
    - 启用插件

- java
  
  - 下载并解压`jdk`
  
  - 配置
    
    ```shell
    # 编辑/etc/profile
    sudo vi /etc/profile # 注：只读权限无法修改
    # 详情如下
    ###
    # 设置JAVA_HOME
    export JAVA_HOME="/home/abs/path/jdk/home" # 注：需要绝对路径
    # 设置CLASS_PATH
    export CLASS_PATH=".:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib"
    # 设置PATH
    export PATH=$PATH:$JAVA_HOME/bin
    ###
    # 热更新
    source /etc/profile
    ```

- maven
  
  - 下载并解压`maven`
  
  - 配置
    
    ```shell
    # 编辑/etc/profile
    sudo vi /etc/profile # 注：只读权限无法修改
    # 详情如下
    ###
    # 设置M2_HOME
    export M2_HOME="/home/abs/path/maven/home" # 注：需要绝对路径
    # 设置PATH
    export PATH=$PATH:$M2_HOME/bin
    ###
    # 热更新
    source /etc/profile
    ```

- emcas
  
  - 安装
    
    ```shell
    sudo pacman -S emacs
    ```
  
  - 配置
    
    - 克隆我的配置库
    
    - 创建相关路径
    
    - 修改`linux.el`中相关的配置
    
    - 在`lsp-java`的GItHub页面下载`eclipse.jdt.ls`并解压到指定位置
    
    - 下载`lombok.jar`并放在指定位置
    
    - 创建`custom.el`文件
    
    - 命令行运行
      
      - 直接运行
        
        ```shell
        emacs -nw
        ```
      
      - 自定义脚本运行
        
        ```shell
        # 创建脚本emacsx
        vi emacsx
        # 详情如下
        ###
        #! /bin/env bash
        emacs -nw $*
        ###
        # 添加可执行权限
        chmod +x emacsx
        # 放到/usr/local/sbin/中
        mv emacsx /usr/local/sbin/
        # 在命令行中打开emacs时使用emacsx命令即可
        emacsx abc.txt
        ```
    
    - **字体配置**
      
      - 外观
      
      - 窗口管理器
      
      - LightDM桌面管理器设置
      
      - 终端（好像识别不了自己安装的字体）
      
      > 需要特别说明的是如果设置自定义字体的时候一定要保证设置的字体没有连接符`-`，如`苹方-简`这个字体会导致`emacs`的桌面程序（GUI）启动时卡死；而设置成`苹方`就可以正常启动了。
      > 
      > 如果必须使用有`-`的字体，可按下面设置解决字体问题（**未验证**）：
      > 
      > ```emacs-lisp
      > (setq initial-frame-alist '((font ."Monospace-10")))
      > (setq default-frame-alist '((font ."Monospace-10")))
      > ```
      > 
      > 另外：由于`xfce4`的默认终端使用的是默认字体，使用`emacs -nw`可以正常启动`emacs`。
    
    - 

- yay
  
  - 配置源
    
    ```shell
    # 编辑/etc/pacman.conf
    sudo emacsx /etc/pacman.conf
    # 详情如下
    ###
    [archlinuxcn]
    Server=https://repo.archlinuxcn.org/$arch
    ###
    # 更新并安装信任密钥
    sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring
    ```
  
  - 配置镜像源
    
    ```shell
    # 安装镜像源
    sudo pacman -S archlinuxcn-mirrorlist-git
    # 编辑/etc/pacman.conf
    sudo emacsx /etc/pacman.con
    # 详情如下
    ###
    [archlinuxcn]
    Include=/etc/pacman.d/archlinuxcn-mirrorlist
    ###
    # 编辑使用的镜像列表
    sudo emacsx /etc/pacman.d/archlinuxcn-mirrorlist # 注：把要启用的源注释打开即可
    # 详情如下
    ###
    ##
    ## Arch Linux CN community repository mirrorlist
    ## Generated on 2021-08-17
    ##
    
    ## Our main server (Amsterdam, the Netherlands) (ipv4, ipv6, http, https)
    Server = https://repo.archlinuxcn.org/$arch
    
    ## OpenTUNA (China CDN) (ipv4, https)
    Server = https://opentuna.cn/archlinuxcn/$arch
    
    ## 北京外国语大学 (北京) (ipv4, ipv6, http, https)
    Server = https://mirrors.bfsu.edu.cn/archlinuxcn/$arch
    
    ## 腾讯云 (Global CDN) (ipv4, http, https)
    Server = https://mirrors.cloud.tencent.com/archlinuxcn/$arch
    
    ## 网易 (China CDN) (ipv4, http, https)
    Server = https://mirrors.163.com/archlinux-cn/$arch
    
    ## 阿里云 (Global CDN) (ipv4, http, https)
    Server = https://mirrors.aliyun.com/archlinuxcn/$arch
    
    ## 华为云 (Global CDN) (ipv4, http, https)
    Server = https://repo.huaweicloud.com/archlinuxcn/$arch
    
    ## 清华大学 (北京) (ipv4, ipv6, http, https)
    Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
    
    ## 中国科学技术大学 (安徽合肥) (ipv4, ipv6, http, https)
    Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
    
    ## SJTUG 软件源镜像服务 (上海) (ipv4, https)
    Server = https://mirrors.sjtug.sjtu.edu.cn/archlinux-cn/$arch
    
    ###
    ```
  
  - 安装yay
    
    ```shell
    sudo pacman -S --needed git base-devel yay
    ```
  
  - 使用yay
    
    ```shell
    # 相看相关的安装包
    yay -Ss abc
    # 安装
    yay -S abc
    ```

- deepin-wine-wechat
  
  ```shell
  # 安装deepin-wine-wechat
  yay -S deepin-wine-wechat # 注：在xfce需要安装并自启动xsettingsd
  # 安装xsettingsd
  sudo pacman -S xsettingsd
  # 在设置->会话与启动->应用程序自启动中添加xsettingsd并重启
  ```

- WPS
  
  ```shell
  # 安装WPS
  yay -S wps-office-cn
  ```

- MarkText
  
  ```shell
  # 安装
  yay -S marktext
  ```

- Remmina
  
  - 在系统自带的软件包管理器中搜索并安装`Remmina`
  
  - 选装`freerdp`组件，可以使用`rdp`协议远程`Windows`桌面

- FireFox
  
  - 官网下载国际版并添加中文语言包
  
  - 创建一个桌面快捷方式

- 安装字体
  
  ```shell
  # 下载要安装的字体到custom-fonts中
  # 复制到/usr/share/fonts下
  sudo cp -r custom-fonts /usr/share/fonts/custom-fonts
  # 查看已安装的字体
  fc-list
  # 查看已安装的中文字体
  fc-list :lang=zh
  # 重建字体索引，更新字体缓存
  sudo mkfontscale # 注：好像非必需
  sudo mkfontdir # 注：好像非必需
  sudo fc-cache -fv
  # 重新登录后生效
  ```

- 安装截图工具`flameshot`
  
  - 安装
    
    ```shell
    # 官方GitHub安装方法
    pacman -S flameshot # 注：我这里找不到
    # 使用yay安装
    yay -S flameshot
    ```
  
  - 配置
    
    - 贴图绑定F3
    
    - 截图绑定F1
      
      - 1. Go to `Keyboard` settings
        
        2. Switch to the tab `Application Shortcuts`
        
        3. Add new entry `flameshot gui` binding `F1`

- 安装`Edge`
  
  - 安装
    
    ```shell
    yay -S microsoft-edge-stable-bin
    # yay -S google-chrome # 安装Chrome
    ```
  
  - 配置
    
    - 禁止跟踪
    
    - 清除浏览记录

- 
