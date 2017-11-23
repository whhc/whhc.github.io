---
layout:  post
title: "Learnyounode #9 Juggling async"
date: 2017-06-09 20:40:00 +0800
categories: articles
excerpt: "Learn node by learnyounode, not really usefull for guys had know somethings about Node."
tags: Node.js
---

### [https://nodeschool.io](https://nodeschool.io/)

很适合入门.我是从[learnyounode](https://www.github.com/workshopper/learnyounode) 开始入手,在今天进行到JUGGLING ASYNC这一步.`Juggling async`之前都很顺利.

`Juggling async`完成后的代码:

```Javascript
var http = require('http');
var bl = require('bl');
var results = [];
var count = 0;

function printResults(){
  for(var i = 0;i < 3;i ++){
    console.log(results[i])
  }
}

function httpGet (index){
  http.get(process.argv[index + 2], function (res) {
    res.pipe(bl(function (err, data) {
      if (err) {
          return console.error(err);
        }
        // results.push(data.toString());
      results[index] = data.toString();
      count++;
      if(count === 3){
          printResults();
      }
    }))
  })
}

for(var i = 0;i < 3;i ++){
  httpGet(i);
}
```

主要是注释掉的这一行,在同步执行中`results.push(data.toString())`和`results[index] = data.toString()`并没有什么区别.但在这段代码里,`http.get()`时并不能保证返回结果的顺序是按照请求时的`URLs`的顺序,所以如果`push`,则`results`中接受到的顺序是返回的顺序,而不是请求的顺序,执行结果如下:

```javascript

1.  ACTUAL:    "He hasn't got a sanger also lets get some fairy floss. Flat out like a grog and she'll be right spewin'. Gutful of ciggies heaps flat out like a g'day. As stands out like rack off mate trent from punchy lurk. "
1.  EXPECTED:  "Shazza got us some hoon bloody she'll be right smokes. As dry as a ratbag no dramas come a aerial pingpong. "

2.  ACTUAL:    "Gutful of lippy when built like a top end. Mad as a bushranger heaps lets throw a sleepout. Built like a troppo also as cross as a dunny. As stands out like larrikin no worries she'll be right truckie. Grab us a chuck a yewy heaps as cross as a whinge. "
2.  EXPECTED:  "He hasn't got a sanger also lets get some fairy floss. Flat out like a grog and she'll be right spewin'. Gutful of ciggies heaps flat out like a g'day. As stands out like rack off mate trent from punchy lurk. "

3.  ACTUAL:    "Shazza got us some hoon bloody she'll be right smokes. As dry as a ratbag no dramas come a aerial pingpong. "
3.  EXPECTED:  "Gutful of lippy when built like a top end. Mad as a bushranger heaps lets throw a sleepout. Built like a troppo also as cross as a dunny. As stands out like larrikin no worries she'll be right truckie. Grab us a chuck a yewy heaps as cross as a whinge. "

```

而`results[index] = data.toString()`由于指定了下标`index`,确保了请求与返回顺序一致,通过了校验.
