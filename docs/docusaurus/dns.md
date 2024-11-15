---
sidebar_position: 4
---

# 在github pages中使用自定义域名

为github pages配置自定义域名，以在自定义域名中使用由github pages托管的网站。需要在github绑定域名。

## 在个人设置页面绑定顶级域名

为了阻止其他人使用你的域名，推荐在个人设置中绑定顶级域名。github会要求在dns设置一个TXT解析，以验证你对这个域名的所有权。

## 在仓库绑定目标域名

提交后会在根目录生成CNAME文件，将该文件挪至static下面，构建静态页面时会自动复制到根目录。

在dns中将目标子域名cname解析到你的仓库github pages。例如我计划使用`www.friesport.ac.cn`作为目标域名，则需要将子域名`www`用cname方法解析到仓库的github pages页面`friesport.github.io/fries-port-website`


## 参考文献

[github pages快速入门](https://docs.github.com/zh/pages/quickstart)

[github pages验证自定义域名](https://docs.github.com/zh/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages)

[github pages配置子域名](https://docs.github.com/zh/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-a-subdomain)

[github pages配置自定义域的dns记录](https://docs.github.com/zh/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#dns-records-for-your-custom-domain)