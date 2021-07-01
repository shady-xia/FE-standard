# CSS / Scss 开发规范

## 前言 :id=start

用更合理的方式写 CSS 和 Scss

## CSS :id=css

### 基础 :id=common

- 使用四个空格键作为缩进
- 每个属性声明末尾都要加分号
- 尽量不要使用id选择器

### 空格 :id=space

以下几种情况需要空格：
- 选择器 与 `{` 之间必须包含空格
- 在属性的冒号` :` 后面加上一个空格
- `!important` '`!`'前
- 属性值中的'`,`'后

```css
/* bad */
.element{
    color:red!important;
    background-color:rgba(0,0,0,.5);
}

/* good */
.element {
    color: red !important;
    background-color: rgba(0, 0, 0, .5);
}
```

- `>`、`+`、`~` 选择器的两边各保留一个空格。

```css
/* bad */
main>nav {
    padding: 10px;
}

label+input {
    margin-left: 5px;
}

input:checked~button {
    background-color: #69C;
}

/* good */
main > nav {
    padding: 10px;
}

label + input {
    margin-left: 5px;
}

input:checked ~ button {
    background-color: #69C;
}
```

### 换行 :id=enter

以下的几种情况需要换行：

- '`{`' 后和 '`}`' 前
- 每个属性独占一行
- 当一个规则包含多个selector时，每个选择器需独占一行

```css
/* bad */
.element{color: red; background-color: black;}

/* bad */
.element, .dialog {
    color: red;
}

/* good */
.element {
    color: red;
    background-color: black;
}

/* good */
.element,
.dialog {
    color: red;
}
```

### 属性的书写顺序 :id=order

css各属性书写顺序的建议：

- 定位属性：`position`, `top`, `right`, `z-index` 等
- 盒子类型：`display`, `float`, `flex`相关，`grid`相关
- 盒子尺寸：`width`, `height`, `padding`, `margin`
- 文字系列：`font`,` line-height`, `letter-spacing`,  `text-align`等
- 盒子样式：`background`, `color`, `border`，`box-shadow`，
  `opacity` 等
- 其他：`animation`, `transition`,`cursor`等

```css
.write-order {
    /* 元素的定位方式 */
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 100;
    
    /* 盒子类型 */
    display: flex;
    flex: auto;
    justify-content: center;
    align-items: center;
    order: 1;
    
    /* 盒子尺寸 */
    width: 100px;
    height: 100px;
    padding: 20px;
    margin: 20px;

    /* 文字系列 */
    font: normal 13px "Helvetica Neue", sans-serif;
    line-height: 1.5;
    text-align: center;
    
   /* 盒子样式 */
    color: #333;
    background-color: #f5f5f5;
    border: 1px solid #e5e5e5;
    border-radius: 3px;
    opacity: 1;
    
    /* 其他 */
    transition: all .3s ease-in-out;
    cursor: pointer;
 }
```

### 值与单位 :id=unit

- 当数值为 0 - 1 之间的小数时，省略整数部分的 0。

```css
/* bad */
panel {
    opacity: 0.8
}

/* good */
panel {
    opacity: .8
}
```

- url() 函数中的路径不加引号。

```css
body {
    background: url(bg.png);
}
```

- 长度为 0 时须省略单位。 (也只有长度单位可省)

```css
/* bad */
body {
    padding: 0px 5px;
}

/* good */
body {
    padding: 0 5px;
}
```

- RGB颜色值必须使用十六进制记号形式 `#rrggbb`。不允许使用 `rgb()`。

> 带有alpha的颜色信息可以使用 rgba()

```css
/* bad */
.success {
    box-shadow: 0 0 2px rgba(0,128,0,.3);
    border-color: rgb(0, 128, 0);
}

/* good */
.success {
    box-shadow: 0 0 2px rgba(0, 128, 0, .3);
    border-color: #008000;
}
```

- 颜色值可以缩写时，必须使用缩写形式。
- 颜色值不允许使用命名色值

```css
/* bad */
.success {
    background-color: #aaccaa;
}

/* bad */
.success {
    color: lightgreen;
}

/* good */
.success {
    background-color: #aca;
}

/* good */
.success {
    color: #90ee90;
}
```

### 命名规则 :id=name
见`命名规范及其他相关约定`

### 其他 :id=others

**响应式**

Media Query 不得单独编排，必须与相关的规则一起定义。

```css
/* Bad */
/* header styles */
/* main styles */
/* footer styles */
@media (...) {
    /* header styles */
    /* main styles */
    /* footer styles */
}

/* Good */
/* header styles */
@media (...) {
    /* header styles */
}

/* main styles */
@media (...) {
    /* main styles */
}

/* footer styles */
@media (...) {
    /* footer styles */
}
```

## Scss :id=scss

### 属性声明的排序 :id=declare

1. 属性声明

首先列出除去 `@include` 和嵌套选择器之外的所有属性声明。

```scss 
.btn-green { 
    background: green;
    font-weight: bold; 
    // ...
} 
``` 

2. `@include` 声明

紧随后面的是 `@include`，这样可以使得整个选择器的可读性更高。

```scss
.btn-green {
    background: green; 
    font-weight: bold;
    @include transition(background 0.5s ease);
    // ...
}
``` 

3. 嵌套选择器

如果有必要用到嵌套选择器，把它们放到最后，在规则声明和嵌套选择器之间要加上空白，相邻嵌套选择器之间也要加上空白。嵌套选择器中的内容也要遵循上述指引。

```scss
.btn {
    background: green;
    font-weight: bold;
    @include transition(background 0.5s ease); 

    .icon {
        margin-right: 10px;
    } 
}
```

### 变量 :id=variable

- 命名变量时不要含糊不清
- 变量名应使用破折号（例如 `$my-variable`）代替 `camelCased` 和 `snake_cased` 风格。

例子：
```scss
$orange: #ffa600; 
$grey: #f3f3f3; 
$blue: #82d2e5; 
$link-primary: $orange; 
$link-secondary: $blue; 
$link-tertiary: $grey; 
$radius-button: 5px;
$radius-tab: 5px;
```

### Mixins :id=mixins

为了让代码遵循 DRY 原则（Don't Repeat Yourself）、增强清晰性或抽象化复杂性，应该使用 `mixin`，这与那些命名良好的函数的作用是异曲同工的。虽然 `mixin` 可以不接收参数，但要注意，假如你不压缩负载（比如通过 gzip），这样会导致最终的样式包含不必要的代码重复。

### 避免使用扩展指令 :id=extend

应避免使用 `@extend` 指令，因为它并不直观，而且具有潜在风险，特别是用在嵌套选择器的时候。即便是在顶层占位符选择器使用扩展，如果选择器的顺序最终会改变，也可能会导致问题。（比如，如果它们存在于其他文件，而加载顺序发生了变化）。

其实，使用 `@extend` 所获得的大部分优化效果，`gzip` 压缩已经帮助你做到了，因此你只需要通过 `mixin` 让样式表更符合 DRY 原则就足够了。

### 避免嵌套层级过多 :id=nesting

**请不要让嵌套选择器的深度超过 3 层！**

```scss 
.page-container { 
    .content { 
        .profile { 
        // STOP! 
        } 
    } 
} 
``` 

在书写的时候，可以适当的将层级结构抽离出来，按照类别写在一起：

```scss
.page-title { 
    font-size: 3em; 
    font-weight: bold; 
    color: red; 
} 

// Posts
.post {
    margin: 2em 0;
}
.post-title { 
    font-size: 2em;
    font-weight: normal; 
} 

// Summaries
.summary { 
    margin: 1em 0;
} 
.summary-title {
    font-size: 1.2em;
    font-weight: bold; 
}
```