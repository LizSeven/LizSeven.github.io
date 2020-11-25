---
layout: post
title: "vue编译自动更新packcage版本"
categories: vue
author: "Liz"
---

vue项目编译时候，需要显示一个版本号，本文的方式可以编译时自动更新packcage.json版本号

vue.config.js中定义插件


```js
chainWebpack: config => {
  const VersionPlugin = require('./src/versionPlugin')
  config.plugin('version').use(VersionPlugin).tap(args => {
    return args
  })
}
```


versionPlugin读取package.json修改版本号


```js
const fs = require('fs')
const path = require('path')
const sep = path.sep

function VersionPlugin (options) {
  this.options = options || {}
}

VersionPlugin.prototype.apply = function (compiler) {
  var self = this
  compiler.plugin('afterPlugins', function (params) {
    const packageJsonPath = path.join(params.context, sep + 'package.json')
    const dateStr = getDateStr()
    let packageJsonStr = fs.readFileSync(packageJsonPath, 'utf8')
    const r = new RegExp('(?<=version\\":\\s*\\")(.*)(?=")')
    packageJsonStr = packageJsonStr.replace(r, "1.0." + dateStr)
    fs.writeFileSync(packageJsonPath, packageJsonStr, 'utf8')
  })
}
function getDateStr () {
  const now = new Date()
  return now.getFullYear() + format(now.getMonth() + 1) + format(now.getDate()) + format(now.getHours()) + format(now.getMinutes())
  function format (num) {
    return num < 10 ? '0' + num : '' + num
  }
}

module.exports = VersionPlugin
```

