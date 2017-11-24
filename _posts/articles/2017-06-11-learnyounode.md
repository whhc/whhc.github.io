---
layout:  post
title: "Learnyounode"
date: 2017-06-11 21:45:00 +0800
excerpt: "Learn node by learnyounode, not really usefull for guys had know somethings about Node."
categories: articles
tags: node
---

### lesson 1 Hello world

```javascript
console.log('HELLO WORLD')
```

### lesson 2 Baby Step

My code:
```javascript
function sum() {
  var arg = process.argv,
    total = 0;
  if (arg[2]) {
    var len = arg.length;
    for (var i = 2; i < len; i++) {
      total += Number(arg[i]);
    }
  }
  return total;
}
console.log(sum())
```

Official code:
```javascript
var result = 0

for (var i = 2; i < process.argv.length; i++) {
  result += Number(process.argv[i])
}

console.log(result)
```

### lesson 3 My first I/O

My code:
```javascript
var fs = require('fs');
var fileName = process.argv[2];
var file = fs.readFileSync(fileName); // console.log(file instanceof Object) => true
var contents = file.toString().split('\n');
console.log(contents.length - 1);
```

Official code:
```javascript
var fs = require('fs')

var contents = fs.readFileSync(process.argv[2])
var lines = contents.toString().split('\n').length - 1
console.log(lines)
```

### lesson 4 My first async I/O

My code:
```javascript
var fs = require('fs');
var filePath = process.argv[2];
fs.readFile(filePath, 'utf8', function (err, data) {
  try {
    var lines = data.split('\n');
    console.log(lines.length - 1)
  } catch (err) {
    console.log('!error:' + err.message)
  }
})
```

Official code:
```javascript
var fs = require('fs')
var file = process.argv[2]
fs.readFile(file, function (err, contents) {
  if (err) {
    return console.log(err)
  }
  // fs.readFile(file, 'utf8', callback) can also be used
  var lines = contents.toString().split('\n').length - 1
  console.log(lines)
})
```
### lesson 5 Filtered LS

My code:
```javascript
var fs = require('fs');
var files = process.argv[2];
var ext = '.' + process.argv[3];
fs.readdir(files, function (err, data) {
  try {
    for (var i = 0; i < data.length; i++) {
      if (data[i].match(ext)) {
        console.log(data[i])
      }
    }
  } catch (err) {
    console.log('!error:' + err.message)
  }
})
```

Official code:
```javascript
var fs = require('fs')
var path = require('path')

var folder = process.argv[2]
var ext = '.' + process.argv[3]

fs.readdir(folder, function (err, files) {
  if (err) return console.error(err)
  files.forEach(function (file) {
    if (path.extname(file) === ext) {
      console.log(file)
    }
  })
})
```

### lesson 6 Make it moduler

My code:

> program.js        

```javascript
var makeItModular = require('./make-it-modular.js');
var dir = process.argv[2];
var files = process.argv[3];

var callback = function (err, list) {
  if (err) {
    throw err;
  }
  list.forEach(function (file) {
    console.log(file);
  });
}

makeItModular(dir, files, callback);
```

> make-it-modular.js        

```javascript
var fs = require('fs');
var path = require('path');

module.exports = function (directory, extension, callback) {
  fs.readdir(directory, function (err, list) {
    if (err) {
      return callback(err);
    } else {
      list = list.filter(function (file) {
        if (path.extname(file) === "." + extension) {
          return true;
        }
      })
      return callback(null, list)
    }
  })
}
```

Official code:
> program.js        

```javascript
var filterFn = require('./solution_filter.js')
var dir = process.argv[2]
var filterStr = process.argv[3]

filterFn(dir, filterStr, function (err, list) {
  if (err) {
    return console.error('There was an error:', err)
  }

  list.forEach(function (file) {
    console.log(file)
  })
})
```
> make-it-modular.js        

```javascript
var fs = require('fs')
var path = require('path')

module.exports = function (dir, filterStr, callback) {
  fs.readdir(dir, function (err, list) {
    if (err) {
      return callback(err)
    }

    list = list.filter(function (file) {
      return path.extname(file) === '.' + filterStr
    })

    callback(null, list)
  })
}
```

### lesson 7 HTTP client

My code:
```javascript
var http = require('http');
var url = process.argv[2];

http.get(url, function (res) {
  res.setEncoding('utf8');
  res.on("data", function (data) {
    console.log(data)
  })
})
```

Official code:
```javascript
var http = require('http')

http.get(process.argv[2], function (response) {
  response.setEncoding('utf8')
  response.on('data', console.log)
  response.on('error', console.error)
}).on('error', console.error)
```

### lesson 8 HTTP collect

My code:
```javascript
var http = require('http');
var info = [];
var url = process.argv[2];
http.get(url, function (res) {
  res.setEncoding('utf8');
  res.on('data', function (data) {
    info.push(data);
  });
  res.on('error', console.error);
  res.on('end', function () {
    console.log(info.join('').length);
    console.log(info.join(''));
  });
});
```
    
Official code:
```javascript
var http = require('http');
var bl = require('bl');

http.get(process.argv[2], function (res) {
  res.pipe(bl(function (err, data) {
    if (err) {
      return console.error(err);
    }
    data = data.toString();
    console.log(data.length);
    console.log(data)
  }))
})
```
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
