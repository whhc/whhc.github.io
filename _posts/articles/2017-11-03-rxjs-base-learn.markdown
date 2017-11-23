---
layout: post
title:  "RxJS"
date:   2017-11-03 22:27:00 +0800
excerpt: "Powerfull JS"
categories: articles
tags: RxJS
---
#### 资料
[RxJS中文版文档](http://cn.rx.js.org/)

### Observable
#### 转换成 `observable`
```javascript
Rx.Observable.of('foo','bar')
Rx.Observable.from([1,2,3])
Rx.Observable.fromEvent(document.querySelector('button'),'click')
Rx.Observable.fromPromise(fetch('/user'))

// 来自回调
//fs.exists = (path, cb(exists))
var exists = Rx.Observable.bindCallback(fs.exists)
exists('file.txt').subscribe(exisits => console.log('Does file exists?', exists))

//
// fs.rename =(pathA, pathB, cb(err, result))
var rename =Rx.Observable.bindNodeCallback(fs.rename)
rename('file.txt', 'else.txt').subscribe(() => console.log('Rename));
```

#### 创建`obervables`

```javascript
// 在外部产生新事件
var myObservable =new Rx.Subject()
myObservable.subscribe(value => console.log(value))
myObservable.next('foo')


//在内部产生新事件
var myObservable =Rx.Observable.create(observer => {
  observer.next('foo');
  setTimeout(() => observer.next('bar), 1000)
})
myObservable.subscribe(value => console.log(value))

```
		
#### 控制流动		
				
```javascript		
var input =Rx.Observable.fromEvent(document.querySelector('button'), 'input')		
		
// 过滤		
input.filter(event => event.target.value.length > 2)		
// 延迟事件		
// input.delay(200)		
// 每200ms只能通过一个事件		
// input.throttleTime(200)		
// 停止输入后200ms方能通过最新的那个事件		
// input.debounceTime(200)		
// 在3次事件后停止事件流		
// input.take(3)		
  .map(event => event.target.value)		
  .subscribe(value => console.log(value))		
		
		
// 直到其他observable触发事件才停止事件流		
var stopStrem =Rx.Observable.fromEvent(document.querySelector('button'),'click')		
input.takeUntil(stopStrem)		
  .map(event => event.target.value)		
  .subscribe(value => console.log(value))		
```