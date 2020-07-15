---
title: 【学习笔记】之es6
tags: [学习笔记, es6]
categories: [学习笔记]
---
### let和const
  > let和const为块级作用域，var为全局变量，const声明一个只读的常量。一旦声明，常量的值就不能改变。
### Arrow Function 箭头函数
  ``` js
  const number = [5, 16, 80, 4]
  number.map(function(num, index){
    console.log(num, index)
  })
  number.map((num,index) => {
    console.log(num)
  })
  ```
### 模板字符
  ``` js
    let template = `User ${user.name} is not authorized to do ${user.age}.`
    let template = `
    <ul>
      <% for(let i=0; i < data.supplies.length; i++) { %>
        <li><%= data.supplies[i] %></li>
      <% } %>
    </ul>
    `
  ```
### 字符串的新增方法
  * includes()：返回布尔值，表示是否找到了参数字符串
  * startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部
  * endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部
  ``` js
  let s = 'Hello world!'
  s.startsWith('world', 6) // true
  s.endsWith('!') // true
  s.endsWith('Hello', 5) // true
  s.includes('Hello', 6) // false
  ```
  * repeat(): 返回一个新字符串，表示将原字符串重复n次
  ``` js
  'x'.repeat(3) // "xxx"
  ```
  * padStart(): 用于头部补全
  * padEnd(): 用于尾部补全
  ``` js
  'x'.padStart(4) // '   x'
  '1'.padStart(10, '0') // "0000000001"
  09-12'.padStart(10, 'YYYY-MM-DD') // "YYYY-09-12"
  ```
  * trim(): 去除空格
  * trimStart()
  * trimStart()
  ``` js
  const s = '  abc  ';
  s.trim() // "abc"
  s.trimStart() // "abc  "
  s.trimEnd() // "  abc"
  ```
### 正则的扩展
  * match()
  * replace()
  * search()
  * split()
  ``` js
  let str="1 plus 2 equal 3"
  str.match(/\d+/g) // 1,2,3
  str.replace(/\d+/g, "@") // @ plus @ equal @
  str.search("plus") // 2
  str.split(" ") // ["1", "plus", "2", "equal", "3"]
  ```
### 数值的扩展
  ``` js
  Number.parseInt('12.34') // 12
  Number.parseFloat('123.45#') // 123.45
  ```
### 函数的扩展
  ``` js
  function foo(x = 5, y = 6) {
    console.log(x, y);
  }
  foo(undefined, null) // 5 null
  // 上面代码中，x参数对应undefined，结果触发了默认值，y参数等于null，就没有触发默认值。
  function foo(...values) {
    console.log(values);
  }
  foo(1, 2, 5, 6) // [1, 2, 5, 6]
  // 上面代码中，利用 rest 参数，可以向该函数传入任意数目的参数。
  ```
### 数组的扩展
  * 扩展运算符 '...'
  ``` js
  function push(array, ...items) {
    array.push(...items);
  }
  function add(x, y) {
    return x + y;
  }
  const numbers = [4, 38];
  add(...numbers) // 42
  ```
  * Array.from(): 将两类对象转为真正的数组
  ``` js
  let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
  }
  let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
  ```
  * Array.of(): 方法用于将一组值，转换为数组
  ``` js
  Array.of(3, 11, 8) // [3,11,8]
  ```
  * find(): 用于找出第一个符合条件的数组成员
  * findIndex(): 返回第一个符合条件的数组成员的位置
  ``` js
  let arr = [1, 2, 3, 4, 5, 6]
  arr.find((value, index, arr) => value > 5) // 6
  arr.findIndex((value, index, arr) => value > 2) // 2
  ```
  * fill(): fill方法使用给定值，填充一个数组
  ``` js
  new Array(3).fill(7) // [7, 7, 7]
  let arr = ['a', 'b', 'r']
  arr.fill(5) // [5, 5, 5]
  ```
  * entries(): 对键值对的遍历
  * keys(): 对键名的遍历
  * values(): 对键值的遍历
  ``` js
  let arr = ['a', 'b', 'r']
  for (let index of arr.keys()) {
    console.log(index);
  }
  // 0
  // 1
  // 2
  for (let item of arr.values()) {
    console.log(item);
  }
  // 'a'
  // 'b'
  // 'r'
  for (let [index, item] of arr.entries()) {
    console.log(index, item);
  }
  // 0 'a'
  // 1 'b'
  // 2 'r'
  ```
  * includes(): 返回一个布尔值，表示某个数组是否包含给定的值
  ``` js
  [1, 2, 3].includes(2)     // true
  [1, 2, 3].includes(4)     // false
  [1, 2, NaN].includes(NaN) // true
  ```
  >indexOf方法有两个缺点，一是不够语义化，它的含义是找到参数值的第一个出现位置，所以要去比较是否不等于-1，表达起来不够直观。二是，它内部使用严格相等运算符（===）进行判断，这会导致对NaN的误判。
  * sort(): 排序
  ``` js
  let arr = [1, 50, 8, 19]
  arr.sort((a, b) => a - b) // [1, 8, 19, 50]
  arr.sort((a, b) => b - a) // [50, 19, 8, 1]
  ```
### 对象的扩展
  * Object.assign(): 用于对象的合并
  ``` js
  const target = { a: 1, b: 1 };
  const source1 = { b: 2, c: 2 };
  const source2 = { c: 3 };
  Object.assign(target, source1, source2);
  target // {a:1, b:2, c:3}
  ```
  * Object.keys()
  * Object.values()
  * Object.entries()
  ``` js
  let {keys, values, entries} = Object
  let obj = { a: 1, b: 2, c: 3 }
  for (let key of Object.keys(obj)) {
    console.log(key); // 'a', 'b', 'c'
  }
  for (let value of Object.values(obj)) {
    console.log(value); // 1, 2, 3
  }
  for (let [key, value] of Object.entries(obj)) {
    console.log([key, value]); // ['a', 1], ['b', 2], ['c', 3]
  }
  ```
### 遍历方法
  * for...in 根据key遍历，遍历对象时会从原型上继承属性，可以用hasOwnProperty（）识别出继承属性。
  * for...of 根据值遍历，for...of用来遍历数据，例如数组中的值，但是也可以遍历字符串，支持Map和Set对象的遍历，避免了所有for...in的弊端，与forEach相比可以正确响应break，continue，return语句
  * forEach 根据index遍历，forEach一般只能适用于数组,功能是从头到尾把数组遍历一遍，可以有三个参数，后两个可以不写，效率和for循环相近
  * map 根据index遍历，和forEach相比，使用方法一样有三个参数，map只能对元素进行加工处理，产生一个新的数组对象
  * filter 对原数组进行过滤筛选，生成新的数组,使用和map样有三个参数。如果对空数组进行筛选，会返回undefined。filter不会改变原数组。
  * for 常规语句遍历，可循环数字,字符串，数组
### Symbol
  > 凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突
  ``` js
  let s1 = Symbol('foo')
  let s2 = Symbol('bar')
  s1 // Symbol(foo)
  s2 // Symbol(bar)
  s1 === s2 // false
  ```
### Set 和 Map 数据结构
  * Set
  ``` js
  let set = new Set([1, 2, 3, 4, 4])

  [...set] // [1, 2, 3, 4]
  Array.from(set) // [1, 2, 3, 4]
  set.add(5) // {1, 2, 3, 4, 5}
  set.size // 5
  set.has(1) // true
  set.delete(3) // 返回一个布尔值，表示删除是否成功
  set.clear() // 清除所有成员，没有返回值

  let set = new Set(['red', 'green', 'blue']);

  for (let item of set) {
    console.log(item)
  }
  // red
  // green
  // blue
  ```
  * Map
  ``` js
  // 对象 转 Map
  let obj = {'name': '张三', 'title': 'Author'}
  const map = new Map(Object.entries(obj))

  [...map.keys()] // ['name', 'title']
  [...map.values()] // ['张三', 'Author']
  [...map] // [['name', '张三'], ['title', 'Author']]
  Array.from(map) // ['name', '张三', 'title', 'Author']

  map.set(1, '男') // {'name'=>'张三', 'title'=>'Author', 1=>'男'}
  map.get(1) // '男'
  map.has('name') // true
  map.size() // 3
  map.delete('title') // 返回一个布尔值，表示删除是否成功。
  map.clear() // 清除所有成员，没有返回值

  // 注意，只有对同一个对象的引用，Map 结构才将其视为同一个键。这一点要非常小心。
  map.set(['a'], 555)
  map.get(['a']) // undefined

  const k1 = ['a']
  const k2 = ['a']
  map.set(k1, 111).set(k2, 222)
  map.get(k1) // 111
  map.get(k2) // 222

  const map = new Map([
    ['F', 'no'],
    ['T',  'yes'],
  ])

  for (let key of map.keys()) {
    console.log(key)
  }
  // "F"
  // "T"

  for (let value of map.values()) {
    console.log(value)
  }
  // "no"
  // "yes"

  for (let [key, value] of map) {
    console.log(key, value)
  }
  // "F" "no"
  // "T" "yes"
  ```
### Proxy 和 Reflect
  > Proxy： 可以对外界的访问进行过滤和改写， Reflect： 可以拿到语言内部的方法
  ``` js
  var obj = new Proxy({}, {
    get: function (target, key, receiver) {
      console.log(`getting ${key}!`);
      return Reflect.get(target, key, receiver);
    },
    set: function (target, key, value, receiver) {
      console.log(`setting ${key}!`);
      return Reflect.set(target, key, value, receiver);
    }
  });
  obj.count = 1
  //  setting count!
  ++obj.count
  //  getting count!
  //  setting count!
  //  2
  ```
### Promise
  > Promise 是异步编程的一种解决方案
  ``` js
  function fetchPost(url, params) {
    return new Promise((resolve, reject) => { // 成功调用resolve，失败调用reject
      axios.post(url, params)
        .then(response => {
          resolve(response.data)
        }, err => {
          reject(err)
        })
        .catch((error) => {
          reject(error)
        })
    })
  }
  // 执行方法
  fetchPost(url, params).then(val => {
    console.log(val)
  }).catch(err => {
    console.log(err)
  })
  ```
### Generator
  > 函数会返回一个遍历器对象,可以依次遍历 Generator 函数内部的每一个状态
  ``` js
  function* helloWorldGenerator() {
    yield 'hello';
    yield 'world';
    return 'ending';
  }
  var hw = helloWorldGenerator();
  hw.next()
  // { value: 'hello', done: false }
  hw.next()
  // { value: 'world', done: false }
  hw.next()
  // { value: 'ending', done: true }
  hw.next()
  // { value: undefined, done: true }
  ```
  > 异步任务的封装
  ``` js
  const fetch = require('node-fetch')
  function* gen() { // 该操作先读取一个远程接口，然后从 JSON 格式的数据解析信息
    const url = 'https://api.github.com/users/github'
    yield fetch(url)
  }
  // 执行方法
  gen().next().value.then(data => {
    return data.json()
  }).then(data => {
    gen().next(data)
  })
  ```
  > Generator和Promise配合使用
  ``` js
  function readFile(fileName) {
    return new Promise((resolve, reject) => {
      require('fs').readFile(fileName, (error, data) => {
        if (error) return reject(error)
        resolve(data)
      })
    })
  }
  function* gen() {
    const f1 = yield readFile('/etc/fstab')
    const f2 = yield readFile('/etc/shells')
    console.log(f1.toStaing())
    console.log(f2.toStaing())
  }
  // 执行方法
  gen().next().value.then(data => { // next(data)中的data===f1
    return gen().next(data).value
  }).then(data => {
      gen().next(data)
    })
  ```
### async
  > Generator 函数的语法糖
  ``` js
  async function gen() {
    const f1 = await readFile('/etc/fstab')
    const f2 = await readFile('/etc/shells')
    console.log(f1.toStaing())
    console.log(f2.toStaing())
  }
  // 任何一个await语句后面的 Promise 对象变为reject状态，那么整个async函数都会中断执行。
  // 解决办法:
  async function f() {
    try {
      await readFile('/etc/fstab')
    } catch(err) {
      console.log(err)
    }
  }
  // 执行方法
  f().then(v => console.log(v))
  ```
### Class
  ``` js
  class Point {
    constructor(x, y) {
      this.x = x;
      this.y = y;
    }

    toString() {
      return '(' + this.x + ', ' + this.y + ')';
    }
  }
  // 类的继承
  class ColorPoint extends Point {
    constructor(x, y, color) {
      super(x, y); // 调用父类的constructor(x, y)
      this.color = color;
    }

    toString() {
      return this.color + ' ' + super.toString(); // 调用父类的toString()
    }
  }
  ```
### static关键字
  > 被static修饰的变量属于类变量，可以通过类名.变量名直接引用，而不需要new出一个类来，
  类似 Array.from()中 from()为static， [1, 2, 3, 4, 5, 6].find()不是static，
  ``` js
  class Point {
    constructor(x, y) {
      this.x = x;
      this.y = y;
    }
    toString() {
      return '(' + this.x + ', ' + this.y + ')';
    }
    static toPoint() {
      return '(' + this.x + ', ' + this.y + ')';
    }
  }
  // 执行方法
  const p = new Point()
  p.toString()
  Point.toPoint()
  ```