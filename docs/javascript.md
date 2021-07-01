# JavaScript开发规范

## 前言 :id=start

本规范以[Airbnb JavaScript Style](https://github.com/airbnb/javascript)为蓝本，根据团队的风格进行派生和改动

> 1、合并了部分条例
> 2、修改了部分文本，以提升可读性
> 3、去除了一些不常用的、明显的错误写法及基本前端常识相关的条例
> 4、根据团队的习惯，对部分风格相关的条例有所修改

## 1. 类型和引用 :id=types

- [1.1](#1.1) **基本类型**: 直接存取基本类型。

+ `字符串`
+ `数值`
+ `布尔类型`
+ `null`
+ `undefined`

```javascript 
const foo = 1; 
let bar = foo; 

bar = 9;

console.log(foo, bar); // => 1, 9 
```

- [1.2](#1.2) **复杂类型**: 通过引用的方式存取复杂类型。

+ `对象`
+ `数组`
+ `函数`

```javascript 
const foo = [1, 2]; 
const bar = foo;

bar[0] = 9;

console.log(foo[0], bar[0]); // => 9, 9
```

- [1.3](#1.3) 在定义不会改动的变量时使用 `const` ；不要使用 `var`。

> 这样能更方便将常量与会变化的变量进行区分

```javascript
// bad
var tabSize = '4';

// good 
const tabSize = 4; 
``` 

- [1.4](#1.4) 如果你一定需要可变动的引用，使用 `let` 代替 `var`。

> 为什么？因为 `let` 是块级作用域，而 `var` 是函数作用域。

```javascript
// bad
var count = 1;
if (true) {
    count += 1;
}

// good, use the let.
let count = 1;
if (true) {
    count += 1;
}
``` 

- [1.5](#1.5)  注意 `let` 和 `const` 都是块级作用域。

```javascript 
// const 和 let 只存在于它们被定义的区块内。
{
    let a = 1;
    const b = 1;
}
console.log(a); // ReferenceError
console.log(b); // ReferenceError
```


## 2. 对象 :id=object

- [2.1](#2.1) 使用字面值创建对象。eslint: [`no-new-object`](http://eslint.org/docs/rules/no-new-object.html)

```javascript
// bad
const item = new Object(); 

// good
const item = {}; 
``` 

- [2.2](#2.2)  当创建一个带有动态属性名的对象时，用计算后属性名

> 为什么？因为这样可以让你在一个地方定义所有的对象属性。

```javascript
function getKey(k) {
    return `a key named ${k}`;
}

// bad
const obj = {
    id: 5,
    name: 'San Francisco',
};
obj[getKey('enabled')] = true;

// good getKey('enabled')是动态属性名
const obj = {
    id: 5,
    name: 'San Francisco',
    [getKey('enabled')]: true,
};
```

- [2.3](#2.3)  使用对象方法的简写。eslint: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html)

```javascript
// bad
const atom = {
    value: 1,

    addValue: function (value) { 
        return atom.value + value; 
    }, 
}; 

// good 
const atom = { 
    value: 1, 

    addValue(value) { 
        return atom.value + value; 
    }, 
}; 
``` 

- [2.4](#2.4) 使用对象属性值的简写。eslint: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html)

> Why? 这样写的更少且更可读

```javascript 
const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = { 
    lukeSkywalker: lukeSkywalker,
}; 

// good
const obj = { 
    lukeSkywalker, 
}; 
```

- [2.5](#2.5) 将你的所有缩写放在对象声明的开始.

> Why? 这样也是为了更方便的知道有哪些属性用了缩写.

```javascript 
const anakinSkywalker = 'Anakin Skywalker'; 
const lukeSkywalker = 'Luke Skywalker'; 

// bad 
const obj = { 
    episodeOne: 1, 
    twoJedisWalkIntoACantina: 2, 
    lukeSkywalker, 
    episodeThree: 3, 
    mayTheFourth: 4, 
    anakinSkywalker, 
};

// good
const obj = { 
    lukeSkywalker, 
    anakinSkywalker, 
    episodeOne: 1, 
    twoJedisWalkIntoACantina: 2, 
    episodeThree: 3, 
    mayTheFourth: 4, 
}; 
``` 

- [2.6](#2.6) 只对那些无效的标示使用引号 `''`. eslint: [`quote-props`](http://eslint.org/docs/rules/quote-props.html)

> Why? 通常我们认为这种方式主观上易读。他优化了代码高亮，并且也更容易被许多JS引擎压缩。

```javascript 
// bad
const bad = {
    'foo': 3,
    'bar': 4, 
    'data-blah': 5,
};

// good
const good = { 
    foo: 3, 
    bar: 4, 
    'data-blah': 5,
}; 
```

- [2.7](#2.7) 不要直接调用 `Object.prototype`上的方法，如`hasOwnProperty`, `propertyIsEnumerable`, `isPrototypeOf`。

> Why? 在一些有问题的对象上， 这些方法可能会被屏蔽掉 - 如：`{ hasOwnProperty: false }` - 或这是一个空对象`Object.create(null)`

```javascript
// bad
console.log(object.hasOwnProperty(key));

// good 
console.log(Object.prototype.hasOwnProperty.call(object, key));

// best 
const has = Object.prototype.hasOwnProperty; // 在模块作用内做一次缓存
/* or */ 
import has from 'has'; // https://www.npmjs.com/package/has 
// ... 
console.log(has.call(object, key)); 
``` 

- [2.8](#2.8) 对象浅拷贝时，更推荐使用扩展运算符`...`，而不是 [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)。
- 获取对象指定的几个属性时，用对象的rest解构运算符`...`更好。

```javascript
// very bad 
const original = { a: 1, b: 2 };
const copy = Object.assign(original, { c: 3 }); // 这会导致original发生变化
delete copy.a;  // 同样会导致original发生变化

// bad
const original = { a: 1, b: 2 };
const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 } 

// good es6扩展运算符 ... 
const original = { a: 1, b: 2 }; 
// 浅拷贝
const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 } 

// rest 赋值运算符
const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
``` 

## 3. 数组 :id=array

- [3.1](#3.1) 使用字面值创建数组。eslint: [`no-array-constructor`](http://eslint.org/docs/rules/no-array-constructor.html)

```javascript 
// bad 
const items = new Array(); 

// good 
const items = []; 
``` 

- [3.2](#3.2) 向数组添加元素时使用 `Arrary#push` 替代直接赋值。

```javascript 
const someStack = []; 

// bad 
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
``` 

- [3.3](#3.3)  使用拓展运算符 `...` 做数组浅拷贝，类似于上面的对象浅拷贝

```javascript
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i++) {
    itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];
```

- [3.4](#3.4) 用 `...` 运算符而不是[`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from)来将一个可迭代的对象转换成数组。

```javascript
const foo = document.querySelectorAll('.foo');

// good
const nodes = Array.from(foo);

// best
const nodes = [...foo];
```

- [3.5](#3.5) 用 [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 去将一个类数组对象转成一个数组。

```javascript
const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 };

// bad
const arr = Array.prototype.slice.call(arrLike);

// good
const arr = Array.from(arrLike);
```

- [3.6](#3.6) 用 [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) 而不是 `...` 运算符去做map遍历。 因为这样可以避免创建一个临时数组。

```javascript
// bad
const baz = [...foo].map(bar);

// good
const baz = Array.from(foo, bar);
```

- [3.7](#3.7) 在数组方法的回调函数中使用 `return` 语句。 如果函数体由一条返回一个表达式的语句组成， 并且这个表达式没有副作用， 这个时候可以忽略return，详见 [8.2](#arrows--implicit-return). eslint: [`array-callback-return`](http://eslint.org/docs/rules/array-callback-return)

```javascript
// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});

// good 函数只有一个语句
[1, 2, 3].map(x => x + 1);

// bad - 没有返回值， 因为在第一次迭代后acc 就变成undefined了
[[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
  const flatten = acc.concat(item);
  acc[index] = flatten;
});

// good
[[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
  const flatten = acc.concat(item);
  acc[index] = flatten;
  return flatten;
});

// bad
inbox.filter((msg) => {
  const { subject, author } = msg;
  if (subject === 'Mockingbird') {
    return author === 'Harper Lee';
  } else {
    return false;
  }
});

// good
inbox.filter((msg) => {
  const { subject, author } = msg;
  if (subject === 'Mockingbird') {
    return author === 'Harper Lee';
  }

  return false;
});
```

- [3.8](#3.8) 如果一个数组有很多行，在数组的 `[` 后和 `]` 前断行。 请看下面示例

```javascript
// bad
const arr = [
  [0, 1], [2, 3], [4, 5],
];

const objectInArray = [{
  id: 1,
}, {
  id: 2,
}];

const numberInArray = [
  1, 2,
];

// good
const arr = [[0, 1], [2, 3], [4, 5]];

const objectInArray = [
  {
    id: 1,
  },
  {
    id: 2,
  },
];

const numberInArray = [
  1,
  2,
];
```


## 4. 解构 :id=destructuring

- [4.1](#4.1) 用对象的解构赋值来获取和使用对象某个或多个属性值。eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

> Why？因为解构能减少临时引用属性。

```javascript
// bad
function getFullName(user) {
    const firstName = user.firstName;
    const lastName = user.lastName;

    return `${firstName} ${lastName}`;
}

// good
function getFullName(obj) {
    const { firstName, lastName } = obj;
    return `${firstName} ${lastName}`;
}

// best
function getFullName({ firstName, lastName }) {
    return `${firstName} ${lastName}`;
}
```

- [4.2](#4.2)  对数组使用解构赋值。

```javascript
const arr = [1, 2, 3, 4];

// bad
const first = arr[0];
const second = arr[1];

// good
const [first, second] = arr;
```

- [4.3](#4.3) 需要回传多个值时，使用对象解构，而不是数组解构。
> 为什么？增加属性或者改变排序不会改变调用时的位置。

```javascript
// bad
function processInput(input) {
    // then a miracle occurs
    return [left, right, top, bottom];
}

// 调用时需要考虑回调数据的顺序。
const [left, __, top] = processInput(input);

// good
function processInput(input) {
    // then a miracle occurs
    return { left, right, top, bottom };
}

// 调用时只选择需要的数据
const { left, right } = processInput(input);
```


## 5. Strings :id=strings

- [5.1](#5.1) 字符串使用单引号 `''` 。eslint: [`quotes`](https://eslint.org/docs/rules/quotes.html)

> 作为习惯性的约定

```javascript
// bad
const name = "Capt. Janeway";

// good
const name = 'Capt. Janeway';
```

- [5.2](#5.2) 超过100个字符的字符串不应该用string串联成多行。

> Why? 被折断的字符串工作起来是糟糕的而且使得代码更不易被搜索。

 ```javascript 
// bad 
const errorMessage = 'This is a super long error that was thrown because\
of Batman. When you stop to think about how Batman had anything to do \ 
with this, you would get nowhere \ 
fast.'; 

// bad 
const errorMessage = 'This is a super long error that was thrown because ' + 
'of Batman. When you stop to think about how Batman had anything to do ' + 
'with this, you would get nowhere fast.'; 

// good 
const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.'; 
```

- [5.3](#5.3) 程序化生成字符串时，使用模板字符串代替字符串连接。

> 为什么？模板字符串更为简洁，更具可读性。

```javascript
// bad
function sayHi(name) {
return 'How are you, ' + name + '?'; 
}

// bad
function sayHi(name) {
return ['How are you, ', name, '?'].join();
}

// good
function sayHi(name) {
return `How are you, ${name}?`;
}
```

- [5.4](#5.4) 永远不要在字符串中用`eval()`，他就是潘多拉盒子。 eslint: [`no-eval`](https://eslint.org/docs/rules/no-eval)

## 6. 函数 :id=functions

- [6.1](#6.1) 把立即执行函数包裹在圆括号里。 eslint: [`wrap-iife`](http://eslint.org/docs/rules/wrap-iife.html)

> Why? 一个立即调用的函数表达式是一个单元，把它和他的调用者（圆括号）包裹起来能更清晰地体现出这一点。
> 另外还请注意：在模块化世界里，你几乎用不着 IIFE

```javascript
// immediately-invoked function expression (IIFE)
(function () {
  console.log('Welcome to the Internet. Please follow me.');
}());
```

- [6.2](#6.2) 不要在非函数块（if、while等等）内声明函数，把这个函数分配给一个变量。【详见`no-loop-func`】

- [6.3](#6.3) **Note:** 在ECMA-262中 [块 `block`] 的定义是： 一系列的语句； 但是函数声明不是一个语句。 函数表达式是一个语句。

```javascript
// bad
if (currentUser) {
  function test() {
    console.log('Nope.');
  }
}

// good
let test;
if (currentUser) {
  test = () => {
    console.log('Yup.');
  };
}
```

- [6.4](#6.4) 不要用`arguments`命名参数。他的优先级高于每个函数作用域自带的 `arguments` 对象， 这会导致函数自带的 `arguments` 值被覆盖

```javascript
// bad
function foo(name, options, arguments) {
  // ...
}

// good
function foo(name, options, args) {
  // ...
}
```

- [6.5](#6.5) 不要使用`arguments`，用rest语法`...`代替。 eslint: [`prefer-rest-params`](http://eslint.org/docs/rules/prefer-rest-params)

> Why? `...`明确你想用那个参数。而且rest参数是真数组，而不是类似数组的`arguments`

```javascript
// bad
function concatenateAll() {
  const args = Array.prototype.slice.call(arguments);
  return args.join('');
}

// good
function concatenateAll(...args) {
  return args.join('');
}
```

- [6.6](#6.6) 用默认参数语法而不是在函数里对参数重新赋值。

```javascript
// really bad
function handleThings(opts) {
  // 第一， 我们不该改arguments
  // 第二： 如果 opts 的值为 false, 它会被赋值为 {}
  // 虽然你想这么写， 但是这个会带来一些细微的bug
  opts = opts || {};
  // ...
}

// still bad
function handleThings(opts) {
  if (opts === void 0) {
    opts = {};
  }
  // ...
}

// good
function handleThings(opts = {}) {
  // ...
}
```

- [6.7](#6.7) 把默认参数赋值放在最后

```javascript
// bad
function handleThings(opts = {}, name) {
  // ...
}

// good
function handleThings(name, opts = {}) {
  // ...
}
```

- [6.8](#6.8) 不要用函数构造器创建函数。 eslint: [`no-new-func`](http://eslint.org/docs/rules/no-new-func)

> Why? 以这种方式创建函数将类似于字符串 eval()，这会打开漏洞。

```javascript
// bad
var add = new Function('a', 'b', 'return a + b');

// still bad
var subtract = Function('a', 'b', 'return a - b');
```

- [6.9](#6.9) 函数签名部分要有空格。eslint: [`space-before-function-paren`](http://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks)

> Why? 统一性好，而且在你添加/删除一个名字的时候不需要添加/删除空格

```javascript
// bad
const f = function(){};
const g = function (){};
const h = function() {};

// good
const x = function () {};
const y = function a() {};
```

- [6.10](#6.10) 不要改参数. eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

> Why? 操作参数对象对原始调用者会导致意想不到的副作用。 就是不要改参数的数据结构，保留参数原始值和数据结构。

```javascript
// bad
function f1(obj) {
  obj.key = 1;
};

// good
function f2(obj) {
  const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
};
```

- [6.11](#6.11) 不要对参数重新赋值。 eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

> Why? 参数重新赋值会导致意外行为，尤其是对 `arguments`。这也会导致优化问题，特别是在V8里

```javascript
// bad
function f1(a) {
  a = 1;
  // ...
}

function f2(a) {
  if (!a) { a = 1; }
  // ...
}

// good
function f3(a) {
  const b = a || 1;
  // ...
}

function f4(a = 1) {
  // ...
}
```

- [6.12](#6.12) 用`spread`操作符`...`去调用多变的函数更好。 eslint: [`prefer-spread`](http://eslint.org/docs/rules/prefer-spread)

> Why? 这样更清晰，你不必提供上下文，而且你不能轻易地用`apply`来组成`new`

```javascript
// bad
const x = [1, 2, 3, 4, 5];
console.log.apply(console, x);

// good
const x = [1, 2, 3, 4, 5];
console.log(...x);

// bad
new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

// good
new Date(...[2016, 8, 5]);
```

- [6.13](#6.13) 调用或者书写一个包含多个参数的函数应该像这个指南里的其他多行代码写法一样： 每行值包含一个参数，每行逗号结尾。

```javascript
// bad
function foo(bar,
             baz,
             quux) {
  // ...
}

// good 缩进不要太过分
function foo(
  bar,
  baz,
  quux,
) {
  // ...
}

// bad
console.log(foo,
  bar,
  baz);

// good
console.log(
  foo,
  bar,
  baz,
);
```

## 7. 箭头函数 :id=arrowFunctions

- [7.1](#7.1) 当你一定要用函数表达式（在回调函数里）的时候就用箭头表达式吧。 eslint: [`prefer-arrow-callback`](http://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](http://eslint.org/docs/rules/arrow-spacing.html)

> Why?
>  他创建了一个`this`的当前执行上下文的函数的版本，这通常就是你想要的；而且箭头函数更简洁

```javascript
// bad
[1, 2, 3].map(function (x) {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```

- [7.2](#7.2) 如果函数体由一个没有副作用的[表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions)语句组成，删除大括号和return。否则，继续用大括号和 `return` 语句。 eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style.html)

> Why? 语法糖，当多个函数链在一起的时候好读

```javascript
// bad
[1, 2, 3].map(number => {
  const nextNumber = number + 1;
  `A string containing the ${nextNumber}.`;
});

// good
[1, 2, 3].map((number) => {
  const nextNumber = number + 1;
  return `A string containing the ${nextNumber}.`;
});

// good
[1, 2, 3].map(number => `A string containing the ${number}.`);

// good
[1, 2, 3].map((number, index) => ({
  [index]: number
}));

// 表达式有副作用就不要用隐式return
function foo(callback) {
  const val = callback();
  if (val === true) {
    // Do something if callback returns true
  }
}

let bool = false;

// bad
// 这种情况会return bool = true, 不好
foo(() => bool = true);

// good
foo(() => {
  bool = true;
});
```

- [7.3](#7.3) 万一表达式涉及多行，把他包裹在圆括号里更可读。

> Why? 这样清晰的显示函数的开始和结束

```js
// bad
['get', 'post', 'put'].map(httpMethod => Object.prototype.hasOwnProperty.call(
    httpMagicObjectWithAVeryLongName,
    httpMethod
  )
);

// good
['get', 'post', 'put'].map(httpMethod => (
  Object.prototype.hasOwnProperty.call(
    httpMagicObjectWithAVeryLongName,
    httpMethod
  )
));
```

- [7.4](#7.4) 如果你的函数只有一个参数并且函数体没有大括号，就删除圆括号。否则，参数总是放在圆括号里。 注意： 一直用圆括号也是没问题，只需要配置 [“always” option](https://eslint.org/docs/rules/arrow-parens#always) for eslint. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html)

> Why? 这样少一些混乱， 其实没啥语法上的讲究，就保持一个风格。

```js
// bad
[1, 2, 3].map((x) => x * x);

// good
[1, 2, 3].map(x => x * x);

// good
[1, 2, 3].map(number => (
  `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
));

// bad
[1, 2, 3].map(x => {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```

- [7.5](#7.5) 避免箭头函数(`=>`)和比较操作符（`<=, >=`）混淆. eslint: [`no-confusing-arrow`](http://eslint.org/docs/rules/no-confusing-arrow)

```js
// bad
const itemHeight = (item) => item.height <= 256 ? item.largeSize : item.smallSize;

// bad
const itemHeight = (item) => item.height >= 256 ? item.largeSize : item.smallSize;

// good
const itemHeight = (item) => (item.height <= 256 ? item.largeSize : item.smallSize);

// good
const itemHeight = (item) => {
  const { height, largeSize, smallSize } = item;
  return height <= 256 ? largeSize : smallSize;
};
```


## 8. Class & Constructors :id=class

- [8.1](#8.1) 常用`class`，避免直接操作`prototype`

> Why? `class`语法更简洁更易理解

```javascript
// bad
function Queue(contents = []) {
  this.queue = [...contents];
}
Queue.prototype.pop = function () {
  const value = this.queue[0];
  this.queue.splice(0, 1);
  return value;
};


// good
class Queue {
  constructor(contents = []) {
    this.queue = [...contents];
  }
  pop() {
    const value = this.queue[0];
    this.queue.splice(0, 1);
    return value;
  }
}
```

- [8.2](#8.2) 用`extends`实现继承

> Why? 它是一种内置的方法来继承原型功能而不打破`instanceof`

```javascript
// bad
const inherits = require('inherits');
function PeekableQueue(contents) {
  Queue.apply(this, contents);
}
inherits(PeekableQueue, Queue);
PeekableQueue.prototype.peek = function () {
  return this.queue[0];
}

// good
class PeekableQueue extends Queue {
  peek() {
    return this.queue[0];
  }
}
```

- [8.3](#8.3) 方法可以返回`this`来实现方法链

```javascript
// bad
Jedi.prototype.jump = function () {
  this.jumping = true;
  return true;
};

Jedi.prototype.setHeight = function (height) {
  this.height = height;
};

const luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20); // => undefined

// good
class Jedi {
  jump() {
    this.jumping = true;
    return this;
  }

  setHeight(height) {
    this.height = height;
    return this;
  }
}

const luke = new Jedi();

luke.jump().setHeight(20);
```

- [8.4](#8.4) 写一个定制的toString()方法是可以的，只要保证它是可以正常工作且没有副作用的

```javascript
class Jedi {
  constructor(options = {}) {
    this.name = options.name || 'no name';
  }

  getName() {
    return this.name;
  }

  toString() {
    return `Jedi - ${this.getName()}`;
  }
}
```

- [8.5](#8.5) 如果没有具体说明，类有默认的构造方法。一个空的构造函数或只是代表父类的构造函数是不需要写的。 eslint: [`no-useless-constructor`](http://eslint.org/docs/rules/no-useless-constructor)

```javascript
// bad
class Jedi {
  constructor() {}

  getName() {
    return this.name;
  }
}

// bad
class Rey extends Jedi {
  // 这种构造函数是不需要写的
  constructor(...args) {
    super(...args);
  }
}

// good
class Rey extends Jedi {
  constructor(...args) {
    super(...args);
    this.name = 'Rey';
  }
}
```

- [8.6](#8.6) 除非外部库或框架需要使用特定的非静态方法，否则类方法应该使用`this`或被做成静态方法。
  作为一个实例方法应该表明它根据接收者的属性有不同的行为。eslint: [`class-methods-use-this`](https://eslint.org/docs/rules/class-methods-use-this)

```javascript
// bad
class Foo {
  bar() {
    console.log('bar');
  }
}

// good - this 被使用了
class Foo {
  bar() {
    console.log(this.bar);
  }
}

// good - constructor 不一定要使用this
class Foo {
  constructor() {
    // ...
  }
}

// good - 静态方法不需要使用 this
class Foo {
  static bar() {
    console.log('bar');
  }
}
```

## 9. 模块 :id=module

- [9.1](#9.1) 用(`import`/`export`) 模块而不是无标准的模块系统。

> Why?
> require/exports 出生在野生规范当中，什么叫做野生规范？即这些规范是 JavaScript 社区中的开发者自己草拟的规则，得到了大家的承认或者广泛的应用。比如 CommonJS、AMD、CMD 等等。import/export 则是名门正派。TC39 制定的新的 ECMAScript 版本，即 ES6（ES2015）中包含进来。

```javascript
// bad
const AirbnbStyleGuide = require('./AirbnbStyleGuide');
module.exports = AirbnbStyleGuide.es6;

// ok
import AirbnbStyleGuide from './AirbnbStyleGuide';
export default AirbnbStyleGuide.es6;

// best
import { es6 } from './AirbnbStyleGuide';
export default es6;
```

- [9.2](#9.2) 不要用import通配符， 就是 `*` 这种方式

> Why? 这确保你有单个默认的导出

```javascript
// bad
import * as AirbnbStyleGuide from './AirbnbStyleGuide';

// good
import AirbnbStyleGuide from './AirbnbStyleGuide';
```

- [9.3](#9.3) 不要直接从import中直接export

> Why? 虽然一行是简洁的，但有一个明确的入口和一个明确的出口更好读。

```javascript
// bad
// filename es6.js
export { es6 as default } from './AirbnbStyleGuide';

// good
// filename es6.js
import { es6 } from './AirbnbStyleGuide';
export default es6;
```

- [9.4](#9.4) 一个路径只 import 一次。 eslint: [`no-duplicate-imports`](http://eslint.org/docs/rules/no-duplicate-imports)
> Why? 从同一个路径下import多行会使代码难以维护

```javascript
// bad
import foo from 'foo';
// … some other imports … //
import { named1, named2 } from 'foo';

// good
import foo, { named1, named2 } from 'foo';

// good
import foo, {
  named1,
  named2,
} from 'foo';
```

- [9.5](#9.5) 不要导出可变的东西
  eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)
> Why? 变化通常都是需要避免，特别是当你要输出可变的绑定。虽然在某些场景下可能需要这种技术，但总的来说应该导出常量。

```javascript
// bad
let foo = 3;
export { foo }

// good
const foo = 3;
export { foo }
```

- [9.6](#9.6) 在一个单一导出模块里，用 `export default` 更好。 eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)

> Why? 鼓励使用更多文件，每个文件只做一件事情并导出，这样可读性和可维护性更好。

```javascript
// bad
export function foo() {}

// good
export default function foo() {}
```

- [9.7](#9.7) `import` 放在其他所有语句之前。eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)
> Why? 让`import`放在最前面防止意外行为。

```javascript
// bad
import foo from 'foo';
foo.init();

import bar from 'bar';

// good
import foo from 'foo';
import bar from 'bar';

foo.init();
```

- [9.8](#9.8) 多行import应该缩进，就像多行数组和对象字面量

> Why?  花括号与样式指南中每个其他花括号块遵循相同的缩进规则，逗号也是。

```javascript
// bad
import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

// good
import {
  longNameA,
  longNameB,
  longNameC,
  longNameD,
  longNameE,
} from 'path';
```

- [9.9](#9.9) 不要包含JavaScript的文件扩展名

```javascript
// bad
import foo from './foo.js';
import bar from './bar.jsx';
import baz from './baz/index.jsx';

// good
import foo from './foo';
import bar from './bar';
import baz from './baz';
```

## 10. 属性和变量 :id=properties

- [10.1](#10.1) 访问属性时使用点符号. eslint: [`dot-notation`](http://eslint.org/docs/rules/dot-notation.html)

```javascript
const luke = {
  jedi: true,
  age: 28,
};

// bad
const isJedi = luke['jedi'];

// good
const isJedi = luke.jedi;
```

- [10.2](#10.2) 当获取的属性是变量时用方括号`[]`取

```javascript
const luke = {
  jedi: true,
  age: 28,
};

function getProp(prop) {
  return luke[prop];
}

const isJedi = getProp('jedi');
```

- [10.3](#10.3)  用`const`或`let`声明变量。不这样做会导致全局变量。我们要避免污染全局命名空间。 eslint: [`no-undef`](http://eslint.org/docs/rules/no-undef) [`prefer-const`](http://eslint.org/docs/rules/prefer-const)

```javascript
// bad
superPower = new SuperPower();

// good
const superPower = new SuperPower();
```

- [10.4](#10.4) 每个变量都用一个 `const` 或 `let `。 eslint: [`one-var`](http://eslint.org/docs/rules/one-var.html)

> Why? 这种方式很容易去声明新的变量，你不用去考虑把`;`调换成`,`，或者引入一个只有标点的不同的变化。

```javascript
// bad
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';

// bad
// (compare to above, and try to spot the mistake)
const items = getItems(),
    goSportsTeam = true;
    dragonball = 'z';

// good
const items = getItems();
const goSportsTeam = true;
const dragonball = 'z';
```

- [10.5](#10.5) `const`放一起，`let`放一起

```javascript
// bad
let i;
const items = getItems();
let dragonball;
const goSportsTeam = true;
let len;

// good
const goSportsTeam = true;
const items = getItems();
let dragonball;
let i;
let length;
```

- [10.6](#10.6) 在你需要的地方声明变量，但是要放在合理的位置

> Why? `let` 和 `const` 都是块级作用域而不是函数级作用域

```javascript
// bad - unnecessary function call
function checkName(hasName) {
  const name = getName();

  if (hasName === 'test') {
    return false;
  }

  if (name === 'test') {
    this.setName('');
    return false;
  }

  return name;
}

// good
function checkName(hasName) {
  if (hasName === 'test') {
    return false;
  }

  // 在需要的时候分配
  const name = getName();

  if (name === 'test') {
    this.setName('');
    return false;
  }

  return name;
}
```

- [10.7](#10.7) 不要使用链接变量分配。 eslint: [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign)

> Why? 链接变量分配创建隐式全局变量。

```javascript
// bad
(function example() {
  // JavaScript 将这一段解释为
  // let a = ( b = ( c = 1 ) );
  // let 只对变量 a 起作用; 变量 b 和 c 都变成了全局变量
  let a = b = c = 1;
}());

console.log(a); // undefined
console.log(b); // 1
console.log(c); // 1

// good
(function example() {
  let a = 1;
  let b = a;
  let c = a;
}());

console.log(a); // undefined
console.log(b); // undefined
console.log(c); // undefined

// `const` 也是如此
```

- [10.8](#10.8) 不要使用一元自增自减运算符（`++`， `--`）. eslint [`no-plusplus`](http://eslint.org/docs/rules/no-plusplus)

> Why? 根据eslint文档，由于一元`++`和`--`运算符会自动插入分号，因此空格之间的差异可能会更改源代码的语义。

```javascript
  // bad
  const array = [1, 2, 3];
  let num = 1;
  num++;
  --num;

  let sum = 0;
  let truthyCount = 0;
  for (let i = 0; i < array.length; i++) {
    let value = array[i];
    sum += value;
    if (value) {
      truthyCount++;
    }
  }

  // good

  const array = [1, 2, 3];
  let num = 1;
  num += 1;
  num -= 1;

  const sum = array.reduce((a, b) => a + b, 0);
  const truthyCount = array.filter(Boolean).length;
```

- [10.9](#10.9) 在赋值的时候避免在 `=` 前/后换行。 如果你的赋值语句超出 [`max-len`](https://eslint.org/docs/rules/max-len.html)， 那就用小括号把这个值包起来再换行。 eslint [`operator-linebreak`](https://eslint.org/docs/rules/operator-linebreak.html).

> Why? 在 `=` 附近换行容易混淆这个赋值语句。

```javascript
// bad
const foo =
  superLongLongLongLongLongLongLongLongFunctionName();

// bad
const foo
  = 'superLongLongLongLongLongLongLongLongString';

// good
const foo = (
  superLongLongLongLongLongLongLongLongFunctionName()
);

// good
const foo = 'superLongLongLongLongLongLongLongLongString';
```

- [10.10](#10.10) 不允许有未使用的变量。 eslint: [`no-unused-vars`](https://eslint.org/docs/rules/no-unused-vars)

> Why? 一个声明了但未使用的变量更像是由于重构未完成产生的错误。这种在代码中出现的变量会使阅读者迷惑。

```javascript
// bad

var some_unused_var = 42;

// 写了没用
var y = 10;
y = 5;

// 变量改了自己的值，也没有用这个变量
var z = 0;
z = z + 1;

// 参数定义了但未使用
function getX(x, y) {
    return x;
}

// 'type' 即使没有使用也可以可以被忽略， 因为这个有一个 rest 取值的属性。
// 这是从对象中抽取一个忽略特殊字段的对象的一种形式
var { type, ...coords } = data;
// 'coords' 现在就是一个没有 'type' 属性的 'data' 对象
```

## 11. 比较运算符 :id=comparison

- [11.1](#11.1) 用 `===` 和 `!==` 而不是 `==` 和 `!=`. eslint: [`eqeqeq`](http://eslint.org/docs/rules/eqeqeq.html)
> 以此避免一些隐式类型转换的坑


- [11.2](#11.2) 条件语句如'if'语句使用强制`ToBoolean'抽象方法来评估它们的表达式，并且始终遵循以下简单规则：

+ **Objects**   计算成 **true**
+ **Undefined** 计算成 **false**
+ **Null**      计算成 **false**
+ **Booleans**  计算成 **the value of the boolean**
+ **Numbers**
    + **+0, -0, or NaN** 计算成 **false**
    + 其他 **true**
+ **Strings**
    + `''` 计算成 **false**
    + 其他 **true**

```javascript
if ([0] && []) {
  // true
  // 数组（即使是空数组）是对象，对象会计算成true
}
```

- [11.3](#[11.3) 布尔值用缩写，而字符串和数字要明确比较对象

```javascript
// bad
if (isValid === true) {
  // ...
}

// good
if (isValid) {
  // ...
}

// bad
if (name) {
  // ...
}

// good
if (name !== '') {
  // ...
}

// bad
if (collection.length) {
  // ...
}

// good
if (collection.length > 0) {
  // ...
}
```

- [11.4](#11.4) 在`case`和`default`分句里用大括号创建一块包含语法声明的区域(e.g. `let`, `const`, `function`, and `class`). eslint rules: [`no-case-declarations`](http://eslint.org/docs/rules/no-case-declarations.html).

> Why? 语法声明在整个`switch`的代码块里都可见，但是只有当其被分配后才会初始化，他的初始化时当这个`case`被执行时才产生。 当多个`case`分句试图定义同一个事情时就出问题了

```javascript
// bad
switch (foo) {
  case 1:
    let x = 1;
    break;
  case 2:
    const y = 2;
    break;
  case 3:
    function f() {
      // ...
    }
    break;
  default:
    class C {}
}

// good
switch (foo) {
  case 1: {
    let x = 1;
    break;
  }
  case 2: {
    const y = 2;
    break;
  }
  case 3: {
    function f() {
      // ...
    }
    break;
  }
  case 4:
    bar();
    break;
  default: {
    class C {}
  }
}
```

- [11.5](#11.5) 三元表达式不应该嵌套，通常是单行表达式。 eslint rules: [`no-nested-ternary`](http://eslint.org/docs/rules/no-nested-ternary.html).

```javascript
// bad
const foo = maybe1 > maybe2
  ? "bar"
  : value1 > value2 ? "baz" : null;

// best
const maybeNull = value1 > value2 ? 'baz' : null;

const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
```


- [11.6](#11.6) 避免不需要的三元表达式

eslint rules: [`no-unneeded-ternary`](http://eslint.org/docs/rules/no-unneeded-ternary.html).

```javascript
// bad
const foo = a ? a : b;
const bar = c ? true : false;
const baz = c ? false : true;

// good
const foo = a || b;
const bar = !!c;
const baz = !c;
```

- [11.7](#11.7) 用圆括号来混合这些操作符。 只有当标准的算术运算符(`+`, `-`, `*`, & `/`)， 并且它们的优先级显而易见时，可以不用圆括号括起来。 eslint: [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators.html)

> Why? 这提高了可读性，并且明确了开发者的意图

```javascript
// bad
const foo = a && b < 0 || c > 0 || d + 1 === 0;

// bad
const bar = a ** b - 5 % d;

// bad
// 别人会陷入(a || b) && c 的迷惑中
if (a || b && c) {
  return d;
}

// good
const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

// good
const bar = (a ** b) - (5 % d);

// good
if (a || (b && c)) {
  return d;
}

// good
const bar = a + b / c * d;
```

## 12. 代码块 :id=blocks

- [12.1](#12.1) 用大括号包裹多行代码块。  eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

```javascript
// bad
if (test)
  return false;

// good
if (test) return false;

// good
if (test) {
  return false;
}

// bad
function foo() { return false; }

// good
function bar() {
  return false;
}
```

- [12.2](#12.2) `if`表达式的`else`和`if`的关闭大括号在一行。 eslint: [`brace-style`](http://eslint.org/docs/rules/brace-style.html)

```javascript
// bad
if (test) {
  thing1();
  thing2();
}
else {
  thing3();
}

// good
if (test) {
  thing1();
  thing2();
} else {
  thing3();
}
```

- [12.3](#12.3) 如果 `if` 语句中总是需要用 `return` 返回， 那后续的 `else` 就不需要写了。 `if` 块中包含 `return`， 它后面的 `else if` 块中也包含了 `return`， 这个时候就可以把 `return` 分到多个 `if` 语句块中。 eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

```javascript
// bad
function foo() {
  if (x) {
    return x;
  } else {
    return y;
  }
}

// bad
function cats() {
  if (x) {
    return x;
  } else if (y) {
    return y;
  }
}

// bad
function dogs() {
  if (x) {
    return x;
  } else {
    if (y) {
      return y;
    }
  }
}

// good
function foo() {
  if (x) {
    return x;
  }

  return y;
}

// good
function cats() {
  if (x) {
    return x;
  }

  if (y) {
    return y;
  }
}

// good
function dogs(x) {
  if (x) {
    if (z) {
      return y;
    }
  } else {
    return z;
  }
}
```

- [12.4](#12.4) 当你的控制语句(`if`, `while` 等)太长或者超过最大长度限制的时候， 把每一个(组)判断条件放在单独一行里。 逻辑操作符放在行首。

> Why? 把逻辑操作符放在行首是让操作符的对齐方式和链式函数保持一致。这提高了可读性，也让复杂逻辑更容易看清楚。

```javascript
// bad
if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
  thing1();
}

// bad
if (foo === 123 &&
  bar === 'abc') {
  thing1();
}

// bad
if (foo === 123
  && bar === 'abc') {
  thing1();
}

// bad
if (
  foo === 123 &&
  bar === 'abc'
) {
  thing1();
}

// good
if (
  foo === 123
  && bar === 'abc'
) {
  thing1();
}

// good
if (
  (foo === 123 || bar === 'abc')
  && doesItLookGoodWhenItBecomesThatLong()
  && isThisReallyHappening()
) {
  thing1();
}

// good
if (foo === 123 && bar === 'abc') {
  thing1();
}
```

- [12.5](#12.5) 不要用选择操作符代替控制语句。

```javascript
// bad
!isRunning && startRunning();

// good
if (!isRunning) {
  startRunning();
}
```

## 13. 注释 :id=comments

- [13.1](#13.1) 多行注释用 `/** ... */`

```javascript
// bad
// make() returns a new element
// based on the passed in tag name
//
// @param {String} tag
// @return {Element} element
function make(tag) {

  // ...

  return element;
}

// good
/**
 * make() returns a new element
 * based on the passed-in tag name
 */
function make(tag) {

  // ...

  return element;
}
```

- [13.2](#13.2) 单行注释用`//`，将单行注释放在被注释区域上面。如果注释不是在第一行，那么注释前面就空一行

```javascript
// bad
const active = true;  // is current tab

// good
// is current tab
const active = true;

// bad
function getType() {
  console.log('fetching type...');
  // set the default type to 'no type'
  const type = this._type || 'no type';

  return type;
}

// good
function getType() {
  console.log('fetching type...');

  // set the default type to 'no type'
  const type = this._type || 'no type';

  return type;
}

// also good
function getType() {
  // set the default type to 'no type'
  const type = this._type || 'no type';

  return type;
}
```

- [13.3](#13.3) 所有注释开头打一个空格，方便阅读。 eslint: [`spaced-comment`](http://eslint.org/docs/rules/spaced-comment)

```javascript
// bad
//is current tab
const active = true;

// good
// is current tab
const active = true;

// bad
/**
 *make() returns a new element
 *based on the passed-in tag name
 */
function make(tag) {

  // ...

  return element;
}

// good
/**
 * make() returns a new element
 * based on the passed-in tag name
 */
function make(tag) {

  // ...

  return element;
}
```

- [13.4](#13.4) 在你的注释前使用`FIXME'或`TODO'前缀， 这有助于其他开发人员快速理解你指出的需要重新访问的问题， 或者您建议需要实现的问题的解决方案。 这些不同于常规注释，因为它们是可操作的。
>`FIXME： - 需要解决 ` 或 `TODO： - 需要实现`。


- [13.5](#13.5) 用`// FIXME:`给问题做注释

```javascript
class Calculator extends Abacus {
  constructor() {
    super();

    // FIXME: shouldn't use a global here
    total = 0;
  }
}
```


- [13.6](#13.6) 用`// TODO:`去注释问题的解决方案

```javascript
class Calculator extends Abacus {
  constructor() {
    super();

    // TODO: total should be configurable by an options param
    this.total = 0;
  }
}
```

- [13.7](#13.7) 函数注释的通用事例：

```JavaScript
/**
  * @desc 说明该方法主要作用
  * @param {Number} [age]    - 参数说明
  * @param {String} [name] - 参数说明
  */
```

## 14. 空格 :id=whitespaces

- [14.1](#14.1) tab用四个空格作为缩进

> 习惯而已，这本来就是个萝卜白菜的问题。（而且个人觉得四个空格看起来没有两个那么小气）

```javascript
 // bad
function baz() {
∙∙const name;
}

// good
function foo() {
∙∙∙∙const name;
}
```

- [14.2](#14.2) 在大括号前空一格。 eslint: [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks.html)

```javascript
// bad
function test(){
  console.log('test');
}

// good
function test() {
  console.log('test');
}

// bad
dog.set('attr',{
  age: '1 year',
  breed: 'Bernese Mountain Dog',
});

// good
dog.set('attr', {
  age: '1 year',
  breed: 'Bernese Mountain Dog',
});
```

- [14.3](#14.3) 在控制语句(`if`, `while` 等)的圆括号前空一格。在函数调用和定义时，参数列表和函数名之间不空格。 eslint: [`keyword-spacing`](http://eslint.org/docs/rules/keyword-spacing.html)

```javascript
// bad
if(isJedi) {
  fight ();
}

// good
if (isJedi) {
  fight();
}

// bad
function fight () {
  console.log ('Swooosh!');
}

// good
function fight() {
  console.log('Swooosh!');
}
```

- [14.4](#14.4) 用空格来隔开运算符。 eslint: [`space-infix-ops`](http://eslint.org/docs/rules/space-infix-ops.html)

```javascript
// bad
const x=y+5;

// good
const x = y + 5;
```

- [14.5](#14.5) 文件结尾空一行. eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

```javascript
// bad
import { es6 } from './AirbnbStyleGuide';
  // ...
export default es6;
```

```javascript
// bad
import { es6 } from './AirbnbStyleGuide';
  // ...
export default es6;↵
↵
```

```javascript
// good
import { es6 } from './AirbnbStyleGuide';
  // ...
export default es6;↵
```


- [14.6](#14.6) 当出现长的方法链（>2个）时用缩进。用点开头强调该行是一个方法调用，而不是一个新的语句。eslint: [`newline-per-chained-call`](http://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](http://eslint.org/docs/rules/no-whitespace-before-property)

```javascript
// bad
$('#items').find('.selected').highlight().end().find('.open').updateCount();

// bad
$('#items').
  find('.selected').
    highlight().
    end().
  find('.open').
    updateCount();

// good
$('#items')
  .find('.selected')
    .highlight()
    .end()
  .find('.open')
    .updateCount();

// bad
const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
    .attr('width', (radius + margin) * 2).append('svg:g')
    .attr('transform', `translate(${radius + margin},${radius + margin})`)
    .call(tron.led);

// good
const leds = stage.selectAll('.led')
    .data(data)
  .enter().append('svg:svg')
    .classed('led', true)
    .attr('width', (radius + margin) * 2)
  .append('svg:g')
    .attr('transform', `translate(${radius + margin},${radius + margin})`)
    .call(tron.led);

// good
const leds = stage.selectAll('.led').data(data);
```

- [14.7](#14.7) 在一个代码块后下一条语句前空一行。

```javascript
// bad
if (foo) {
  return bar;
}
return baz;

// good
if (foo) {
  return bar;
}

return baz;

// bad
const obj = {
  foo() {
  },
  bar() {
  },
};
return obj;

// good
const obj = {
  foo() {
  },

  bar() {
  },
};

return obj;

// bad
const arr = [
  function foo() {
  },
  function bar() {
  },
];
return arr;

// good
const arr = [
  function foo() {
  },

  function bar() {
  },
];

return arr;
```

- [14.8](#14.8) 不要用空白行来填充块。 eslint: [`padded-blocks`](http://eslint.org/docs/rules/padded-blocks.html)

```javascript
// bad
function bar() {

  console.log(foo);

}

// also bad
if (baz) {

  console.log(qux);
} else {
  console.log(foo);

}

// good
function bar() {
  console.log(foo);
}

// good
if (baz) {
  console.log(qux);
} else {
  console.log(foo);
}
```

- [14.9](#14.9)不要在代码之间使用多个空白行填充。 eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

```javascript
// bad
class Person {
  constructor(fullName, email, birthday) {
    this.fullName = fullName;


    this.email = email;


    this.setAge(birthday);
  }


  setAge(birthday) {
    const today = new Date();


    const age = this.getAge(today, birthday);


    this.age = age;
  }


  getAge(today, birthday) {
    // ..
  }
}

// good
class Person {
  constructor(fullName, email, birthday) {
    this.fullName = fullName;
    this.email = email;
    this.setAge(birthday);
  }

  setAge(birthday) {
    const today = new Date();
    const age = getAge(today, birthday);
    this.age = age;
  }

  getAge(today, birthday) {
    // ..
  }
}
```

- [14.10](#14.10) 圆括号里不要加空格。 eslint: [`space-in-parens`](http://eslint.org/docs/rules/space-in-parens.html)

```javascript
// bad
function bar( foo ) {
  return foo;
}

// good
function bar(foo) {
  return foo;
}

// bad
if ( foo ) {
  console.log(foo);
}

// good
if (foo) {
  console.log(foo);
}
```

- [14.11](#14.11) 方括号里不要加空格。看示例。 eslint: [`array-bracket-spacing`](http://eslint.org/docs/rules/array-bracket-spacing.html)

```javascript
// bad
const foo = [ 1, 2, 3 ];
console.log(foo[ 0 ]);

// good， 逗号分隔符还是要空格的
const foo = [1, 2, 3];
console.log(foo[0]);
```

- [14.12](#14.12) 花括号里加空格。 eslint: [`object-curly-spacing`](http://eslint.org/docs/rules/object-curly-spacing.html)

```javascript
// bad
const foo = {clark: 'kent'};

// good
const foo = { clark: 'kent' };
```

- [14.13](#14.13) 避免一行代码超过100个字符（包含空格）。
- 注意： 对于[上面——strings--line-length](#strings--line-length)，长字符串不受此规则限制，不应分解。 eslint: [`max-len`](http://eslint.org/docs/rules/max-len.html)

> Why? 这样确保可读性和可维护性

```javascript
// bad
const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

// bad
$.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

// good
const foo = jsonData
  && jsonData.foo
  && jsonData.foo.bar
  && jsonData.foo.bar.baz
  && jsonData.foo.bar.baz.quux
  && jsonData.foo.bar.baz.quux.xyzzy;

// good
$.ajax({
  method: 'POST',
  url: 'https://airbnb.com/',
  data: { name: 'John' },
})
  .done(() => console.log('Congratulations!'))
  .fail(() => console.log('You have failed this city.'));
```

- [14.14](#14.14) 作为语句的花括号内也要加空格 —— `{` 后和 `}` 前都需要空格。 eslint: [`block-spacing`](https://eslint.org/docs/rules/block-spacing)

```javascript
// bad
function foo() {return true;}
if (foo) { bar = 0;}

// good
function foo() { return true; }
if (foo) { bar = 0; }
```

- [14.15](#14.15) `,` 前不要空格， `,` 后需要空格。 eslint: [`comma-spacing`](https://eslint.org/docs/rules/comma-spacing)

```javascript
// bad
var foo = 1,bar = 2;
var arr = [1 , 2];

// good
var foo = 1, bar = 2;
var arr = [1, 2];
```

- [14.16](#14.16) 计算属性内要空格。参考上述花括号和中括号的规则。  eslint: [`computed-property-spacing`](https://eslint.org/docs/rules/computed-property-spacing)

```javascript
// bad
obj[foo ]
obj[ 'foo']
var x = {[ b ]: a}
obj[foo[ bar ]]

// good
obj[foo]
obj['foo']
var x = { [b]: a }
obj[foo[bar]]
```

- [14.17](#14.17) 调用函数时，函数名和小括号之间不要空格。 eslint: [`func-call-spacing`](https://eslint.org/docs/rules/func-call-spacing)

```javascript
// bad
func ();

func
();

// good
func();
```

- [14.18](#14.18) 在对象的字面量属性中， `key` `value` 之间要有空格。 eslint: [`key-spacing`](https://eslint.org/docs/rules/key-spacing)

```javascript
// bad
var obj = { "foo" : 42 };
var obj2 = { "foo":42 };

// good
var obj = { "foo": 42 };
```

- [14.19](#14.19) 行末不要空格。 eslint: [`no-trailing-spaces`](https://eslint.org/docs/rules/no-trailing-spaces)

- [14.20](#14.20) 避免出现多个空行。 在文件末尾只允许空一行。 eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

```javascript
// bad
var x = 1;



var y = 2;

// good
var x = 1;

var y = 2;
```

## 15. 逗号 :id=commas

- [15.1](#15.1) 不要前置逗号。 eslint: [`comma-style`](http://eslint.org/docs/rules/comma-style.html)

```javascript
// bad
const story = [
    once
  , upon
  , aTime
];

// good
const story = [
  once,
  upon,
  aTime,
];
```

- [15.2](#15.2) 额外结尾逗号: **要** eslint: [`comma-dangle`](http://eslint.org/docs/rules/comma-dangle.html)

> Why? 这导致git diffs更清洁。 此外，像Babel这样的转换器会删除转换代码中的额外的逗号，这意味着你不必担心旧版浏览器中的[结尾逗号问题](https://github.com/airbnb/javascript/blob/es5-deprecated/es5/README.md#commas)。

```difff
// bad - 没有结尾逗号的 git diff
const hero = {
     firstName: 'Florence',
-    lastName: 'Nightingale'
+    lastName: 'Nightingale',
+    inventorOf: ['coxcomb chart', 'modern nursing']
};

// good - 有结尾逗号的 git diff
const hero = {
     firstName: 'Florence',
     lastName: 'Nightingale',
+    inventorOf: ['coxcomb chart', 'modern nursing'],
};
```

```javascript
// bad
const hero = {
  firstName: 'Dana',
  lastName: 'Scully'
};

const heroes = [
  'Batman',
  'Superman'
];

// good
const hero = {
  firstName: 'Dana',
  lastName: 'Scully',
};

const heroes = [
  'Batman',
  'Superman',
];

// bad
function createHero(
  firstName,
  lastName,
  inventorOf
) {
  // does nothing
}

// good
function createHero(
  firstName,
  lastName,
  inventorOf,
) {
  // does nothing
}

// good (note that a comma must not appear after a "rest" element)
function createHero(
  firstName,
  lastName,
  inventorOf,
  ...heroArgs
) {
  // does nothing
}

// bad
createHero(
  firstName,
  lastName,
  inventorOf
);

// good
createHero(
  firstName,
  lastName,
  inventorOf,
);

// good (note that a comma must not appear after a "rest" element)
createHero(
  firstName,
  lastName,
  inventorOf,
  ...heroArgs
)
```

## 16. 分号 :id=semicolons

- [16.1](#21.1) **关于是否加分号** eslint: [`semi`](http://eslint.org/docs/rules/semi.html)

总的来说，加不加分号都有自己的理由，只要在团队中保持规则一致即可。

**加分号：**

> 当 JavaScript 遇到没有分号结尾的一行，它会执行[自动插入分号 `Automatic Semicolon Insertion`](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion)
> 而如果JavaScript在你的断行里错误的插入了分号，就会出现一些古怪的行为。而JavaScript后续出了很多新功能，你需要记住这些复杂的规则以避免意外出现。

所以，显式地结束语句，并通过配置代码检查去捕获没有带分号的地方可以帮助你防止这种错误。

**不加分号：**

如果你觉得自己可以记住“哪些地方必需加分号”，或者“如何避免断行插入分号”，这类规则的话，你完全可以在日常写js的过程中不加分号。


[Read more](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214%237365214).

## 17. 类型转换 :id=typeCasting

- [17.1](#17.1) 在语句开始执行强制类型转换。

- [17.2](#17.2)  Strings: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

```javascript
// => this.reviewScore = 9;

// bad
const totalScore = new String(this.reviewScore); // typeof totalScore is "object" not "string"

// bad
const totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

// bad
const totalScore = this.reviewScore.toString(); // 不保证返回string

// good
const totalScore = String(this.reviewScore);
```

- [17.3](#17.3) Numbers: 用 `Number` 做类型转换，`parseInt`转换string常需要带上基数。 eslint: [`radix`](http://eslint.org/docs/rules/radix)

```javascript
const inputValue = '4';

// bad
const val = new Number(inputValue);

// bad
const val = +inputValue;

// bad
const val = inputValue >> 0;

// bad
const val = parseInt(inputValue);

// good
const val = Number(inputValue);

// good
const val = parseInt(inputValue, 10);
```

- [17.4](#17.4) 请在注释中解释为什么要用移位运算和你在做什么。无论你做什么狂野的事，比如由于 `parseInt` 是你的性能瓶颈导致你一定要用移位运算。 请说明这个是因为[性能原因](https://jsperf.com/coercion-vs-casting/3),

```javascript
// good
/**
 * parseInt是代码运行慢的原因
 * 用Bitshifting将字符串转成数字使代码运行效率大幅增长
 */
const val = inputValue >> 0;
```

- [17.5](#17.5) 布尔:

```javascript
const age = 0;

// bad
const hasAge = new Boolean(age);

// good
const hasAge = Boolean(age);

// best
const hasAge = !!age;
```


## 18. 命名规范 :id=namingConventions

- [18.1](#18.1) 避免用一个字母命名，让你的命名可描述。 eslint: [`id-length`](http://eslint.org/docs/rules/id-length)

```javascript
// bad
function q() {
  // ...
}

// good
function query() {
  // ...
}
```

- [18.2](#18.2) 用小驼峰式命名你的对象、函数、实例。 eslint: [`camelcase`](http://eslint.org/docs/rules/camelcase.html)

```javascript
// bad
const OBJEcttsssss = {};
const this_is_my_object = {};
function c() {}

// good
const thisIsMyObject = {};
function thisIsMyFunction() {}
```

- [18.3](#18.3) 用大驼峰式命名类。 eslint: [`new-cap`](http://eslint.org/docs/rules/new-cap.html)

```javascript
// bad
function user(options) {
  this.name = options.name;
}

const bad = new user({
  name: 'nope',
});

// good
class User {
  constructor(options) {
    this.name = options.name;
  }
}

const good = new User({
  name: 'yup',
});
```

- [18.4](#18.4) 不要用前置或后置下划线。 eslint: [`no-underscore-dangle`](http://eslint.org/docs/rules/no-underscore-dangle.html)

> Why? JavaScript 没有私有属性或私有方法的概念。尽管前置下划线通常的概念上意味着“private”，事实上，这些属性是完全公有的，因此这部分也是你的API的内容。这一概念可能会导致开发者误以为更改这个不会导致崩溃或者不需要测试。

```javascript
// bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';
this._firstName = 'Panda';

// good
this.firstName = 'Panda';
```

- [18.5](#18.5) 不要保存引用`this`， 用箭头函数或[函数绑定——Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

```javascript
// bad
function foo() {
  const self = this;
  return function () {
    console.log(self);
  };
}

// bad
function foo() {
  const that = this;
  return function () {
    console.log(that);
  };
}

// good
function foo() {
  return () => {
    console.log(this);
  };
}
```

- [18.6](#18.6) export default导出模块A，则这个文件名也叫A.*， import 时候的参数也叫A。 大小写需完全一致。

```javascript
// file 1 contents
class CheckBox {
  // ...
}
export default CheckBox;

// file 2 contents
export default function fortyTwo() { return 42; }

// file 3 contents
export default function insideDirectory() {}

// in some other file
// bad
import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

// bad
import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
import forty_two from './forty_two'; // snake_case import/filename, camelCase export
import inside_directory from './inside_directory'; // snake_case import, camelCase export
import index from './inside_directory/index'; // requiring the index file explicitly
import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

// good
import CheckBox from './CheckBox'; // PascalCase export/import/filename
import fortyTwo from './fortyTwo'; // camelCase export/import/filename
import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
// ^ supports both insideDirectory.js and insideDirectory/index.js
```

- [18.7](#18.7) 当你export-default一个函数时，函数名用小驼峰，文件名需要和函数名一致。

```javascript
function makeStyleGuide() {
  // ...
}

export default makeStyleGuide;
```


- [18.8](#18.8) 当你export一个结构体/类/单例/函数库/对象 时用大驼峰。

```javascript
const AirbnbStyleGuide = {
  es6: {
  }
};

export default AirbnbStyleGuide;
```

- [18.9](#18.9) 简称和缩写应该全部大写或全部小写。

> Why? 名字都是给人读的，不是为了适应电脑的算法的。

```javascript
// bad
import SmsContainer from './containers/SmsContainer';

// bad
const HttpRequests = [
  // ...
];

// good
import SMSContainer from './containers/SMSContainer';

// good
const HTTPRequests = [
  // ...
];

// also good
const httpRequests = [
  // ...
];

// best
import TextMessageContainer from './containers/TextMessageContainer';

// best
const requests = [
  // ...
];
```

- [18.10](#18.10) 你可以用全大写字母设置静态变量，他需要满足三个条件。
1. 导出变量
1. 是 `const` 定义的， 保证不能被改变
1. 这个变量是可信的，他的子属性都是不能被改变的

```javascript
// bad
const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file';

// good 语义化较好
export const API_KEY = 'SOMEKEY';

// bad - 不必要的大写键，没有增加任何语义
export const MAPPING = {
  KEY: 'value'
};

// good
export const MAPPING = {
  key: 'value'
};
```

## 19. jQuery :id=jquery

- [19.1](#19.1) jQuery对象用`$`变量表示。

```javascript
// bad
const sidebar = $('.sidebar');

// good
const $sidebar = $('.sidebar');

// good
const $sidebarBtn = $('.sidebar-btn');
```

- [19.2](#19.2) 暂存jQuery查找

```javascript
// bad
function setSidebar() {
  $('.sidebar').hide();

  // ...

  $('.sidebar').css({
    'background-color': 'pink'
  });
}

// good
function setSidebar() {
  const $sidebar = $('.sidebar');
  $sidebar.hide();

  // ...

  $sidebar.css({
    'background-color': 'pink'
  });
}
```