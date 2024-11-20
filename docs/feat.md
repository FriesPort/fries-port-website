---
sidebar_position: 1
---
# 待办

- [ ] 部署storj节点
- [ ] 添加一个自定义侧边栏
- [ ] 找一个不限流量支持udp的内网穿透
- [ ] 学习frp的使用
- [x] ~~调整网站颜色红色#ea5a47,黄色#fcea2b~~ 最后选了ant design预设金色#faad14

---

## 笔记
记录一些内网穿透供应商，防止后面找不到

### ChmlFrp
https://www.chmlfrp.cn/

frp穿透，限制带宽不限流量。有永久套餐。
### fnat
https://www.fnat.cc

frp穿透，永久套餐限带宽流量，月付套餐只限制带宽。

### cloudflare tunnel
https://www.cloudflare.com/zh-cn/products/tunnel/

服务器与客户端均为自研软件，客户端需要安装cloudflared，公网域名仅支持tcp穿透，不支持udp。cloudflare作为赛博大善人，没有特别针对tunnel收费。除了延迟有点高之外其他都很棒，无可挑剔。

### 天霆网络
https://www.idc35.com/cloud/hktehui

服务器，位于香港，最低99元/年。不限流量，带宽10M。价格还行。