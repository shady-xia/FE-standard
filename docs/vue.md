
# Vue 项目开发规范

## 前言 :id=start

Vue 项目规范以 Vue官方规范（[https://cn.vuejs.org/v2/style-guide/](https://cn.vuejs.org/v2/style-guide/)）为基础进行开发，同时参照了诸多开发团队的经验，尽量保证了该规范在不同vue团队下的的通用性和统一性。

> 注：文中提到的书写规范
> `PascalCase`（大骆驼拼写法，首字母大写）
> `camelCase`（骆驼拼写法，首字母小写）
> `kebab-case`（短横拼写法）

## 1. 组件命名规范 :id=naming

A. 组件的文件名称、组件的`name`属性、导入导出组件时，统一使用大驼峰 `PascalCase`  的书写规范

B. 模板中的组件采用短横线 `kebab-case` 的书写规范。


**1.1 组件文件**

vue组件的文件名使用 `PascalCase`  的拼写方式

```
components/
    |- SearchButtonClear.vue
    |- SearchButtonRun.vue
    |- SearchInputQuery.vue
```

**1.2 组件名称**

组件的 `name`使用 `PascalCase`  的拼写方式

```javascript
export default {
    name: 'SearchButton', 
    // ... 
}
```

**1.3 引入组件**

对于组件的导出/导入，使用大驼峰 `PascalCase`

```javascript
import MyComponent from './MyComponent.vue'

export default {
    components: 'MyComponent', 
    // ... 
}
```

**1.4 模板中组件名的大小写**

无论是在DOM模板，还是在字符串模板中，统一使用 `kebab-case` 拼写方式，且要手动闭合组件。

```html
<!-- bad -->
<MyComponent></MyComponent>

<!-- bad -->
<my-component/>

<!-- good -->
<my-component></my-component>
```


## 2. Prop 定义规范 :id=prop

**2.1 在声明 `prop` 时，遵循以下的规范：**
- 必须指定类型
- 必须加上注释，表明其含义
- 必须加上 `required` 或者 `default`，两者二选其一
- 如果有业务需要，加上 `validator` 验证

```javascript
props: {
  // 组件状态，用于控制组件的颜色
   status: {
     type: String,
     required: true,
     validator: function (value) {
       return [
         'succ',
         'info',
         'error'
       ].indexOf(value) !== -1
     }
   },
    // 用户级别，用于显示皇冠个数
   userLevel：{
      type: String,
      required: true
   }
}
```

**2.2 完整单词的组件名**

组件名应该倾向于完整单词而不是缩写。

bad:
```
components/ 
    |- SdSettings.vue
    |- UProfOpts.vue
```

good:
```
components/ 
    |- StudentDashboardSettings.vue
    |- UserProfileOptions.vue
```


**2.3 Prop 名大小写**

在声明 prop 的时候，其命名应该始终使用 `camelCase`，而在模板和 JSX 中应该始终使用 `kebab-case`。

```javascript
// greetingText
props: { 
    greetingText: String
}
```

```html
<!-- greeting-text -->
<welcome-message greeting-text="hi"></welcome-message>
```

## 3. Vue 中的书写顺序 :id=order

**3.1 vue 选项的声明顺序**

vue 组件/实例的选项应该遵循统一的顺序：

```js
// 全局的
- name

// 模板依赖 (模板内使用的资源)
- components
- directives
- filters
- mixins

// 接口 
- props    

// 本地状态 (本地的响应式 property)
- data     
- computed

// 事件和声明周期
- watch
- created
- mounted
- updated
- activited
- update
- beforeDestroy
- destroyed

// 非响应式的 property
- methods   
```

so，一个vue单文件组件大致就是这样的：

```vue
<template> 
    <div></div> 
</template>

<script>
export default { 
    name: '',
    components: {}, 
    data() {
        return {}; 
    }, 
    computed: {},
    watch: {},
    mounted() {},
    methods: {}, 
}; 
</script> 

<style lang="scss" scoped>
</style>
```

**3.2 元素属性的顺序**

元素 (包括组件) 的 `attribute` 应该有统一的顺序，原生属性放在前面，指令其次，传参和方法放在最后

```
- class, id, ref 
- name, data-*, src, alt, for, type, href, value, max, min 
- title, placeholder, aria-*, role 
- required, readonly, disabled 
- v-model, v-for, key, v-if, v-show, v-bind,:
- foo="a" bar="b" baz="c"
```

## 4. Vue 通用规范 :id=common

**4.1 多个 attribute 的元素应该分多行撰写，每个 attribute 一行**

```html
<my-component
    foo="a" 
    bar="b" 
    baz="c">
</my-component>
```

**4.2 统一使用指令缩写**

```html
<input
    :value="newTodoText"
    :placeholder="newTodoInstructions"
>

<input
    @input="onInput"
    @focus="onFocus"
>

<template #header> 
    <h1>Here might be a page title</h1> 
</template>

<template #footer> 
    <p>Here's some contact info</p> 
</template>
```

**4.3 公共组件必须保证是独立的, 可复用的，拒绝定制代码。要写组件使用说明。**

```html
<!--
  /**
  * 组件名称
  * @desc 组件描述
  * @author 组件作者
  * @param {Number} [age]    - 参数说明
  * @param {String} [name] - 参数说明
  * @example 调用示例
  *  <person :age="age" :columns="columns" :name="name"></person>
  */
   -->
   
   <inputFiled 
        type="number" :placeholder="$t('login.input')" 
        iconClass="icon-ico_shoujihao"
        max="11" 
    />
```

**4.4 关于组件内样式**

1）为组件样式设置作用域，保证样式只在当前组件内生效

```
/* bad */ 
<style>
.btn-close { 
    background-color: red;
} 
</style> 

/* good */ 
<style scoped>
.button-close { 
    background-color: red; 
}
</style>
```

2）覆盖第三方库的样式时，需加上顶级作用域

```
<style>
/* bad */ 
.el-input { width: 254px !important; } 

/* good */ 
.page-user .el-input {
    width: 254px !important; 
} 
/* .customerForm为当前组件的顶级dom */
</style> 
```

3）通用组件不要设置 `scoped` 作用域，这样就没法在其他组件中复写了。


**4.5 尽量不在 mounted、created 之类的方法写逻辑。**

把逻辑抽离出去，在生命周期函数中只做引用

**4.6 组件内不要使用复杂的行内表达式**

可以使用 `method` 或是 `computed` 属性来替代其功能。复杂的行内表达式难以阅读，也不能复用。

**4.7 模块的命名尽量和后端保持统一**

模块命名尽量和后端对模块的命名保持一致，这样方便对接api，也保证了前后端的一致性

## 5. Vue Router 规范 :id=router

**5.1 使用懒加载机制**

**5.2 路由都添加 name 字段，路由跳转时，统一使用 `name` 进行跳转，方便管理**

```html
<router-link :to="{ name: 'home' }">Home</router-link>
```
```javascript
this.$router.push({ name: 'home' })
```

## 6. Vue 项目目录规范 :id=contents

针对一个完整的 `Vue` 项目，它的目录应遵循以下的规范

```
src                               源码目录
|-- api                              所有api接口
|-- assets                         静态资源，images, icons等
|-- components                 公用组件
|-- config                         配置信息
|-- constants                     常量信息，项目所有Enum, 全局常量等
|-- directives                     自定义指令
|-- filters                          过滤器，全局工具
|-- libs                               外部引用的插件存放及修改文件
|-- plugins                        插件，全局使用
|-- router                          路由，统一管理
|-- store                            vuex, 统一管理
|-- styles                           样式文件
|-- views                           视图目录
|   |-- User                            User模块名
|   |-- |-- UserList.vue                    User列表页面
|   |-- |-- UserEdit.vue                   User编辑页面
|   |-- |-- UserDetail.vue                User详情页面
|   |-- |-- components                   User模块通用组件文件夹
|   |-- Employee                         Employee模块
```

注意事项：
1. 全局通用的组件放在 `/src/components` 下
2. 其他业务页面中的组件，放在各自页面下的 `./components` 文件夹下
3. `components` 和 `views` 下面的文件夹和vue文件，都使用大驼峰 `PascalCase`  的命名规则 （除了`index.vue`）

