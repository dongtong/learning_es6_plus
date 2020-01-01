# Learning ES6+
* Let 和 Const 命令
* 箭头函数
* 变量, 数组解构
* 字符串扩展
* 数组扩展
* 对象扩展
* Async/Await(ES7)
* Promise(Bluebird)
* Class
* Module 语法
* 编程风格(Guideline)


## 定义对象

### let

var 定义变量是函数级的作用域.

变量申明提升(hoisting)

```js
for (var i = 0; i < 10; i++) {
  console.log(i);
}

console.log("Out i:", i);

if (true) {
  var j = 1;
}
console.log("Out j:", j);
```

let 是块级别作用域,声明之前不能被读写（**暂时性死区**）。

```js
if (false) {
  let a = 100;
}

console.log(a);

b++; // 暂时性死区
let b = 1;
```

### const

const 定义变量的本质是什么？

指针引用，不能直接改变变量类型。

能用const 定义的变量不要用let 去定义。

```js
const name = "foobar";
name = "no one";

const person = {
  name: "foobar"
};

person.name = "no none";
console.log(person.name);

const arr = [];
arr.push(1)
console.log(arr);
```

## 数组使用

迭代器方法取代原始的循环(for, while)

原始遍历(< ES6)

```js
const arr = ["JavaScript", "CSS", "HTML", "Node"];
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
```

在处理数组对象的时候，想想用什么方法处理比较合适。

- forEach

```js
const arr = ["JavaScript", "CSS", "HTML", "Node"];
// arr.forEach(lang => console.log(lang));
function print(item) {
  console.log(item);
}
arr.forEach(print);
```

可以将匿名函数分离出来, 在业务比较复杂，或者匿名函数可以重用的情况下，这个迭代器的优势就体现出来。
以下几个方法都适用。

```js
const arr = ["JavaScript", "CSS", "HTML", "Node"];
let langs = "I am good at: ";
const iMaster = lang => {
  langs += lang + ",";
};

arr.forEach(iMaster);
console.log(langs);
```

使用场景： ???

- map

转化数组中的元素, 会创建一个新的数组。

```js
const arr = [1, 2, 3];
const arr2 = arr.map(item => item * item);
console.log(arr2);
console.log(arr);
```

上面的 forEach 能实现这样的功能吗？

过滤一个对象数组，只获取特定的属性值

```js
const arr = [
  {
    lang: "JavaScript",
    rate: 5
  },
  {
    lang: "Node",
    rate: 4
  }
];

const langs = arr.map(item => item.lang);
console.log(langs);
```

- filter

从一个数组中过滤出符合条件的元素, 生成新的数组。

```js
const arr = [
  {
    lang: "JavaScript",
    rate: 5
  },
  {
    lang: "Node",
    rate: 4
  },
  {
    lang: "Ruby",
    rate: 3
  }
];
const targets = arr.filter(
  item => item.lang === "Node" || item.lang === "Ruby"
);
console.log(targets);
console.log(arr);
```

- find

找到第一个符合条件的元素。

```js
const arr = [
  {
    lang: "JavaScript",
    rate: 5
  },
  {
    lang: "Node",
    rate: 4
  },
  {
    lang: "Ruby",
    rate: 4
  }
];
const target = arr.find(item => item.rate === 4);
console.log(target);
console.log(arr);
```

- every

判断每一个元素是否满足特定条件

```js
const arr = [
  {
    lang: "JavaScript",
    rate: 5
  },
  {
    lang: "Node",
    rate: 4
  },
  {
    lang: "Ruby",
    rate: 4
  }
];
const everyIsCold = arr.every(item => item.rate <= 4);
console.log(everyIsCold);
```

例如实际场景中会用来判断一个 form 表单，是不是每一个必填字段都有值。

- some

只要有一个元素满足条件。

```js
const arr = [
  {
    lang: 'JavaScript',
    rate: 5,
  },
  {
    lang: 'Node',
    rate: 4,
  },
  {
    lang: 'Ruby',
    rate: 4,
  },
];
const someIsHot = arr.some(item => item.rate > 4);
console.log(someIsHot);
```

- reduce

将数组中所有的元素累加

```js
let sum = 0;
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
// sum = sum + item;
const total = arr.reduce((sum, item) => sum + item);
console.log(total);
```

将数组中对象的属性分离出来组成一个数组(也可以使用 map 实现)。

```js
const arr = [
  {
    lang: "JavaScript",
    rate: 5
  },
  {
    lang: "Node",
    rate: 4
  },
  {
    lang: "Ruby",
    rate: 4
  }
];

// langArr 初始值是[]
const langs = arr.reduce((langArr, item) => {
  langArr.push(item.lang);
  return langArr;
}, []);

console.log(arr.map(item => item.lang))

console.log(langs);
```

## 字符串模版

常用的字符串拼接

```js
let str = "Hello";
str = str + " World!"; // 其实这个效率是高
```

字符串模版，可以格式化，更加易读, ${} 可以是任何JavaScript 表达式。

```js
let world = "world";
let str = `Hello
  ${world}`;
console.log(str);
```

```js
const getWorld = () => "World";
let world = "world";
let str = `Hello
  ${getWorld()}`;
console.log(str);
```

## 箭头函数

和普通函数对比

- 精简易读
- 作用域扁平化
- 可以bind到另外一个作用域

```js
  const obj = {
    greet: () => console.log('foobar')
  };

  const a = ((obj) => {
    console.log(global)
    obj.greet();
  }).bind(global, obj);

  a();
```

## 对象字面量表示

一般创建对象

```js
  const person = {
    name: 'foobar',
    age: 34,
  };
```

新语法可以这样

```js
  const name = 'foobar';
  const age = 34;
  const person = {
    name,
    age,
  }
```

## 函数参数默认值

默认参数只能作为最后一个？(最后一个没有设置默认值的)

```js
  const greet = (name, message = 'Hello World') => {
    console.log(`${name} is speaking: ${message}`);
  }

  greet('You');
```

解构对象作为默认参数


选择对象中某些属性。

```js
  const greet = (params = {name, message}) => {
    const { name, message } = params;
    console.log(`${name} is speaking: ${message}`);
  }

  greet({name: 'He', message: 'fuckoff'});
```

## 剩余参数和展开操作符号(...)

拿一部分参数，剩下的参数打包成剩余参数。

剩余参数是一个数组，然后用展开操作符，把数组元素展开。

```js
  const greet = (firstGuy, ...rest) => {
    console.log(`First Guy is: ${firstGuy}`);
    console.log(`The rest are: ${rest}`);
  }

  greet('张三', '李四', '赵五');
```

分解然后再合并(flatten)

```js
  const langs1 = ['JavaScript', 'Ruby'];
  const langs2 = ['Java', 'Rust', 'Go'];
  const langs = ['Python', ...langs1, ...langs2];
  console.log(langs);
```


## 对象解构

用在访问对象中的部分或者全部的值。

```js
  function greet ({name, message}) {
    console.log(`${name} is speaking: ${message}`);
  }

  greet({name: 'foobar', message: 'Hello World'});
```

注意，在性能要求过高的场景，这种方式有性能损耗。不能解开复杂对象(有嵌套)


## 数组解构

访问数组中的每个元素， 数量要匹配

```js
  const arr = [1, 2, 3];
  const [a, b, c] = arr;
  console.log(a);
  console.log(b);
  console.log(c);
```

访问部分元素

```js
  const arr = [1, 2, 3];
  const [a, b] = arr;
  console.log(a);
  console.log(b);
```

打包剩下的元素(结合展开操作符)

```js
  const arr = [1, 2, 3];
  const [a, ...reset] = arr;
  console.log(a);
  // 这个是一个数组
  console.log(reset);
```

返回数组长度

```js
  const arr = [1, 2, 3];
  const {length} = arr;
  console.log(length);
```

交换元素

```js
  const arr = [1, 2];
  let [a, b] = arr;
  console.log(a, b);
  [b, a] = [a, b];
  console.log(a, b);
```

获取数组对象的值

```js
  const arr = [
    {
      lang: "JavaScript",
      rate: 5
    },
    {
      lang: "Node",
      rate: 4
    },
    {
      lang: "Ruby",
      rate: 4
    }
  ];
  // 先解构数组，然后解构对象
  const [{lang}] = arr;
  console.log(lang);
```

将数组变成对象

```js
  const arr = [
    ['foobar', 34],
    ['hello', 35],
  ];
  // =>
  // const people = [
  //   {
  //     name: 'foobar',
  //     age: 34
  //   },
  //   {
  //     name: 'hello',
  //     age: 35
  //   },
  // ]
```

```js
  console.log(performance.now)
  const arr = [
    ['foobar', 34],
    ['hello', 35],
  ];

  const people = arr.map(item => {
    [name, age] = item;
    return {name, age};
  });

  // const people = arr.map(([name, age]) => ({name, age}));
  console.log(performance.now)
  console.log(people);
```

## Benchmark

浏览器可以使用performance.now()结对

```js
  performance.now();
  //...
  performance.now();
```

```js
  console.time();
  const arr = [
    ['foobar', 34],
    ['hello', 35],
  ];

  const people = arr.map(item => {
    [name, age] = item;
    return {name, age};
  });

  // const people = arr.map(([name, age]) => ({name, age}));
  console.timeEnd();
  console.log(people);
```

```js
  console.time();
  const arr = [
    ['foobar', 34],
    ['hello', 35],
  ];

  const people = arr.map(item => {
    return {name: item[0], age: item[1]};
  });

  // const people = arr.map(([name, age]) => ({name, age}));
  console.timeEnd();
  console.log(people);
```

下面的效率会更好一些。今后浏览器净化，或者node V8引擎 升级后，两者的差距会更小，甚至ES6的写法会更高效。


## Class

ES5 的语法糖

ES5

```js
  function Person(options) {
    this.name = options.name;
    this.age = options.age;
  }

  Person.prototype.greet = function() {
    console.log(this.name + ' age: ' + this.age);
  }

  var p = new Person({
    name: 'Foobar',
    age: 34,
  });

  p.greet();

  function GoodMan(options) {
    // 调用父类的构造方法
    Person.call(this, options);
    this.slogan = options.slogan;
  }

  // 原型链继承
  GoodMan.prototype = Object.create(Person.prototype);
  // 可以通过访问constructor, 来判断是GoodMan对象还是Person对象
  // GoodMan.prototype.constructor = GoodMan;


  var gMan = new GoodMan({
    name: 'Foobar',
    age: 34,
    slogan: 'I am a good man.'
  });

  gMan.greet();
```

class 降级后其实是ES5的定义方式。

```js
  class Person {
    constructor(options) {
      this.name = options.name;
      this.age = options.age;
    }

    greet() {
      console.log(this.name + ' age: ' + this.age);
    }
  }

  let p = new Person({
    name: 'Foobar',
    age: 34,
  });

  p.greet();

  // extends 实现继承
  class GoodMan extends Person {
    constructor(options) {
      const {name, age, slogan} = options;
      // super 必须调用
      super({name, age});
      this.slogan = 'I am a good man.';
    }
  }

  let gMan = new GoodMan({
    name: 'Michle',
    age: 26,
  });

  gMan.greet();
```

静态方法

```js
  class Person {
    static greet() {
      console.log(' I am a good person.');
    }
  }

  Person.greet();
```

静态属性/类属性

```js
  class Person {
    // 显示声明, 不是赋值处理
    static job='Programmer'

    constructor() {
      console.log(`I am a ${Person.job}`)
    }

    describe() {
      console.log(Person.job)
    }
  }

  const p = new Person();
  p.describe();
```

## Asynchronous

### 什么是异步 (Asynchronous)

不租塞, 提供高性能访问，适合高性能的Web应用，以及API服务。

- 同步(Synchronous)

```js
  const content = readFile('./test.txt');
  const connection = connect('host', 8080);
  write(content, connection);
  close(connection);
```

语句按顺序执行，代码符合认知，容易理解

- 异步

```js
  http.get('/todos', res => {
    //....
  });
  console.log('Print this firstly...');
```

- 小结
  - 同步代码容易预测
  - 异步代码不容易预测，但是提供了高性能访问， 不租塞I/O


### 阻塞和非阻塞 (Bloking vs unBlocking)

- 阻塞

同步执行代码， 这种术语只是开发人员为了更容易开发代码而创造的一个术语。

```js
  const content = readFile('./test.txt');
  const connection = connect('host', 8080);
  write(content, connection);
  close(connection);
```

CPU, 硬盘，内存

CPU 高速硬盘调取文件内容，硬盘把内容放到内存，于此同时CPU去处理别的任务，硬盘告诉CPU 内容已经放到内存了，这时CPU 去内存读取内容，可以再写到别的目的地。

所以硬件是非阻塞的， 是异步的。程序语言造成了同步。

执行到特定语句时，CPU阻塞，切换上下文，调度资源(硬盘，内存)，等这些资源准备好了，CPU再接着处理程序。

- 非阻塞

异步执行代码，提供了良好的性能。


### 多线程编程

长久以来，提供程序性能，都采取的是多线程处理程序。什么是线程？

每一个处理器(process), 调度硬盘，内存等资源，处理程序。一般最大线程数和处理器的数量是相同的。这是简单模型，但是也可以多个线程共享资源，注意安全调度。所以线程很难控制(Go 用Channel实现)，对共享的资源读写。后来有了锁的控制方式。

锁也分为死锁和乐观锁。。。

多线程可以实现高性能的Blocking 代码， 每一个线程处理一个blocking 任务，只有当所有线程都blocking了，CPU 才Blocking， 在开始的时候，几个线程可以同时处理。

所以线程是共享内存的处理器。但是共享内存，就会发生条件竞争或者叫资源竞争(Race),使用死锁或者乐观锁会增加复杂度。当一个线程block, 另外一个线程接管任务，编写高性能多线程代码是复杂的。

### 事件驱动编程(Event Driven)

Node 的异步是通过事件循环来实现的。

- Chrome, Node以及其他很多语言采取这种编程模型。

- Node 把所有的I/O 操作都抽象到Event, 这些Event都通过显示的eventloop 来处理。Chrome 和它差不多，但是还有一些区别。

- 首先Node是一个单线程。 JavaScript 也是单线程的。Node 使用一个主线程，然后使用Event来同步状态。

```js
  const fs = require('js');

  fs.readFile('test.txt', (err, data) => {
    // 告诉主线程，数据已经读取完毕
  });
  console.log('Call this before file loaded');
```

过程: 在一个主线程中(事件循环中)，遇到fs.readFile, 然后起另外一个线程，将读取文件的任务推到队列中，这个线程去硬盘读取文件，主线程正常做其他操作。等队列中的读取文件完成，推到内存里面，然后告诉CPU, CPU知道后，告诉我主线程。主线程处理popup 出来的任务。

代码没有阻塞，是因为一直有一个事件循环在运行。


理解一下：

```js
  http.get('...', res => {
    res.on('data', chunck => {

    });
    res.on('end', () => {

    })
  });

  console.log('Call this before http response comes back.');
```

事件驱动编程模型，让Node 可以容易编写高性能的程序。

### 异步模式

- Callback

- Promise

- Async/Await

- Generators

- setInterval (+浏览器)

- setTimeout (+浏览器)

- setImmediate

- process.nextTick

- readFile


### 回调(Callback)

以下代码为什么回出错？

```js
function doAsyncTask(cb) {
  cb();
}

doAsyncTask(_ => console.log(message));

let message = "Callback Called";
```

callback 默认不是异步的， 这里是一个同步调用```_ => console.log(message)```

可以将callback 使用setImmediate或者process.nextTick包装。

```js
function doAsyncTask(cb) {
  setImmediate(() => {
    console.log('Async Task Calling Callback');
    cb();
  });
}

doAsyncTask(_ => console.log(message));

let message = "Callback Called";
```

或者

```js
function doAsyncTask(cb) {
  process.nextTick(() => {
    console.log("Async Task Calling Callback");
    cb();
  });
}

doAsyncTask(_ => console.log(message));

let message = "Callback Called";
```


callback处理错误时，错误对象是第一个参数。下面的代码没有把错误继续传递下去。如何将当前错误stack传递到下一个callback里面

```js
const fs = require("fs");

function readFileThenDo(next) {
  fs.readFile("./blah.nofile", (err, data) => {
    next(data);
  });
}

readFileThenDo(data => {
  console.log(data);
});
```

解决方法

```js
const fs = require("fs");

// next 是下一个callback
function readFileThenDo(next) {
  fs.readFile("./blah.nofile", (err, data) => {
    if (err) {
      // 下一个callback同样遵循第一个是错误对象
      next(err);
    } else {
      next(null, data);
    }
  });
}

// 这里的error,对应上面的next(err),如果没有error对应上面的next(null, data)
readFileThenDo((err, data) => {
  if (err) {
    console.error(err);
  } else {
    console.log(data);
  }
});
```

所以callback 传递必须遵守```(err, data, ....) => {}``` 这种格式。

什么是回调地狱(callback hell)

```js
function doAsyncTask(cb) {
  // 记得上面还有什么方法让callback编程异步？
  setImmediate(() => {
    console.log("Async Task Calling Callback");
    cb();
  });
}

// 这里的callback 不一定是同一个， 只要callback 是异步的即可。
doAsyncTask(() => {
  doAsyncTask(() => {
    doAsyncTask(() => {
      doAsyncTask(() => {
        doAsyncTask(() => {
          doAsyncTask(() => {
            doAsyncTask(() => {
              doAsyncTask(() => {
                doAsyncTask(() => {
                  doAsyncTask(() => {});
                });
              });
            });
          });
        });
      });
    });
  });
});
```

如何捕捉回调地狱的异常

```js
const fs = require("fs");

function readFileThenDo(next) {
  fs.readFile("./blah.nofile", (err, data) => {
    // 这里发生了异常怎么捕捉
    if (err) throw err;
    next(data);
  });
}
// Hint use try..catch
readFileThenDo(data => {
  console.log(data);
});
```

如果错误严重，需要throw出来。```使用try...catch并不能完美的捕捉异步代码(callback)的异常```，它仅仅对同步代码起作用。在异步代码里面出错，就把Error抛出来。

```js
const fs = require("fs");

function readFileThenDo(next) {
  // 异步代码
  fs.readFile("./blah.nofile", (err, data) => {
    // 出错就抛出
    if (err) throw err;
    next(null, data);
  });
}

try {
  // 异步代码
  readFileThenDo((_, data) => console.log(data));
} catch (err) {
  // 根本捕捉不到callback异步代码
  // 这里只能等待上面的执行结束后，如果有错误才能捕捉到
  console.error('catch error:', err);
}
```


### Promise

ES6 有内建的异步模式(Promise)

```promise作为未来的保留字，所以这里用Promise```

它提供了和Callback 相同的功能，但是它的语法更加清晰，也很容易捕捉异步代码的异常。

实例化一个promise对象(使用new Promise()), callback里面有两个参数，一个是resolve(成功回调函数), 一个是reject(失败回调函数).

```js
const promise = new Promise((resolve, reject) => {
  // resolve? reject?
});
```


在里面执行一段异步代码，然后调用`resolve()`正常退出。

```js
const promise = new Promise((resolve, reject) => {
  // 1m 以后打印，正常退出
  setTimeout(() => {
    console.log("Async Work Complete");
    resolve(); // <-- Resolving
  }, 1000);
});
```

通常，我们封装一个函数，将函数promise化, promise 里面需要resolve或者reject。

```js
function doAsyncTask() {
  // <-- NOTE: Not passing in a callback

  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("Async Work Complete");
      resolve(); // <-- Resolving
    }, 1000);
  });
  return promise; // <-- Return the promise
}
```

失败调用reject callback. promise 执行完成后，如果成功(resolve)，调用then, 如果失败(reject)，调用catch。

```js
function doAsyncTask() {
  let error = true;
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("Async Work Complete");
      if (error) {
        reject(); // <-- Rejecting
      } else {
        resolve();
      }
    }, 1000);
  });
  return promise;
}
doAsyncTask()
.then(() => {
  console.log('success...');
})
.catch(() => {
  console.log('failed...');
});
```

then 可以接受两个参数，第一个是resolve handler, 第二个是reject handler

```js
function doAsyncTask() {
  let error = true;
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("Async Work Complete");
      if (error) {
        reject(); // <-- Rejecting
      } else {
        resolve();
      }
    }, 1000);
  });
  return promise;
}
doAsyncTask()
.then(() => {
  // resolve handler
  console.log('success...');
}, () => {
  // reject handler
  console.log('failed...');
});
```

注意上面的then()...catch()和这里的then 接受两个参数的处理效果是一样的。

resolve 和 reject可以接受参数

```js
function doAsyncTask() {
  let error = true;
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("Async Work Complete");
      if (error) {
        reject('failed!'); // <-- Rejecting
      } else {
        resolve('success!');
      }
    }, 1000);
  });
  return promise;
}
doAsyncTask()
.then((val) => {
  // resolve handler
  console.log('resolved value: ', val);
}, (val) => {
  // reject handler
  console.log('reject value: ', val);
});
```

我们可以创建一个立即resolve或者reject 的promise, 如下

```js
let resolvedPromise = Promise.resolve('done');
let rejectedPromise = Promise.reject('reject');
```

这个有什么用？

可以将一个非promise的值或者函数变成promise, 后面将会介绍，使用它可以产生一个promise chain(链).

```js
  let resolvedPromise = Promise.resolve('done');
  resolvedPromise.then(val => console.log(val));
```

即使上面Promise.resolve立即调用了，但是promise 还会执行then 回调，接受的参数和之前的一样。

还记得callback 的开始的示例吗?, 现在用promise实现

```js
function doAsyncTask() {
  return Promise.resolve();
}

doAsyncTask().then(_ => console.log(message)); // <-- Unlike callbacks, promises are always async
// 有点暂时性死区的意思
let message = "Promise Resolved";
```

#### Promise 链(Chain)

可以把多个then串联起来。

```js
const promise = Promise.resolve("done");
promise
  .then(val => {
    console.log(val);
    return "done2"; // <-- !NOTE: We have to return something, otherwise it doesn't get passed
  })
  .then(val => console.log(val));
// 'done'
// 'done2'
```

这里注意，必须要在每个then 里面return 值，这样then可以传递下去。

```js
const promise = Promise.resolve("done");
promise
  .then(val => {
    console.log(val);
  })
  .then(val => console.log(val));
// 'done'
// 'undefined'
```

then不能断，如果断了，promis 值从头开始

```js
const promise = Promise.resolve("done");
promise.then(val => {
  console.log(val);
  return "done2";
});

promise.then(val => console.log(val)); // <-- Doesn't get passed the result of the previous then
// 'done'
// 'done'
```

一个Promise执行中，可以等待另外一个promise执行完成

```js
Promise.resolve("done")
  .then(val => {
    console.log(val);

    return new Promise(resolve => {
      setTimeout(() => resolve("done2"), 1000);
    });

    // The next then waits for this promise to resolve before continueing
  })
  .then(val => console.log(val));
```

也可以同时等待几个promise一起执行完成(使用原生的，或者bluebird)

```js
Promise.all([
  Promise.resolve("done"),
  new Promise(resolve => {
    setTimeout(() => resolve("done2"), 1000);
  })])
  .then(([p1, p2]) => {
    console.log('p1: ', p1)
    console.log('p2: ', p2)
  })
```

Bluebird,有一个Promise.join(), 这个性能更好一些。

#### 错误处理

Promise的错误会顺着链(chain)往下传递，直到找到错误处理(handler)。不需要为每一个then定义错误处理，只需要在最后加一个catch

```js
Promise.reject("fail")
  .then(val => console.log(val)) // <-- Note we dont have an error handler here!
  .then(val => console.log(val), err => console.error(err));
```

在Promise链上，如果throw 错误，promise传递将停止，并且被reject.这时候error handler将会被立刻执行。

```js
new Promise((resolve, reject) => {
    throw "fail";
  })
  .then(val => {
    // 这个不会被执行
    console.log(val);
  })
  .then(val => console.log(val), err => console.error(err));
// [Error: fail]
```

```js
Promise.resolve("done")
  .then(val => {
    throw "fail";
  })
  .then(val => console.log(val), err => console.error(err));
// [Error: fail]
```

上面的Error handler的另一种实现是使用catch

```js
Promise.resolve("done")
  .then(val => {
    throw "fail";
  })
  .then(val => console.log(val))
  .catch(err => console.error(err))
// [Error: fail]
```


#### Finally

不论成功或者失败，都会被执行（这个要借助第三方Promise, 目前ES6还没有实现）

```js
Promise.resolve("done")
  .then(val => {
    throw new Error("fail");
  })
  .then(val => console.log(val))
  .catch(err => console.error(err))
  .finally(_ => console.log("Cleaning Up")); // <-- Comming soon!
```

### 多个Promise一起执行

使用Node 内建工具util. (Bluebird有promisify). 可以将函数转化成Promise,前提是这个函数的参数，第一个是error。

```js
const util = require("util");
const fs = require("fs");
const readFile = util.promisify(fs.readFile);

const files = ["./files/demofile.txt", "./files/demofile.other.txt"];

let promises = files.map(name => readFile(name, "utf8"));
Promise.all(promises)
  .then(values => {
    // values 是一个数组
    // <-- Uses .all
    console.log(values);
  })
  .catch(err => console.error("Error: ", err));
```

Promise 竞争(Race)

当数组中第一个promise resolve 或者 reject的时候，返回第一个resolve 或者reject的值

```js
let car1 = new Promise(resolve => setTimeout(resolve, 1000, "Car 1."));
let car2 = new Promise(resolve => setTimeout(resolve, 500, "Car 2."));
let car3 = new Promise(resolve => setTimeout(resolve, 3000, "Car 3."));

Promise.race([car1, car2, car3]).then(value => {
  console.log("Promise Resolved", value);
});
```


```js
let car1 = new Promise((resolve, reject) =>
  setTimeout(reject, 3000, "Car 1 Crashed.")
);
let car2 = new Promise(resolve => setTimeout(resolve, 1000, "Car 2."));
let car3 = new Promise(resolve => setTimeout(resolve, 3000, "Car 3."));

Promise.race([car1, car2, car3])
  .then(value => {
    console.log("Promise Resolved", value);
  })
  .catch(err => {
    console.log("Promise Rejected", err);
  });
```

### Async/Await

之前创建一个异步的可以使用

```js
const doAsyncTask = () => Promise.resolve("done");
```

使用Promise, 可以使用then串联

```js
const doAsyncTask = () => Promise.resolve("done");
doAsyncTask().then(val => console.log(val));
console.log("here"); // <-- this is called first!
```

如果使用Async/Await, 不需要使用then

```js
const doAsyncTask = () => Promise.resolve("done");
async function asimc() {
  // <-- mark it as `async`
  // change async to sync
  let value = await doAsyncTask(); // <-- Don't need to call .then
  console.log(value);
}
asimc();

// asimc().then(val => console.log('again: ', val))
```

util.promisify工作原理和bluebird里面的Promise.promisify类似

```js
const util = require('util')
function asy(data, aa, cb) { //cb 是一个函数
  try {
    return cb(null, aa)；
  }catch(err) {
    if(err) return cb(err);
  }
}

const a = util.promisify(asy)
a(222, 333).then(val => console.log('aaa->', val)).catch(err => console.error(err))
```

Async/Await 和Promise可以混用。

既然异步代码性能更好，为什么await变成同步？（程序流程更加清晰，更加可控，这种性能损耗可以忽略）。

可以理解Asycn/Await 是promise 一种语法糖。

还可以用立即执行函数(IIFE)

```js
const doAsyncTask = () => Promise.resolve("done");
(async function() {
  // <-- IIFE, note the async
  let value = await doAsyncTask(); // <-- Don't need to call .then
  console.log(value);
})();
```

遇到await 就block了, 虽然block, 但是程序变得可控。外面还是异步的。

```js
const doAsyncTask = () => Promise.resolve("1");
(async function() {
  let value = await doAsyncTask();
  console.log(value);
  console.log("2"); //----> This waits before it's printed
})();
```

如果没有await 就变成异步

```js
const doAsyncTask = () => Promise.resolve("1");
(async function() {
  doAsyncTask().then(console.log);
  console.log("2");
})();
```

#### async 函数返回的是一个promise

既然是promise，肯定可以then, 也可以用catch 来捕捉异常。

```js
const doAsyncTask = () => Promise.resolve("1");
let asyncFunction = async function() {
  let value = await doAsyncTask();
  console.log(value);
  console.log("2");
  return "3"; // Whatever we return is like a resolve
};
asyncFunction().then(v => console.log(v)); // We can attach a then to it
```

#### 错误捕捉

异步代码使用try catch捕捉不到。后来用.catch, 或者then 的第二个函数参数捕捉。

因为通过await, 把async 变成了sync, 所以可以使用try...catch 来捕捉。catch的值是reject的值。

```js
const doAsyncTask = () => Promise.reject("hey!");
const asyncFunction = async function() {
  try {
    const value = await doAsyncTask();
  } catch (e) {
    console.error("Catch reject value: ", e);
    return;
  }
};
asyncFunction();
```

#### 租塞意味着慢

因为租塞，所以并不高效。例如读取多个文件。可以使用console.time测试一下。

```js
const util = require("util");
const fs = require("fs");
const readFile = util.promisify(fs.readFile);

const files = ["./files/demofile.txt", "./files/demofile.other.txt"];

(async () => {
  for (let name of files) {
    console.log(await readFile(name, "utf8")); // <-- One file loaded at a time, instead of all files at once
  }
})();
```

#### async 其实并不神秘

下面代码执行的结果是什么

```js
async function printLine1() {
  console.log("1");
}

async function printLine2() {
  console.log("2");
}

async function main() {
  printLine1();
  printLine2();
}
main();
console.log("Finished");
```

上面这个是同步的，下面的是异步的。

```js
async function printLine1() {
  const val = await Promise.resolve("1");
  return val;
}

async function printLine2() {
  const val = await Promise.resolve("2");
  return val;
}

async function main() {
  printLine1().then(val => console.log(val));
  printLine2().then(val => console.log(val));
}
main();
console.log("Finished");
```

async需要配上await, 才可以生成异步。

#### async 迭代器

之前提过Promise.all, 它是等待多个promise执行完成后返回一个array值。那么async 有一个实验特性 (async 迭代器)

还没有完全支持在浏览器和node中, 可以这样执行

```
node --harmony-async-iteration working.js
```

```js
(async () => {
  const util = require("util");
  const fs = require("fs");
  const readFile = util.promisify(fs.readFile);

  const files = ["./files/demofile.txt", "./files/demofile.other.txt"];
  const promises = files.map(name => readFile(name, "utf8"));
  for await (let content of promises) {
    //<-- See the await is on the for
    console.log(content);
  }
})();
```

#### 自定义迭代器

我们可用```for-await-of```迭代一个内建promise 数组。

数组是一个迭代器， 迭代器里面的元素有一个属性叫```Symbol.iterator```。 它指向迭代器中一个元素。这个元素有一个next()方法，
它返回一个对象```{done: false, value: ???}```。如果迭代器结束，返回的done是true。

使用for...of 迭代一个自定义迭代器。

```js
const customIterator = () => ({
  [Symbol.iterator]: () => ({
    x: 0, // initial value
    next() {
      // iterator <100 number
      if (this.x > 100) {
        return {
          done: true,
          value: this.x
        };
      }
      return {
        done: false,
        value: this.x++
      };
    }
  })
});

for (let x of customIterator()) {
  console.log(x);
}
```

#### 自定义async 迭代器

参照上面的列子，可以定义一个async迭代器，通过```Symbol.asyncIterator```使用for...await...of语法访问。但是确保里面返回的是一个promise对象

```js
const customAsyncIterator = () => ({
  [Symbol.asyncIterator]: () => ({
    x: 0,
    next() {
      if (this.x > 100) {
        return Promise.resolve({
          done: true,
          value: this.x
        });
      }

      let y = this.x++;

      return Promise.resolve({
        done: false,
        value: y
      });
    }
  })
});

(async () => {
  for await (let x of customAsyncIterator()) {
    console.log(x);
  }
})();
```

### Generators

Generators定义的函数和普通函数有区别

```js
  function* greet() {
    //...
  }
  let g = greet();
```

调用后不会立刻执行。普通函数声明调用后会执行完毕，Generators是运行，停止 -> 保留现场出去， 再回来运行。

- 一般函数运行后，要么完成，要么出错
- Generators可以在运行过程中停止，让你做一些别的事(异步?), 别的事做完了，再回来，从刚才出去的位置继续运行。
- 外部无法阻止内部运行, Generators只能通过自己的yield来暂停执行
- 自己停止的，自己恢复(next())

```js
function* demo() {
  console.log("1");
  yield;
  console.log("2");
}
console.log("start");
const it = demo(); // Doesn't execute the body of the function
console.log("before iteration");
console.log(it.next()); // Executes generator and prints out whats yielded
console.log(it.next()); // Returns done: true
console.log(it.next()); // Returns same ended iterator
console.log("after iteration");
```

注意next()返回的是一个对象```{value: xxx, done: <boolean>```


通过yield 把数据抛出来

```js
function* range() {
  for (let i = 0; i < 4; i++) {
    yield i; // <-- We can return data from yield
  }
  yield "moo";
  return "foo"; // 提前标志done: true, 不然到下一个next()才知道是不是done
}
const it = range();
console.log(it.next()); // { value: 0, done: false }
console.log(it.next()); // { value: 1, done: false }
console.log(it.next()); // { value: 2, done: false }
console.log(it.next()); // { value: 3, done: false }
console.log(it.next()); // { value: 'moo', done: false }
console.log(it.next()); // { value: 'foo', done: true }
console.log(it.next()); // { value: undefined, done: true }
```

遇到return 表明函数已经执行结束，后面再next() 将会获得undefined value。

使用迭代器迭代generators

```js
function* range() {
  for (let i = 0; i < 10; i++) {
    yield i;
  }
}

for (let x of range()) { // 隐式调用next()
  console.log(x); // Just prints the value
}
```

yield 通信是双向的，可以传递值出去，也可以接收值进来.

```js
function* sayWhat() {
  console.log(yield);
  console.log("World");
}
const it = sayWhat();
it.next(); // 遇到第一个yield,停止
it.next("Hello"); // 从上次停止的地方继续执行，将值传到上次yield的地方
```

#### 自定义异步Generators

使用generators和for-await-of

```js
function* range() {
  for (let i = 0; i < 10; i++) {
    yield Promise.resolve(i);
  }
}

(async () => {
  for (let x of range()) {
    console.log(x); // <-- This just prints out the promise
  }
})();

console.log('end...');
```

注意上面还是同步执行的。打印出promise对象。你可以使用for await,但是只能在迭代器自己上await.

```js
function* range() {
  for (let i = 0; i < 10; i++) {
    yield Promise.resolve(i);
  }
}

(async () => {
  for await (let x of range()) { // <-- Await in the iterator
    console.log(x); // <-- This just prints out the promise
  }
})();

console.log('end...');
```

上面就变成异步的了。（async/await要配对）