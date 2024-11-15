# 静态挂载硬盘

列出所有存储设备
```bash title="list block devices"
lsblk
```
获取硬盘的唯一id（uuid），例如查找sda1的信息。
```bash
lsblk -d -fs /dev/sda1
```
记下UUID和FSTYPE（file system type，文件系统类型），稍后需要用。

在/mnt中随意创建一个文件夹，例如fries-mount

```bash
sudo mkdir /mnt/fries-mount
```
修改配置文件
```bash
sudo nano /etc/fstab
```
添加配置到文件中
```bash
UUID=<your HD UUID> /mnt/fries-mount <FSTYPE> defaults 0 2
```
保存文件, 按Ctrl-X, Y, Enter。

重新挂载所有硬盘
```bash
sudo mount -a
```
重新运行该命令以确认挂载成功
```bash
lsblk -d -fs /dev/sda1
```

## 参考资料
[storj文档](https://storj.dev/node/faq/linux-static-mount)