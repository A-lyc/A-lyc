---
title: vue服务器配置示例
date: 2022-02-27 18:32:25
tags: vue服务器配置示例
category: vue服务器配置示例
---
# Apache
```shell 

<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>
```
也可以使用 FallbackResource 代替 mod_rewrite

# nginx
```shell
location / {
  try_files $uri $uri/ /index.html;
}
```

# 原生 Node.js
```shell 
const http = require('http')
const fs = require('fs')
const httpPort = 80

http
  .createServer((req, res) => {
    fs.readFile('index.html', 'utf-8', (err, content) => {
      if (err) {
        console.log('We cannot open "index.html" file.')
      }

      res.writeHead(200, {
        'Content-Type': 'text/html; charset=utf-8',
      })

      res.end(content)
    })
  })
  .listen(httpPort, () => {
    console.log('Server listening on: http://localhost:%s', httpPort)
  })

```


#  Express + Node.js

 对于 Node.js/Express，可以考虑使用 connect-history-api-fallback 中间件。
 
 Internet Information Services (IIS)#
 安装 IIS UrlRewrite
 在网站的根目录下创建一个 web.config 文件，内容如下：
```shell
 <?xml version="1.0" encoding="UTF-8"?>
 <configuration>
   <system.webServer>
     <rewrite>
       <rules>
         <rule name="Handle History Mode and custom 404/500" stopProcessing="true">
           <match url="(.*)" />
           <conditions logicalGrouping="MatchAll">
             <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
             <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />
           </conditions>
           <action type="Rewrite" url="/" />
         </rule>
       </rules>
     </rewrite>
   </system.webServer>
 </configuration>
```

# 其他看地址
vue router后台配置：https://router.vuejs.org/zh/guide/essentials/history-mode.html#netlify
