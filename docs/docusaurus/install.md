---
sidebar_position: 2
---

# Docusurus安装

## 先决条件
- 已安装node.js
- (可选)魔法上网

## 步骤
在终端运行命令，你需要将`fries-port-website`改为你自己的项目名。
```bash
npx create-docusaurus@latest fries-port-website classic
```
我的命令使用经典模板，且没有使用typescript，如果你需要启用typescript，请在后面加上参数`--typescript`。其他包管理请参见参考文档。

进入文件夹安装依赖后可以尝试启动服务，如果成功，应该可以在[`http://localhost:3000`](http://localhost:3000)访问。
```bash
cd fries-port-website
npm install
npm run start
```


## 参考文档
[Docusurus安装流程](https://docusaurus.io/zh-CN/docs/installation)