# Module 是什么

### 1.什么是模块

   模块：一个一个的局部作用域的代码块

### 2.什么是模块系统

   模块系统需要解决的主要问题

   ① 模块化的问题

   ② 消除全局变量

   ③ 管理加载顺序

以前使用的RequireJS seaJS

现在的模块ES Module

# module的基本用法

### Live Server

服务器环境是http

建立本地服务器的vs插件

base文件最后

```js
...
//导出
export default BaseSlider;
```

slider文件上下

```js
import BaseSlider from './base.js';
。。。
export default Slider;
```

index文件

```js
import Slider from './slider.js';

new Slider(document.querySelector('.slider'));
```

模块化的引用

```js
<script src="./index.js" type="module"></script>
```

js中./是同级当前目录下

# module的导入和导出

### 没有导出也是可以导入的

**多次导入也只执行一遍**

```js
const age=18;
console.log(age);
```

```js
<script type="module">
        没有导出也是可以导入的
        多次导入也只执行一遍
        import './base.js'
        import './base.js'//打印出1
    </script>
```

### export default只能有一个

```js
const age=18;
console.log(age);
export default age
// export default 只能有一个
// export default sex
```

```js
<script type="module">
        //aaa这里可以随便取名
        import aaa from './base.js'
        console.log(aaa);
    </script>
```



# export 和对应的 import

### export 后面声明或语句

导出

```js
export age; //错
export const age = 18;//对
/或者
export { age };
```

导入

```js
import { age } from './module.js';
      console.log(age);
```

### 多个导出

**一个一导出**

```js
//function fn() {}
 export fn; // ×

export function fn() {}//√
//或
export {fn}//√
// export function () {} // 匿名不行
```

**一个导入**

```js
import { fn } from './module.js';
```



**多个导出**

```js
export { fn as func, className, age };//module文件
```

**多个导入**

```js
import { func, age, className } from './module.js'
```

导出导入改名

```js
export { fn as func, className, age };
import { func, age, className as Person } from './module.js';
```

### 整体导入

会导入所有导出，包括通过 export default 导出的

export default 和export可以同时存在

```js
 import * as obj from './module.js';
      console.log(obj);
```

### 同时导入

一定是 export default 的在前

```js
      import age2, { func, age, className } from './module.js';
```

# Module 的注意事项

### 1.模块顶层的 this 指向

模块中，顶层的 this 指向 undefined//顶层：不在块级作用域中

```js
console.log(this);

if (typeof this !== 'undefined') {//使用this判断是否以模块形式导出
  throw new Error('请使用模块的方式加载');
}
-------------------
import './module.js';

```

### 2.import 和 import()

   import 命令具有提升效果，会提升到整个模块的头部，率先执行

   ```js
   console.log('沙发');
   
      console.log('第二');
   
      import './module.js';//优先执行
   ```

import 执行的时候，代码还没执行

import 和 export 命令只能在模块的顶层，不能在代码块中执行

```js
if (PC) {
        import 'pc.js';
      } else if (Mobile) {
        import 'mobile.js';
      }//x
```

   import() 可以按条件导入

 ```js
 if (PC) {
         import('pc.js').then().catch();//得到promise对象
       } else if (Mobile) {
         import('mobile.js').then().catch();
       }
 ```

