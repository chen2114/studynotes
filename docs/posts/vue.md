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