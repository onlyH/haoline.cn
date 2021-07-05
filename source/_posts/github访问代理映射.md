---
title: github访问代理映射 
date: 2021-07-02 10:39:40 
tags: [编程,技巧]
keywords: github访问 
copyright: true
---
<blockquote class="blockquote-center">
为本地 hosts 文件添加 Github 相关映射
</blockquote>
<!-- more -->

1. 打开本地 hosts 文件 win + r => `C:\WINDOWS\system32\drivers\etc`进入
2. 获取 Github 相关网站的 IP

- 访问 https://www.ipaddress.com
- 输入 github.global.ssl.fastly.net 和 github.com，查询ip地址

3. 将查询到的内容填写到hosts文件中：

```shell
140.82.112.3      github.com
199.232.69.194    github.global.ssl.fastly.net
```

4. 使用`ping github.com`查询是否配置成功
    