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

- 
