# 前端代码的模块化

## 为什么需要模块化

- 全局变量污染
- 代码的封装与复用
- 减少请求次数（减少 script 标签）

## 演变史
### 原始
把相关的函数放在一起，就算一个模块
```
 function f1(){
     // do something
 }
 function f2(){
     // do something
 }
```
缺点：
1. 污染了全局变量
2. 有与其他模块发生变量名冲突的风险。
3.  模块成员之间的关系

### 对象写法
```
var module = new Object({
    f1: function(){
        // do something
    },
    f2: function(){
        // do something
    }
});

module.f1();//通过.来调用对象内的方法
```
改善了原始写法的缺点，
缺点：暴露了模块成员，内部状态可以被外部所改写。
### 函数作用域和块作用域

#### 立即执行函数表达式（IIFE）

```
var module = (function(){
    var _count = 0;
    
    var f1 = function(){
        // operate count
    };

    var  f2 = function(){
        // operate count
    };
    return {
        f1,
        f2
    }
})();
```
通过暴露出来的方法来对内部私有数据进行操作，外部无法直接操作内部私有变量。
### AMD(requireJS)
Asynchronous Module Definition
它采用异步加载模块，模块的加载不影响后面语句的执行，所有依赖这个模块的语句都定义在一个回调函数中，等到模块加载完成后，这个回调才会执行
特点：提前执行，前置依赖
```
define('./index.js',function(code){
    //code就是index.js返回的内容
})
```
```
@param [module] , 要加载的模块，
@callback， 加载成功的回调
require([module],callback);

```
### CMD(seaJS)
Common Module Definition，提供了模块定义和按需加载执行模块
特点：延迟执行，就近依赖。需要时引用
``` 
define(factory)//define为一个全局函数，用来定义模块，这里factory可以是函数，字符串或者对象
```
```
//@param {Function} require 同步方法
// require.async()是异步加载方法。
 define(function(require,exports,module){
     var indexCode = require('index.js);
 })
```

### CommonJS: node.js中自带的模块化
```
var fs = require('fs');
```
### UMD 兼容AMD，CommonJS模块化语法
### ES6 模块
定义：模块是自动运行在严格模式下且没有办法退出运行的JavaScript代码。
模块特性：
1. 在模块的顶部，this的值是undefined；
2. 模块不支持HTML风格的代码注释
#### 导出的基本语法
使用`export`关键字
```
/* a.js */
// 导出数据
export var color = "red";
export let name = "george";
exoprt const arr = "hangzhou";
// 导出函数
export function add(num1, num2){
    return num1+num2;
}

// 导出类
export class Rectangle(){
    constructor(length, width){
        this.length = length;
        this.width = width;
    }
}

// private function
function priFunc(){
    console.log('You can't see me!');
}
// 定义一个函数
function home(){
    console.log('spring festival!');
}
//将模块中的模块导出
export home;
```
### 导入的基本语法
使用`import`关键字在另外一个模块进行导入。
```
//     要导入的标识符     标识符应当从那个模块导入 
import {add, home} from './a.js' 
```
`import`后面的大括号表示从给定模块导入的绑定（binding），关键词`from`表示从那个模块导入给定的绑定。模块由表示模块路径的字符串指定（模块说明符）。
浏览器使用的路径格式与传递给`<script>`元素的相同。也就是说，必须把文件扩展名也加上。
Node.js则遵循基于文件系统前缀区分本地文件和包的惯例。例如， `test`是一个包而`test.js`是一个本地文件。

当从模块上导入一个绑定时，就好像使用const定义的一样。你无法定义同名变量（包括导入另外一个绑定）。
也无法在import语句前使用标识符或改变绑定的值。
### 导入整个模块
特殊情况下，可以导入整个模块作为一个单一的对象。然后所有的导出都可以作为对象的属性使用。
```
import * as a from './a.js'
console.log(a.color);// red
console.log(a.home()); // spring festival!
```
这种方式叫做命名空间导入，a被称作命名空间对象。
不管在`import`语句中把一个模块写了多少次，该模块只执行一次。导入模块的代码执行后，实例化后的模块被保存在了内存中，只要另一个`import`语句引用它就可以重复使用它。
```
import {color} from './a.js'
import {home} from './a.js'
import {add} from './a.js'
```
在上述代码中，import只会执行一次。
import 和 export被设计为静态的。
<font style="color: red">export 和import必须在其他语句和函数之外使用</font>
<b>在顶部使用import和export</b>

### 特点
 ES6的 `import`语句为变量、函数和类创建的是只读绑定，而不是像正常变量一样简单地引用原始绑定。
 ```
 //b.js
 export var name = "george"
 export function setName(newName){
     name = newName
 }
 ```
 ```
 import {name, setName} from './b.js';
 console.log(name);//"george"
 setName('paul');
 console.log(name);//"paul"
 name = "galio";//error
 ```
 调用`setName('paul')`会回到导入`setName()`的模块上去执行，并将`name`置为`"pual"`。
 此更改会自动在导入的name绑定上体现。原因是，`name`是导出的`name`标识符的本地名称。本段代码中所使用的`name`和模块中导入的`name`不是同一个。
 ### 导出时和导入时重命名
 使用<b>as</b>关键字
 <b>export</b> 本地名称 <b>as</b> 导出名
 ```
 //c.js
 function sum(num1,num2){
     return num1+num2;
 }
 export {sum as add}
 ```
 导入时：
 ```
 import {add} from './c.js';
 console.log(add(1,2));//3
 ```
 导入时使用不同的名称，使用<b>as</b>关键字
 ```
 import {add as sum} from './c.js';
 console.log(sum(1,2));//3
 ```
 ### 模块的默认值
 <b>default</b>关键字
 #### 导出默认值
 由于函数被模块所代表，因此不需要名称
 ```
 // d.js
 export default function(num1,num2){
     return num1+num2
 }
 ```
 或者先定义，再导出
 ```
 function sum(num1,num2){
     return num1 + num2;
 }
 export default sum;
 ```
 或者使用重命名语法
 ```
 function sum(num1,num2){
     return num1 + num2;
 }
 export {sum as default};
 ```

 #### 导入默认值
 1. 只导入一个默认值
 ```
 // 导入默认值
 import sum from './d.js'
 console.log(sum(1,2));// 3
 ```
 这儿没有使用大括号，与非默认导入情况不同，本地名称<b>sum</b>用于表示模块导出的任何默认函数，这种语法是最纯净的。
2. 导入默认值以及多个非默认绑定。
```
//d.js
export let name = "george";
export default function(newName){
    name = newName;
}
```
```
import setName,{name} from './d.js';
console.log(name);
setName('paul');
```
<font style="color:red">默认值必须排在非默认值前面</font>
导入时使用重命名语法
```
import {default as setName, name} from './d.js';
```

#### 重新导出一个绑定
```
import { setName } from './d.js';
export { setName };
```
或者
```
export { setName } from './d.js';
```
对值进行重命名
```
export { setName as st} from './d.js';
```
导出一个模块的所有值
所有值代表默认值以及命名导出值。如果d.js中有默认的导出值，使用此方法时无法定义一个新的默认导出。
```
export * from './d.js';
```
#### 无绑定导入
某些模块可能不导出任何东西，相反，他们可能只修改全局作用域中的对象。尽管模块中的顶层变量、函数和类不会自动地出现在全局作用域中，但这并不意味着模块无法访问全局作用域。内建对象的共享定义可以在模块中定义，对这些对象所做的更改将反应在其他模块中。

用处：polyfill和shim