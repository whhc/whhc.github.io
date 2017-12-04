---
layout: post
title:  "RxJS"
date:   2017-11-03 22:27:00 +0800
excerpt: "Powerfull JS"
categories: articles
tags: rxjs
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
rename('file.txt', 'else.txt').subscribe(() => console.log('Rename'));
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
  setTimeout(() => observer.next('bar'), 1000)
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

#### Event  

```javascript
const inpt = document.querySelector('#input')

const inputStream$ = Rx.Observable.fromEvent(input, 'keyup')

inputStream$.debounceTime(150)
  .subscribe(
    e => {
      // console.log(e.target.value)
      console.log(e.currentTarget.value)
    },
    (err) => {
      console.log(err)
    },
    () => {
      console.log('completed')
    }
  )
```
#### Array, Set, Map  

```javascript
const array = [1,2,3,4,5];

const array$ = Rx.Observable.from(array);

array$.subscribe(
  v => {
    console.log(v)
  },
  err => {
    console.log(err)
  },
  () => {
    console.log('completed')
  }
)
```

#### Scratch  

```javascript
const source$ = new Rx.Observable(observable => {
  console.log('Creating Observable')
	observable.next('Nest step')
	observable.next('Another value')

	// observable.error(new Error('Error: Something went wrong'))
	
	setTimeout(() => {
		observable.next('Yet another value')
		observable.complete()
	}, 3000)
})

source$
	.catch(err => Rx.Observable.of(err))
	.subscribe(
	x => {
		console.log(x)
	},
	err => {
		console.log(err)
	},
	() => {
		console.log('completed')
	}
)
```

#### Promise  

```javascript
function getUser(username) {
	return $.ajax({
		url: `https://api.github.com/users/${username}`,
		dataType: 'jsonp'
	}).promise()
}

const inputSource$ = Rx.Observable.fromEvent($("#input2"), 'keyup')

inputSource$
	.debounceTime(500)
	.subscribe(e => {
	Rx.Observable.fromPromise(getUser(e.target.value))
		.subscribe(
			x => {
				console.log(x)
				$("#name").html(x.data.name)
				$("#blog").html(x.data.url)
				$("#repos").html("Public Repos: " + x.data.public_repos)
			}
		)
})
```

#### Interval, Timer & Range  

```javascript
// const source$ = Rx.Observable.interval(1000)
// 	.take(5)
// 
// source$.subscribe(
// 	x => {
// 		console.log(x)
// 	},
// 	err => {
// 		cconsole.log(err)
// 	},
// 	() => {
// 		console.log('completed')
// 	}
// )

// const source$ = Rx.Observable.timer(5000, 1000)
// 	.take(5)
// 
// source$.subscribe(
// 	x => {
// 		console.log(x)
// 	},
// 	err => {
// 		cconsole.log(err)
// 	},
// 	() => {
// 		console.log('completed')
// 	}
// )

const source$ = Rx.Observable.range(0, 5)

source$.subscribe(
	x => {
		console.log(x)
	},
	err => {
		cconsole.log(err)
	},
	() => {
		console.log('completed')
	}
)
```

#### Map & Pluck

```javascript
// const source$ = Rx.Observable.interval(1000)
// 	.take(10)
// 	.map(v => v * 2)
// 
// source$.subscribe(
// 	v => {
// 		console.log(v)
// 	}
// )

// const source$ = Rx.Observable.from(['John', 'Tom', 'Shawn'])
// 	.map(v => v.toUpperCase())
// 	.map(v => `I am ${v}`)
// 
// source$.subscribe(
// 	v => {
// 		console.log(v)
// 	}
// )

const users = [
	{name: 'Will', age: 34},
	{name: 'Mike', age: 33},
	{name: 'Paul', age: 35}
]

const users$ = Rx.Observable.from(users)
	.pluck('name')

users$.subscribe(
	x => {
		console.log(x)
	}
)
```

#### Merge & Concat  

```javascript
// Rx.Observable.of('Hello')
// 	.merge(Rx.Observable.of('Everyone'))
// 	.subscribe(
// 		x => {
// 			console.log(x)
// 		}
// 	)
// 
// Rx.Observable.interval(2000)
// 	.merge(Rx.Observable.interval(500))
// 	.take(25)
// 	.subscribe(x => console.log(x))

// const source1$ = Rx.Observable.interval(2000).map(v => `Merge1: ${v}`)
// const source2$ = Rx.Observable.interval(500).map(v => `Merge2: ${v}`)
// 
// Rx.Observable.merge(source1$, source2$)
// 	.take(25)
// 	.subscribe(x => console.log(x))
	
const source1$ = Rx.Observable.range(0, 5).map(v => `source1: ${v}`)
const source2$ = Rx.Observable.range(6, 5).map(v => `source2: ${v}`)

Rx.Observable.concat(source1$, source2$)
	.subscribe(x => console.log(x))
```
