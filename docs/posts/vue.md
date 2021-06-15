---
title: 【学习笔记】之vue
tags: [学习笔记, vue]
categories: [学习笔记]
---
## 注册全局
`require.context(搜索的目录，是否还搜索其子目录，匹配文件的正则表达式)`
### 注册全局组件
``` js
// index.js
import Vue from 'vue'

// 获取该文件夹下所有 .vue 后缀的文件
const files = require.context('.', false, /\.vue$/)
files.keys().forEach(key => {
  // 注册为全局组件
  Vue.component(files(key).default.name, files(key).default)
})
```
### 引入项目中所有的svg文件
``` js
const requireContext = require.context('./svg', false, /\.svg$/)
const requireAll = context => context.keys().map(context)
requireAll(requireContext)
```
### store模块化
```
├── store
    ├── modules
        └── index.js
    └── index.js
```
modules -> index.js
``` js
const files = require.context('.', false, /\.js$/)
const modules = {}

files.keys().forEach(key => {
  if (key === './index.js') return
  modules[key.replace(/(\.\/|\.js)/g, '')] = files(key).default
})

export default modules
```
store -> index.js
``` js
import Vue from 'vue'
import Vuex from 'vuex'
import modules from './modules'
Vue.use(Vuex)

export default new Vuex.Store({
  modules
})
```
## 父组件和子组件的使用
### 传值
父组件：
``` vue
<template>
  <child
    message="父组件的值"
    @handle-click="handleChange"
  />
</template>
<script>
import child from './components/child'
export default {
  name: 'parent',
  components: {
    child
  },
  methods: {
    handleChange (data) {
      console.log(data)
    }
  }
}
</script>
```
子组件：
``` vue
<template>
  <h1>这是子组件</h1>
  <div>{{ message }}</div>
  <button
    :size="btnSize"
    @click="handleClick"
  >
    向父组件传值
  </button>
</template>
<script>
export default {
  name: child,
  props: {
    message: {
      type: String,
      default: null
    },
    btnSize: {
      // 按钮尺寸
      type: String,
      default: null,
      validator: value => {
        return ['medium', 'small', 'mini'].indexOf(value) !== -1
      }
    }
  },
  methods: {
    /**
      * @description 函数描述的 必填
      * @param id {Number} 传入需要获取名称的人物id 参数必填
      * @return {String} 返回的姓名 返回值必填，空为void
      * @author shi 2015/07/21 作者可选
      * @version 1.1.0 可以不写 版本可选
      * @example 示例代码，可选
      */
    handleClick () {
      this.$emit('handle-click', '子组件的值')
    }
  }
}
</script>
```
### 双向绑定
父组件：
``` vue
<template>
  <child v-model="parentMsg" />
</template>
<script>
import child from './components/child'
export default {
  name: 'parent',
  components: {
    child
  },
  data {
    return {
      parentMsg: '父组件的model值'
    }
  }
}
</script>
```
子组件：
``` vue
<template>
  <input
    type="text"
    v-model='message'
  />
</template>
<script>
export default {
  name: child,
  computed: {
    message: {
      get () {
        return this.value
      },
      set (val) {
        this.$emit('input', val)
      }
    }
  },
  props: {
    value: {
      // v-model 绑定的值
      type: String,
      default: null
    }
  }
}
</script>
```
### .sync使用
> .sync修饰符作为一个语法糖，他会扩展为一个自动更新父组件属性的v-on监听器

父组件：
``` vue
<template>
  <child :visible.sync="dialogVisible" />
</template>
<script>
import child from './components/child'
export default {
  name: 'parent',
  components: {
    child
  },
  data {
    return {
      dialogVisible: true
    }
  }
}
</script>
```
子组件：
``` vue
<template>
  <el-dialog
    title="弹窗"
    :visible.sync="dialogVisible"
  >
    <span>这是一段信息</span>
  </el-dialog>
</template>
<script>
export default {
  name: child,
  computed: {
    dialogVisible: {
      get () {
        return this.visible
      },
      set (val) {
        this.$emit('update:visible', val)
      }
    }
  },
  props: {
    visible: {
      // .sync 绑定的值
      type: Boolean,
      default: false
    }
  }
}
</script>
```
### ref使用
> 注意：需在 dom 元素加载完成后 $refs 才能获取到该元素，所以需要判断 $refs 是否获取到元素才可继续，否则会报错

父组件：
``` vue
<template>
  <child ref="child" />
  <Button
    type="primary"
    @click="prentClick"
  >
    点击调用子组件方法
  </Button>
</template>
<script>
import child from './components/child'
export default {
  name: 'parent',
  components: {
    child
  },
  data {
    return {
      parentMsg: '父组件的属性'
    }
  },
  methods: {
    prentMethod () {
      console.log('父组件的方法')
    },
    prentClick () {
      this.$refs.child.childMethod() // 调用子组件的方法
      console.log(this.$refs.child.childMsg) // 获取子组件的属性
    },
    // 获取子组件的ref
    getChildRef () {
      const thisRef = this.$refs.child
      if (thisRef) {
        const tableRef = thisRef.$refs.ehTable
        if (tableRef) {
          ...
        }
      }
    }
  }
}
</script>
```
子组件：
``` vue
<template>
  <el-table ref="ehTable" />
  <Button
    type="primary"
    @click="childClick"
  >
    点击调用父组件方法
  </Button>
</template>
<script>
export default {
  name: child,
  data {
    return {
      childMsg: '子组件的属性'
    }
  },
  methods: {
    childMethod () {
      console.log('子组件的方法')
    },
    childClick () {
      this.$parent.prentMethod() // 调用父组件的方法
      console.log(this.$parent.parentMsg) // 获取父组件的属性
    }
  }
}
</script>
```
## 兄弟组件传值
在main.js注册一个全局bus：
``` js
Vue.prototype.bus = new Vue()
```
兄弟组件A向组件B传值
组件A:
``` vue
<template>
  <Button
    type="primary"
    @click="sendClick"
  >
    点击调用兄弟组件B方法
  </Button>
</template>
<script>
export default {
  name: 'A',
  data {
    return {
      aMsg: '兄弟组件A的属性'
    }
  },
  methods: {
    sendClick () {
      this.bus.$emit('sendBybus', this.aMsg)
    }
  }
}
</script>
```
组件B:
``` vue
<template>
  {{bMsg}}
</template>
<script>
export default {
  name: 'B',
  data {
    return {
      bMsg: '兄弟组件B的属性'
    }
  },
  methods: {
    sendClick () {
      this.bus.$on('sendBybus', data => {
        console.log(data)
        this.bMsg = data
      })
    }
  }
}
</script>
```
## DOM的异步更新
``` html
<div>
  <span>
    {{index}}
  </span>
  <button @click="handleChange">
    点击
  </button>
</div>
```
### nextTick
``` js
handleChange () {
  this.$nextTick(() => {
    this.index++
  })
}
```
### forceUpdate
``` js
handleChange () {
  this.index++
  this.$forceUpdate()
}
```
## vue多项目配置
### 安装
    npm install cross-env
### 配置
在`package.json`文件配置变量
``` js
"scripts": {
  "serve": "vue-cli-service serve",
  "serve:stage": "cross-env VUE_APP_PREFIX=stage vue-cli-service serve"
  "build": "vue-cli-service build",
  "build:stage": "cross-env VUE_APP_PREFIX=stage vue-cli-service build",
  "lint": "vue-cli-service lint"
},
```
自定义一个`cross-env`变量`VUE_APP_PREFIX`，在项目中接收到变量，然后通过变量来区分项目和运行环境，因此需要在`vue.config.js`配置
``` js
module.exports = {
  configureWebpack: {
    plugins: [
      new webpack.DefinePlugins({
        "process.env": {
          VUE_APP_PREFIX: JSON.stringify(process.env.VUE_APP_PREFIX)
        }
      })
    ]
  }
}
```
### 使用
``` js
if (process.env.VUE_APP_PREFIX === 'stage') {
  // 执行相应的操作
}
```