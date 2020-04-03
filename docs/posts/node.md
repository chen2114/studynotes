---
title: 【学习笔记】之node
tags: [学习笔记, node, npm, mongodb]
categories: [学习笔记]
---
## NPM
### npm命令
#### 初始化
    npm init -y

  自动生成`package.json`和`package-lock.json`文件，该文件保存依赖项信息并锁定依赖项版本
#### 安装依赖包
    npm install 包名
    npm i 包名
#### 删除依赖项
    npm uninstall 包名

  加上`--save`删除依赖项同时删除依赖信息
#### npm帮助
    npm --help
#### 查看npm配置信息
    npm config list
### 使用淘宝服务器下载依赖
#### 方法一
    npm install --global cnpm
    cnpm install
#### 方法二
    npm config set registry https://registry.npm.taobao.org
    npm install
## Node.js
### Node.js REPL(交互式解释器)
#### 使用方法
    $ node
    > 1 + 4
    5
    > console.log("Hello World")
    Hello World
    undefined
#### REPL 命令
  - ctrl + c - 退出当前终端
  - ctrl + c 按下两次 - 退出 Node REPL
  - ctrl + d - 退出 Node REPL
  - tab 键 - 列出当前命令
  - .help - 列出使用命令
  - .break - 退出多行表达式
  - .clear - 退出多行表达式
  - .save filename - 保存当前的 Node REPL 会话到指定文件
  - .load filename - 载入当前 Node REPL 会话的文件内容。
### 代码修改自动重启服务器
    npm install --global nodemon
    nodemon app.js
### 回调函数
``` js
function fn (callback) {
  setTimeout(function () {
    var data = 'hello wold'
    callback(data)
  }, 1000)
}
// 如果需要获取一个异步操作的结果，则必须通过回调函数来获取
fn (function (data) {
  console.log(data)
})
```
### 模块加载
``` js
// b.js
exports.foo = 'hello'
module.exports = 'hello'

// a.js
const b = require('./b')

console.log(b.foo) // 'hello'
console.log(b) // 'hello'
```
> `require`加载优先从缓存区中加载<br>
> `require`在加载第三方包时会读取`node_modules`文件夹下的包名中的`package.json`中的`{'main': 'index.js'}`入口文件
### fs模块
  ``` js
  const fs = require('fs')
  // 该相对路径是执行 node 命令下的路径
  fs.readFile('./assets/hello.txt', function (error, data) {
    // data 为二进制
    console.log(data.toString())
  })
  fs.writeFile('./assets/write.text', '### 你好，nodejs', function (error) {
    console.log('文件写入成功')
  })
  ```
### path模块
  ``` js
  const path = require('path')

  path.parse('c:/a/b/index.js')
  // {
  //   root: 'c:/',
  //   dir: 'c:/a/b',
  //   base: 'index.js',
  //   ext: '.js',
  //   name: 'index'
  // }

  path.join('c:/a', 'b')
  // 'c:\\a\\b'
  ```
### node 中的全局对象
  #### __dirname
  动态获取当前文件所属目录的绝对路径
  > 在涉及文件操作的相对路径都是执行 node 命令下的路径，建议使用动态获取文件路径`path.join(__dirname, './文件.js')`
  #### __filename
  动态获取当前文件的绝对路径
### url模块
``` js
const url = require('url')

// 第二个参数 true，可以将请求参数转为对象 { name: 'chen', message: '黑马程序员' }
let obj = url.parse('https://www.bilibili.com/video/av27670326?name=chen&message=黑马程序员', true)

console.log(obj)
// obj = {
//   protocol: 'https:',
//   slashes: true,
//   auth: null,
//   host: 'www.bilibili.com',
//   port: null,
//   hostname: 'www.bilibili.com',
//   hash: null,
//   search: '?name=chen&message=黑马程序员',
//   query: [Object: null prototype] { name: 'chen', message: '黑马程序员' },
//   pathname: '/video/av27670326',
//   path: '/video/av27670326?name=chen&message=黑马程序员',
//   href:
//    'https://www.bilibili.com/video/av27670326?name=chen&message=黑马程序员'
// }
```
### 模板引擎
#### 安装
    npm install art-template
#### 使用
  ``` js
  const template = require('art-template')

  template.render(`<h1>${content}</h1>`, {
    content: '你好！node.js'
  })
  ```
#### 语法
  - 遍历
  ``` html
  <ul>
    {{ each list }}
    <li>索引 {{ $index + 1 }} ：{{ $value.name }}</li>
    {{ /each }}
  </ul>
  ```
  - 条件表达式
  ``` html
  {{ if admin }}
    <p>admin</p>
  {{ else if code > 0 }}
    <p>master</p>
  {{ else }}
    <p>error!</p>
  {{ /if }}
  ```
  - 调用子模板
  ```
  {{include './header.html'}}
  ```
  - 继承
  ```
  {{ extend './layout.html' }}
  ```
  - 插槽
  ``` html
  <!-- layout.html -->
  {{ block 'body' }}
  <h1>默认内容</h1>
  {{ /block }}

  <!-- index.html -->
  {{ extend './layout.html' }}

  {{ block 'body' }}
  <div>
    <a href="#">替换内容</a>
  </div>
  {{ /block }}
  ```
### Node.js 连接 MongoDB
#### 开启数据库
  打开控制台输入命令

    $ mongod

  `mongodb`默认数据存储在 C 盘中的 data => bin 文件夹下，所以需在 C 盘下启动
#### 安装
    $ npm install mongoose
#### 连接
  ``` js
  const mongoose = require('mongoose')

  // 连接数据库
  mongoose.connect('mongodb://localhost/test', {useNewUrlParser: true})
  ```
#### 创建
  ``` js
  // 设计数据模型
  const userSchema = new mongoose.Schema({
    username: {
      type: String,
      required: true
    },
    password: {
      type: String,
      required: true
    },
    email: {
      type: String
    }
  })

  // 发布数据模型
  const User = mongoose.model('User', userSchema)
  ```
#### 操作
  ``` js
  // 添加数据
  let admin = new User({
    username: 'chen',
    password: 'cwb123',
    email: 'chen@chen.com'
  })
  admin.save(function (err, ret) {
    if (err) {
      console.log(err)
    } else {
      console.log(ret)
    }
  })

  // 查询数据
  User.find({// 可选参数，搜索条件
    username: 'chen'
  }, function (err, ret) {
    if (err) {
      console.log(err)
    } else {
      console.log(ret)
    }
  })

  // 删除数据
  User.remove({
    username: 'admin'
  }, function (err, ret) {
    if (err) {
      console.log(err)
    } else {
      console.log('删除成功')
    }
  })

  // 更新数据
  User.update({
    username: 'chen'
  }, {
    email: 'chen@qq.com'
  }, function (err, ret) {
    if (err) {
      console.log(err)
    } else {
      console.log('更新成功')
    }
  })
  ```
### 创建 Node.js 应用
在  server.js 文件下
``` js
// server.js
const http = require('http')

// request 请求对象：可获取客户端信息
// response 响应对象：给客户的发送响应消息
http.createServer(function (request, response) {
  // 设置编码格式，解决中文乱码
  response.setHeader('Content-Type', 'text/plain; charset=utf-8')

  if (request.url === '/') {
    let pro = {
      name: '陈文彬',
      age: 24
    }
    response.end(JSON.stringify(pro)) // 发送响应数据, 响应消息只能是字符串和二进制
  } else {
    response.end('404 not found!')
  }
})
  .listen(3000, function () {// 设置端口，启动服务器
    console.log('请输入 http://127.0.0.1:3000/ 进行访问')
  })
```
使用 node 命令在终端执行以上的代码：
```
$ nodemon server.js
请输入 http://127.0.0.1:3000/ 进行访问
```
打开浏览器访问 http://127.0.0.1:3000/
### 开放目录
``` js
const fs = require('fs')
const http = require('http')

http.createServer(function (req, res) {
  if (req.url.indexOf('/public/') === 0) {
    fs.readFile('.' + req.url, function (err, data) {
      if (err) {
        return res.end('获取文件失败')
      }
      res.end(data)
    })
  }
})
```
### 模板引擎的使用
  ``` js
  http.createServer(function (req, res) {
    fs.readFile('./views/index.html', function (err, data) {
      if (err) {
        return res.end(err.toString())
      }
      let htmlStr = template.render(data.toString(), {
        content: content
      })
      res.end(htmlStr)
    })
  })
  ```
### 重定向
``` js
response.statusCode = 302 // 状态码设置为 302 临时重定向
response.setHeader('Location', '/') // 设置重定向地址
response.end()
```
## Express
### 创建 Node.js 应用
  ``` js
  const express = require('express')

  const app = express()

  app.get('/', function (req, res) {
    res.send()
  })
    .listen(3000, function () {
      console.log('请输入 http://127.0.0.1:3000/ 进行访问')
    })
  ```
### 开放目录
  浏览器收到 html 响应内容后从上到下依次解析，遇到获取静态资源如：link、script、img、iframe...等带有 src 和 href 属性标签的时候会自动发起新的请求，因此需要将这些静态资源文件开放
  > a 标签需用户点击才出发请求
  ``` html
  <!-- href 中的地址是客户端发起请求的地址，也就是服务端中 req.url -->
  <link href="/node_modules/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet">
  <link href="/public/css/main.css" rel="stylesheet">
  ```
  ``` js
  // url地址 '/public/' 指向文件路径 './public/'
  // 若 '/public/' 省略， 则 url 地址 '/' 指向文件路径 './public/'
  app.use('/public/', express.static('./public/'))
  app.use('/node_modules/', express.static('./node_modules/'))
  ```
### 模板引擎的使用
#### 安装
    npm install art-template
    npm install express-art-template
#### 配置
  ``` js
  // html 为查找文件的后缀名
  app.engine('html', require('express-art-template'))
  ```
#### 使用
  ``` js
  app.get('/', function (req, res) {
    // html 文件默认在 views 文件夹下查找
    res.render('index.html', {
      content: content
    })
  })
  ```
### Get 和 Post 请求
#### Get
  获取请求参数
  ``` js
  app.get('/post', function (req, res) {
    // req.query === {}
    console.log(req.query)
  })
  ```
#### Post
  获取请求参数：<br>
  express 不提供 post 请求的 api，需安装 body-parser 中间插件
  ```
  npm install body-parser
  ```
  ``` js
  const bodyParser = require('body-parser')

  // body-parser 的配置
  app.use(bodyParser.urlencoded({ extended: false}))
  app.use(bodyParser.json())

  // POST 请求
  app.post('/post', function (req, res) {
    // req.body === {}
    console.log(req.body)
  })
  ```
### Router
  ``` js
  // router.js
  const express = require('express')

  // 创建路由容器
  const router = express.Router()

  // 把路由挂载到路由容器中
  router.get('/', function(req, res) {
    res.render('index.html')
  })

  module.exports = router
  ```
  ``` js
  // app.js
  const express = require('express')
  const router = require('./router')

  const app = express()

  // 把路由容器挂载到 app 服务中
  app.use(router)
  ```
#### Ajax
  当客户端`ajax`请求的数据类型设为`dataType: 'json'`时，服务端需返回`json`数据
  ``` js
  router.post('/login', function (req, res) {
    res.status(200).json({
      code: 0,
      message: '登录成功'
    })
  })
  ```
### 重定向
  ``` js
  res.redirect('/')
  ```
### Session
#### 安装
    npm install express-session
#### 使用
  ``` js
  const session = require('express-session')

  // session 配置
  app.use(session({
    secret: 'keyboard cat', // 配置加密字符串
    resave: false,
    saveUninitialized: true
  }))

  app.get('/', function (req, res) {
    req.session.user = {
      name: 'chen',
      age: 24
    }
    console.log(req.session.user)
  })
  ```