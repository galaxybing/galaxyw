### [ECMAScript](https://github.com/tc39/ecma262) [ek'ma'script]
- [tc39 ECMA的第39号技术专家委员会（Technical Committee 39，简称TC39）负责制订ECMAScript标准，成员包括Microsoft、Mozilla、Google等大公司](https://tc39.github.io/ecma262/)
- [《ECMAScript 6入门》](http://es6.ruanyifeng.com/#docs/let)是一本开源的JavaScript语言教程，全面介绍ECMAScript 6新增的[语法特性](https://github.com/ruanyf/es6tutorial)

***

### 版本要点
#### es 命名:
  - TC39最终决定，标准在每年的6月份正式发布一次，作为当年的正式版本;这样，就不需要以前的版本号了，只要用年份标记就可以了。
  - ES6既是一个历史名词，也是一个泛指

    ```
    ECMAScript 1.0 是1997年发布
    ECMAScript 2.0（1998年6月）
    ECMAScript 3.0（1999年12月）-> 初学者一开始学习JavaScript，其实就是在学3.0版的语法
      _________/  \______________________________
     |						 |
     |						 |
     |						 |
     |						 |
    中止ECMAScript 4.0的开发，		  JavaScript.next继续开发，后来演变成ECMAScript 6
    将其中涉及现有功能改善的一		    (所以，ES6 制定的起点其实是2000年)
    小部分，发布为ECMAScript 3.1
    ECMAScript 3.1 就改名为 ECMAScript 5
     |
    ECMAscript 5.1 2011年6月 ------------->     ECMAScript 6 2015年6月，但简称 ES2015
    					       (从2000年算起，这时已经过去了15年!)
    						  |
      ---------------------------------------- ES2016 2016年6月
     | 		  			       (只新增了数组实例的includes方法和指数运算符)
     |
    ES2017标准 (根据计划，2017年6月将发布，当前提案有 await\async)
    ```
1. #### es3 - 初学者一开始学习JavaScript，其实就是在学3.0版的语法
    - 语法、类型、语句
    - 关键字、保留字、操作符
    - 对象
    - 引用类型
    - 闭包
    - 作用域链
    - 内置数据类型
        1. Undefined
        * Null
            - [null 能得到它的典型情况](https://jsfiddle.net/galaxybing/Lur1u5fu/)
        * Boolean
        * Number
        * String
        * Object
        * Symbol, es2015 的第 7 种数据类型
    - context 上下文
        1. [apply call](http://blog.csdn.net/JimmyLuo17/article/details/59123075)
        * 两种侧重点：
        ```
        第一 为 this对象环境借用一下方法; Array.prototype.slice.apply(类array, 0);
        第二 为方法内的 this.变量指定运行时环境。
        ```
    - Higher-order function 高阶函数
        1. 高阶函数的意思是它接收另一个函数作为参数。
        ```
        在 javascript 中，函数是一等公民，允许函数作为参数或者返回值传递。
        ```
        * Array.reduce()
          - [add(1)(2)(3)(4)(5)](http://web.jobbole.com/90654/#comment-97102)

* #### es5 - 中止ECMAScript 4.0的开发，发布ECMAScript 3.1 ，且改名为 ECMAScript 5，2011年6月 ECMAscript 5.1
    - [颜海镜 专注Web前端 -  关于部分不属于规范，本规范是 w3c 规范 ??? 的镜像版，目前以100%完成，但尚未校订，本规范为阅读而生，意在优化阅读体验](http://yanhaijing.com/es5/#null)
    - [Object.defineProperty 配置数据属性、访问器属性](https://jsfiddle.net/galaxybing/92kkaft5/)
    - "use strict"
        1. SpiderMonkey 在 2009 年实现严格模式
    - bind
        1. [apply call bind](http://blog.csdn.net/JimmyLuo17/article/details/59123075)
        * 使用 bind() 会创建一个绑定函数，它被执行的时候，它的 this 会被设置成带入的参数
        * 希望改变上下文环境之后并非立即执行，而是回调执行的时候，使用 bind() 方法。而 apply/call 则会立即执行函数。
* #### es2015 - es6文档页数由 5.1 时的 245 页扩充到 600 页
    - [ECMAScript 2015的 详尽文档](http://www.ecma-international.org/ecma-262/6.0/index.html)
    - [es6features]
        1. [Overview of ECMAScript 6 features](https://github.com/lukehoban/es6features#readme)
            - [中文翻译](http://www.tuicool.com/articles/Abim6f)
        * arrows
        * classes
        * enhanced object literals
        * template strings
        * destructuring
        * default + rest + spread
        * let + const
        * iterators + for..of
        * generators
        * unicode
        * modules 模块 import
        * module loaders
        * map + set + weakmap + weakset
            - [Set 数据结构的一个应用示例，数组元素去重](https://jsfiddle.net/galaxybing/qj3c4tpk/)
            - [Map 数据结构，值-值的Hash 形式](https://jsfiddle.net/galaxybing/3tzkj5s2/)
        * proxies
        * symbols
            - 一种类似于字符串的数据类型(https://jsfiddle.net/galaxybing/h255896f/) es2015 的第 7 种数据类型
            - 凡是属性名属于Symbol类型，就都是独一无二的，可以保证不会与其他属性名产生冲突
        * subclassable built-ins
        * promises
        * math + number + string + array + object APIs
        * binary and octal literals
        * reflect api
            - [Reflect 对象与Proxy对象一样，都是 ES6 为了操作对象而提供的新 API]
            - 现阶段，某些方法同时在Object和Reflect对象上部署，未来的新方法将只部署在Reflect对象上。也就是说，从Reflect对象上可以拿到语言内部的方法。
                [绑定一个函数的this对象](https://jsfiddle.net/galaxybing/4cdw2vn9/)
                ```
                var youngest = Reflect.apply(Math.min, Math, ages);
                ```
            - [new Proxy 与 Reflect.set()](https://jsfiddle.net/galaxybing/o5a76ma7/)
            - [Reflect.construct 与 new操作](https://jsfiddle.net/galaxybing/8xcbtztx/)
        * [tail calls][Reference-tail-calls]
    - "use strict"
        1. SpiderMonkey 在 2012 年实现了 ES6 里的默认参数值
        * class 和 module 都是默认严格模式的，没必要你写 "use strict" 了
        * [带有“非简单参数”的函数为什么不能包含 "use strict" 指令](http://www.cnblogs.com/ziyunfei/p/5926123.html)
    - 有6种声明变量的方法:
        ```
        var
        function
        let
        const
        import
        class
        ```

* #### ES2016 - 提案
    - Array.prototype.includes()
        - [].includes.call(arguments, 'a') 在函数的arguments对象上调用的includes() 方法
    - 指数运算符
    - 异步方法
    - 定型对象
    - observables
* #### ES2017
    - await\async

***

### cookbook
- kangax 维护
    - [浏览器版本，对ES6的支持](http://kangax.github.io/compat-table/es6/)

- [ES-Checker模块](http://ruanyf.github.io/es-checker/) 用来检查各种运行环境对ES6的支持情况，检测浏览器支持es6
    ```
    $ npm install -g es-checker
    $ es-checker
    ```
- zhangmingkai
    1. [ES2015系列(四) Symbol的使用](http://cnodejs.org/topic/56d1aee8a3e318b766ffb9bc)
    * [介绍了关于es6和es5的对比 - ES6-jsmean](http://jsmean.com/es6#/?_k=dtds7z)

- [es2015的块级作用域-在ie浏览器里面明显不支持](https://jsfiddle.net/galaxybing/qf4hzk4s/)
- javascript的顶层对象
	- 浏览器里面，顶层对象是window，但 Node 和 Web Worker 没有window。
	- 浏览器和 Web Worker 里面，self也指向顶层对象，但是Node没有self。
	- Node 里面，顶层对象是global，但其他环境都不支持。
    ```
	勉强可以使用的获取 global 对象的方法：
	var getGlobal = function () {
	  if (typeof self !== 'undefined') { return self; }
	  if (typeof window !== 'undefined') { return window; }
	  if (typeof global !== 'undefined') { return global; }
	  throw new Error('unable to locate global object');
	};
    ```
- destructuring 解构
    - (从数组或对象)提取值,对变量进行```赋值```
      - ->  var { foo, bar } = { foo: "aaa", bar: "bbb" };
    - [函数参数的解构，对参数指定默认值](https://jsfiddle.net/galaxybing/5e5pe9en/)
    - [rest 运算符/...扩展(spread)运算符](https://jsfiddle.net/galaxybing/5z1bns80/)
      - ->  var b= ["name", "bingbing"];
      ```
        ...b // name bingbing 两个字符被提取出来
        扩展运算符只能遍历带有 Iterator接口的对象
        var myIterable = {b: "family", 1: 31, a:"name"};// myIterable[Symbol.iterator] is not a function
      ```
      - rest 运算符的解构赋值
      ```
        (从数组或对象)提取值对变量（z是解构赋值所在的对象）进行赋值
        ->  let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
            let { x, ...z, y } = { x: 1, y: 2, a: 3, b: 4 };// 解构赋值所在对象如果不放最后，会报名错；
            * 但是= 号右边使用扩展运算符，而不是解构赋值时，不会报错的；
            let aWithOverrides = { ...a, x: 1, y: 2 };
      ```
      - [ES7有一个提案，将Rest运算符（解构赋值）/扩展运算符（...）引入对象。Babel转码器已经支持这项功能](https://jsfiddle.net/galaxybing/7g5d1by4/)

- [用setPrototypeOf(obj, proto) 设置的原型属性可以直接读取](https://jsfiddle.net/galaxybing/o1zeapcw/)
- [getOwnPropertyDescriptors 获取对象属性的使用](https://jsfiddle.net/galaxybing/p2brozrz/)

- [generator（生成器）,带有 yield 的函数](https://jsfiddle.net/galaxybing/exj5nbdd/)
    - [Python](http://www.ibm.com/developerworks/cn/opensource/os-cn-python-yield/)
    - [C#](https://msdn.microsoft.com/zh-cn/library/9k7k7cf0.aspx)
    - [javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/yield)
      - [模拟 iterator 的 next() 方法](https://jsfiddle.net/galaxybing/kh0g62et/)
      - [Symbol.iterator属性本身是一个函数，就是当前数据结构默认的遍历器生成函数](https://jsfiddle.net/galaxybing/eetaap2L/)
      - [Symbol.iterator函数方法的最简单实现，还是使用Generator函数](https://jsfiddle.net/galaxybing/smL1ppeL/)
    - 异步编程
      - [Generator 函数的惰性执行](https://jsfiddle.net/galaxybing/c9xkx7tj/)
      - [Promise是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。](https://jsfiddle.net/galaxybing/8fzoug95/)
        * 一旦状态改变，就不会再变，任何时候都可以得到这个结果;这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的
        - [resolve函数的参数除了正常的值以外，还可能是另一个Promise实例](https://jsfiddle.net/galaxybing/v8j35tek/)

###### URI 标识
[Reference-tail-calls]:tail-calls.md

