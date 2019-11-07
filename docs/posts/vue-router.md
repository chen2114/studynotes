---
title: 【学习笔记】之router
tags: [学习笔记, router]
categories: [学习笔记]
---
###  起步
  ``` html
  <!-- 使用 router-link 组件来导航，通过传入 `to` 属性指定链接，<router-link> 默认会被渲染成一个 `<a>` 标签 -->
  <router-link to="/foo">Go to Foo</router-link>
  <router-link to="/bar">Go to Bar</router-link>
  <!-- 路由出口，路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
  ```
  ``` js
  // 0. 如果使用模块化机制编程，导入Vue和VueRouter，要调用 Vue.use(VueRouter)
  // 1. 定义 (路由) 组件。
  // 可以从其他文件 import 进来
  const Foo = { template: '<div>foo</div>' }
  const Bar = { template: '<div>bar</div>' }
  // 2. 定义路由
  // 每个路由应该映射一个组件。 其中"component" 可以是通过 Vue.extend() 创建的组件构造器，或者，只是一个组件配置对象。
  const routes = [
    { path: '/foo', component: Foo },
    { path: '/bar', component: Bar }
  ]
  // 3. 创建 router 实例，然后传 `routes` 配置，还可以传别的配置参数。
  const router = new VueRouter({
    routes // (缩写) 相当于 routes: routes
  })
  // 4. 创建和挂载根实例。
  const app = new Vue({
    router
  }).$mount('#app')
  ```
  > 通过注入路由器，我们可以在任何组件内通过 this.$router 访问路由器，也可以通过 this.$route 访问当前路由
### 动态路由匹配
  ``` js
  const router = new VueRouter({
    routes: [
      // 动态路径参数 以冒号开头
      {
        path: '/user/:id',
        component: {
          template: '<div>User {{ $route.params.id }}</div>',
          watch: {
            '$route' (to, from) {
              // 对路由变化作出响应...
            }
          },
          beforeRouteUpdate (to, from, next) {
            // react to route changes...
            // don't forget to call next()
          }
        }
      }
    ]
  })
  ```
### 嵌套路由
  ``` js
  const router = new VueRouter({
    routes: [
      { path: '/user/:id',
        component: {
          template: `
            <div class="user">
              <h2>User {{ $route.params.id }}</h2>
              <router-view></router-view>
            </div>
          `
        },
        children: [
          {
            // 当 /user/:id/profile 匹配成功，
            // UserProfile 会被渲染在 User 的 <router-view> 中
            path: 'profile',
            component: UserProfile
          },
          {
            // 当 /user/:id/posts 匹配成功
            // UserPosts 会被渲染在 User 的 <router-view> 中
            path: 'posts',
            component: UserPosts
          }
        ]
      }
    ]
  })
  ```
### 编程式的导航
  > 注意：在 Vue 实例内部，你可以通过 $router 访问路由实例。因此你可以调用 this.$router.push。
  * router.push(location, onComplete?, onAbort?)<br>
  想要导航到不同的 URL，则使用 router.push 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。
  ``` js
  const router = new VueRouter({
    routes: [
      {
        path: '/user/:userId',
        name: 'user',
        component: User
      }
    ]
  })
  // 命名的路由， 变成 /user/123
  router.push({ name: 'user', params: { userId: '123' }})
  // 带查询参数，变成 /register?plan=private
  router.push({ path: 'register', query: { plan: 'private' }})
  ```
  * router.replace(location, onComplete?, onAbort?)<br>
  跟 router.push 很像，唯一的不同就是，它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 history 记录。
  * router.go(n)<br>
  这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 window.history.go(n)。
  ``` js
  // 在浏览器记录中前进一步，等同于 history.forward()
  router.go(1)
  // 后退一步记录，等同于 history.back()
  router.go(-1)
  ```
### 命名视图
  ``` html
  <!-- 如果 router-view 没有设置名字，那么默认为 default。 -->
  <router-view class="view one"></router-view>
  <router-view class="view two" name="a"></router-view>
  <router-view class="view three" name="b"></router-view>
  ```
  ``` js
  // 一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 components 配置 (带上 s)
  const router = new VueRouter({
    routes: [
      {
        path: '/',
        components: {
          default: { template: '<div>foo</div>' },
          a: { template: '<div>Bar</div>' },
          b: { template: '<div>Baz</div>' }
        }
      }
    ]
  })
  ```
### 重定向和别名
#### 重定向
  “重定向”的意思是，当用户访问 /a时，URL 将会被替换成 /b，然后匹配路由为 /b
  ``` js
  const router = new VueRouter({
    routes: [
      { path: '/a', redirect: '/b' },
      { path: '/a', redirect: { name: 'foo' }},
      { path: '/a', redirect: to => {
        // 方法接收 目标路由 作为参数
        // return 重定向的 字符串路径/路径对象
      }}
    ]
  })
  ```
#### 别名
  /a 的别名是 /b，意味着，当用户访问 /b 时，URL 会保持为 /b，但是路由匹配则为 /a，就像用户访问 /a 一样。
  ``` js
  const router = new VueRouter({
    routes: [
      { path: '/a', component: A, alias: '/b' }
    ]
  })
  // “别名”的功能让你可以自由地将 UI 结构映射到任意的 URL，而不是受限于配置的嵌套路由结构。
  ```
### 路由组件传参
  ``` js
  // 使用 props 将组件和路由解耦：
  const User = {
    props: ['id'],
    template: '<div>User {{ id }}</div>'
  }
  const router = new VueRouter({
    routes: [
      { path: '/user/:id', component: User, props: true },
      // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
      {
        path: '/user/:id',
        components: { default: User, sidebar: Sidebar },
        props: { default: true, sidebar: false }
      },
      // URL /search?q=vue 会将 {query: 'vue'} 作为属性传递给 SearchUser 组件
      { path: '/search', component: SearchUser, props: (route) => ({ query: route.query.q }) }
    ]
  })
  ```
### HTML5 History 模式
  ``` js
  const router = new VueRouter({
    mode: 'history',
    routes: [...]
  })
  ```
### 导航守卫
#### router.beforeEach 全局前置守卫
  ``` js
  const router = new VueRouter({ ... })
  router.beforeEach((to, from, next) => {
    // .....
  })
  ```
  > 当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于 等待中。
  * to: Route: 即将要进入的目标 路由对象
  * from: Route: 当前导航正要离开的路由
  * next: Function: 确保要调用 next 方法，否则钩子就不会被 resolved，如果全部钩子执行完了，则导航的状态就是 confirmed (确认的)。
    * next(): 进行管道中的下一个钩子。
    * next(false): 中断当前的导航。
    * next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。
    * next(error): (2.4.0+) 如果传入 next 的参数是一个 Error 实例，则导航会被终止且该错误会被传递给 router.onError() 注册过的回调。
#### router.beforeResolve 全局解析守卫
  > 这和 router.beforeEach 类似，区别是在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用。
### router.afterEach 全局后置钩子
  ``` js
  router.afterEach((to, from) => {
    // ...
  })
  ```
#### beforeEnter 路由独享的守卫
  ``` js
  // 这些守卫与全局前置守卫的方法参数是一样的。
  const router = new VueRouter({
    routes: [
      {
        path: '/foo',
        component: Foo,
        beforeEnter: (to, from, next) => {
          // ...
        }
      }
    ]
  })
  ```
#### 组件内的守卫
  * beforeRouteEnter
  * beforeRouteUpdate (2.2 新增)
  * beforeRouteLeave
  ``` js
  const Foo = {
    template: `...`,
    beforeRouteEnter (to, from, next) {
      // 在渲染该组件的对应路由被 confirm 前调用
      // 不！能！获取组件实例 `this`，因为当守卫执行前，组件实例还没被创建
      next(vm => {
        // 通过 `vm` 访问组件实例
      })
    },
    beforeRouteUpdate (to, from, next) {
      // 在当前路由改变，但是该组件被复用时调用
      // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候， 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
      // 可以访问组件实例 `this`
    },
    beforeRouteLeave (to, from, next) {
      // 导航离开该组件的对应路由时调用
      // 可以访问组件实例 `this`
    }
  }
  ```
#### 完整的导航解析流程
  1. 导航被触发
  2. 在失活的组件里调用离开守卫
  3. 调用全局的 beforeEach 守卫
  4. 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)
  5. 在路由配置里调用 beforeEnter
  6. 解析异步路由组件
  7. 在被激活的组件里调用 beforeRouteEnter
  8. 调用全局的 beforeResolve 守卫 (2.5+)
  9. 导航被确认
  10. 调用全局的 afterEach 钩子
  11. 触发 DOM 更新
  12. 用创建好的实例调用 beforeRouteEnter 守卫中传给 next 的回调函数
### 路由元信息
  ``` js
  // 配置 mate 字段
  const router = new VueRouter({
    routes: [
      {
        path: '/foo',
        component: Foo,
        children: [
          {
            path: 'bar',
            component: Bar,
            // a meta field
            meta: { requiresAuth: true }
          }
        ]
      }
    ]
  })
  // 访问这个 meta 字段
  router.beforeEach((to, from, next) => {
    if (to.matched.some(record => record.meta.requiresAuth)) {
      // this route requires auth, check if logged in
      // if not, redirect to login page.
      if (!auth.loggedIn()) {
        next({
          path: '/login',
          query: { redirect: to.fullPath }
        })
      } else {
        next()
      }
    } else {
      next() // 确保一定要调用 next()
    }
  })
  ```
### 过渡动效
  ``` js
  // 单个路由的过渡
  const Foo = {
    template: `
      <transition name="slide">
        <div class="foo">...</div>
      </transition>
    `
  }
  // 基于路由的动态过渡
  <transition :name="transitionName">
    <router-view></router-view>
  </transition>
  ```
### 滚动行为
  > 注意: 这个功能只在支持 history.pushState 的浏览器中可用
  ``` js
  // scrollBehavior 方法接收 to 和 from 路由对象。第三个参数 savedPosition 当且仅当 popstate 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用，返回滚动位置的对象信息
  const router = new VueRouter({
    routes: [...],
    scrollBehavior (to, from, savedPosition) {
      // 对于所有路由导航，简单地让页面滚动到顶部
      if (savedPosition) {
        return savedPosition
      } else {
        return { x: 0, y: 0 }
      }
    }
  })
  ```
### 路由懒加载
  > 当路由被访问的时候才加载对应组件，更加高效。如果您使用的是 Babel，你将需要添加 [syntax-dynamic-import](https://babeljs.io/docs/en/babel-plugin-syntax-dynamic-import/) 插件，才能使 Babel 可以正确地解析语法。
  ``` js
  // 定义一个能够被 Webpack 自动代码分割的异步组件，把组件按组分块
  const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
  const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
  const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
  ```