# Emacs 配置开发 IDE 之路

[GNU Emacs 官方网站](https://www.gnu.org/software/emacs/emacs.html)

## 下载安装

- win

    - 下载 [emacs-27.2-x86_64.zip](https://mirror.us-midwest-1.nexcess.net/gnu/emacs/windows/emacs-27/emacs-27.2-x86_64.zip)

    - 解压到 D:\\emacs
    - 运行 D:\\emacs\bin\runemacs.exe

- linux

    - 略

- mac

    - 略

## 配置

### 默认配置路径说明

> 默认的会找用户目录下的相关配置文件
>
> Linux 有用户目录，可以将`.emacs`和`.emacs.d`放到用户目录下，即`/home/xxx`或`/root/`
>
> Windows 没有 home 目录的概念，所以 Emacs 就按如下方式来查找配置文件：
>
> 1. 如果设置了HOME环境变量，那么就用它的值作为home目录~
> 2. 如果存在注册表键值HKCU\SOFTWARE\GNU\Emacs\HOME，就用它的值作为home目录~ (只针对当前用户)
> 3. 如果存在注册表键值HKLM\SOFTWARE\GNU\Emacs\HOME，就用它的值作为home目录~（针对所有用户）
> 4. 如果存在C:\\.emacs，就用C:\作为home目录~
> 5. 如果以上都不存在的话，就使用<system root>\Users\<user  name>\AppData\Roaming作为home目录~
> 6. 对于XP和较早windows用户，需要到Documents and  Settings目录下去找AppData\Roaming目录

1. 单配置文件`.emacs`，Windows下可以改成`_emacs`

2. 配置目录`.emacs.d/init.el`

3. 新配置目录`.config/emacs/init.el`， 这种方式要求版本高于27

> 本文使用第三种方式，并配置当前用户的 Emacs 用户目录
>
> - 打开注册表，找到HKEY_LOCAL_MACHINE\SOFTWARE\GNU\Emacs（如果没有则手动添加项）
> - 在此项下添加字符串值，名称为HOME，值为D:\emacs。
> - 这样做的目的是让D:\emacs成为Emacs的home路径（传说中 的home path，以后你将会经常看到“home目录”、“home directory”等等）
> - 在D:\\emacs中创建.config\emacs目录
> - 创建`D:\emacs\.config\emacs\init.el`文件

### 基本配置

- 配置入口文件

    - 配置文件路径

        `D:\emacs\.config\emacs\`

    - init.el

        ```lisp
        ;;配置文件入口
        (add-to-list 'load-path
        	     (expand-file-name (concat user-emacs-directory (convert-standard-filename "lisp/"))))
        ;;将手动修改保存到指定文件
        (setq custom-file (expand-file-name "lisp/custom.el" user-emacs-directory))
        (load-file custom-file)
        ;;引入其它配置文件
        (require 'init-elpa)
        (require 'init-ui)
        ```

- 自定义配置文件

    - 配置文件路径

        `D:\emacs\.config\emacs\lisp\`

    - init-elpa.el

        ```lisp
        ;;设置 package 源为清华镜像源
        (setq package-archives '(("melpa" . "http://mirrors.tuna.tsinghua.edu.cn/elpa/melpa/")
        			 ("gnu" . "http://mirrors.tuna.tsinghua.edu.cn/elpa/gnu/")
        			 ("org" . "http://mirrors.tuna.tsinghua.edu.cn/elpa/org/")))
        
        (provide 'init-elpa)
        ```

    - init-ui.el

        ```lisp
        ;;设置启动界面
        (tool-bar-mode -1) ;; 关闭工具栏
        (scroll-bar-mode -1) ;; 关闭滚动条
        (setq inhibit-startup-screen t) ;; 关闭开始页
        (setq make-backup-files nil) ;; 关闭备份文件的生成
        ;;设置字符编码
        (prefer-coding-system 'utf-8)
        (set-default-coding-systems 'utf-8)
        (set-terminal-coding-system 'utf-8)
        (set-keyboard-coding-system 'utf-8)
        (setq default-buffer-file-coding-system 'utf-8)
        ;;设置垃圾回收阈值，加快启动速度
        (setq gc-cons-threshold most-positive-fixnum)
        
        (provide 'init-ui)
        ```
        
    - M-x customize-group 设置的内容会保存到`lisp/custom.el`文件中
    
    - 

### 配置 LSP

> 参考 Emacs配置Java开发环境