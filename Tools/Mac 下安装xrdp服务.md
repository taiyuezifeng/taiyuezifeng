# Mac 下安装xrdp服务

## 准备工作

1. Google搜索`xrdp`

2. 下载`xrdp`和`xorgxrdp`并解压

3. 根据[文档](https://github.com/neutrinolabs/xrdp/wiki/Building-on-OSX-(not-official))安装编译依赖
   
   - Install Xcode command line tools: `xcode-select --install`
   - Install Homebrew: http://brew.sh/
   - Install OpenSSL from Homebrew: `brew install openssl`
   - Install Automake + Autoconf: `brew install automake`
   - Instal Libtool: `brew install libtool`
   - Install pkgconfig: `brew install pkgconfig`
   - Install nasm: `brew install nasm` (needed by librfxcodec)
   - Install XQuartz: https://www.xquartz.org/

## 编译安装

1. 编译安装`xrdp`
   
   ```shell
   # 进入xrdp目录
   ./bootstrap
   ./configure PKG_CONFIG_PATH=/usr/local/opt/openssl/lib/pkgconfig
   make
   sudo make install
   ```

2. 编译安装`xorgxrdp`
   
   ```shell
   # 进入xorgxrdp目录
   ./bootstrap
   # 这一步有2个问题，参见下面详情说明
   ./configure PKG_CONFIG_PATH=/opt/X11/lib/pkgconfig
   make
   sudo make install
   ```
   
   > configure时出现的问题：
   > 
   > 1. 需要`xorg-x11-server-devel`或其它`xorg-server`的依赖
   > 
   > 2. 报错提示：`configure: error: please install xserver-xorg-dev, xorg-x11-server-sdk or xorg-x11-server-devel`
   > 
   > 3. 解决方法：
   >    
   >    - 安装`XCode`应用
   >    
   >    - 处理命令行不能同意协议前的问题
   >      
   >      - `sudo xcode-select -s /Applications/Xcode.app/Contents/Developer`
   >    
   >    - 同意协议
   >      
   >      - `sudo xcodebuild -license`
   >    
   >    - 安装`MacPorts`
   >    
   >    - 安装`xorg-server-devel`
   >      
   >      - `sudo port install xorg-server-devel`
   > 
   > 4. `pkgconfig`的安装路径问题
   > 
   > 5. 报错提示：
   > 
   > 6. 解决方法：
   >    
   >    - `./configure PKG_CONFIG_PATH=/opt/X11/lib/pkgconfig:/usr/local/lib/pkgconfig`

## 启动服务

```shell
sudo /usr/local/sbin/xrdp
sudo /usr/local/sbin/xrdp-sesman
```

## 查看日志

```shell
tail -f /var/log/xrdp.log
tail -f /var/log/xrdp-sesman.log
```

## 配置

配置文件：/etc/xrdp/xrdp.ini

- Xorg (xrogxrdp)
  
  - most recommended by we xrdp developer, fully-functional
  - 把`libxup.so`修改成`libxup.dylib`

- Xvnc
  
  - old-fashioned former session type, kept for compatibility
  - we don't recommend this if Xorg (Xorgxrdp) is usable
  - 把`libvnc.so`修改成`libvnc.dylib`

- vnc-any
  
  - connect to another VNC host via xrdp (like a proxy)

- neutrinordp-any
  
  - connect to another RDP host via xrdp (like a proxy)
  - requires NeutrinoRDP (not well-maintained)
