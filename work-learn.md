## 插件

### **file-saver**

导出文件

### **vuedraggable**

Vue.Draggable是一款基于Sortable.js实现的vue拖拽插件

### **vue-pdf**

pdf文件在线预览

### **vue-count-to**

数字滚动插件

### **qs**

增加了一些安全性的查询字符串解析和序列化字符串的库

> qs.parse()是将URL解析成对象的形式
>
> - qs 允许在查询字符串中使用`[]`的方式创建嵌套的对象
>
>   ```
>   assert.deepEqual(qs.parse('foo[bar]=baz'), { foo: {  bar: 'baz' } })
>   ```
>
> - 可以解析 URI 编码
>
>   ```
>   assert.deepEqual(qs.parse('a%5Bb%5D=c')  ==  { a: { b: 'c' } })
>   ```
>
> qs.stringify()是将对象 序列化成URL的形式，以&进行拼接
>
> - 默认情况下，对象序列化后进行URL编码后输出
>
>   ```
>   assert.equal(qs.stringify({ a: { b: 'c' } }), 'a%5Bb%5D=c');
>   ```
>
> - 通过设置 `encode` 为 `false` 禁止 URI 编码
>
>   ```
>   var unencoded = qs.stringify({ a: { b: 'c' } }, { encode: false });
>   assert.equal(unencoded, 'a[b]=c');
>   ```
>
> - 通过设置 `encodeValuesOnly` 为 `true`，可以禁用对 key 进行URI 编码
>
>   ```
>   var encodedValues = qs.stringify(
>     	    { a: 'b', c: ['d', 'e=f'], f: [['g'], ['h']] },
>     	    { encodeValuesOnly: true }
>     	);
>     	assert.equal(encodedValues,'a=b&c[0]=d&c[1]=e%3Df&f[0][0]=g&f[1][0]=h');
>   ```
>
>   
>
> ------
>
>   [^assert.equal(actual, expected[, message\])]: 判断实际值(actual)与期望徝(expected)是否相等(==)，如果不相等，则抛出一个message的错误。



## Blob

blob对象是二进制数据，但它是类似文件对象的二进制数据，因此可以像操作File对象一样操作Blob对象，实际上，File继承自Blob。

- 可以通过Blob的构造函数创建Blob对象：

```
var data3 = "<div style='color:red;'>This is a blob</div>";
console.log(blob3);  //输出：Blob {size: 44, type: ""}
```

- Blob对象能够添加到表单中，作为上传数据使用：

  ```
  const content = '<a id="a"><b id="b">hey!</b></a>';
  const blob = new Blob([content], {type: 'text/xml'});
  
  formData.append('webmasterfile', blob);
  ```

- Blob URL

  Blob URL可以通过`URL.createObjectURL(blob)`创建。在绝大部分场景下，我们可以像使用Http协议得URL一样使用Blob URL。常见得场景有： 作为文件得下载地址和作为图片资源地址

  ```
  axios({ //导出excel
  method: 'post',
  	url: '/a/haems/aeRepInfo/exRepData',
  	data: params,
  	responseType: 'blob'
  }).then((res) => {
      const link = document.createElement('a')
      let blob = new Blob([res.data], {
      	type: 'application/vnd.ms-excel'
      });
      link.style.display = 'none'
      link.href = URL.createObjectURL(blob);
      link.setAttribute('download', '事件明细' + moment().format("YYYYMMDD") + '.xls')
      document.body.appendChild(link)
      link.click()
      document.body.removeChild(link)
      this.$message({
          message: '导出成功',
          type: 'success'
      });
  
  }).catch(error => {})
  ```

- URL.revokeObjectURL()

  在每次调用createObjectURL()方法时，都会创建一个新的URL对象，即使你已经用相同的对象作为参数创建过。当不再需要这些URL对象时，每个对象必须通过调用URL.revokeObjectURL()方法来释放。浏览器会在文档退出时自动释放它们，但是为了获得最佳性能和内存使用状况，你应该在安全的时机主动释放掉它们。

  ```xml
  <!DOCTYPE html>
  <html lang="en">
  
  <head>
    <meta charset="UTF-8">
    <title>Blob Test</title>
    <script>
      function handleFile(e) {
        var file = e.files[0];
        var blob = URL.createObjectURL(file);
        var img = document.getElementByTagName("img")[0];
        img.src = blob;
        img.onload = function(e) {
          URL.revokeObjectURL(this.src); //释放createObjectURL创建得对象
        }
      }
    </script>
  </head>
  
  <body>
    <input type="flie" accept="image/*" onchange="handleFile(this)" />
    <br/>
    <img style="width:200px;height:200px;">
  </body>
  
  </html>
  ```

## 路由懒加载的几种方式

- 结合 Vue 的[异步组件 (opens new window)](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#异步组件)和 Webpack 的[代码分割功能](https://doc.webpack-china.org/guides/code-splitting-async/#require-ensure-/)

  ```
  component: () => import('../views/About.vue')
  ```

- 把某个路由下的所有组件都打包在同个异步块 (chunk) 中

  ```
  component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  ```

- `resolve`的异步机制，用`require`代替了`import`,实现按需加载

  ```
  component: resolve => require(['../views/About.vue'],resolve)
  ```

- require.ensure

  ```
  component: resolve => require.ensure([], () => resolve(require('@/components/'+componentName)), 'webpackChunkName')
  ```


## URI和URL是什么，以及他们的区别

- URL，`Uniform Resource Locator`，统一资源定位符。用于表示网络上服务器的资源所在位置，比如我们输入浏览器的地址。
- URI，`Uniform Resource Identifier`，统一资源标识符。统一资源标志符URI就是在某一规则下能把一个资源独一无二地标识出来。URL起到了URI的作用，所以URL是URI的子集

**为什么URL要编码？**
答：URL是表示网络上各种资源的地址，表示网络上各资源的地址可能是由各种各样的字符组成，可能包含许多特殊字符，甚至一些不可打印的字符。而用URL表示这些地址，基于要适应各种协议、应用程序的要求，URL必须是通用性强，而已还必须对人们是可读的。所以，就有了将URL编码的方法，用一些安全的字符集合编码表示URL，简单来说，就是用一串安全的字符表示原先的可能包含特殊字符、不可打印字符的地址。

URL的编码方式是怎么样的？
答：用安全的字符表示不安全的字符。转义后的安全字符字符为16进制数，每两位前面加%，见以下示例：

比如&，它在ASCII表用10进制38表示，16进制26表示，所以，URL编码后为%26。
比如“你好”，它的UTF-8编码用16进制表示是e4bda0e5a5bd，所以，URL编码后为%e4%bd%a0%e5%a5%bd。
比如“你好”，它的GB2312编码用16进制表示是c4e3bac3，所以，URL编码后为%c4%e3%ba%c3。

## axios关于QS 序列化问题

前后台在对接的时候，约定好入参格式：

1. 如果约定为纯JSON格式入参，即Content-Type=application/json。后台使用@RequestBody注解，接收前端传递的参数，前台直接使用axios.post即可，不再需要使用QS序列化。
2. 如果约定为纯表单格式入参，即Content-Type=application/x-ww-form-urlencoder。后台使用@RequestParam注解，接收前端传递的参数，前台使用axios.post提交参数，需要对data进行QS序列化操作。

## 模块化编程方案

### 什么是模块化？

模块化就是将变量和函数 放入不同的文件中 

模块的作用域是私有的 内部定义的代码只能在当前文件中使用 外部使用那么需要将此模块暴露出去

减少全局变量 避免变量名和函数命名冲突

提高代码的复用性和维护性

### AMD、CMD、CommonJs、ES6的对比:

​	AMD、CMD、CommonJs是ES5中提供的模块化编程的方案

​	前端浏览器一般使用AMD、CMD、ES6等定义模块化开发

​	nodeJs后台采用CommonJs规范

- AMD

  ```
  AMD是RequireJs在推广过程中对模块定义的规范化产出,RequireJS是对AMD的实现
  两者的关系就好像是JavaScript语言是对ECMAScript规范的实现
  特点：
  	。异步加载多个js文件, 减少网页加载的等待
      。设置某两个js文件前后顺序加载,管理模块间的依赖性
  
  // 定义
  define("模块名", ['其它模块名',...], function (参数,...){
   //成员: 变量/方法
   return {
      要抛出的成员
   }
  }); //其中，参数指代前面数组中引入的模块
  
  //引入
  require(["子模块",...], function(参数, ...){
    //参数指代子模块对象
    //这里可以调用子模块中的成员了
  })
  
  
  ```

  

- CMD

  ```
  CMD---是SeaJS在推广过程中对模块定义的规范化产出，是一个同步模块定义，SeaJS是淘宝团队提供的一个模块开发的js框架.
  
  //所有模块通过defined来定义
  define(function(require,export,module){
    //通过require引入依赖
    var $=require('jqurey');
    var spinning=require('./spinning');
  })
  ```

  

- CommonJs

  ```
  通过module.exports定义的，在前端浏览器里面并不支持module.exports,通过node.js后端使用的。Nodejs端是使用CommonJS规范的，
  
  导出模块使用：module.exports、exports.xxx(相当于module.exports={xxx})
  			exports 仅仅是 module.exports 的一个引用,如果先使用 exports 对外暴露属性或方法，再使用 module.exports 暴露对象，会使得 exports 上暴露的属性或者方法失效
  引入使用：require("path")
  ```

  

- ES6

  ```
  导出：export default xxx 或 export const f = xxx 或 export {a,b,c}
  引入：import name from 路径
  ```

### ES6与CommonJs的区别

- CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。

  - commonjs对于基本数据类型，采用复制副本的方式

  - commonjs对于复杂数据类型，采用对象引用的方式

```
    也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。请看下面这个模块文件lib.js的例子。
    
    // lib.js
    let count = 3;
    let obj = {
        num: 3
    }
    const addCount = function() {
        count++;
        obj.num++
    }
    
    module.exports = {
        count,
        obj,
        addCount: addCount,
        };
    
    上面代码输出内部变量counter和改写这个变量的内部方法incCounter。然后，在main.js里面加载这个模块。
    
     // main.js
    const lib = require('./lib.js');
    console.log(lib.count,lib.obj.num);//3 3
    lib.addCount();
    console.log(lib.count,lib.obj.num);//3 4 
```



- CommonJS模块是运行时加载，ES6 模块是编译时输出接口。
  - CommonJS模块无论加载多少次，都只会在第一次加载时运行一次，以后再加载，就返回缓存中第一次创建的模块对象
  - ES6模块是先分析模块之间的依赖关系，然后再严格按照依赖关系，先后执行。而不是在第一次引入时执行。

- CommonJs 是单个值导出，ES6 Module可以导出多个

- CommonJs 是动态语法可以写在判断里，ES6 Module 静态语法只能写在顶层

- CommonJs 的 this 是当前模块，ES6 Module的 this 是 undefined

