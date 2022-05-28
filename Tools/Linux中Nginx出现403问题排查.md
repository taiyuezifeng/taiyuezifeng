# Linux中Nginx出现403问题排查

> Linux中配置了新项目路径后启动并访问出现`403`问题，一般情况下优先考虑后两种情况并解决之。

## 一、启动用户与`nginx`工作用户不一致

```shell
vim /etc/nginx/nginx.conf
...
user nginx;
...
```

## 二、项目中没有`index.html`等

```shell
vim /etc/nginx/nginx.conf
...
index main.html default.html index.php; # 指定首页
...
```

## 三、项目路径权限访问受限

- 修改项目路径权限

  - 更改`nginx`启动用户为项目路径所属用户
  
  ```shell
  vim /etc/nginx/nginx.conf
  ...
  user xxx;
  ...
  ```
  
  - 更改项目路径所属用户为`nginx`启动用户
  
  ```shell
  chown -R nginx:nginx www
  ```
  
  - 更改项目路径读写权限
  
  ```shell
  chmod -R 777 www # 上述两种方法不行再试
  ```

## 四、`SELinux`未禁用

```shell
# 查看selinux的d状态
/usr/sbin/sestatus
# 如果SELINUX=enforcing，则修改为disabled
vim /etc/selinux/config
...
SELINUX=disabled
...
# 重启才能生效
reboot
```