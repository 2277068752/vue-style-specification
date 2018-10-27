# vue-style-specification

基于 Vue2.0 的前端开发规范

---

### 一、Vue 组件部分

##### 1、组件命名

组件名应该更倾向于完整单词而不是缩写

###### 1.1、单文件名为多个单词

组件名应为多个英文单词的拼接（根组件 App.vue 除外），且为小驼峰命名方式；不能含有特殊字符、空格。

正例：

```
export default {
  name: 'associationAnalysis'
}
```

反例:

```
export default {
  name: 'Associationanalysis'
}

export default {
  name: 'associationanalysis'
}

...
```

###### 1.2、基础组件命名

应用特定的单个基础组件命名应该以特定的前缀开头，比如 Base、App、i、v

正例：

```
components/
|- baseButton.vue
|- baseTable.vue
|- baseIcon.vue
```

反例

```
components/
|- MyButton.vue
|- VueTable.vue
|- Icon.vue
```

###### 1.3、单例组件名

单个活跃实例的组件应该以 The 前缀命名，以表示其唯一性。且该类组件应该和父类在同一文件夹并且具有独立的文件目录（如：component）或者以 _The + 父类组件名_ 的方式命名

##### 2、组件数据

组件的 data 必须是一个函数。
当在组件中使用 data 属性的时候，它的值必须是返回一个对象的函数

正例：

```
export default {
  data () {
    return {
      name: 'bar'
    }
  }
}
```

反例：

```
export default {
  data: {
    name: 'bar'
  }
}
```

##### 3、Props 定义

Prop 子项的定义应该尽量详细，至少需要制定其类型，必要时且应该加上注释，方便调用

正例：

```
  props: {
      list: {
      type: Array,
      required: true,
      default () {
        return []
      } // prop:表头绑定的地段，label：表头名称，align：每列数据展示形式（left, center, right），width:列宽
    } // 数据列表
  }
```

反例：

```
props: {
  list: {},
  str: ''
}
```

##### 4、为 v-for 设置键值

更倾向于将 v-for 循环作用在 template 元素上，

正例：

```
 <template v-for="(column, key) in columns">
   <div :key="key"></div>
 </template>

 <ul>
   <li v-for="todo in todos" :key="todo.id"></li>
 </ul>
```

反例：

```
 <template v-for="column in columns">
   <div></div>
 </template>
```

##### 5、避免 v-if 和 v-for 用在一起

永远不要把 v-if 和 v-for 同时用在同一个元素上，这种情况下，如果想避免 dom 层级太深，template 元素就体现出价值了

正例：

```
<template v-for="(column, key) in columns">
   <div :key="key" v-if="column.show"></div>
</template>

 <ul>
   <li v-for="todo in todos" :key="todo.id">
    <div v-if="todo.show"></div>
   </li>
 </ul>
```

反例

```
 <ul>
   <li v-for="todo in todos" :key="todo.id" v-if="todo.show">
   </li>
 </ul>
```

##### 6、为组件样式设置作用域

对于应用来讲，顶级 App 组件和布局组件中的样式可以是全局的，但是其它所有组件应该是有作用域的，下面有几种方式：

###### 6.1、给样式添加 scoped，（适合独立功能模块的组件，不太建议使用这种模式）

```
<style lang="scss" scoped>
 ...
</style>
```

###### 6.2、css 的 BEM 策略

_BEM 的意思就是块（block）、元素（element）、修饰符（modifier）_

```
<template>
  <button class="c-Button c-Button--close">X</button>
</template>

<!-- 使用 BEM 约定 -->
<style>
.c-Button {
  border: none;
  border-radius: 2px;
}

.c-Button--close {
  background-color: red;
}
</style>
```

###### 6.3、为组件顶部 dom 元素添加修饰符 + less、scss 预处理语言进行嵌套(注意嵌套层级，最好不要超过 3 级)

```
<template>
  <div class="lukou-airport-over-view-page">
    <div class="content">
      ...
    </div>
  </div>
</template>
<style lang="scss">
 .lukou-airport-over-view-page {
    .content {
      ...
    }
 }
</style>
```

##### 7、在模板中组件名采用小驼峰

正例：

```
import Nation from '../../../components/iEcharts/overViewTouristSourceAna'
```

反例：

```
const ijiangsuMap = () => import('../../../components/iEcharts/JiangsuMapIndexFlow')
 ...
```

##### 8、模板中的表达式

组件模板中应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或者方法。

复杂表达式会让你的模板变得不那么声明式。我们应该尽量的描述此处出现什么，而不是如何计算得出的那个值。而且计算属性和方法使得代码可以重用。

正例：

```
<!-- 在模板中 -->
{{ normalizedFullName }}
// 复杂表达式已经移入一个计算属性
computed: {
  normalizedFullName: function () {
    return this.fullName.split(' ').map(function (word) {
      return word[0].toUpperCase() + word.slice(1)
    }).join(' ')
  }
}
```

反例：

```
{{
  fullName.split(' ').map(function (word) {
    return word[0].toUpperCase() + word.slice(1)
  }).join(' ')
}}
```

##### 9、单文件组件的顶级元素的顺序

单文件组件应该总是让

```
<template>、<script>、<style>
```

标签的顺序保持一致。且

```
<style>
```

要放到最后。

正例：

```
<!-- ComponentA.vue -->
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>
```

##### 10、vuex 中 state、getters、mutations、actions 中各部分 命名规范

state、getters 中定义的属性、变量遵循 js 中变量的命名规范

正例：

```
const state = {
  list: [],
  activeColumn: 4 // 当前板块
}
```

```
const getters = {
  list: state => {
    return state.list
  }
}
```

mutations：Vue 建议我们 mutation 类型用大写常亮表示
正例：

```
// 持久化目录
SET_MENUS_LIST (state, val) {
  state.list = val
}
```

action:全小写或者使用\_连接各单词

```
// 持久化目录
set_menus_list ({ commit }, val) {
  commit('SET_MENUS_LIST', val)
}
```

++在 vue 中，只有 mutation 才能改变 state. mutation 类似事件，每一个 mutation 都有一个类型和一个处理函数，因为只有 mutation 才能改变 state, 所以处理函数自动会获得一个默认参数 state. 所谓的类型其实就是名字，action 去 comit 一个 mutation++

##### 11、Vue 组件各模块声明顺序

Vue 组件中，各模块声明顺序按照 Vue 生命周期顺序

正例：

```
<script>
export default {
  components: {},
  data () {
    return {}
  },
  created () {
  },
  mounted () {
  },
  computed: {},
  methods: {}
}
</script>
```

### 二、JS 部分

##### 1、变量命名

命名方式：小驼峰式

正例：

```
let tempData = this.data.find(_x => _x.key === key).value
```

反例：

```
let tempdata = this.data.find(_x => _x.key === key).value
...
```

##### 2、函数

命名方式：小驼峰式（构造函数使用大驼峰式（帕斯卡命名法））

正例：

```
// 初始化客流来源数据
initUserFlowMap () {
  this.BLL.flowCityDay()
}
```

反例：

```
inituserflowmap () {
  this.BLL.flowCityDay()
}
...
```

##### 3、常量

命名方式：全部大写，使用大写字母和下划线来组合命名，下划线用以分割单词。

正例：

```
const KEY = 'over-view-line' // 取json数据的key
```

反例：

```
const key = 'over-view-line' // 取json数据的key
const lineNoAreaKey = 'over-view-line' // 昨日通行折线图key
```

##### 4、使用严格相等

总是使用 === 精确的比较操作符，避免在判断的过程中，由 JavaScript 的强制类型转换所造成的困扰

正例：

```
log('0' == 0); // true
log('' == false); // true
log('1' == true); // true
log(null == undefined); // true
```

##### 5、[尽量使用 ES6 语法](http://es6.ruanyifeng.com/)

##### 6、条件判断尽量使用三元运算符

##### 7、强制遵循 eslint 规范

### 三、注释规范

##### 1、单行注释 （//）

- 单独一行：// （双斜杠）与注释文字之间保留一个空格
- 在代码后面添加注释：//（双斜杠）与代码之间保留一个空格，同事与注释文字之间保留一个空格
- 注释代码：与代码之间保留一个空格

正例：

```
// 创建Map实例
let map = new BMap.Map('mapContent', { enableMapClick: true })

ctrl.setAnchor(BMAP_ANCHOR_TOP_RIGHT) // eslint-disable-line

//  map.addControl(ctrl)
```

##### 2、多行注释 （/_ 注释说明 _/）

- 第一行为 /_，最后一行为 _/，其他行以 _ 开始，并且注释文字与 _ 之间保留一个空格

正例：

```
/* this.tableCurrentPagination = {
           pageIndex: 1,
           pageSize: size
         } */
```

##### 3、函数（方法）注释

函数(方法)注释也是多行注释的一种 [特殊要求](https://baike.baidu.com/item/javadoc)

正例：

```
  /**
   * 取全国的客流来源数据
   * @param param
   */
```

##### 4、Html 块级注释：可以将 html dom 接口按照功能块级折叠，方便阅读和维护

正例：

```
<!--region 选择框-->
    <el-table-column v-if="options.mutiSelect" type="selection" style="width: 55px;">
    </el-table-column>
<!--endregion-->
```

##### 5、css 注释规范: _同 js 中的单行注释和多行注释_

++项目的注释量不能低于整个项目可读性文件代码量的 30%++

##### 6、JS 文件 文件头注释

在 webstorm 设置中，添加自动生成注释头信息

```
/**
 * @Author : ChangJun
 * @Date : 2018/10/27
 * @Version : 1.0
 * @Content : 接口层
 */
```

### 四、css 规范

##### 1、ID 和 Class 的命名

id 为小驼峰命名方式，class 为小写单词并且以 - 横线连接

正例：

```
.lukou-airport-over-view-page {
  ...
}

#Test {
  ...
}
```

一般情况下 ID 不应用于样式，并且 ID 的权重很高，所以不使用 ID 解决样式问题，而是使用 Class

##### 2、尽量使用缩写属性

尽量使用缩写属性对于代码效率和可读性是很有用的、

正例：

```
border-top: 0;
padding: 0 1em 2em;
```

反例：

```
padding-bottom: 2em;
padding-left: 1em;
padding-right: 1em;
padding-top: 0;
```

##### 3、0 后面不带单位

正例：

```
padding-bottom: 0;
margin: 0;
```

反例：

```
padding-bottom: 0px;
margin: 0em;
```

##### 4、使用 less、sass 预处理语言，在使用之前，烦请务必认真阅读相关文档

[less](https://less.bootcss.com/)

[sass](https://www.sass.hk/)

| 日期       | 作者  | 备注   |
| ---------- | ----- | ------ |
| 2018-04-28 | 常 竣 | 第一版 |
| 2018-10-26 | 常 竣 | 第二版 |
