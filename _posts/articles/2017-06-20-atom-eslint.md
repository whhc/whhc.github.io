---
layout: post
title:  "Use ESLint in Atom"
date:   2017-06-20 22:36:00 +0800
categories: articles
tags: Atom Node.js
---

### Atom ESLint

* 安装`linter`和`linter-eslint`;
* `Disable when no ESLint config is found`选项去掉(取消勾选是为了能够应用默认配置)
* 勾选`Use global ESLint installation`(使用全局配置)

### 安装ESLint

* <del>`npm install eslint -g`;</del>
* <del> `eslint -v`(验证安装是否成功);</del>
* <del>`eslint --init`(非必须);</del>
* <del>`npm install eslint-config-airbnb -g`(Airbnb标准);</del>

> 由于版本的原因,`eslint`现已更新至4.0.0;参照官网上的安装步骤应该是先确定`eslint-config-airbnb`依赖的版本号再安装相应依赖,当前(2017/6/20)正确步骤如下:

* `npm info "eslint-config-airbnb@latest" peerDependencies`显示依赖;
* `npm install --save-dev eslint-config-airbnb e:slint@^#.#.# eslint-plugin-jsx-a11y@^#.#.# eslint-plugin-import@^#.#.# eslint-plugin-react@^#.#.#`;
* 创建`.eslintrc`文件,简单示例如下:

```javascript
{
  "extends": "airbnb",
  "installedESLint": true,
  "plugins": [
    "react"
  ],
  "env": {
    "browser": true,
    "node": true,
    "jasmine": true
    },
  "rules": {
    "linebreak-style": 0,
    "no-console": 0
    }
}
```

> 很多检查出的错误可能并不是语法或者格式的错误,可以根据[ESLint规则列表](http://eslint.org/docs/rules/)来检查出错的原因,然后判断是否需要在`rules`中添加规则;

如果全局应用在windows系统存放在`C:\user\username\`下;否则就在项目根目录下;

