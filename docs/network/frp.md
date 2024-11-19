[Unit]
# 服务名称，可自定义
Description = frp client
After = network.target syslog.target
Wants = network.target

[Service]
Type = simple
# 启动frpc的命令，需修改为您的frpc的安装路径
ExecStart = /usr/local/sbin/frpc -c /opt/frp/frpc.ini

[Install]
WantedBy = multi-user.target
