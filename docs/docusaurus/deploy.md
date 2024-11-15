---
sidebar_position: 3
---

# 部署Docusurus至github pages

有多种方式部署，最简单的就是直接在服务器上部署，只需要执行下述命令
```bash
npm run build
npm run serve
```
最后映射3000端口至公网即可。可以配合cloudflare tunnel或者内网隧穿等方式实现。

但是我手边没有能够24小时稳定连接网络的设备，因此我选择托管到免费的github pages。理论上Docusaurus可以部署到任何提供对象存储或者提供静态网站托管的服务商。

## 部署到github pages
每个仓库的网站会托管到指定域名，对于薯条港的fpac仓库来说，这个域名是[`friesport.github.io/fpac`](https://friesport.github.io/fpac)。github会从根目录的`.github/workflows`文件夹读取action配置。从官网抄作业，在这个文件夹下创建action配置文件。注意，官网使用的是yarn，这里我替换为npm，如果需要yarn包管理器的请摘抄官网的。
```yaml title=".github\workflows\deploy.yml"
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main
    # Review gh actions docs if you want to further define triggers, paths, etc
    # https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#on

jobs:
  build:
    name: Build Docusaurus
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - uses: actions/setup-node@v4
        with:
          node-version: 18
          cache: npm

      - name: Install dependencies
        run: npm ci
      - name: Build website
        run: npm run build

      - name: Upload Build Artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: build

  deploy:
    name: Deploy to GitHub Pages
    needs: build

    # Grant GITHUB_TOKEN the permissions required to make a Pages deployment
    permissions:
      pages: write # to deploy to Pages
      id-token: write # to verify the deployment originates from an appropriate source

    # Deploy to the github-pages environment
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```
另外官网还提供了一份test-deploy的文件，不确定有没有用，反正我也修改了。
```yaml title=".github\workflows\test-deploy.yml"
name: Test deployment

on:
  pull_request:
    branches:
      - main
    # Review gh actions docs if you want to further define triggers, paths, etc
    # https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#on

jobs:
  test-deploy:
    name: Test deployment
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - uses: actions/setup-node@v4
        with:
          node-version: 18
          cache: npm

      - name: Install dependencies
        run: npm ci
      - name: Test build website
        run: npm run build
```

在github新建仓库，在本地根目录初始化git仓库，在git中关联远程和本地仓库。然后提交代码并推送。

```git
git init
git remote add github ...
git add *
git commit -m "first commit"
git push github main
```

创建一个新分支gh-pages，创建后在仓库的pages中设置推送到gh-pages。

可能需要修改环境保护规则，使main分支能读写github-pages环境。

耐心等待action完成任务。

## 参考文档
[Docusurus部署](https://docusaurus.io/zh-CN/docs/deployment)

