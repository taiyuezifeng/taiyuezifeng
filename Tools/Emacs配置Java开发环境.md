# Emacs配置Java开发环境

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
> 2. 如果存在注册表键值`HKEY_CURRENT_USER\Software\GNU\Emacs\HOME`，就用它的值作为home目录~ (只针对当前用户)
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

### 配置LSP

> 参考 [LSP 官方 lsp-java 文档](https://emacs-lsp.github.io/lsp-java/)
>
> 参考 [LSP 官方 lsp-java Tutorial 文档](https://xpressrazor.wordpress.com/2020/11/04/java-programming-in-emacs/)



1. ### 基础配置

- **Java 与 Maven 配置**

    - java
        - JAVA_HOME
        - CLASS_PATH
    - maven
        - M2_HOME

    - path

        > 把相关的可执行路径添加到 path 中



2. ### 克隆 Tutorial 文档中提供的 Emacs 配置文档

```shell
$ git clone https://github.com/neppramod/java_emacs.git ~/.config/emacs
```



3. ### 修改相关配置项，以适配本机需求

- init.el

    ```lisp
    ;; Create a variable to indicate where emacs's configuration is installed
    (setq EMACS_DIR "~/.config/emacs/") ;; 这里需要指定成自己的 Emacs 配置文档路径
    
    ;; Avoid garbage collection at statup
    (setq gc-cons-threshold most-positive-fixnum ; 2^61 bytes
          gc-cons-percentage 0.6)
    
    ;; All the settings and package installation is set in configuration.org
    (org-babel-load-file "~/.config/emacs/emacs-configuration.org") ;; 这里也需要将路径修改
    
    (add-hook 'emacs-startup-hook
      (lambda ()
        (setq gc-cons-threshold 300000000 ; 300mb	
              gc-cons-percentage 0.1)))
    ```
    
    
    
- emacs-configuration.org

    ```org
    #+TITLE: Java Programming in Emacs
    * Introduction
      This configuration file contains setup of Emacs packages for Language Server Protocol (LSP). I will use ~use-package~ for package management.
    * Setup
      Since this is an org file, I am using ~org-babel-load-file~ command to load this file from [[init.el]] file. Emacs loads [[init.el]] when it starts. I have setup a variable called ~EMACS_DIR~ to point to *.emacs.d* as the setup directory. Adjust it to match your system. To speed up loading time of emacs, I have ~used gc-cons-threshold~ and ~gc-cons-percentage~ variables, before and after loading this configuration file. I have set ~gc-cons-threshold~ value to 300 mb after startup. Adjust it to comfortable value according to memory in your system. ~lsp-java~ package however has ~1GB~ setup as default.
    
    
    ** Setup repository in org file
    In the following code block, we will initialize package repositories and after that install ~use-package~. This package is used to install other packages.
    
     #+BEGIN_SRC emacs-lisp
     (require 'package)
    
    ;; erhu::不用这个配置
    ;; (setq package-archives '(
    ;;      ("melpa" . "https://melpa.org/packages/")
    ;;      ("elpa" . "https://elpa.gnu.org/packages/")
    ;;			("org" . "https://orgmode.org/elpa/")
    ;; ))
    ;; erhu::使用镜像
    (setq package-archives '(
      ("melpa" . "http://mirrors.cloud.tencent.com/elpa/melpa/")
      ("gnu" . "http://mirrors.cloud.tencent.com/elpa/gnu/")
      ("org" . "http://mirrors.cloud.tencent.com/elpa/org/")))
    
    
     (package-initialize)
    
     ; Fetch the list of packages available 
     (unless package-archive-contents (package-refresh-contents))
    
     ; Install use-package
     (setq package-list '(use-package))
     (dolist (package package-list)
     (unless (package-installed-p package) (package-install package)))
    
     #+END_SRC
    
    ** Environment Setup
    In some operating systems Emacs does not load environment variables properly. Therefore, below we install a package called ~exec-path-from-shell~ and initialize it.
     #+begin_src emacs-lisp
     ;; erhu::windows上暂时先不用这个配置
     ;;(use-package exec-path-from-shell :ensure t)
     ;;(exec-path-from-shell-initialize)
     #+end_src
    
    ** Operating System specific variable setup
       Since we want to use same emacs configuration for each of our operating systems, we want to separate values that are different between different operating systems. In my setup, I have different values for ~JAVA_HOME~ in my linux.el and mac.el. Adjust these values accordingly for your setup if you are using different versions of Java. Following code loads mac.el, linux.el or windows.el based on where you run this configuration.
    
    #+BEGIN_SRC emacs-lisp
     ;; Load platform specific variables using specific files. E.g linux.el. 
     ;; Make necessary changes as needed
     (cond ((eq system-type 'windows-nt) (load (concat EMACS_DIR "windows")))
     ((eq system-type 'gnu/linux) (load (concat EMACS_DIR "linux")))
     ((eq system-type 'darwin) (load (concat EMACS_DIR "mac")))
     (t (load-library "default")))
     #+END_SRC
    
    ** Basic setup
    Here, I add some basic emacs setup like loading the language, disabling the toolbar, setting up backup directory etc. I have added comments to each setting.
    
    #+BEGIN_SRC emacs-lisp
    ;; Disable annoying ring-bell when backspace key is pressed in certain situations
    ;; erhu::先不用这个配置
    ;;(setq ring-bell-function 'ignore)
    
    ;; Disable scrollbar and toolbar
    (scroll-bar-mode -1)
    (tool-bar-mode -1)
    
    ;; Set language environment to UTF-8
    (set-language-environment "UTF-8")
    (set-default-coding-systems 'utf-8)
    
    ;; Longer whitespace, otherwise syntax highlighting is limited to default column
    (setq whitespace-line-column 1000) 
    
    ;; Enable soft-wrap
    (global-visual-line-mode 1)
    
    ;; Maintain a list of recent files opened
    (recentf-mode 1)            
    (setq recentf-max-saved-items 50)
    
    ;; Move all the backup files to specific cache directory
    ;; This way you won't have annoying temporary files starting with ~(tilde) in each directory
    ;; Following setting will move temporary files to specific folders inside cache directory in EMACS_DIR
    
    (setq user-cache-directory (concat EMACS_DIR "cache"))
    (setq backup-directory-alist `(("." . ,(expand-file-name "backups" user-cache-directory)))
          url-history-file (expand-file-name "url/history" user-cache-directory)
          auto-save-list-file-prefix (expand-file-name "auto-save-list/.saves-" user-cache-directory)
          projectile-known-projects-file (expand-file-name "projectile-bookmarks.eld" user-cache-directory))
    
    ;; erhu::增加配置
    (setq make-backup-files nil) ;; 关闭备份文件的生成
    (setq auto-save-default nil) ;; 关闭自动保存文件的功能
    
    ;; Org-mode issue with src block not expanding
    ;; This is a fix for bug in org-mode where <s TAB does not expand SRC block
    (when (version<= "9.2" (org-version))
    (require 'org-tempo))
    
    ;; Coding specific setting
    
    ;; Automatically add ending brackets and braces
    (electric-pair-mode 1)
    
    ;; Make sure tab-width is 4 and not 8
    (setq-default tab-width 4)
    
    ;; Highlight matching brackets and braces
    (show-paren-mode 1) 
    #+END_SRC
    
    * Looks
    ** Theme
       I tend to like *doom-themes* package. Below we will install doom theme. In addition, I will also install a package called *heaven-and-hell*. This allows us to toggle between two themes using a shortcut key. I will assign ~F6~ key to toggling the theme and ~C-c F6~ to set to default theme.
    
    #+BEGIN_SRC emacs-lisp
    
    ;; erhu::自定义配置
    ;; 关闭开始页
    ;;(setq inhibit-startup-screen t)
    (setq inhibit-splash-screen t)
    ;; 开启全屏模式
    (setq initial-frame-alist (quote ((fullscreen . maximized))))
    ;; 设置光标模式为单坚线
    (setq-default cursor-type 'bar)
    ;; 开启高亮匹配
    ;;(add-hook 'emacs-lisp-mode-hook 'show-paren-mode)
    ;; 开启当前行高亮显示
    (global-hl-line-mode t)
    ;; 开启行号
    (global-linum-mode t)
    ;; 自定义主题
    (use-package monokai-theme
    :ensure t 
    :init 
    (load-theme 'monokai t))
    
    ;; erhu::不使用这个配置
    ;;(use-package doom-themes
    ;;:ensure t 
    ;;:init 
    ;;(load-theme 'doom-palenight t))
    
    ;;(use-package heaven-and-hell
    ;;  :ensure t
    ;;  :init
    ;;  (setq heaven-and-hell-theme-type 'dark)
    ;;  (setq heaven-and-hell-themes
    ;;        '((light . doom-acario-light)
    ;;          (dark . doom-palenight)))
    ;;  :hook (after-init . heaven-and-hell-init-hook)
    ;;  :bind (("C-c <f6>" . heaven-and-hell-load-default-theme)
    ;;         ("<f6>" . heaven-and-hell-toggle-theme)))
    
    #+END_SRC
    
    If you press F6 key in your keyboard, it should switch between doom-palenight and doom-acario-light themes. If you want to go back to the default theme press ~Ctrl + C and F6~.
    
    ** Disable ansi color in compilation mode
       This will help eliminate weird escape sequences during compilation of projects.
       #+begin_src emacs-lisp
    
       (defun my/ansi-colorize-buffer ()
       (let ((buffer-read-only nil))
       (ansi-color-apply-on-region (point-min) (point-max))))
       
       (use-package ansi-color
       :ensure t
       :config
       (add-hook 'compilation-filter-hook 'my/ansi-colorize-buffer)
       )
       #+end_src
    * Custom Packages
      In this section we will install some of the packages that we will use for various project and file management.
    
    ** Key-Chord
       Key-Chord allows us to bind regular keyboard keys for various commands without having to use prefix keys such as Ctrl, Alt or Super etc.
    
    #+begin_src emacs-lisp
    (use-package use-package-chords
    :ensure t
    :init 
    :config (key-chord-mode 1)
    (setq key-chord-two-keys-delay 0.4)
    (setq key-chord-one-key-delay 0.5) ; default 0.2
    )
    #+end_src
    Here, we changed the delay for the consecutive key to be little higher than default. Adjust this to what you feel comfortable.
    
    ** Projectile
       Projectile helps us with easy navigation within a project. Projectile recognizes several source control managed folders e.g *git, mercurial, maven, sbt*, and a folder with empty *.projectile* file. You can use ~C-c p~ to invoke any projectile command. This is a very useful key to remember.
    
    #+begin_src emacs-lisp
    (use-package projectile 
    :ensure t
    :init (projectile-mode +1)
    :config 
    (define-key projectile-mode-map (kbd "C-c p") 'projectile-command-map)
    )   
    #+end_src
    ** Helm
    Helm allows for easy completion of commands. Below, we will replace several of the built in functions with helm versions and add keyboard shortcuts for couple of new useful commands.
    
    #+BEGIN_SRC emacs-lisp
    (use-package helm
    :ensure t
    :init 
    (helm-mode 1)
    (progn (setq helm-buffers-fuzzy-matching t))
    :bind
    (("C-c h" . helm-command-prefix))
    (("M-x" . helm-M-x))
    (("C-x C-f" . helm-find-files))
    (("C-x b" . helm-buffers-list))
    (("C-c b" . helm-bookmarks))
    (("C-c f" . helm-recentf))   ;; Add new key to recentf
    (("C-c g" . helm-grep-do-git-grep)))  ;; Search using grep in a git project
    #+END_SRC
    
    I want to point out, couple of interesting things from above setup. Just like we added ~C-c p~ as a prefix for projectile, here we added ~C-c h~ for helm. We also enabled fuzzy matching, so that your search text don't need to be very stict. Also, I added ~C-c g~ to helm-grep-do-git-grep. I can search files with specific text within a git project (make sure to commit it first).
    
    ** Helm Descbinds
    Helm descbinds helps to easily search for keyboard shortcuts for modes that are currently active in the project. This can be helpful to discover keyboard shortcuts to various commands. Use ~C-h b~ to bring up helm-descbinds window.
    
    #+begin_src emacs-lisp
    (use-package helm-descbinds
    :ensure t
    :bind ("C-h b" . helm-descbinds))
    #+end_src
    
    E.g. In helm-descbinds window you could type "helm" and "projectile" and see all the shortcuts assigned to various commands.
    
    ** Helm swoop
    Helm swoop allows to quickly search for text under cursor or new text within current file. I am sure you are already using ~C-s~ and ~C-r~ to search within the file. This package compliments rather than replace it. You can quickly type ~js~ to search and jump to the target line. To go back to where you started searching, use ~jp~. You can use ~M-m~ from ~C-s~ and ~C-r~ search to start using helm-swoop as described in below setting.
    
    #+begin_src emacs-lisp
    (use-package helm-swoop 
    :ensure t
    :chords
    ("js" . helm-swoop)
    ("jp" . helm-swoop-back-to-last-point)
    :init
    (bind-key "M-m" 'helm-swoop-from-isearch isearch-mode-map)
    
    ;; If you prefer fuzzy matching
    (setq helm-swoop-use-fuzzy-match t)
    
    ;; Save buffer when helm-multi-swoop-edit complete
    (setq helm-multi-swoop-edit-save t)
    
    ;; If this value is t, split window inside the current window
    (setq helm-swoop-split-with-multiple-windows nil)
    
    ;; Split direction. 'split-window-vertically or 'split-window-horizontally
    (setq helm-swoop-split-direction 'split-window-vertically)
    
    ;; If nil, you can slightly boost invoke speed in exchange for text color
    (setq helm-swoop-speed-or-color nil)
    
    ;; ;; Go to the opposite side of line from the end or beginning of line
    (setq helm-swoop-move-to-line-cycle t)
    
    )
    #+end_src
    
    ** Avy Goto
       Avy allows you to quickly jump to certain character, word or line within the file. Use ~jc~, ~jw~ or ~jl~ to quickly jump within current file. Change it to other keys, if you feel you are using this set of keys for other purposes. 
    
    #+begin_src emacs-lisp
    (use-package avy 
    :ensure t
    :chords
    ("jc" . avy-goto-char)
    ("jw" . avy-goto-word-1)
    ("jl" . avy-goto-line))
    #+end_src
    
    ** Which Key
    For some prefix commands like ~C-c p~ or ~C-c h~ we want Emacs to visually guide you through the available options. Following package allows us to do that.
    #+begin_src emacs-lisp
    (use-package which-key 
    :ensure t 
    :init
    (which-key-mode)
    )
    #+end_src
    ** Run Code
    We can use quickrun package to execute code (if it has main). E.g. If you have a java file with main method, it will run with the associated shortcut key ~C-c r~ or quickrun command. Quickrun has support for several languages.
    #+begin_src emacs-lisp
    (use-package quickrun 
    :ensure t
    :bind ("C-c r" . quickrun))
    #+end_src
    
    * Language Server Protocol (LSP)
      With above setup done, below we will setup several packages closely related to LSP.
    
    ** Company
    Complete anything aka Company provides auto-completion. Company-capf is enabled by default when you start LSP on a project. You can also invoke ~M-x company-capf~ to enable capf (completion at point function).
    #+begin_src emacs-lisp
    (use-package company :ensure t)
    #+end_src
    
    ** Yasnippet
    Yasnippet is a template system for Emacs. It allows you to type abbreviation and complete the associated text.
    
    #+begin_src emacs-lisp
    (use-package yasnippet :config (yas-global-mode))
    (use-package yasnippet-snippets :ensure t)
    #+end_src
    
    E.g. In java mode, if you type ~pr~ and hit ~<TAB>~ it should complete to ~System.out.println("text");~
    
    To create a new snippet you can use ~yas-new-snippet~ command. 
    
    ** FlyCheck
    FlyCheck checks for errors in code at run-time.
    #+begin_src emacs-lisp
    (use-package flycheck :ensure t :init (global-flycheck-mode))
    #+end_src
    
    ** Dap Mode
    Emacs Debug Adapter Protocol aka DAP Mode allows us to debug your program. Below we will integrate ~dap-mode~ with ~dap-hydra~. ~Dap-hydra~ shows keys you can use to enable various options and jump through code at runtime. After we install dap-mode we will also install ~dap-java~.
    
    #+begin_src emacs-lisp
    (use-package dap-mode
      :ensure t
      :after (lsp-mode)
      :functions dap-hydra/nil
      :config
      (require 'dap-java)
      :bind (:map lsp-mode-map
             ("<f5>" . dap-debug)
             ("M-<f5>" . dap-hydra))
      :hook ((dap-mode . dap-ui-mode)
        (dap-session-created . (lambda (&_rest) (dap-hydra)))
        (dap-terminated . (lambda (&_rest) (dap-hydra/nil)))))
    
    (use-package dap-java :ensure nil)
    #+end_src
    
    ** Treemacs
    Treemacs provides UI elements used for LSP UI. Let's install lsp-treemacs and its dependency treemacs. We will also Assign ~M-9~ to show error list.
    #+begin_src emacs-lisp
    (use-package lsp-treemacs
      :after (lsp-mode treemacs)
      :ensure t
      :commands lsp-treemacs-errors-list
      :bind (:map lsp-mode-map
             ("M-9" . lsp-treemacs-errors-list)))
    
    (use-package treemacs
      :ensure t
      :commands (treemacs)
      :after (lsp-mode))
    #+end_src
    
    ** LSP UI
    LSP UI is used in various packages that require UI elements in LSP. E.g ~lsp-ui-flycheck-list~ opens a windows where you can see various coding errors while you code. You can use ~C-c l T~ to toggle several UI elements. We have also remapped some of the xref-find functions, so that we can easily jump around between symbols using ~M-.~, ~M-,~ and ~M-?~ keys.
    
    #+begin_src emacs-lisp
    (use-package lsp-ui
    :ensure t
    :after (lsp-mode)
    :bind (:map lsp-ui-mode-map
             ([remap xref-find-definitions] . lsp-ui-peek-find-definitions)
             ([remap xref-find-references] . lsp-ui-peek-find-references))
    :init (setq lsp-ui-doc-delay 1.5
          lsp-ui-doc-position 'bottom
    	  lsp-ui-doc-max-width 100
    ))
    #+end_src
    
    Go through this [[https://github.com/emacs-lsp/lsp-ui/blob/master/lsp-ui-doc.el][link]]  to see what other parameters are provided.
    
    ** Helm LSP
    Helm-lsp provides various functionality to work with the code. E.g Code actions like adding *getter, setter, toString*, refactoring etc. You can use ~helm-lsp-workspace-symbol~ to find various symbols (classes) within your workspace.
    
    LSP's built in symbol explorer uses ~xref-find-apropos~ to provide symbol navigation. Below we will replace that with helm version. After that you can use ~C-c l g a~ to find workspace symbols in a more intuitive way.
    
    #+begin_src emacs-lisp
    (use-package helm-lsp
    :ensure t
    :after (lsp-mode)
    :commands (helm-lsp-workspace-symbol)
    :init (define-key lsp-mode-map [remap xref-find-apropos] #'helm-lsp-workspace-symbol))
    #+end_src
    
    ** Install LSP Package
    Let's install the main package for lsp. Here we will integrate lsp with which-key. This way, when we type the prefix key ~C-c l~ we get additional help for compliting the command. 
    
    #+begin_src emacs-lisp
    (use-package lsp-mode
    :ensure t
    :hook (
       (lsp-mode . lsp-enable-which-key-integration)
       (java-mode . #'lsp-deferred)
    )
    :init (setq 
        lsp-keymap-prefix "C-c l"              ; this is for which-key integration documentation, need to use lsp-mode-map
        lsp-enable-file-watchers nil
        read-process-output-max (* 1024 1024)  ; 1 mb
        lsp-completion-provider :capf
        lsp-idle-delay 0.500
    )
    :config 
        (setq lsp-intelephense-multi-root nil) ; don't scan unnecessary projects
        (with-eval-after-load 'lsp-intelephense
        (setf (lsp--client-multi-root (gethash 'iph lsp-clients)) nil))
    	(define-key lsp-mode-map (kbd "C-c l") lsp-command-map)
    )
    #+end_src
    
    You can start LSP server in a java project by using ~C-c l s s~. Once you type ~C-c l~ ~which-key~ package should guide you through rest of the options. In above setting I have added some memory management settings as suggested in [[https://emacs-lsp.github.io/lsp-mode/page/performance/][this guide]]. Change them to higher numbers, if you find *lsp-mode* sluggish in your computer.
    
    ** LSP Java
    This is the package that handles server installation and session management.
    #+begin_src  emacs-lisp
    (use-package lsp-java 
    :ensure t
    :config (add-hook 'java-mode-hook 'lsp))
    #+end_src
    
    * Conclusion
    Go through [[https://github.com/emacs-lsp/lsp-java#supported-commands][Supported commands]] section of lsp-java github page to see commands provided in lsp-mode. Most of these commands are available under lsp's ~C-c l~ option. I hope this configuration file was useful.
    
    ```

    

- Emacs 启动时会根据 emacs-configuration.org 提取 emacs lisp 代码片段生成 emacs-configuration.el 文件

- **由于 lsp-java 的 jdtls 服务需要 jdk11以上的版本，故需要在 windows.el 中指定相关的配置**

    ```lisp
    ;; Your windows specific settings
    
    ;;
    ;;If emacs complains about java version during installation of LSP please add following settings to operating system specific files, and adjust the folders accordingly. It may not be a bad idea to set them up anyway.
    ;;
    ;;(setenv "JAVA_HOME"  "path_to_java_folder/Contents/Home/")
    ;;(setq lsp-java-java-path "path_to_java_folder/Contents/Home/bin/java")
    ;;
    (setq lsp-java-java-path "D:/jdk/AdoptOpenJDK_11.0.7_10/bin/java")
    
    ;; 添加以下内容，将手动修改保存到指定文件
    (setq custom-file (expand-file-name "custom.el" user-emacs-directory))
    (load-file custom-file)
    ```





### 运行与调试

- 使用 lsp-java-spring-initializer 创建项目时存在问题

    - curl 不 支持 https 协议或 libcurl 禁用

    - 没有 unzip 命令去解压文件

        > 可以下载 unzip.exe 放入 windows 路径中

    - start.spring.io 生成的临时文件不能解压

        > 临时文件存在，但大小是 0KB

- 项目启动

    - 使用 projectile 的 `C-c p u`运行`mvn spring-boot:run`会失败
    - 使用 eshell 运行`mvn spring-boot:run`可以成功启动项目

- 项目调试

    - 手动切换到**`dap-java-debug`**模式
    - 调试命令窗口关闭后可以使用`M-F5`重新打开
    - 使用`sl`查看变量
    - 使用`sb`查看断点
    - 使用`n` `i` `o` `c` 指令跟踪调试
    - 使用`Q`断开调试
    - 使用`q`关闭调试命令窗口
    - 使用`C-c C-k`关闭调试服务 



### 自定义设置保存

- 在`emacs-configuration.org`中添加配置

    ```org
    * Save GUI Options to custom.el 指定手动修改内容的保存文件
    #+begin_src  emacs-lisp
    ;; 添加以下内容，将手动修改保存到指定文件
    (setq custom-file (expand-file-name "custom.el" user-emacs-directory))
    (load-file custom-file)
    #+end_src
    ```

    

- custom.el

    ```lisp
    (custom-set-variables
     ;; custom-set-variables was added by Custom.
     ;; If you edit it by hand, you could mess it up, so be careful.
     ;; Your init file should contain only one such instance.
     ;; If there is more than one, they won't work right.
     '(package-selected-packages
       '(lsp-java helm-lsp yasnippet-snippets which-key use-package-chords quickrun projectile lsp-ui helm-swoop helm-descbinds flycheck dap-mode company)))
    (custom-set-faces
     ;; custom-set-faces was added by Custom.
     ;; If you edit it by hand, you could mess it up, so be careful.
     ;; Your init file should contain only one such instance.
     ;; If there is more than one, they won't work right.
     )
    ```

    

### 整合 Spring Boot

- 在`emacs-configuration.org`中添加配置

    ```org
    * Integration Spring Boot 整合 Spring Boot
    #+begin_src  emacs-lisp
    ;;(require 'lsp-java-boot)
    ;; to enable the lenses
    (add-hook 'lsp-mode-hook #'lsp-lens-mode)
    (add-hook 'java-mode-hook #'lsp-java-boot-lens-mode)
    #+end_src
    ```

- 如果项目不使用 Spring Boot，则可以不使用上面的配置

- 如果不配置上面的配置，则可以手动开启`lsp-lens-mode`或`lsp-java-boot-lens-mode`，**未验证**

### 整合Lombok

- ~~克隆 [lsp-java-lombok](https://github.com/sei40kr/lsp-java-lombok.git) 项目~~

- ~~把`lsp-java-lombok.el`文件拷贝到`init.el`的下级路径`lisp`目录中~~

- 在`emacs-configuration.org`中添加配置

    ```org
    * Custom Options file load path
    #+begin_src  emacs-lisp
    ;; 配置文件路径设定
    ;;(add-to-list 'load-path (expand-file-name (concat user-emacs-directory (convert-standard-filename "lisp/"))))
    #+end_src
    
    * Integration Lombok
    #+begin_src  emacs-lisp
    ;; lsp-java-lombok.el
    ;;(require 'lsp-java-lombok)
    ;;(with-eval-after-load 'lsp-java (lsp-java-lombok))
    ;;(setq lsp-java-lombok-jar-path (expand-file-name ".cache/lombok.jar" user-emacs-directory))
    ;;;;
    ;;以上配置不生效，故不使用 lsp-java-lombok.el 的配置方式
    ;;;;
    
    ;;;;
    ;; 以下配置可以生效
    ;;;;
    ;;(setq path-to-lombok (car (file-expand-wildcards "/lombok.jar") ) )
    (setq path-to-lombok (expand-file-name ".cache/lombok.jar" user-emacs-directory))
    (setq lsp-java-vmargs
          `("-noverify"
            "-Xmx1G"
            "-XX:+UseG1GC"
            "-XX:+UseStringDeduplication"
            ,(concat "-javaagent:" path-to-lombok)
            ,(concat "-Xbootclasspath/a:" path-to-lombok)))
    #+end_src
    ```

    

- 