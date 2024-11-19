---
sidebar_label: "部署storj节点"
title: "部署storj节点"
---

:::warning
笔者尚未成功部署storj node，最接近成功的一次是看到了14002端口的面板，但是提示QUIC未配置好。修改参数后重新运行docker容器就再也进不去面板了，说是配置不正确。这期间还有一些官方文档没提到的坑，虽然没部署成功，但仍希望分享这些经历给你。文章省略的很多，仅作为官方文档的参考。
:::


# 部署storj节点


## 步骤1：设备要求
建议使用Linux，最好使用LAN有线连接，如果时常断电建议使用UPS应急电源，最好使用现有设备部署节点，而不是专门购买全新的设备去运行storj节点。用户的使用是不可预测的，无法保证收入。不推荐smr硬盘，即使可以工作。硬盘的文件系统最好与操作系统一致，即Windows下使用NTFS，Linux下使用ext4。

### 推荐配置
部分简单化翻译，具体要求看原文档[^1]
- 每个节点进程单独使用一个处理器核心
- 每个节点一个非smr硬盘，因为我没有raid，NAS配置按下不表
- 每个节点进程预留2TB存储空间
- 预估每个节点每月1.5TB流量，上不封顶
- 每TB预留3Mbps上行带宽
- 每TB预留5Mbps下行带宽
- 每月在线时间99.5%以上
  
[^1]: https://storj.dev/node/get-started/prerequisites#hardware-and-network-requirements

### 最低配置
- 每个节点进程单独使用一个处理器核心
- 每个节点一个硬盘，NAS配置看文档
- 每个节点进程预留500GB存储空间
- 预估每个节点每月1.5TB流量，上不封顶
- 每TB预留1Mbps上行带宽
- 每TB预留3Mbps下行带宽
- 每月在线时间99.3%以上，最多下线5小时

## 步骤2：获取身份令牌
https://storj.dev/node/get-started/auth-token#get-your-token

前往官网文档获取身份令牌

## 步骤3：设置端口转发
官网提供了一个使用noip进行ddns端口转发的教程。但是该教程的需求是需要用户有公网ip，但是国内的ipv4地址紧缺，部分地区甚至无法申请即使是动态的公网ip，只有企业才有资格加钱购买静态公网ip。

因此，对于普通用户，需要更多的设置。
### 公网ipv6
国内目前正在推进ipv6，以去除NAT44，你可以尝试一下家庭的网络是否可以直接使用公网ipv6（同时你也需要一个支持ipv6的路由器）。可能需要查看其他教程与攻略。我在校园网环境，没有权限设置网关，无法提供有用的建议，需要大家自己上心。

### 内网穿透
如果没有权限管理网关（例如在校园网中），无法使用ipv6，那么就要考虑使用内网穿透服务。

目前我找到一个限制带宽但是不限流量，且免费用户也可以适量使用的服务是[ChmlFrp](https://www.chmlfrp.cn/)，基于fpr二次开发的内网穿透服务器，限制带宽，但不限流量。

下载本机架构对应的frpc客户端，编写systemd脚本，由systemd将frpc作为服务进行管理。在ChmlFrp中添加隧道本机28967端口的隧道，该端口tcp和udp协议各一个，下载配置放到合适的文件夹，在服务脚本中指定该配置文件的位置。

## 步骤4：设置QUIC
Linux默认udp缓存比较小，以root身份执行下面命令
```bash
echo "net.core.rmem_max=2500000" >> /etc/sysctl.d/udp_buffer.conf

sysctl -w net.core.rmem_max=2500000
```
## 步骤5：创建身份
需要生成一个识别节点的配置，如果计算机性能比较弱可能会花费几小时甚至几天，建议在性能较强的电脑上完成这一步后将配置迁移会目标服务器。
### 下载软件
以Windows为例，打开终端下载软件，无需管理员身份。

```powershell title="powershell"
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; curl https://github.com/storj/storj/releases/latest/download/identity_windows_amd64.zip -o identity_windows_amd64.zip; Expand-Archive ./identity_windows_amd64.zip . -Force
```
### 生成身份
```powershell title="powershell"
./identity.exe create storagenode
```
程序会一直运行，直到复杂等级达到36级以上。
### 认证身份
使用步骤2生成的身份令牌，对身份进行签名
```powershell title="powershell"
./identity.exe authorize storagenode <email:characterstring>
```
### 验证身份

```powershell title="powershell"
(sls BEGIN "$env:AppData\Storj\Identity\storagenode\ca.cert").count
# 返回值应该为2
```

```powershell title="powershell"
(sls BEGIN "$env:AppData\Storj\Identity\storagenode\identity.cert").count
# 返回值应该为3
```
如果返回值不为2、3，说明出错了，请重新完成步骤5。

### 备份、迁移身份
运行该命令打开身份文件夹，并备份。
```powershell title="powershell"
start "$Env:APPDATA/Storj/Identity/storagenode"
```

将备份的文件复制到Linux的`~/.local/share/storj/identity/storagenode`文件夹下。

运行下面的命令查看是否迁移成功
```bash
grep -c BEGIN ~/.local/share/storj/identity/storagenode/ca.cert
# 返回值应该为2
```

```bash
grep -c BEGIN ~/.local/share/storj/identity/storagenode/identity.cert
# 返回值应该为3
```

## 步骤6：安装节点
需要安装docker。自行查看[官方docker engine安装手册](https://docs.docker.com/engine/install/)，不再赘述。
### 拉取镜像
拉取镜像，如果无法连接`docker.io`可以使用镜像站`dockerpull.org`。
```bash
docker pull storjlabs/storagenode:latest
```
### 设置镜像
填写下面相关信息，并以root身份运行。`<identity-dir>`为身份文件夹，`<storage-dir>`为存储数据的硬盘。
```bash
docker run --rm -e SETUP="true" \
    --user $(id -u):$(id -g) \
    --mount type=bind,source="<identity-dir>",destination=/app/identity \
    --mount type=bind,source="<storage-dir>",destination=/app/config \
    --name storagenode storjlabs/storagenode:latest
```
这步脚本会尝试访问`version.storj.io`，该网站国内无法直接访问，推荐安装V2RayA，在web页面管理代理服务器。

### 运行节点
下载下面命令中的以太坊钱包`WALLET`，邮箱`EMALL`，公网地址`ADRESS`，身份文件夹`<identity-dir>`，存储文件夹`<storage-dir>`。如果你还没有地址就选
```bash title="以太坊收款"
docker run -d --restart unless-stopped --stop-timeout 300 \
    -p 28967:28967/tcp \
    -p 28967:28967/udp \
    -p 14002:14002 \
    -e WALLET="0xXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" \
    -e EMAIL="user@example.com" \
    -e ADDRESS="domain.ddns.net:28967" \
    -e STORAGE="2TB" \
    --user $(id -u):$(id -g) \
    --mount type=bind,source="<identity-dir>",destination=/app/identity \
    --mount type=bind,source="<storage-dir>",destination=/app/config \
    --name storagenode storjlabs/storagenode:latest
```

如果是ZkSync收款则需要在最后加参数

```bash title="ZkSync收款" {13}
docker run -d --restart unless-stopped --stop-timeout 300 \
    -p 28967:28967/tcp \
    -p 28967:28967/udp \
    -p 14002:14002 \
    -e WALLET="0xXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" \
    -e EMAIL="user@example.com" \
    -e ADDRESS="domain.ddns.net:28967" \
    -e STORAGE="2TB" \
    --user $(id -u):$(id -g) \
    --mount type=bind,source="<identity-dir>",destination=/app/identity \
    --mount type=bind,source="<storage-dir>",destination=/app/config \
    --name storagenode storjlabs/storagenode:latest \
    --operator.wallet-features=zksync-era
```


例如我的钱包地址是`0x0dd710677e7fe8efdd0650bc1820277796195535`（欢迎给我转钱！但是不许转诈骗博彩搞来的黑钱！），身份文件夹是`/home/vincent/.local/share/storj/identity/storagenode`，存储文件夹是`/mnt/storj`

理论上你的节点已经在运行了。

### 查看仪表盘
web方式
```
http://<your-nodes-local-ip>:14002/
```
cli方式
```bash
docker exec -it storagenode /app/dashboard.sh
```

## 参考文献
[storj节点部署文档](https://storj.dev/node/get-started/setup)

[docker engine安装手册](https://docs.docker.com/engine/install)

[ZkSync支付方式](https://storj.dev/node/payouts/zk-sync-opt-in-for-snos)