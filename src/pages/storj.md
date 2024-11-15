# storj的静态网站托管

storj是一个全球化的分布式存储方案，发送给storj的文件会被膨胀切割为文件片，存在不同的矿工托管的服务器上。在需要时发回给用户，站长支付storj代币给矿工。相比于oss或者s3，storj的价格比较便宜，存储`$0.003/Gb/mon`，下行`$0.007/Gb/mon`。由于使用人数尚未到达需要可以需要无视下行费用的cloudfare（下文简称cf）的R2对象存储的地步，又顺便尝试一下新东西，因此将静态网站托管在storj上。

## 设置storj

需要提前设置uplink

```bash
./uplink.exe share --dns <hostname> sj://<bucket>/<prefix> --tls --not-after=none
```
薯条港的静态网站参数如下，因为没有特殊的设置，所以直接使用根目录

```bash
./uplink.exe share --dns docs.friesport.ac.cn sj://fpa-website --tls --not-after=none
```

uplink提供了如下参数
```
Remember to update the $ORIGIN with your domain name. You may also change the $TTL.
$ORIGIN example.com.
$TTL    3600
docs.friesport.ac.cn            IN      CNAME   link.storjshare.io.
txt-docs.friesport.ac.cn        IN      TXT     storj-root:fpa-website
txt-docs.friesport.ac.cn        IN      TXT     storj-access:XXXXXXXXXXXXXXXXXXXX
txt-docs.friesport.ac.cn        IN      TXT     storj-tls:true
```
## 设置dns
我的dns已经迁移到cf，因此展示cf的设置
需要4个参数，1个cname，3个text

## 参考资料
[官方教程](https://storj.dev/dcs/code/static-site-hosting)