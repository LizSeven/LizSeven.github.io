---
layout: post
title: "vue运行时偶尔会出现无法找到`node-sass`的问题"
categories: vue
author: "Liz"
---

可使用以下命令
`
npm i node-sass -D
`

不过有时候上一个命令会安装失败，那么可继续尝试下下面这个命令

`
npm install node-sass --sass-binary-site=http://npm.taobao.org/mirrors/node-sass
`