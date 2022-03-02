# Class 是什么

具体实例：对象，实例对象

### 1.认识 Class

人类：类

具体的人：实例、对象

**类可以看做是对象的模板，用一个类可以创建出许多不同的对象**

### 2.Class 的基本用法

**类名一般大写**

实例化时执行构造方法，所以必须有构造方法，但可以不写出来

```js
class Person {
	//实例化时执行构造方法，所以必须有构造方法，但可以不写出来
            constructor(name, sex) {
                //构造函数方法
                this.name = name;
                this.sex = sex;
                //实例化会占用内存，每个实例都会产生该方法，一般不放在这里
                this.speak = () => { }
            }
            speak() {
                console.log('111');
            }
        }
// 使用
        const zs = new Person('zs', '男')
        console.log(zs.sex);
        console.log(zs.name);
        zs.speak()
```

### 构造函数的区别

```js
  // 构造函数
        function Pr(name, age) {
            this.name = name;
            this.age = age
        }
        Pr.prototype.speak = function () {
            console.log('speak');
        }
        const ls = new Pr('ls', 40)
        console.log(ls.name);
        ls.speak()
```

# Class 的两种定义形式

### 1.声明形式

```js
 class Person{
            constructor(name,age){
                this.name=name;
                this.age=age
            }
            speak(){
                
            }
        }
```

### 2.表达式形式

```js
//构造函数形式
        function Person() { }
        const Person = function () { }
 // 表达式形式
	const Person = class {
            constructor() { }
            speak() { }
        }

```

### 3.立即执行的匿名类

```js
(function () {
      //   console.log('func');
      // })();
      // func()

      // 立即执行的匿名类
      new (class {
        constructor() {
          console.log('constructor');
        }
      })();

```

# 实例属性、静态方法和静态属性

### 1.实例属性

方法就是值为函数的特殊属性

```js
class Person {
            //默认的属性和方法
            name = '小明';
            age = 18;
            speak = function () {
                console.log(111);
            }
            constructor() {
                // 实例属性
            }
        }
        const p = new Person()
        console.log(p.age);
        p.speak()
```

### 2.静态方法

   // 类的方法

```js
class Person {
            //默认的属性和方法
            name = '小明';
            age = 18;
            speak = function () {
                console.log(111);
                console.log(this);//指向实例对象
            }
    //静态方法
            static sing = function () {
                console.log('唱歌');
                console.log(this);//指向class
            }
            constructor() {
                // 实例属性
            }
            //使用方法添加类的属性
    //静态属性
            static getSex = function () {
                return 'nan'
            }
        }
        const p = new Person()
        Person.sing()
        p.speak()
        console.log(Person.sing);
        console.log(Person.getSex());

```

# 。。私有属性和方法

### 1.为什么需要私有属性和方法

  **一般情况下，类的属性和方法都是公开的**

  **公有的属性和方法可以被外界修改，造成意想不到的错误**

```js
class Person {
        constructor(name) {
          this.name = name;
        }

        speak() {
          console.log('speak');
        }
	//属性改成方法，让修改变得困难
        getName() {
          return this.name;
        }
      }
      const p = new Person('Alex');
      console.log(p.name);
      p.speak();

      // ....
	//误修改了
      p.name = 'zs';
      console.log(p.name);
```

### 2.模拟私有属性和方法

  **2.1._ 开头表示私有**

```js
class Person {
        constructor(name) {
          this._name = name;
        }

        speak() {
          console.log('speak');
        }

        getName() {
          return this._name;
        }
      }
      const p = new Person('Alex');
      // console.log(p.name);
	//修改为带下划线前缀的名称，再次修改不会影响，增加了修改的难度
      p.name = 'zd';
      console.log(p.getName());
```

### 2.2.将私有属性和方法移出类

```js
(function () {
    //name的作用域是这个函数
        let name = '';
	
        class Person {
          constructor(username) {
            // this.name = name;
              name=username;
              {
              //name是函数私有的,只能通过实例对象调用
          speak() {
            console.log('speak');
          }
	//自己做接口
          getName() {
            return name;
          }
        }
	//Person转全局作用域
        window.Person = Person;
      })();
//只有Person是全局作用域
      (function () {
        const p = new Person('Alex');
        console.log(p.name);//undefined
        console.log(p.getName());//Alex
      })();

```

# extends

### 1.子类继承父类

```js
class Person {
            constructor(name, age) {
                this.name = name;
                this.age = age;
                this.say = function () {
                    console.log('say');
                }
            }
            speak() {
                console.log('speak');
            }
            static sing = function () {
                console.log('sing');
            }
        }
        Person.verson = 1.0
//继承
        class Programmer extends Person {
            constructor(name, age) {
                super(name, age)
            }
        }
        const xz = new Programmer('xz', 18)
        console.log(xz.name);
        console.log(xz.age);
        xz.say()
        xz.speak()
        Programmer.sing()
        console.log(Programmer.verson);
```

### 2.同名覆盖



```js
class Person {
            constructor(name, age) {
                this.name = name;
                this.age = age;
                this.say = function () {
                    console.log('say');
                }
            }
            speak() {
                console.log('speak');
            }
            static sing = function () {
                console.log('sing');
            }
        }
        Person.verson = 1.0
        class Programmer extends Person {
            constructor(name, age, gz) {
                // this一定要在super后面
                super(name, age)
                this.say = function () {
                    console.log('pg say');
                }
                this.gz = 'gao'
            }
            speak() {
                console.log('pg speak');
            }
            hi() {
                console.log('hi');
            }
            static sing = function () {
                console.log('pg sing');
            }
        }
        Programmer.verson = 2.0
        const xz = new Programmer('xz', 18)
        console.log(xz.name);
        console.log(xz.age);
        console.log(xz.gz);
        xz.say()
        xz.speak()
        xz.hi()
        Programmer.sing()
        console.log(Programmer.verson);
```

# super

**super指带父类，指向子类，承上启下**

**super作为函数调用，指代父类的构造方法，this指向子类实例**
**super作为对象使用，**
**1.在static静态函数中使用**
**super指代父类，this指向子类**
**2.在子类方法中使用，super指代父类原型，指向子类实例对象**

### 1.作为函数调用

代表父类的构造方法，只能用在子类的构造方法中，用在其他地方就会报错

super 虽然代表了父类的构造方法，但是内部的 this 指向子类的实例

```js
class Person {
            constructor(name) {
                this.name = name
                console.log(this);// 指向实例p1
            }
            speak() {
                console.log(this + 2);
            }
        }
        class Programmer extends Person {
            constructor(name) {
                super(name)
                console.log(this + 3);
            }
            speak() {
                super.speak()
            }
        }
        const p1 = new Programmer('zs')
        // console.log(p1.name);
        p1.speak()
```

### 2.作为对象使用

   2.1.在构造方法中使用或一般方法中使用

   super 代表父类的原型对象 Person.prototype

   所以定义在父类实例上的方法或属性，是无法通过 super 调用的

   通过 super 调用父类的方法时，方法内部的 this 指向当前的子类实例

```js
class Person {
        constructor(name) {
          this.name = name;

          console.log(this);
        }
	
        speak() {
          console.log('speak');
          // console.log(this);
        }

        static speak() {
          console.log('Person speak');
          console.log(this);
        }
      }
      class Programmer extends Person {
        constructor(name, sex) {
            //super代表父类的构造方法
            //在super中this指向子类的实例
          super(name, sex);

          // console.log(super.name);
            //super作为对象使用，代表了父类的原型对象
            //调用父类的方法,在写子类的方法
          // super.speak();
        }
	//只能在父类的构造函数中使用,在其他地方使用会报错
        // hi() {
        //   super(); // ×
        // }

        speak() {
          super.speak();
          console.log('Programmer speak');
        }
       
```

### 2.2.在静态方法中使用

  指向父类，而不是父类的原型对象
   通过 super 调用父类的方法时，方法内部的 this 指向当前的子类，而不是子类的实例

```js
 class Person {

            constructor(name) {
                this.name = name
                console.log(this);// 指向实例p1
            }
            speak() {//父类的方法
                console.log('speak');
                console.log(this);//子类的实例对象
            }
            static speak() {
                console.log('Person speak');
                console.log(this);//子类
            }
        }
        class Programmer extends Person {
            constructor(name) {
                super(name)
                // console.log(this + 3);
            }
            speak() {
                super.speak()//speak可以调用父类的方法
                console.log('pg speak');//pg speak
            }
            //在静态方法中使用
            static speak() {
                super.speak();//Person speak
                console.log('Programmer speak');
            }
        }
        const p1 = new Programmer('zs')
        // console.log(p1.name);
        p1.speak()
        Programmer.speak()
```

### 3.注意事项

   使用 super 的时候，必须显式指定是作为函数还是作为对象使用，否则会报错

```js
class Person {
        constructor(name) {
          this.name = name;
        }

        speak() {
          console.log('speak');
        }
      }
      class Programmer extends Person {
        constructor(name, sex) {
          super(name, sex);

          // console.log(super);
          // console.log(super());
          // console.log(super.speak);
        }
      }
```

当子类继承父类时，如果不需要通过constructor设置属性或者继承父类constructor中的属性，那么就可以不写constructor和super，否则写了constructor就必须加上super
