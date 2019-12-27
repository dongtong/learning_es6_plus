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