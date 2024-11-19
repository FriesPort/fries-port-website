---
sidebar_label: "部署storj节点"
---

# 设置节点

硬盘的文件系统最好与操作系统一致，即Windows下使用NTFS，Linux下使用ext4。

## 预设


分区
```bash
gdisk /dev/sda
n # 添加新分区
1 # 分区代码
w # 保存工作

```
使用的单个硬盘单个分区，剩下的一路回车就完事。
```bash
mkfs.ext4 /dev/sda1 # 格式化刚刚添加的分区
```
格式化刚刚创建的分区。


## 获取授权key

## 设置端口转发

## 设置QUIC

## 创建认证

## 安装节点

## 参考文献
[storj部署文档](https://storj.dev/node/get-started/prerequisites)