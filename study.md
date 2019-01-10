# let命令，用于声明变量，但是所声明的变量旨在let命令所在的代码块内有效
# ES6module的语法：
(css中有@import）
## ES模块不是对象，而是通过export命令现实指定输出的代码，再通过import命令输入。
eg.
import {stat, exists, readFile } from 'fs';
//上面的代码实质是从fs模块加载3个方法，其他方法不加载
这在加载方式称为“编译时加载”或者“静态加载”，即ES6可以在编译时完成加载
这导致了没法引用ES6模块本身，因为他不是对象。
ps:宏（macro)和检验类型(tpye system)这些只能靠静态分析实现的功能。
## 严格模式
ES6的模块自动采用严格模式，不管你有没有在模块头部加上“use strict"。
this的限制。ES6模块之中，顶层的this指向undefined，即不应该在顶层的代码使用this。
## export命令
export命令用于规定模块的对外接口，import命令用于输入其他模块提供的功能。
一个模块就是一个独立的文件。该文件内部的所有变量，外部无法获取
如果你希望外部能够读取模块的某个变量，就必须使用export关键字输出变量。
示例：
```bash
export var firstName = 'Michael';
export var lastName = 'Jackson';
export var year = 1958;
```
### 为了保存用户信息。ES6将其视为一个模块，里面用export命令对外部输出了三个变量。
除了上述示例，还有另外一种写法
```bash
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};
```
上面代码在export命令后面，使用大括号指定所要输出的一组变量
它与前一种写法（直接放置在var语句前面）是等价的
但是优先考虑这种写法,因为这样就可以放在脚本尾部。
export命令除了输出变量，还可以输出函数或类（Class）
```bash
export function multiply(x, y) {
  return x * y;
};
```
上面代码对外输出一个函数multiply。
通常情况下，export输出变量就是本来的名字，但是可以使用as关键字重命名。
```bash
function v1() { ... }
function v2() { ... }
export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};
```
上面代码使用as关键字，重命名了函数v1和v2的对外接口。重命名后，v2可以用不同的名字输出两次。
需要特别注意: export，命令规定的是对外接口，必须与模块内部变量建立一一对应的关系。
export输出的是接口，不可以是具体值。正确的输出得在接口和模块内部变量之间建立一一的对应关系。
另外，export语句输出接口，与其对应的值是动态绑定关系，即通过该接口，可以取到模块内部实时的值。
```bash
export var foo = 'bar';
setTimeout(() => foo = 'baz', 500);
```

上面代码输出变量foo，值为bar，500毫秒之后变成baz。
ps:export，命令可以出现在模块的任何位置，只要处于模块的顶层就可以
如果处于块级作用域内，就会报错。
根据现在的理解，顶层的意思就是所有的函数之外。
## import命令
其他文件通过import命令加载这个模块
import，命令接受一对大括号，里面指定要从模块导入的变量名，必须与被导入模块对外接口的名称相同。
为输出的命令起新名字，import命令使用as关键字，将输入的变量重命名。
eg.
```bash
import { lastName as surname } from './profile.js';
```
imoprt命令输入的变量都是只读的，因为他的本质是输入接口。也就是说，不允许在加载模块的脚本里面改写接口。
```bash
import {a} from './xxx.js'

a = {}; // Syntax Error : 'a' is read-only;
```
上面代码中，脚本加载了变量a，对其重新赋值就会报错
因为a是一个只读接口,但是a是一个对象，改写a的属性是允许的。
```bash
import {a} from './xxx.js'

a.foo = 'hello'; // 合法操作
```
这种写法很难查错，建议凡是输入变量，都当作完全只读，轻易不要改变他的属性。
由于import是静态执行，所以不能使用表达式和变量，这些只有在运行时才能得到结果的语法结构。
如果多次重复执行同一句import语句，那么会执行一次，而不会执行多次。
import语句是Singleton模式。
CommonJS模块的require命令和ES6模块的import命令,可以写在同一个模块里面
但是最好不要这样做，下面的代码可能不会得到预期的结果。
```bash
require('core-js/modules/es6.symbol');
require('core-js/modules/es6.promise');
import React from 'React';
```
## 模块的整体加载
除了指定加载某个输出值，还可以使用整体加载，即用星号(*)指定一个对象
所有的输出值都加载在这个对象上面
下面的文件输出两个方法area和circumference
```bash
// circle.js

export function area(radius) {
  return Math.PI * radius * radius;
}

export function circumference(radius) {
  return 2 * Math.PI * radius;
}

```
现在加载这个模块
```bash
// main.js 注意加载法

import { area, circumference } from './circle';

console.log('圆面积：' + area(4));
console.log('圆周长：' + circumference(14));

import * as circle from './circle';//整体加载法

console.log('圆面积：' + circle.area(4));
console.log('圆周长：' + circle.circumference(14));

```
注意，模块整体加载所在的那个对象（上例是circle）
应该是可以静态的分析的，所以不允许运行时改变。
```bash
//下面两行都是错误的
circle.foo = 'hello';
circle.area = function () {};
```
## export default 命令
从前面的例子可以看出，使用import命令的时候，用户需要知道所要加载的变量名或函数名，否则无法加载。
export default命令，为模块指出默认输出。
```bash
// export-default.js
export default function () {
  console.log('foo');
}

```
上面代脉的import命令，可以用任意名称指向export-default.js输出的方法
不需要的知道原模块输出的函数名。这时import，命令后面不使用大括号。
export default命令用在非匿名函数前，也是可以的。
```bash
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
上面代码中，foo函数的函数名，在模块外部是无效的。
加载的时候，视同匿名函数加载。
export default用于指定模块的默认输出。因此该命令只能使用一次。
本质上，export default就是输出一个叫default的变量或方法
然后系统允许你为他随意取名字。所以下面的写法有效：
```bash
// modules.js
function add(x, y) {
  return x * y;
}
export {add as default};
// 等同于
// export default add;

// app.js
import { default as foo } from 'modules';
// 等同于
// import foo from 'modules';
```
export后面不能跟变量声明语句。
export default命令的本质是将后面的值，赋值给default变量
所以可以直接写在export default之后
```bash
// 正确
export default 42;

// 报错
export 42; //没有指定对外接口为default

```
如果想在同一条export语句中，同时输入默认反方法和其他接口，可以写成下面这样
```bash
import _, { each, each as forEach } from 'lodash';
```
对应的export语句如下：
```bash
export default function (obj) {
  // ···
}

export function each(obj, iterator, context) {
  // ···
}

export { each as forEach };//暴露出forEach接口，默认指向each接口，即forEach和each指向同一个方向
```
### export default也可以用来输出类
```bash
// MyClass.js
export default class { ... }

// main.js
import MyClass from 'MyClass';
let o = new MyClass();
```
## export 与 import 的复合写法
如果在一个模块之中，先输入后输出同一个模块，import语句可以和export语句写在一起。
```bash
export { foo, bar } from 'my_module';

// 可以简单理解为
import { foo, bar } from 'my_module';
export { foo, bar };
```
上面代码中，export和import语句可以结合到一起，写成一行
写成一行后，foo和bar实际上并没有被导入当前模块，只是相当于对外转发了这两个接口，导致当前模块不能直接使用foo和bar
模块的改名接口和整体输出，也可以采用这种写法
```bash
// 接口改名
export { foo as myFoo } from 'my_module';

// 整体输出
export * from 'my_module';

```
默认接口的写法如下
```bash
export { default } from 'foo';
```
### 具名接口改为默认接口的方法如下
```bash
export { es6 as default } from './someModule';

// 等同于
import { es6 } from './someModule';
export default es6;

```
默认接口改为具名接口
```bash
export { default as es6 } from './someModule';
```
下面三种import语句，没有对应的复合写法
```bash
import * as someIdentifier from "someModule";
import someIdentifier from "someModule";
import someIdentifier, { namedIdentifier } from "someModule"
```
这三种复合写法的提案
```bash
export * as someIdentifier from "someModule";
export someIdentifier from "someModule";
export someIdentifier, { namedIdentifier } from "someModule";

```
## 模块的继承
假设有一个circleplus模块，继承circle模块。
```bash
// circleplus.js

export * from 'circle';
export var e = 2.71828182846;
export default function(x) {
  return Math.exp(x);
}
```
# 类和继承
## 类
根据我的理解，类就是把一个函数的算法与结果合并到一个类里面
eg.
```bash
//定义类
class Point {
    constructor(x, y) {    //constructor 构造方法
        this.x = x;
        this.y = y;
    }

    toString() {
        return '(' + this.x + ', ' + this.y + ')';
    }
}

var p = new Point(1, 2);
```
根据构造函数的prototpye属性，在ES6的类上面继续存在。
事实上，类的所有方法都还是定义在类的prototype上面。
### constructor方法
constructor方法是类的默认方法。通过new命令生成对象实例时，自动调用该方法/
一个类必须有constructor方法，如果没有显式定义，一个空的constructor会被默认添加。
## 继承
Class之间可以通过extends关键字实现继承
# CSSModules
CSS Modules很容易学，因为它的规则很少，同时又非常有用，可以保证某个组件的样式，不会影响到其他组件。
根据教程，，，，把别人的库拉下来。
安装依赖
```bash
$ cd css-modules-demos
$ npm install
```
还是npm i 比较靠谱
npm run demo01运行 打开浏览器，访问http://localhost:8000
## 局部作用域
CSS的规则都是全局的，任何一个组件的样式规则，都对整个页面有效。
产生局部作用域的唯一方法，就是使用独一无二的Class的名字，不会与其他选择器重名。这是CSSModules的做法。
示例：下面是一个React组件APP.js
```bash
import React from 'react';
import style from './App.css';

export default () => {
  return (
    <h1 className={style.title}>
      Hello World
    </h1>
  );
};
```
上面代码中，我们将样式文件App.css输入到style对象，然后引用style.title代表一个样式。
构建工具会将类名style.title编译成一个哈希字符串，APP.css也会同时编译。这样，这个类名就变成独一无二了，只对App组件有效。
CSS Modules 提供各种插件，支持不同的构建工具。
## 全局作用域
CSS Modules允许使用 :global(.ClassName)的语法，声明一个全局规则。凡是这样声明的Class,都不会被编译成哈希字符串。
全局作用域可以这样写 <h1 className="title">
CSS Modules 还提供一种显示的局部作用域语法 :local(.ClassName) 等同于 .ClassName
## 定义哈希类名
css-loader默认的哈希算法是[hash:base64]，webpack.config.js里面可以定制哈希字符串的格式。
4.Class的组合
在CSS Modules中，一个选择器可以继承另一个选择器，这称为“组合”("composition")。
在App.css中让 .title继承 .ClassName。
```bash
    .className {
      background-color: blue;
    }

    .title {
      composes: className;
      color: red;
    }
 ```
5.输入其他模块
选择器也可以继承其他CSS文件里面的规则。
```bash
    .title {
      composes: className from './another.css';
      color: red;
    }
```
6.输入变量
CSS Modules 支持变量，不过需要安装 PostCSS 和 postcss-modules-values。
把postcss-loader加入webpack.config.js 。
接着在colors.css里面定义变量。
App.css可以引用这些变量。
