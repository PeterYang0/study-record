#### let 和 const

ES6 新增了 let 和 const 来声明变量，主要是解决 var 声明变量所造成的困扰和问题：

```
var不能用于定义常量
var可以重复声明变量
var存在变量提升
var不支持块级作用域
```

而 let 和 const 解决了以上问题，具体操作如下：
1）不可以重复声明变量
2）不存在变量提升

```
console.log(site);
let site = 'itLike'; // site is not defined
```

3）可以定义常量
不能给常量重新赋值，但如果是引用类型的话可以进行修改。

4. 块级作用域
   如果用 var 定义变量，变量是通过函数或者闭包拥有作用域；但，现在用 let 定义变量，不仅仅可以通过函数/闭包隔离，还可以通过块级作用域隔离。
   块级作用域用一组大括号定义一个块，使用 let 定义的变量在大括号的外部是访问不到的，此外，let 声明的变量不会污染全局作用域。

```
{let site = 'itLike';}

console.log(site); //site is not defined
```

#### 解构赋值

用于分解 js 中对象的结构。
1） 用于数组的结构

```
//  普通写法
let nameArr = ['撩课', '小撩', '小煤球'];
let name1 = nameArr[0];
let name2 = nameArr[1];
let name3 = nameArr[2];
//  解构写法
let [name1, name2, name3] = nameArr;
console.log(name1, name2, name3);
```

2）对象的解构

```
//  写法1

let {name, age, sex}

 = {name: '小煤球', age: 1, sex: '公'};

// 结果: 小煤球 1 公

console.log(name, age, sex);

//  写法2： 解构重命名

let {name: lkName, age: lkAge, sex: lkSex}

= {name: '小煤球', age: 1, sex: '公'};

// 结果: 小煤球 1 公

console.log(lkName, lkAge, lkSex);

//  写法3： 可以设置默认值

let {name, age, sex = '公'}

= {name: '小煤球', age: 1};

console.log(sex);  // 公

//  写法4：省略解构

let [, , sex] = ['小煤球', 1, '公 '];

console.log(sex);
```

#### 延展操作符

1）延展数组

```
 let arr1 = [ 'a', 'b', 'c'];
 let arr2 = [1, 2, 3];
 let result = [...arr1, ...arr2];
 console.log(result);
  //  [ "a", "b", "c", 1, 2, 3 ]
```

2）延展对象

```
 let smallDog = {name:'小煤球', age: 1};
 let bigDog = {name: 'Python', age: 2};
 let dog = {...smallDog, ...bigDog};
 console.log(dog);
 // {name: "Python", age: 2}
```

ps: 如果对象中的属性一致, 会被覆盖

ex:

```
 function getMinValue() {
      console.log(Math.min(...arguments));
 }
 getMinValue(1, -99, 22, 10, 9); // -99
```

#### 字符串操作

新增字符串方法

1. startsWith() 判断字符串是否以 XX 开头

```
let url = 'http://www.itlike.com';

console.log(url.startsWith('http'));  // true
```

2. endsWith() 判断字符串是否以 XX 结尾

```
let file = 'index.html';

console.log(file.endsWith('html'));  // true
```

3. includes 判断字符串中是否包含 XX 适用数组

```
let str = 'liaoke';

console.log(str.includes('ao')); // true
```

4. repeat() //复制

```
let title = '标题';

console.log(title.repeat(10));
```

5. padStart() / padEnd()
   padStart()用于头部补全，padEnd()用于尾部补全;第一个参数用来指定字符串的最小长度，第二个参数是用来补全的字符串。

```
//  "2030111111"

let y1 = '2030'.padEnd(10, '1');

//   "2030-11-22"

let y2 = '11-22'.padStart(10, '2030-MM-DD');

console.log(y1, y2);
```

#### 箭头函数

箭头函数简化了函数的的定义方式;一般以 "=>" 操作符左边为输入的参数;而右边则是进行的操作以及返回的值 inputs => output。

```
 ["赵薇", "王菲", "那英"].forEach(
       val => console.log(val)
 );
```

- 箭头函数根本没有自己的 this，所以内部的 this 就是外层代码块的 this。 正是因为它没有 this，从而避免了 this 指向的问题；
- 箭头函数中没有 arguments 对象

#### 数组新增方法

ES6 中在原有数组的方法上又新增了一些好用的方法，比如：forEach、findIndex、find、map、reduce、filter、every、some 等。
1）Array.from()
将一个数组或者类数组变成新数组，是拷贝操作。

```
 const oldArr = [

      '小煤球', 10,

      {df1: '小Python',  df2: '土豆'}

 ];

 const newArr = Array.from(oldArr);
 oldArr[0]=1;
 console.log(newArr, oldArr); // 不受影响
```

2）Array.of()

```
// [ , , , , , , ]
 console.log(Array(8));

  // [8]
 console.log(Array.of(8));
```

3）Array.fill()
用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。语法结构：arr.fill(value[, start[, end]])

```
 let array1 = [1, 2, 3, 4, 5];

 // 用 撩 替换 索引[2,5]中的内容
 console.log(array1.fill('撩', 2, 5));

 // 用 撩 替换 索引[1]中的内容
 console.log(array1.fill('撩', 1));

 // 用 撩 替换 数组中所有内容
  console.log(array1.fill('撩'));
```

#### Object.is 对比两个值是否相等

```
Object.is(1, 1) //true
```

#### 集合操作

1）set
一个 Set 是一堆东西的集合，Set 有点像数组，不过跟数组不一样的是，Set 里面不能有重复的内容；

```
 // 创建一个集合
 let set = new Set(['张三', '李四', '王五', '张三', '李四']);
  console.log(set);
 // 一个属性
  console.log(set.size);
  // 四个方法
 // add
 console.log(set.add('刘德华').add('旋之华'));
 console.log(set);
 // delete
 console.log(set.delete('张三'));
 console.log(set.delete('李四'));
 console.log(set);

 // has
 console.log(set.has('张三'));
 console.log(set.has('张三1'));

 // clear
 console.log(set.clear()); // undefined
  console.log(set);
```

#### Generator

```
 function* showNames() {

     yield '张三';
     yield '李四';

     return '王五';
}

 let show = showNames();

  // {done: false, value: "张三"}

 console.log(show.next());

 // {done: false, value: "李四"}

 console.log(show.next());

 // {done: true, value: "王五"}

 console.log(show.next());

 // {done: true, value: undefined}
  console.log(show.next());
```
