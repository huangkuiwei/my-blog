---
title: ES6中的export、import、export default详解
date: 2019-04-08 22:46:24
author: huangkuiwei
img: http://5b0988e595225.cdn.sohucs.com/images/20170910/4b1328ea36b14afa8a30a69f32eb9a44.jpeg
tags: 
    - ES6
    - JavaScript
---
## 一、前言
在ES5中，如果你的js文件是依赖于其他js文件（例如：jquery、bootsrap.js等），那么你必须在html中先加载这些依赖，也就是要控制好每个js的加载顺序。想想为什么不能像java和Python中的import方式来解决呢？

其实在ES6中就引入了export与import概念，将一个大程序拆分成互相依赖的小文件，再用简单的方法拼装起来。

## 二、export
官方对export的解释是：
> 一个模块就是一个独立的文件。该文件内部的所有变量，外部无法获取。如果你希望外部能够读取模块内部的某个变量，就必须使用export关键字输出该变量。

这句话的意思其实可以这样理解，一个模块可以理解为一个类，引用这个类的文件无法获取类里面的所有变量（类似于类属性设置为private），想要让引用它的文件可以访问内部变量必须用export关键字输出（类似于类属性设置为public），总之一句话，要使用引用文件内部变量只能使用export关键字输出。
那么应该如何使用export？
**1、示例用法：**
```javascript
// 输出变量用法1
export let firstName = 'Michael';
export let lastName = 'Jackson';
export let year = 1958;

// 输出变量用法2
let firstName = 'Michael';
let lastName = 'Jackson';
let year = 1958;
export {firstName, lastName, year};

// 输出函数用法1
export function multiply(x, y) {
  return x * y;
};
// 输出函数用法2
function v1() { ... }
function v2() { ... }

export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};

// 输出类
export default class { ... }
```
**2、错误示例：**
```javascript
// 报错
export 1;
let m = 1;
export m;
//上面两种写法都会报错，因为没有提供对外的接口，两种错误其实都是因为直接输出值了
// 报错
function f() {}
export f;
//以上错误跟上面的相同，必须提供的是接口
//报错
function foo() {
  export default 'bar' // SyntaxError
}
foo()
//这种错误是因为export命令必须处于模块顶层才可以，也就是不能出现在代码块里面
```

## 三、import
使用export命令定义了模块的对外接口以后，其他 JS 文件就可以通过import命令加载这个模块。
**1、用法示例**
```javascript
// main.js
import {firstName, lastName, year} from './profile';

function setName(element) {
  element.textContent = firstName + ' ' + lastName;
}
```
> 1. 上面代码的import命令，用于加载profile.js文件，并从中输入变量。import命令接受一对大括号，里面指定要从其他模块导入的变量名。大括号里面的变量名，必须与被导入模块（profile.js）对外接口的名称相同。
> 2. 如果想为输入的变量重新取一个名字，import命令要使用as关键字，将输入的变量重命名。

```javascript
import { lastName as surname } from './profile';
```

**2、提升效果**
import命令具有提升效果，会提升到整个模块的头部，首先执行。
```javascript
foo();

import { foo } from 'my_module';
```
上面的代码不会报错，因为import的执行早于foo的调用。这种行为的本质是，import命令是编译阶段执行的，在代码运行之前。

**3、不能使用表达式和变量**
由于import是静态执行，所以不能使用表达式和变量，这些只有在运行时才能得到结果的语法结构。
```javascript
/ 报错
import { 'f' + 'oo' } from 'my_module';

// 报错
let module = 'my_module';
import { foo } from module;

// 报错
if (x === 1) {
  import { foo } from 'module1';
} else {
  import { foo } from 'module2';
}
```
上面三种写法都会报错，因为它们用到了表达式、变量和if结构。在静态分析阶段，这些语法都是没法得到值的。

**4、不重复加载**
```javascript
import { foo } from 'my_module';
import { bar } from 'my_module';

// 等同于
import { foo, bar } from 'my_module';
```
上面代码中，虽然foo和bar在两个语句中加载，但是它们对应的是同一个my_module实例。也就是说，import语句是 Singleton 模式。

**5、同export一样的import不能出现在代码块**
由于import是编译时加载

## 四、export default
从前面的例子可以看出，使用import命令的时候，用户需要知道所要加载的变量名或函数名，否则无法加载。但是，用户肯定希望快速上手，未必愿意阅读文档，去了解模块有哪些属性和方法。

为了给用户提供方便，让他们不用阅读文档就能加载模块，就要用到export default命令，为模块指定默认输出。
```javascript
// export-default.js
export default function () {
  console.log('foo');
}
```
上面代码是一个模块文件export-default.js，它的默认输出是一个函数。
其他模块加载该模块时，import命令可以为该匿名函数指定任意名字。
```javascript
// import-default.js
import customName from './export-default';
customName(); // 'foo'
```
上面代码的import命令，可以用任意名称指向export-default.js输出的方法，这时就不需要知道原模块输出的函数名。需要注意的是，这时import命令后面，不使用大括号。
export default命令用在非匿名函数前，也是可以的。
```javascript
// export-default.js
export default function foo() {
  console.log('foo');
}
// 或者写成
function foo() {
  console.log('foo');
}
export default foo;
```
上面代码中，foo函数的函数名foo，在模块外部是无效的。加载的时候，视同匿名函数加载。
下面比较一下默认输出和正常输出。
```javascript
// 第一组
export default function crc32() { // 输出
  // ...
}
import crc32 from 'crc32'; // 输入
// 第二组
export function crc32() { // 输出
  // ...
};
import {crc32} from 'crc32'; // 输入
```
上面代码的两组写法，第一组是使用export default时，对应的import语句不需要使用大括号；第二组是不使用export default时，对应的import语句需要使用大括号。

export default命令用于指定模块的默认输出。显然，一个模块只能有一个默认输出，因此export default命令只能使用一次。所以，import命令后面才不用加大括号，因为只可能对应一个方法。

本质上，export default就是输出一个叫做default的变量或方法，然后系统允许你为它取任意名字。所以，下面的写法是有效的。

```javascript
// modules.js
function add(x, y) {
  return x * y;
}
export {add as default};
// 等同于
// export default add;

// app.js
import { default as xxx } from 'modules';
// 等同于
// import xxx from 'modules';
// 正是因为export default命令其实只是输出一个叫做default的变量，所以它后面不能跟变量声明语句。

// 正确
export var a = 1;

// 正确
let a = 1;
export default a;

// 错误
export default let a = 1;
```
上面代码中，export default a的含义是将变量a的值赋给变量default。所以，最后一种写法会报错。

同样地，因为export default本质是将该命令后面的值，赋给default变量以后再默认，所以直接将一个值写在export default之后。

```javascript
// 正确
export default 42;

// 报错
export 42;
```
上面代码中，后一句报错是因为没有指定对外的接口，而前一句指定外对接口为default。

有了export default命令，输入模块时就非常直观了，以输入 lodash 模块为例。
```javascript
import _ from ‘lodash’;
```
如果想在一条import语句中，同时输入默认方法和其他变量，可以写成下面这样。
```javascript
import _, { each } from ‘lodash’;
```
对应上面代码的export语句如下。
```javascript
export default function (obj) {
  // ...
}

export function each(obj, iterator, context) {
  // ...
}

export { each as forEach };
```
上面代码的最后一行的意思是，暴露出forEach接口，默认指向each接口，即forEach和each指向同一个方法。

export default也可以用来输出类。
```javascript
// MyClass.js
export default class { ... }

// main.js
import MyClass from 'MyClass';
let o = new MyClass();
```
ES6是未来，虽然现在很多浏览器都还不支持（现在可以通过babel将es6转成es5），但是有些很好的框架已经运用es6语法了，比如Vue，所以了解es6语法还是很有必要的。比如Vue组件化开发就采用export default去构建一个组件
