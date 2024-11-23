```bash
docker run --rm v2fly/v2fly-core help

docker run --name v2ray v2fly/v2fly-core $v2ray_args (help, eun etc...)

docker run -d --name v2ray -v /path/to/config.json:/etc/v2ray/config.json -p 10086:10086 v2fly/v2fly-core run -c /etc/v2ray/config.json 

# If you want to use v5 format config
docker run -d --name v2ray -v /etc/v2ray/config.json:/etc/v2ray/config.json -p 10086:10086 v2fly/v2fly-core run -c /etc/v2ray/config.json -format jsonv5
```


[clash for windows 0.20.39 中文](https://p74h-my.sharepoint.com/:f:/g/personal/minddance_p74h_onmicrosoft_com/EhsBAzXAI05IjRzJ885boe4B-4yN2mBnY1q6YwAM61diZg)