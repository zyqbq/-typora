# 初识Ajax

### 1.Ajax 是什么

   Ajax 是 Asynchronous JavaScript and XML（异步JavaScript 和 XML）的简写



   Ajax 中的异步：可以**异步地向服务器发送请求**，在**等待响应**的过程中，**不会阻塞当前页面**，浏览器可以做自己的事情。直到成功获取响应后，浏览器才开始处理响应数据



   XML（可扩展标记语言）是前后端数据通信时传输数据的一种格式



   XML 现在已经**不怎么用了**，现在比较常用的是 JSON



   Ajax 其实就是浏览器与服务器之间的一种**异步通信方式**



   使用 Ajax 可以在不重新加载整个页面的情况下，对页面的某部分进行更新



   ① 慕课网**注册检测**

   ② 慕课网**搜索提示**



### 2.搭建 Ajax 开发环境

   Ajax 需要**服务器环境**，非服务器环境下，很多浏览器无法正常使用 Ajax



   Live Server



   windows phpStudy

   Mac MAMP

# Ajax 的基本用法

### 1.XMLHttpRequest

   console.log(Ajax);



   Ajax 想要实现浏览器与服务器之间的异步通信，需要依靠 **XMLHttpRequest**，它是一个**构造函数**



   不论是 XMLHttpRequest，还是 Ajax，都**没有**和具体的某种数据格式绑定



### 2.Ajax 的使用步骤

#### 2.1.创建 xhr 对象

   const xhr = new XMLHttpRequest();



#### 2.2.监听事件，处理响应

   当获取到响应后，会触发 xhr 对象的 readystatechange 事件，可以在该事件中对响应进行处理



   ```js
   //不考虑兼容性问题
   xhr.addEventListener('readystatechange', () => {}, fasle);
   
   
   //兼容性最好
      xhr.onreadystatechange = () => {
   
   ​    if (xhr.readyState !== 4) return;
   
   
   
    // HTTP CODE
   
    // 获取到响应后，响应的内容会自动填充 xhr 对象的属性
   
   // xhr.status：HTTP  200 404
   
   // xhr.statusText：HTTP 状态说明 OK Not Found
   
   if ((xhr.status >= 200) & (xhr.status < 300) || xhr.status === 304) {
   
    // console.log('正常使用响应数据');
   
    console.log(xhr.responseText);
   
    }
   
      };
   
   
   ```



   readystatechange 事件也可以配合 addEventListener 使用，不过要注意，IE6~8 不支持 addEventListener

   为了兼容性，readystatechange 中不使用 this，而是直接使用 xhr

   由于兼容性的原因，最好放在 open 之前



   readystatechange 事件监听 readyState 这个**状态**的变化

   它的值从 0 ~ 4，一共 5 个状态

   0：未初始化。尚未调用 open()

   1：启动。已经调用 open()，但尚未调用 send()

   2：发送。已经调用 send()，但尚未接收到响应

   3：接收。已经接收到部分响应数据

   4：完成。已经接收到全部响应数据，而且已经可以在浏览器中使用了



### 2.3.准备发送请求

   xhr.open(

​    'HTTP 方法 GET、POST、PUT、DELETE',

​    '地址 URL https://www.imooc.com/api/http/search/suggest?words=js ./index.html ./index.xml ./index.txt',

​    true

   );



   调用 open 并不会真正发送请求，而只是做好发送请求前的准备工作



### 2.4.发送请求

   调用 send() 正式发送请求



   send() 的参数是通过**请求体**携带的数据

   xhr.send(null);



#### 3.使用 Ajax 完成前后端通信.

```js
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

      const xhr = new XMLHttpRequest();
      xhr.onreadystatechange = () => {
        if (xhr.readyState !== 4) return;

        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
          console.log(xhr.responseText);
          console.log(typeof xhr.responseText);
        }
      };
      xhr.open('GET', url, true);
      xhr.send(null);
```

# GET 请求

### 1.携带数据

   GET 请求**不能通过请求体携带数据**，但可以通过**请求头**携带

   ？后面以名值对的形式添加&连接

const url =

​    'https://www.imooc.com/api/http/search/suggest?**words=js**&username=alex&age=18';

  ```js
  const url =
          'https://www.imooc.com/api/http/search/suggest?words=js&username=alex&age=18';
        const xhr = new XMLHttpRequest();
  
        xhr.onreadystatechange = () => {
          if (xhr.readyState != 4) return;
  
          if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
            console.log(xhr.responseText);
          }
        };
  
        xhr.open('GET', url, true);
  
        xhr.send(null);
  ```

xhr中的Payload中的Query String Parameters查看

调用 send() 正式发送请求



   send() 的参数是通过**请求体**携带的数据

 不会报错，但不会发送数据

  xhr.send('sex=male');

form表单的提交类似，但是提交网址不要带任何数据

### 2.数据编码

  如果携带的数据是非英文字母的话，比如说汉字，就需要编码之后再发送给后端，不然会造成乱码问题

  可以使用 encodeURIComponent() 编码

 ```js
  const url = `https://www.imooc.com/api/http/search/suggest?words=${encodeURIComponent('前端')}`;
 ```

请求体携带的数据在请求负载中Request Payload

# POST 请求

### 1.携带数据

   POST 请求主要通过请求体携带数据，同时也可以通过请求头携带

 ```js
 const url = 'https://www.imooc.com/api/http/search/suggest?words=js';
       const xhr = new XMLHttpRequest();
 
       xhr.onreadystatechange = () => {
         if (xhr.readyState != 4) return;
 
         if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
           console.log(xhr.responseText);
         }
       };
 
       xhr.open('POST', url, true);
 ```





   如果想发送数据，直接写在 send() 的参数位置，一般是字符串

  ```js
   xhr.send('username=alex&age=18');
  ```

   **不能直接传递对象**，需要先将对象转换成字符串的形式

   ```js
   xhr.send({
   ​    username: 'alex',
   ​    age: 18
      });
   
      [object Object]
   ```

   2.数据编码

   xhr.send(`username=${encodeURIComponent('张三')}&age=18`);

# 初识 JSON

#### 1.JSON 是什么

   Ajax 发送和接收数据的一种格式

   XML

   username=alex&age=18

   JSON

```js
 {"code":200,"data":[{"word":"jsp"},{"word":"js"},{"word":"json"},{"word":"js \u5165\u95e8"},{"word":"jstl"}]}
```

  JSON 全称是 JavaScript Object Notation，就是js对象表示法、

xlm是标签形式携带数据的，会导致数据变大，json有效信息更多，数据更小

#### 2.为什么需要 JSON

​    JSON 有 3 种形式，每种形式的写法都和 JS 中的数据类型很像，可以很轻松的和 JS 中的数据类型互相转换

json作为一个能方便前后端的解析，前后端都有解析的方式

   JS->JSON->PHP/Java

   PHP/Java->JSON->JS

# JSON 的 3 种形式

### 1.简单值形式

   .json



   JSON 的简单值形式就对应着 JS 中的基础数据类型

   数字、字符串、布尔值、null



   注意事项

   ① JSON 中**没有 undefined** 值

   ② JSON 中的字符串必须使用**双引号**

   ③ JSON 中是**不能注释**的

### 2.对象形式

   JSON 的对象形式就对应着 JS 中的对象



   注意事项

   JSON 中对象的属性名必须用双引号，属性值如果是字符串也必须用双引号

   JSON 中只要涉及到字符串，就必须使用双引号

   不支持 undefined、

```json
{
  "name": "张三",
  "age": 18,
  "hobby": ["足球", "乒乓球"],
  "family": {
    "father": "张老大",
    "mother": "李四"
  }
}
```

#### 3.数组形式

   JSON 的数组形式就对应着 JS 中的数组



   [1, "hi", null]



   注意事项

   数组中的字符串必须用双引号

   JSON 中只要涉及到字符串，就必须使用双引号

   不支持 undefined

```js
[
  {
    "id": 1,
    "username": "张三",
    "comment": "666"
  },
  {
    "id": 2,
    "username": "李四",
    "comment": "999 6翻了"
  }
]
```

# JSON 的常用方法

### 1.JSON.parse()

   JSON.parse() 可以将 **JSON 格式的字符串解析成 JS 中的对应值**

   一定要是合法的 JSON 字符串，否则会报错

解析结果可以直接使用js的相关属性

   ```js
   const xhr = new XMLHttpRequest();
   
         xhr.onreadystatechange = () => {
           if (xhr.readyState != 4) return;
   
           if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
             console.log(xhr.responseText);
             console.log(typeof xhr.responseText);
   
             console.log(JSON.parse(xhr.responseText));
             //console.log(JSON.parse(xhr.responseText).data);
           }
         };
    // xhr.open('GET', './plain.json', true);
         // xhr.open('GET', './obj.json', true);
         xhr.open('GET', './arr.json', true);
   
         // xhr.open(
         //   'GET',
         //   'https://www.imooc.com/api/http/search/suggest?words=js',
         //   true
         // );
      xhr.send(null);
   ```

 



### 2.JSON.stringify()

   JSON.stringify() 可以将 **JS 的基本数据类型、对象或者数组转换成 JSON 格式的字符串**

```js
console.log(
        JSON.stringify({
          username: 'alex',
          age: 18
        })
      );

      const xhr = new XMLHttpRequest();

      xhr.open(
        'POST',
        'https://www.imooc.com/api/http/search/suggest?words=js',
        true
      );
      xhr.send(
        JSON.stringify({
          username: 'alex',
          age: 18
        })
      );
```

3.使用 JSON.parse() 和 JSON.stringify() 封装 localStorage

```js
import { get, set, remove, clear } from './storage.js';

set('username', 'alex');
console.log(get('username'));

set('zs', {
name: '张三',
age: 18
});
console.log(get('zs'));

remove('username');
clear();
```

storage.js

```js
const storage = window.localStorage;

// 设置
const set = (key, value) => {
  // {
  //   username: 'alex'
  // }
    //转换为json字符串形式
  storage.setItem(key, JSON.stringify(value));
};

// 获取
const get = key => {
  // 'alex'
  // {
  //   "username": "alex"
  // }
    //转换为js格式的复杂数据类型
  return JSON.parse(storage.getItem(key));
};

// 删除
const remove = key => {
  storage.removeItem(key);
};

// 清空
const clear = () => {
  storage.clear();
};

export { set, get, remove, clear };
```

# 初识跨域

### 1.跨域是什么

   同域，不是跨域

   const url = './index.html';

   http://127.0.0.1:5500



   **不同域，跨域，被浏览器阻止**

   const url = 'https://www.imooc.com';

   const xhr = new XMLHttpRequest();



   Access to XMLHttpRequest at 'https://www.imooc.com/' from origin 'http://127.0.0.1:5500' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource



   向一个域发送请求，如果要请求的域和当前域是不同域，就叫跨域

   不同域之间的请求，就是跨域请求



   xhr.onreadystatechange = () => {

​    if (xhr.readyState != 4) return;



​    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {

​     console.log(xhr.responseText);

​    }

   };



   xhr.open('GET', url, true);



   xhr.send(null);



   2.什么是不同域，什么是同域

   **https（协议）://www.imooc.com（域名）:443（端口号）/course/list（路径）**



   **协议、域名、端口号，任何一个不一样，就是不同域**

   与路径无关，路径一不一样无所谓



   不同域

   https://www.imooc.com:443/course/list

   http://www.imooc.com:80/course/list



   http://www.imooc.com:80/course/list

   http://m.imooc.com:80/course/list

   http://imooc.com:80/course/list



   同域

   http://imooc.com:80

   http://imooc.com:80/course/list



   3.跨域请求为什么会被阻止

   阻止跨域请求，其实是**浏览器**本身的一种**安全策略--同源策略**



   **其他客户端或者服务器都不存在跨域被阻止的问题**



   4.跨域解决方案

   ① CORS 跨域资源共享

   ② JSONP



   优先使用 **CORS 跨域资源共享**，如果浏览器不支持 CORS 的话，再使用 JSONP

# CORS 跨域资源共享

1.CORS 是什么

   const url = 'https://www.imooc.com';



   const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

   const xhr = new XMLHttpRequest();



   xhr.onreadystatechange = () => {

​    if (xhr.readyState != 4) return;



​    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {

​     console.log(xhr.responseText);

​    }

   };



   xhr.open('GET', url, true);



   xhr.send(null);



   Access-Control-Allow-Origin: *

   表明允许所有的域名来跨域请求它，* 是通配符，没有任何限制



   只允许**指定域名**的跨域请求

   Access-Control-Allow-Origin: http://127.0.0.1:5500



   2.使用 CORS 跨域的过程

   ① 浏览器发送请求

   ② 后端在响应头中添加 Access-Control-Allow-Origin 头信息

   ③ 浏览器接收到响应

   ④ 如果是同域下的请求，浏览器不会额外做什么，这次前后端通信就圆满完成了

   ⑤ 如果是跨域请求，浏览器会从响应头中查找是否允许跨域访问

   ⑥ 如果允许跨域，通信圆满完成

   ⑦ 如果没找到或不包含想要跨域的域名，就丢弃响应结果



   3.CORS 的兼容性

   **IE10** 及以上版本的浏览器可以正常使用 CORS



   https://caniuse.com/



   JSONP

# JSONP

 1.JSONP 的原理

   script **标签跨域不会被浏览器阻止**

   JSONP 主要就是利用 script 标签，加载跨域文件



   2.使用 JSONP 实现跨域

   服务器端准备好 JSONP 接口

   https://www.imooc.com/api/http/jsonp?callback=handleResponse

手动加载 JSONP 接口

```js
      // 声明函数
      const handleResponse = data => {
        console.log(data);
      };
</script>
<script src="https://www.imooc.com/api/http/jsonp?callback=handleResponse"></script>
<script>
```

动态加载 JSONP 接口

```js
const script = document.createElement('script');//创建节点
      script.src =
        'https://www.imooc.com/api/http/jsonp?callback=handleResponse';
      document.body.appendChild(script);//上树，内部最后一个子节点
```

# XHR 的属性

### 1.responseType 和 response 属性

   const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

   const xhr = new XMLHttpRequest();



   xhr.onreadystatechange = () => {

​    if (xhr.readyState != 4) return;



​    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {

​     // 文本形式的响应内容

​     // responseText 只能在没有设置 responseType 或者 responseType = **'' 或 'text'** 的时候才能使用

​     // console.log('responseText:', xhr.responseText);



**response可以用来替代 responseText**

​     console.log('response:', xhr.response);

​     // console.log(JSON.parse(xhr.responseText));

​    }

   };

   xhr.open('GET', url, true);在send和open之间添加



   // xhr.responseType = '';

   // xhr.responseType = 'text';

   xhr.responseType = 'json';



   xhr.send(null);



   IE6~9 不支持，**IE10 开始支持**



### 2.timeout 属性

   设置请求的超时时间（单位 ms）

   const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

   const xhr = new XMLHttpRequest();



   xhr.onreadystatechange = () => {

​    if (xhr.readyState != 4) return;



​    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {

​     console.log(xhr.response);

​    }

   };



   xhr.open('GET', url, true);



   **xhr.timeout = 10000;**



   xhr.send(null);



   IE6~7 不支持，**IE8 开始支持**

### withCredentials 属性

   指定使用 Ajax 发送请求时是否携带 Cookie



   使用 Ajax 发送请求，默认情况下，**同域时**，会携带 Cookie；跨域时，不会

   xhr.withCredentials = true;

   **最终能否成功跨域携带 Cookie，还要看服务器同不同意**



   const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

   // const url = './index.html';



   const xhr = new XMLHttpRequest();



   xhr.onreadystatechange = () => {

​    if (xhr.readyState != 4) return;



​    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {

​     console.log(xhr.response);

​    }

   };



   xhr.open('GET', url, true);



   xhr.withCredentials = true;



   xhr.send(null);



   IE6~9 不支持，**IE10 开始支持**

# XHR 的方法

### 1.abort()

   **终止当前请求**

   一般配合 abort 事件一起使用

  ```js
  const url = 'https://www.imooc.com/api/http/search/suggest?words=js';
  
        const xhr = new XMLHttpRequest();
  
        xhr.onreadystatechange = () => {
          if (xhr.readyState != 4) return;
  
          if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
            console.log(xhr.response);
          }
        };
  
        xhr.open('GET', url, true);
  
        xhr.send(null);
  //发送请求后立即使用
        xhr.abort();
  ```





###    2.setRequestHeader()

   可以设置请求头信息

设置在send和open中间

告诉后端send数据的格式

   ```js
   xhr.setRequestHeader(头部字段的名称, 头部字段的值);
         const url = 'https://www.imooc.com/api/http/search/suggest?words=js';
         const url = 'https://www.imooc.com/api/http/json/search/suggest?words=js';
   
         const xhr = new XMLHttpRequest();
   
         xhr.onreadystatechange = () => {
           if (xhr.readyState != 4) return;
   
           if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
             console.log(xhr.response);
           }
         };
         xhr.open('POST', url, true);
   // 请求头中的 Content-Type 字段用来告诉服务器，浏览器发送的数据是什么格式的
   //名值对的形式
       xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
   //json格式
         xhr.setRequestHeader('Content-Type', 'application/json');
   
         // xhr.send(null);
         // xhr.send('username=alex&age=18');
         xhr.send(
           JSON.stringify({
             username: 'alex'
           })
         );
   ```

# XHR 的事件

### 1.load 事件

   响应数据**可用**时触发

 ```js
 const url = 'https://www.imooc.com/api/http/search/suggest?words=js';
       const xhr = new XMLHttpRequest();
 
       // xhr.onload = () => {
       //   if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
       //     console.log(xhr.response);
       //   }
       // };
       xhr.addEventListener(
         'load',
         () => {
           if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
             console.log(xhr.response);
           }
         },
         false
       );
 
       xhr.open('GET', url, true);
 
       xhr.send(null);
 ```



   IE6~8 不支持 load 事件和addEventListener同步使用，都是ie9开始兼容的



### 2.error 事件

   **请求**发生错误时触发

```js
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';
      const url = 'https://www.iimooc.com/api/http/search/suggest?words=js';
      const xhr = new XMLHttpRequest();

      xhr.addEventListener(
        'load',
        () => {
          if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
            console.log(xhr.response);
          }
        },
        false
      );
      xhr.addEventListener(
        'error',
        () => {
          console.log('error');
        },
        false
      );

      xhr.open('GET', url, true);

      xhr.send(null);
```

IE10 开始支持



### 3.abort 事件

   调用 abort() 终止请求时触发

```js
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

      const xhr = new XMLHttpRequest();

      xhr.addEventListener(
        'load',
        () => {
          if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
            console.log(xhr.response);
          }
        },
        false
      );
      xhr.addEventListener(
        'abort',
        () => {
          console.log('abort');
        },
        false
      );

      xhr.open('GET', url, true);

      xhr.send(null);

      xhr.abort();
```

   IE10 开始支持



### 4.timeout 事件

   请求超时后触发

```js
const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

      const xhr = new XMLHttpRequest();

      xhr.addEventListener(
        'load',
        () => {
          if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
            console.log(xhr.response);
          }
        },
        false
      );
      xhr.addEventListener(
        'timeout',
        () => {
          console.log('timeout');
        },
        false
      );

      xhr.open('GET', url, true);

      xhr.timeout = 10;

      xhr.send(null);
```



IE8 开始支持

# FormData

### 使用ajax提交表单

输入内容对应name的value也就是填写的值

```js
<form
      id="login"
      action="https://www.imooc.com/api/http/search/suggest?words=js"
      method="POST"
      enctype="multipart/form-data"
    >
      <input type="text" name="username" placeholder="用户名" />
      <input type="password" name="password" placeholder="密码" />
      <input id="submit" type="submit" value="登录" />
    </form>
```

1.使用 Ajax 提交表单



2.FormData 的基本用法

   通过 HTML 表单元素创建 FormData 对象

```js
   const fd = new FormData(表单元素); 
   xhr.send(fd);
```

   通过 append() 方法添加数据

   const fd = new FormData(表单元素);

  // 添加数据

   fd.append('age', 18);

   fd.append('sex', 'male');

   xhr.send(fd);



   IE10 及以上可以支持

```js

      const login = document.getElementById('login');//获取表单节点
      // console.log(login.username);得到两个框的节点
      // console.log(login.password);
      const { username, password } = login;login相当于对象，打点调用可以得到里面的内容
      const btn = document.getElementById('submit');
      const url = 'https://www.imooc.com/api/http/search/suggest?words=js';

      btn.addEventListener(
        'click',
        e => {
          // 阻止表单自动提交
          e.preventDefault();

          // 表单数据验证

          // 发送 Ajax 请求
          const xhr = new XMLHttpRequest();

          xhr.addEventListener(
            'load',
            () => {
              if (
                (xhr.status >= 200 && xhr.status < 300) ||
                xhr.status === 304
              ) {
                console.log(xhr.response);
              }
            },
            false
          );

          xhr.open('POST', url, true);

          // 组装数据
            方式一，自己组装
          // const data = `username=${username.value}&password=${password.value}`;
   // xhr.setRequestHeader(
          //   'Content-Type',
          //   'application/x-www-form-urlencoded'
          // );
          // FormData 可用于发送表单数据
            方法二
          const data = new FormData(login);
           // 添加数据
          // data.append('age', 18);
          // data.append('sex', 'male');
          // console.log(data);
          // for (const item of data) {
          //   console.log(item);
          // }

       

          xhr.send(data);

          // xhr.send('username=alex&password=12345');
        },
        false
      );
```



