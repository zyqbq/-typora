# 认识 Promise

## Promise 是异步操作的一种解决方案

```js
// 回调函数
 document.addEventListener(
   'click',
   () => {
     console.log('这里是异步的');
   },
   false
 );
 console.log('这里是同步的');
```

### 2.什么时候使用 Promise

**Promise 一般用来解决层层嵌套的回调函数（回调地狱 callback hell）的问题**

```css
<style>
  * {
    padding: 0;
    margin: 0;
  }

  #box {
    width: 300px;
    height: 300px;
    background-color: red;
    transition: all 0.5s;
  }
</style>
```



```js
 // 运动
      const move = (el, { x = 0, y = 0 } = {}, end = () => {}) => {
        el.style.transform = `translate3d(${x}px, ${y}px, 0)`;

        el.addEventListener(
          'transitionend',
          () => {
            // console.log('end');
            end();
          },
          false
        );
      };
      const boxEl = document.getElementById('box');

      document.addEventListener(
        'click',
        () => {
          move(boxEl, { x: 150 }, () => {
            move(boxEl, { x: 150, y: 150 }, () => {
              move(boxEl, { y: 150 }, () => {
                // console.log('object');
                move(boxEl, { x: 0, y: 0 });
              });
            });
          });
        },
        false
      );
```

# 1.实例化构造函数生成实例对象

## Promise 解决的不是回调函数，而是回调地狱

### 基本写法

```js
const p=new Promise(()=>{})
```

###  2.Promise 的状态

**Promise 有 3 种状态，一开始是 pending（未完成），**

**执行 resolve，变成fulfilled(resolved)，已成功**

**执行 reject，变成 rejected，已失败**

**Promise 的状态一旦变化，就不会再改变了**

```js
 const p = new Promise((resolve, reject) => {
            // resolve();//fulfilled
            reject()//undefined
        })
        console.log(p);
```

### 3.then 方法

```js
p.then(() => {
        console.log('成功');//成功会执行前面的
    },
        () => {
            console.log('失败');//失败会执行后面的
        })
```

### 4.resolve 和 reject 函数的参数

**redolve对应第一个参数，reject对应第二个参数**

```js
const p = new Promise((resolve, reject) => {
            resolve({ name: 'xiaoming' });
            // reject()
        })
        console.log(p);
        p.then((data) => {
            console.log('成功', data);//成功 {name: 'xiaoming'}
        },
            () => {
                console.log('失败');
            })
```

# then()

### 1.什么时候执行

**pending->fulfilled 时，执行 then 的第一个回调函数**

**pending->rejected 时，执行 then 的第二个回调函数**

### 2.执行后的返回值

**then 方法执行后返回一个新的 Promise 对象**

```js
const p = new Promise((resolve, reject) => {
        resolve('111');
    })
    p.then((data) => {
        console.log('成功', data);
        // return 222//成功的情况可以直接简写
        // return new Promise((resolve, reject) => {
        //     reject(333);//失败需要写全
        // })
        return undefined;//相当于不传值
       / 不返回值默认返回undefined
    }, () => {
        console.log('失败');
    }).then((data) => {
        console.log('chenggong', data);
    }, (data) => {
        console.log('shibai', data);
    })
    console.log(p);
```

# catch()

**catch 专门用来处理 rejected状态 catch 本质上是 then 的特例**

```js
new Promise((resolve, reject) => {
            reject('111')
        }).catch((data) => {
            console.log('shibai', data);
        })
//catch相当于
        new Promise((resolve, reject) => {
            reject('111')
        }).then(null, (data) => {
            console.log('shibai', data);
        })
```

### 2.基本用法

```js
new Promise((resolve, reject) => {
            reject('111')
        }).then(data => {
            console.log(data);
            return '2222'
        }).catch(err => {
            console.log(err);
            //专门用来生成错误的throw
            throw new Error('reson')
        }).then(data => {
            console.log(data);
        }).catch(err => {
            console.log(err);
        })
```

**catch() 可以捕获它前面的错误**

 **一般总是建议，Promise 对象后面要跟 catch 方法，这样可以处理 Promise 内部发生的错误**

# finally()

**相当于没写，起承接作用**

finally() 本质上是 then() 的特例

```js
new Promise((resolve, reject) => {
            // resolve('222')
            reject('111')
        }).finally()
            .then(result => {
                console.log(result);
            })
            .catch(err => {
                console.log(err);
            })
        // 相当于
        new Promise((resolve, reject) => {
            // resolve('222')
            reject('111')
        }).then(result => {
            return result
        }, err => {
            return new Promise((resolve, reject) => {
                reject(err)
            })
        }).then(result => {
            console.log(result);
        })
            .catch(err => {
                console.log(err);
            })
```

# Promise.resolve()

### 是成功状态 Promise 的一种简写形式

```js
const p = new Promise((resolve, reject) => {
    resolve(111)
})
//相当于
p = Promise.resolve(111)
```

### Promise

**当 Promise.resolve() 接收的是 Promise 时，直接运行这个Promise**

```js
const p1 = new Promise(resolve => {
    setTimeout(() => {
        resolve(111)
    }, 1000)
})
Promise.resolve(p1).then(result => {
    console.log(result);//111
})
console.log(Promise.resolve(p1) === p1);//true
```

### 当 resolve 函数接收的值是 Promise 时，后面的 then 会根据传递的 这个Promise的状态变化决定执行哪一个回调，与上方一样

```js
const p1 = new Promise(resolve => {
    setTimeout(() => {
        resolve(111)
    }, 1000)
})
new Promise(resolve => {
    resolve(p1)
}).then(result => {
    console.log(result);
})
```

### 具有 then 方法的对象

调用对象中的then方法，后面的then不再调用，因为这个promise为pending状态，在对象的then中加入形式参数，才能确定后面的状态

```js
const thenObj = {
    then() {
        console.log(333);
    }
}
Promise.resolve(thenObj).then(data => {
    console.log(444);
}, err => {
    console.log(555);
})
console.log(Promise.resolve(thenObj));//Promise {<pending>}
```

# Promise.reject()

**失败状态 Promise 的一种简写形式**

```js
const p = new Promise((resolve, reject) => {
    reject(111);
})
//相当于
Promise.reject(111)
```

### 不管什么参数，都会原封不动地向后传递，作为后续方法的参数

**p1=err**

```js
const p1 = new Promise(resolve => {
    setTimeout(() => {
        resolve(111)
    }, 1000)
})
Promise.reject(p1).catch(err => {
    console.log(err);
    console.log(err === p1);
})
```

# Promise.all()

### 1.有什么用

**Promise.all() 关注多个 Promise 对象的状态变化**

**传入多个 Promise 实例，包装成一个新的 Promise 实例返回**

### 2.基本用法

**如果两个都成功，那么最终执行成功的**

**如果一个失败,不论另外一个是成功还是失败，失败执行后立马执行最终的失败**

```js
const delay = ms => {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, ms)
    })
}
const p1 = delay(3000).then(() => {
    console.log('p1已经完成')
    return 111
})
const p2 = delay(2000).then(() => {
    console.log('p2已经完成')
    // return 222
    return Promise.reject(222)
})
const p = Promise.all([p1, p2])
p.then(
    data => {
        console.log(data);
    },
    err => {
        console.log(err)
    }
)
```

# Promise.race() 和 Promise.allSettled()

## Promise.race()

### 第一个完成的决定接下来是运行什么，后运行的对他没有影响

```js
const delay = ms => {
            return new Promise(
                resolve => {
                    setTimeout(resolve, ms);
                }
            )
        }
        const p1 = delay(2000).then(() => {
            console.log('p1,完成了');
            return Promise.reject(555)
            // return 111
        })
        const p2 = delay(1000).then(() => {
            console.log('p2,完成了');
            // return Promise.reject(555)
            return 222
        })
        const p_rece = Promise.race([p1, p2])
        p_rece.then(
            data => {
                console.log(data, 'succ');
            },
            err => {
                console.log(err, 'shibai');
            }
        )
```

## Promise.allSettled()

### 前面的完成情况不影响后面的执行，只是返回前面的完成情况，并且只执行成功的情况

```js
const delay = ms => {
    return new Promise(
        resolve => {
            setTimeout(resolve, ms);
        }
    )
}
const p1 = delay(2000).then(() => {
    console.log('p1,完成了');
    return Promise.reject(555)
    // return 111
})
const p2 = delay(1000).then(() => {
    console.log('p2,完成了');
    // return Promise.reject(555)
    return 222
})
const p_rece = Promise.allSettled([p1, p2])
p_rece.then(
    data => {
        console.log(data, 'succ');
    },
    err => {
        console.log(err, 'shibai');
    }
)
```

# 扩展Promise.any()

### 只要有成功的传入，最终就执行成功的情况



```js
// 失败
const p1 = new Promise((resolve, reject) => {
    reject()
});
// 失败
const p2 = new Promise((resolve, reject) => {
    reject()
});
// 成功
const p3 = new Promise(resolve => {
    resolve()
});
const res = Promise.any([p1, p3, p2])
console.log(res) // 返回成功状态的Promise

```

### 返回是第一个成功的值，后面就不管了，只执行第一个完成的情况，后续的会终止运行

```js
const p1 = new Promise((resolve, reject) => {
    reject("失败");
});

const p2 = new Promise((resolve, reject) => {
    setTimeout(resolve, 500, "最后完成");
});

const p3 = new Promise((resolve, reject) => {
    setTimeout(resolve, 100, "第一个完成");
});

const res = Promise.any([p1, p2, p3])
res.then((value) => {
    console.log(value);
})
```

# Promise 的注意事项

**1.推荐在调用 resolve 或 reject 函数的时候加上 return，不再执行它们后面的代码**

```js
 const p = new Promise((resolve, reject) => {
     return resolve(111)
     console.log('执行PROMISE');
 })
```

### 2.Promise.all/race/allSettled 的参数问题

**参数如果不是 Promise 数组，会将不是 Promise 的数组元素转变成 Promise 对象**



```js
Promise.all([1, 2, 3])
//等价于
Promise.all[
    Promise.resolve(1),
    Promise.resolve(2),
    Promise.resolve(3)
]
//并且默认是成功状态的传值
// 不只是数组，任何可遍历的都可以作为参数
// 数组、字符串、Set、Map、NodeList、arguments
Promise.all(new Set([1,2,3]))
```

### 3.Promise.all/race/allSettled 的错误处理

**错误既可以单独处理，也可以统一处理**

**一旦被处理，就不会在其他地方再处理一遍**

```js
const delay = ms => {
    return new Promise(resolve => {
        setTimeout(resolve, ms);
    });
};
const p1 = delay(1000).then(() => {
    console.log('p1完成了');
    return 111
    // return Promise.reject(222)
})
const p2 = delay(2000).then(() => {
    console.log('p2完成了');
    // return 222
    return Promise.reject(222)
})
// .catch(err => {
//     console.log(err);
// })
Promise.all([p1, p2]).then(
    data => {
        console.log(data);
    }
).catch(err => {
    console.log(err);
})
```

# promise应用

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8" />
    <title>Promise 的应用</title>
    <style>
        #img {
            width: 80%;
            padding: 10%;
        }
    </style>
</head>

<body>
    <img src='https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fpic.jj20.com%2Fup%2Fallimg%2F1114%2F0G320103935%2F200G3103935-1-1200.jpg&refer=http%3A%2F%2Fpic.jj20.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1643646551&t=e1b2b7114cedae8908b2f719a230604d'
        alt="" id="img" />

    <script>

        // 异步加载图片
        //url作为参数传入大的函数中
        //大函数运行会生成一个promise实例对象
        //new Image()对象的用法
        // 创建一个Image对象：var a=new Image();
        // 定义Image对象的src:  a.src=”xxx.gif”;
        // 这样做就相当于给浏览器缓存了一张图片
        //这里处理的是第一张图片的加载情况
        const loadImgAsync = url => {
            return new Promise((resolve, reject) => {
                // 相当于给浏览器缓存了一张图片
                const img = new Image();
                //图片缓存完成，onload事件才能触发，加载完成，执行成功，向then中传入img缓存
                img.onload = () => {
                    resolve(img);
                };
                //图片缓存失败，执行onerror，向catch中传入不能导入图片+图片链接
                img.onerror = () => {
                    reject(new Error(`Could not load image at ${url}`));
                };
                //图片的src要写在onload后面,表示缓存地址
                img.src = url;
            });
        };
        // 第一张图片处理完，传入第二张图片成功就替换图片，
        const imgDOM = document.getElementById('img');
        // 第二张图片链接传入函数中
        loadImgAsync('https://t7.baidu.com/it/u=1819248061,230866778&fm=193&f=GIF')
            // 图片缓存成功会替换第一张图片
            .then(img => {
                console.log(img);
                // 并且在1s后执行替换图片
                setTimeout(() => {
                    imgDOM.src = img.src;
                }, 1000);
            })
            // 如果第二张图片缓存失败
            // 缓存图片的链接
            .catch(err => {
                console.log(err);
            });
    </script>
</body>

</html>
```

# 扩展async和await

**ansyc函数返回一个Promise对象，可以继续使用then方法。当函数执行的时候，一旦遇到await就会先返回，等到触发的异步操作完成，在继续执行函数体后面的**

**await后面是Promise对象，会等待Promise返回结果再继续执行后面的代码**

```js
const timeout= time => {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            resolve();
        }, time);
    })
};
const start = async () => {
    // 在这里使用起来就像同步代码那样直观
    //先执行adync中的
    console.log('start');
    await timeout(3000);//3秒后执行
    console.log('end');
    return "imooc"
};
start().then(()=> {
    console.log('test....')
});

```

### 3.导入导出的复合写法

   复合写法导出的，无法在当前模块中使用

```js
export { age } from './module.js';
      console.log(age);//不可

      // 等价于
      import { age } from './module.js';
      export { age } from './module.js';
      console.log(age);//可用
```

