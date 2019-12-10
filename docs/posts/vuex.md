---
title: 【学习笔记】之vuex
tags: [学习笔记, vuex]
categories: [学习笔记]
---
## 安装
### vuex
  在 Vue 之后引入 vuex 会进行自动安装：
  ``` html
  <script src="/path/to/vue.js"></script>
  <script src="/path/to/vuex.js"></script>
  ```
  **npm**
  ``` sh
  npm install vuex --save
  ```
  **yarn**
  ``` sh
  yarn add vuex
  ```
  在一个模块化的打包系统中，您必须显式地通过 Vue.use() 来安装 Vuex：
  ``` js
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)

  const store = new Vuex.Store({
    state: {
      // ....
    },
    // ....
  })

  new Vue({
    el: '#app',
    store,
    router,
    components: { App },
    template: '<App/>'
  })
  ```
### 依赖
  Vuex 依赖 Promise。如果你支持的浏览器并没有实现 Promise (比如 IE)，那么你可以使用一个 polyfill 的库，例如 es6-promise。

  你可以通过 CDN 将其引入：
  ``` html
  <script src="https://cdn.jsdelivr.net/npm/es6-promise@4/dist/es6-promise.auto.js"></script>
  ```
  **npm** || **yarn**
  ``` sh
  npm install es6-promise --save # npm
  yarn add es6-promise # Yarn
  ```
## 核心概念
### State
  作为一个唯一数据源而存在
  ``` js
  const store = new Vuex.Store({
    state: {
      count: 0
    }
  })
  ```
  ``` js
  import { mapState } from 'vuex'
  export default {
    computed: {
      ...mapState(['count']) // 将 `this.count` 映射为 `this.$store.state.count`
    }
  }
  ```
### Getter
  当需要从 state 中派生出一些状态进行处理时使用 getter

  例如对列表进行过滤并计数:
  ``` js
  const store = new Vuex.Store({
    state: {
      todos: [
        { id: 1, text: '...', done: true },
        { id: 2, text: '...', done: false }
      ]
    },
    getters: {
      // ...
      getTodoById: (state) => (id) => {
        return state.todos.find(todo => todo.id === id)
      }
    }
  })
  ```
  ``` js
  import { mapGetters } from 'vuex'
  export default {
    // ...
    computed: {
    // 使用对象展开运算符将 getter 混入 computed 对象中
      ...mapGetters([
        'getTodoById', // 将 `this.getTodoById(2)` 映射为 `this.$store.getters.getTodoById(2)`
        // ...
      ])
    }
  }
  ```
  > 注意，getter 在通过方法访问时，每次都会去进行调用，而不会缓存结果。
  ### Mutation
  更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。**注意： mutation 必须是同步函数**
  ``` js
  // mutation-types.js
  export const SOME_MUTATION = 'SOME_MUTATION' // 使用常量替代 mutation 事件类型, 在需要多人协作的大型项目中，这会很有帮助
  ```
  ``` js
  import Vuex from 'vuex'
  import { SOME_MUTATION } from './mutation-types'
  const store = new Vuex.Store({
    state: {
      count: 1
    },
    mutations: {
      [SOME_MUTATION] (state) {
        // mutate state
      },
      incrementBy (state, payload) {
        // 变更状态
        state.count += payload.amount
      }
    }
  })
  ```
  ``` js
  import { mapMutations } from 'vuex'
  export default {
    // ...
    methods: {
      ...mapMutations([
        'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

        // `mapMutations` 也支持载荷：
        'incrementBy' // 将 `this.incrementBy({amount: 10})` 映射为 `this.$store.commit('incrementBy', {amount: 10})`
      ]),
      ...mapMutations({
        add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
      })
    }
  }
  ```
### Action
  Action 类似于 mutation，不同在于：
  * Action 提交的是 mutation，而不是直接变更状态。
  * Action 可以包含任意异步操作。
  ``` js
  const store = new Vuex.Store({
    state: {
      count: 0
    },
    mutations: {
      increment (state) {
        state.count++
      }
    },
    actions: {
      // Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象
      increment (context) {
        context.commit('increment')
      },
      incrementBy ({ commit, state }, amount) {
        commit('increment')
      }
    }
  })
  ```
  ``` js
  import { mapActions } from 'vuex'
  export default {
    // ...
    methods: {
      ...mapActions([
        'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`

        // `mapActions` 也支持载荷：
        'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
      ]),
      ...mapActions({
        add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
      })
    }
  }
  ```
### Module
  为了解决 store 对象变得臃肿，Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割：
  ``` js
  const moduleA = {
    state: { count: 0 },
    mutations: { ... },
    actions: { 
      incrementIfOddOnRootSum ({ state, commit, rootState, rootGetters }) {
        // 这里的 `state` 对象是模块的局部状态， `rootState` 对象是模块的根节点状态
        if ((state.count + rootState.count) % 2 === 1) {
          commit('increment')
        }
      }
    },
    getters: {
      // 模块内部的 getter，根节点状态会作为第三个参数暴露出来
      sumWithRootCount (state, getters, rootState) {
        return state.count + rootState.count
      }
    }
  }

  const moduleB = {
    state: { ... },
    mutations: { ... },
    actions: { ... }
  }

  const store = new Vuex.Store({
    state: { count: 10 },
    mutations: { ... },
    actions: { ... },
    modules: {
      a: moduleA,
      b: moduleB
    }
  })
  ```
  ``` js
  import { mapState, mapActions } from 'vuex'
  export default {
    computed: {
      ...mapState('some/nested/module', {
        a: state => state.a, // -> this.$store.state.a
        b: state => state.b // -> this.$store.state.b
      })
    },
    methods: {
      ...mapActions('some/nested/module', [
        'foo', // -> this.foo()
        'bar' // -> this.bar()
      ])
    }
  }
  ```
## 项目结构
  需要遵守的规则: 
  1. 应用层级的状态应该集中到单个 store 对象中。
  2. 提交 mutation 是更改状态的唯一方法，并且这个过程是同步的。
  3. 异步逻辑都应该封装到 action 里面。
  ``` sh
  ├── index.html
  ├── main.js
  ├── api
  │   └── ... # 抽取出API请求
  ├── components
  │   ├── App.vue
  │   └── ...
  └── store
      ├── index.js          # 我们组装模块并导出 store 的地方
      ├── actions.js        # 根级别的 action
      ├── mutations.js      # 根级别的 mutation
      └── modules
          ├── cart.js       # 购物车模块
          └── products.js   # 产品模块
  ```
### 严格模式
  开启严格模式，仅需在创建 store 的时候传入 strict: true：
  ``` js
  const store = new Vuex.Store({
    // ...
    strict: true
  })
  ```
  在严格模式下，无论何时发生了状态变更且不是由 mutation 函数引起的，将会抛出错误。这能保证所有的状态变更都能被调试工具跟踪到

  **不要在发布环境下启用严格模式！** 严格模式会深度监测状态树来检测不合规的状态变更——请确保在发布环境下关闭严格模式，以避免性能损失。
  
  使用构建工具来处理这种情况：
  ``` js
  const store = new Vuex.Store({
    // ...
    strict: process.env.NODE_ENV !== 'production'
  })
  ```
## 表单处理
  当在严格模式中使用 Vuex 时，在属于 Vuex 的 state 上使用 v-model 会抛出一个错误。
  ``` html
  <input v-model="message">
  ```
  ``` js
  // ...
  computed: {
    message: {
      get () {
        return this.$store.state.obj.message
      },
      set (value) {
        this.$store.commit('updateMessage', value)
      }
    }
  }
  ```
