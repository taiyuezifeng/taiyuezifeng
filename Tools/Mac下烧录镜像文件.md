# Mac下烧录镜像文件

## 查看磁盘

```shell
diskutil list
```

## 抹掉磁盘

## unmount

```shell
diskutil unmount /dev/disk2
```

> 这步必须做，要不然`dd`命令会提示`资源繁忙`。

## 烧录镜像

```shell
sudo dd if=/path/file.iso of=/dev/rdisk2 bs=4m
```

> 1. 使用`rdisk`可提高写入速度

2. 也可以烧录`dmg`文件，转换命令是`$ hdiutil convert -format UDRW -o /path/file /path/file.iso`

3. 如果要跟踪写入状态，可以添加`status=progress`参数

## 推出磁盘

```shell
diskutil eject /dev/disk2
```

> 如果有窗口提示要不要推出磁盘，使用上面的命令推出。然后忽略即可。
