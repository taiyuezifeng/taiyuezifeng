# Manjaro美化Mac主题

> 参考YouTube视频[Customize Your Xfce Desktop Look Like MacOS Big Sur](https://www.youtube.com/watch?v=oQ8RWtD3MTQ)

注意事项：

- 安装`vala-panel-appmenu-xfce-git`时如果有问题，则使用命令行指定参数`gobject-introspection`进行安装即可
  
  ```shell
  yay -S vala-panel-appmenu-xfce-git gobject-introspection
  ```

- `Plank`无法右键打开`Preferences`时可以使用命令行指定参数`--preferences`打开
  
  ```shell
  plink --preferences
  ```

- 

## 基本设置

- 外观中的字体及字号

- 窗口管理器中的按钮布局

## 下载与安装

> 下载地址：https://www.pling.com

1. 安装主题
   
   - 下载`WhiteSur Gtk Theme`主题
     
     > 下载地址：https://www.pling.com/p/1403328
   
   - 安装
     
     - 在用户根目录下创建`.themes`文件夹
     
     - 解压下载的主题文件并放进`.themes`文件夹中
     
     - 在外观的样式中选择主题`WhiteSur-light`

2. 安装`Icon`主题
   
   - 下载`WhiteSur-icon-theme`主题
     
     > 下载地址：https://www.pling.com/p/1405756
   
   - 安装
     
     - 在用户根目录下创建`.icons`文件夹
     
     - 解压下载的图标主题文件并放进`.icons`文件夹中
     
     - 在外观的图标中选择`WhiteSur`

3. 安装`Cursor`主题
   
   - 下载`McMojave cursors`主题
     
     > 下载地址：https://www.pling.com/p/1355701/
   
   - 安装
     
     - 解压下载的图标主题文件并放进`.icons`文件夹中
     
     - 在鼠标和触摸板的主题中选择`McMojave Cursors`

4. 安装全局菜单面板
   
   - 使用`yay`安装相关包
     
     ```shell
     # vala-panel-appmenu-common-git
     # vala-panel-appmenu-xfce-git
     yay -S vala-panel-appmenu-xfce-git gobject-introspection
     # vala-panel-appmenu-registrar-git
     yay -S vala-panel-appmenu-registrar-git # 注：可在GUI中安装
     # appmenu-gtk-module
     yay -S appmenu-gtk-moudle # 注：可在GUI中安装
     ```
   
   - 新建全局菜单面板
     
     - 右键任务栏选中`Panel Preferences`
     
     - 新加一个面板并拖放到顶部
     
     - 锁定面板
     
     - 长度设置为100
     
     - 设置
       
       ```shell
       xfconf-query -c xsettings -p /Gtk/ShellShowsMenubar -n -t bool -s true
       xfconf-query -c xsettings -p /Gtk/ShellShowsAppmenu -n -t bool -s true
       ```
     
     - 在项目中添加需要的项
       
       - 分隔符，样式设置为透明
       
       - 启动器，添加`panther_launcher`
       
       - 分隔符，样式设置为透明
       
       - 全局菜单插件，选中使用粗体和扩展插件
       
       - 分隔符，样式设置为透明，选中扩展
       
       - 通知插件
       
       - 状态栏插件，需要从任务栏中移除后再添加，设置自动调整大小
       
       - `PulseAudio`插件
       
       - 启动器，添加`synapse`
       
       - 时钟，设置自定义格式：`%Y年%b%_d日 %R 星期%a`
       
       - 分隔符，样式设置为透明
       
       - 注销动作按钮，只保留锁屏即可
     
     - 
   
   - 

5. 安装`Plank`
   
   - 安装
     
     - 通过系统软件仓库安装
     
     - 命令行安装
   
   - 配置
     
     - 把`.themes`使用的主题中的`plank`文件夹拷贝到`.local/share/plank/themes/`中并修改成主题的名称
     
     - 在`Plank`的`Preferences`中设置想要的主题
       
       ```shell
       plink --preferences # 右键无法打开时可使用此命令
       ```
     
     - 其它设置可根据自己的需求设定
     
     - 添加启动项

6. 安装`Launcher`
   
   - 安装`Synapse`
   
   - ~~在全局菜单面板中添加`Synapse`的启动器~~
   
   - 在`Preferences`中设置主题为`Virgilio`、登录时启动
   
   - 修改激活快捷键为`Super-Space`，避免与切换输入法的快捷键的冲突

7. 安装`Login & Lock Screen`
   
   - 安装`lightdm-webkit2-greeter`
   
   - 安装`lightdm-webkit2-theme-glorious`
   
   - 修改`/etc/lightdm/lightdm.conf`配置文件
     
     ```conf
     # greeter-session=lightdm-gtk-greeter # 注释掉这一行
     greeter-session=lightdm-webkit2-greeter # 新增这一行
     ```
   
   - 修改`/etc/lightdm/lightdm-webkit2-greeter.conf`
     
     ```conf
     # webkit_theme = antergos
     webkit_theme = glorious
     ```
   
   - 可以给`/usr/share/backgrounds/`中拷贝一些壁纸
   
   - 重启后设置一下启动界面的壁纸

8. 安装文件管理包`Files(nautilus)`
   
   - 安装`File(nautilus)`
   
   - 设置默认应用程序中的文件管理器为`Nautilus`

9. 在设置中的`关于我`中设置一下头像
