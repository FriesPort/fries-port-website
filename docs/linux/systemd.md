# systemd守护进程

https://gofrp.org/zh-cn/docs/setup/systemd/

不受包管理器管理、面向用户而非系统的可执行文件放在`/usr/local/bin`或者`/usr/local/sbin`文件夹下，sbin代表需要`sudo`，可以在`/opt`文件夹下存放数据和配置文件。

系统服务通常在`/etc/systemd/system/xxx.service`存放配置文件，并使用systemctl命令进行管理。