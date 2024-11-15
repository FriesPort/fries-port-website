---
sidebar_position: 4
---

# 自定义域名

需要在github绑定域名

## 在个人设置页面绑定顶级域名

为了阻止其他人使用你的域名，推荐在个人设置中绑定顶级域名。github会要求在dns设置一个TXT解析，以验证你对这个域名的所有权。

## 在仓库绑定目标域名

提交后会在根目录生成CNAME文件，将该文件挪至static下面，构建静态页面时会自动复制到根目录。

在dns中将目标子域名cname解析到你的仓库github pages。例如我计划使用`www.friesport.ac.cn`作为目标域名，则需要将域名解析到github pages页面`friesport.github.io/fpac`