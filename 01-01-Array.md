# Array

## 缘

数组与对象在日常工作中应该是使用较多的数据类型了，他们的各种操作几乎也是前端开发的日常。对于我这种较为时长不带脑子的人来说虽说系统的看过几次，依然不能熟读在心，索性就好好整理一下吧，再不济就拿着这篇记录多看看。

之前的学习风气不太好，没有从根本上了解知识点的习惯，大多是搜索引擎找一下怎么用，copy使用。未曾想过为什么会是这么用，ES是怎么去定义的，直至看了这两本书，在此也讲这两本书推荐给与我一样的小白们。

看书了才知道很多我们很少回去搜索的东西，例如数组最多可以包含4 294 967 295 个元素。

这些知识点大多摘自于我们的红宝书（第四版）再就是好几个小伙伴向我推荐的《你不知道的JavaScript》

### 初识

ECMAScript数组是一组有序的数据，数组中的每个槽位可以存储任务类型的数据。这就意味着可以创建一个数组，它的第一个元素试试字符串，第二个元素是数值，第三个元素是对象。ECMAScript数组也是动态大小的，会随着数据添加而自动增长。

### 面试题

- 类型转换-转字符串-字符串转数组
- js数组和字符串有哪些原生方法,列举一下？数组乱序, 数组扁平化
- es6 怎么判断一个数组？
- 求一个数组最大子项的和，要求这些子项在数组中的位置不是连续的
- 获取arr数组的最后一位并转为大写
- 求数组里面最大连续项的和
- 数组拍平
- 数组洗牌

### 创建数组

#### 构造函数创建

```js
const arr = new Array(3) // [empty × 3]
const names = new Array('Grey') //[ 'Grey']

//也可以省略new，结果事实一样的
const arr = Array(3) // [empty × 3]
const names = Array('Grey') // ['Grey']
```

#### 数组字面量创建

> 与对象一样，在使用数组字面量表示法创建数组时不会调用Array构造函数

```js
const arr = [] // 创建空数组
const names = ['Grey','Shelly','John'] // 创建三个元素的数组
const values = [1,2,] // 创建两个元素的数组
```

#### 静态方法创建（ES6新增）

- from()将类数组结构转换为数组实例

> Array.from()的第一个参数是一个类数组对象，即任何可迭代的结构，或者有一个length属性和可索引元素的结构

```js
// 1.将字符串拆分为数组
Array.from('Matte') // ['M','a','t','t','e']

// 2.将集合和映射转换为一个新的数组
const m = new Map().set(1,2)
                   .set(3,4)
const s = new Set().add(1)
                   .add(2)
Array.from(m) // [[1,2],[3,4]]
Array.from(s) // [1,2]

// 3.对现有数组进行浅复制
const a1 = [1,2,3,4,[5,[6]]]
const a2 = Array.from(a1)

console.log(a1) // [1,2,3,4,[5,[6]]]
console.log(a1 === a2) //false

// 4.可以将arguments 对象转换为数组
function getArr(){
    return Array.from(arguments)
}
getArr(1,2,3,4) // [1,2,3,4]

// 5.将带有必要属性的自定义对象转为数组
const arrLikeObj={
    0:1,
    1:2,
    2:3,
    length:3
}
Array.from(arrLikeObj) // [1,2,3]
```

> Array.from()还可以接收第二个可选的映射函数参数，这个函数可以直接增强新数组的值。还可以接收第三个可选参数，用于指定映射函数中的this的值（不适用于箭头函数）

```js
const a1 = [1,2,3,4]
const a2 = Array.from( a1,x => x**2 )
const a3 = Array.from( a1, function(x) { return x**this.exponent }, { exponent:2 } )
console.log(a2) // [1,4,9,16]
console.log(a3s) // [1,4,9,16]
```

- of()将一组参数转换为数组实例

> Array.of()用来替代ES6之前的Array.prototype.slice.call(arguments)

```js
console.log(Array.of(1,2,3)) //[1,2,3]
```

### 数组空位（hole）

> 一般是通过字面量进行初始化空位数组，ES6将这些空位当成存在的元素，值为undefined。ES6之前会忽略这个空位，但具体的行为也会因方法而异

```js
const options = [,,,,,] // 包含5个空位的数组
const option1 = [1,,,5]

// map会跳过这个空位
conssole.log(options.map(() => 6)) // [6,undefined,undefined,undefined,6]

// join视空位为空字符串
conssole.log(options.join('-') // "1---5"
```

> 由于行为不一致和存在性能隐患，因此实践中要避免使用控组空位。如果确实需要，则可以显式的使用undefined值代替

### 检测数组

- value instanceof Array
instanceof在只有一个全局作用域的情况下是可以满足的，如果有多个框架，涉及两个及以上的全局执行上下文，会有多个不同版本的Array构造函数。如果把数组从一个框架传给另一个框架，则这个数组的构造函数将有别于第二个框架内本地创建的数组。

- Array.isArray(value)(ES6新增)

### 迭代器方法(ES6新增)

在ES6中，Array的原型上暴露了3个用于检索数组内容的方法：keys()返回数组索引的迭代器,values()返回数组元素的迭代器和entries()返回索引/值对的迭代器。

```js
const a = ["foo", "bar", "baz", "qux"];
//返回迭代器，所以可以通过Array.from()转为数组实例
console.log(a.keys()) //Array Iterator {}
console.log(Array.from(a.keys())) // [0,1,2,3]
console.log(Array.from(a.values())) // ["foo", "bar", "baz", "qux"]
console.log(Array.from(a.entries())) // [[0,"foo"], [1,"bar"], [2,"baz"], [3,"qux"]]
```

### 数组复制和填充(ES6新增)

> 这两个方法的函数签名类似，都需要指定既有数组实例上的一个范围，使用这个方法创建的数组不能缩放

- fill()
  - 可以向一个已有的数组中插入全部或部分相同的值。
  - 开始索引用于指定开始填充的位置（可选）。
  - 如果不提供结束索引则一直填充到数组末尾。
  - 负值索引从数组末尾开始计算。
  - 静默忽略抄书数组边界、零长度及方向相反的索引范围
  - 索引部分可用，填充可用部分

```js
  // 1. 没有开始搜索引和结束索引
  const zeroes = [0,0,0,0,0]
  zeroes.fill(5) //[5,5,5,5,5]

  // 2.提供开始索引,从第三个索引开始到结束
  zeroes.fill(6,3) //[0,0,0,6,6]

  // 3.提供开始结束索引,从第一个索引开始到第三个索引结束
  zeroes.fill(7,1,3) //[0,7,7,7,0]

  // 4.提供负的开始结束索引,从后往前第四个开始索引到倒数第三个结束索引
  zeroes.fill(8,-4，-2) //[0,8,8,0,0]

  // 5.索引过低，忽略
  zeroes.fill(9,-10,-6)

  // 6.索引部分可用，填充可用部分
  zeroes.fill(10,3,20) //[0,0,0,10,10]
```

- copyWithin()

  > 按照指定范围浅复制数组中的部分内容，然后将他们插入到指定索引开始的位置。
  >
  > JavaScript引擎在插值前会完整赋值范围内的值，因此赋值期间不存在重写的风险

```js
  //插入到索引5开始的位置,从ints中复制索引0开始的内容，
  let ints = [0,1,2,3,4,5,6,7,8,9]
  ints.copyWithin(5)
  console.log(ints)//(10) [0, 1, 2, 3, 4, 0, 1, 2, 3, 4]
  
  //插入到索引0开始的位置,从ints中复制索引5开始的内容，
  let ints = [0,1,2,3,4,5,6,7,8,9]
  ints.copyWithin(0,5)
  console.log(ints)//(10) [5, 6, 7, 8, 9, 5, 6, 7, 8, 9]
  
  //插入到索引4开始的位置,从ints中复制索引0开始的内容到索引3结束的内容
  let ints = [0,1,2,3,4,5,6,7,8,9]
  ints.copyWithin(4,0,3)
  console.log(ints)(10) //[0, 1, 2, 3, 0, 1, 2, 7, 8, 9]
  
//支持负值索引
  let ints = [0,1,2,3,4,5,6,7,8,9]
ints.copyWithin(-4,-7,-3)
  console.log(ints)(10) //(10) [0, 1, 2, 3, 4, 5, 3, 4, 5, 6]
```

### 数组转换方法

> 所有的对象都有toLocaleString(),toString()和valueOf()方法。

- valueOf()返回的是数组本身
- toString()返回由数组中每个值的等效字符串拼接而成的，一个逗号分隔的字符串。对数组的每个值都会调用其toString()方法，以得到最终的字符串。

```js
let colors = ['red','blue','green']
colors.toString()//"red,blue,green"
colors.toLocaleString()//"red,blue,green"
```

### 数组栈方法

栈是一种先进后出（LIFO，Last-In-First-Out）的结构。数据的插入（推入，push）和删除（弹出，pop）只在栈顶发生

- push（），接收任意数量的参数，并将它们添加到数组末尾，并返回数组最新的长度。

  ```js
  let colors = ["red","green"]
  let count = colors.push("yellow")
  console.log(count)// 3 返回的是数组长度
  ```

- pop（），用于删除数组的最后一项，同时减少数组的length值，返回被删除的项

```js
let colors = ["red","green"]
let item = colors.pop()
console.log(item) //green
```

### 队列方法

队列以先进先出（FIFO，First-In-First-Out）形式限制访问。队列在列表末尾添加数据，但从列表开头获取数据。shift（）删除数组的第一项并返回被删除的项

```js
let colors = ["red","green"]
let item = colors.shift()
console.log(item) // red 
console.log(colors) // ["green"]
```

EcMAScript也为数组提供了unshift（）方法，在数组开头添加任意多个值，然后返回新的长度。

```js
let colors = ["red","green"]
let count = colors.unshift("black")
console.log(count) // 3
```

### 排序方法

- reverse（）反向排列

```js
let values = [1,2,3,4,15]
values.reverse()
console.log(values) //[5,4,3,2,1]
```

- sort（）会按照升序重新排列数组元素，即最小的值在前面，最大的值在后面。为此sort（）会在每一项上调用String（）转型函数，然后比较字符串来决定顺序。即使数组的元素都是数值，也会先把数组转换为字符串再比较、排序。

  sort（）方法可以接收一个比较函数，用于判断哪个值该排在前面。

```js
let values = [1,2,3,4,15]
values.sort()
console.log(values) //[1, 15, 2, 3, 4]
```

```js
let values = [1,2,3,4,15]
function compare(value1, value2){
      return value2 - value1;
}
values.sort(compare)
console.log(values) //[1,2,3,4,15]
```

### 操作方法

- concat（），在现有数组全部元素基础上创建一个新数组。它首先会创建一个当前数组的副本，然后再把他的参数添加到副本末尾，最后返回这个新构建的数组。如果传入一个或多个数组，则concat（）会把这些数组的每一项都添加到结果数组。如果参数不是数组，则直接把他们添加到结果数组末尾。

  >原始数组保持不变

    ```js
    let colors = ["red", "green", "blue"]
    let colors1 = colors.concat("yellow",["white","purple"])
    console.log(colors1)//"red", "green", "blue", "yellow", "white", "purple"]
    ```

    打平数组参数的行为可以重写，方法是在参数数组上指定一个特殊的符号：Symbol.isConcat-Spreadables。这个符号能够阻止concat（）打平参数数组。相反，把这个值设置为true可以强制打平类数组对象。
  
  ```js
  let colors = ["red", "green", "blue"];
      let newColors = ["black", "brown"];
      let moreNewColors = {
        [Symbol.isConcatSpreadable]: true,
        length: 2,
        0: "pink",
        1: "cyan"
      };
      newColors[Symbol.isConcatSpreadable] = false;
  // 强制不打平数组
  let colors2 = colors.concat("yellow", newColors);
  // 强制打平类数组对象
  let colors3 = colors.concat(moreNewColors);
  console.log(colors); // ["red", "green", "blue"]
  console.log(colors2); // ["red", "green", "blue", "yellow", ["black", "brown"]] console.log(colors3); // ["red", "green", "blue", "pink", "cyan"]
  ```

- slice（）用于创建一个包含原有数组中一个或多个元素的新数组。可以接收一个或两个参数：数组的开始索引和结束索引

> 这个操作不影响原始数组

```js
let colors = ["red", "green", "blue", "yellow", "purple"];
let colors2 = colors.slice(1);
let colors3 = colors.slice(1, 4);
console.log(colors2);  // ["green", "blue", "yellow", "purple"]
console.log(colors3);  // ["green", "blue", "yellow"]
```

> 如果slice的参数有负值，那么就以数值长度加上这个负值的结果确定位置。例如：在包含5个元素的数组上调用slice（-2，-1），就相当于调用slice（3，4）。如果结束位置小于开始位置，则返回空数组。

- splice（），主要目的是在数组中间插入元素。但有3种不同的方式使用这个方法：
  - 删除。
  - 插入。
  - 替换。

### 搜索方法

- 严格相等搜索

  - indexOf（），从数组前头（第一项）开始向后搜索，返回要查找的元素在数组中的位置，如果没有找到则返回-1。
  - lastIndexOf（），从数组末尾（最后一项）开始向前是搜索，返回要查找的元素在数组中的位置，如果没有找到则返回-1。
  - includes（）（ES7新增）从数组前头（第一项）开始向后搜索，返回布尔值，表示是否至少找到一个与指定元素匹配的项。在比较第一个参数跟数组每一项时，会使用全等(===)比较，也就是说两项必须严格相等。

  这些方法都接收两个参数：要查找的元素和一个可选的起始搜索位置

  ```js
  let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
  console.log(numbers.indexOf(4));//3
  console.log(numbers.lastIndexOf(4));//3
  console.log(numbers.includes(4));//true
  console.log(numbers.indexOf(4, 4));//5
  console.log(numbers.lastIndexOf(4, 4));//5
  console.log(numbers.includes(4, 7));//false
  let person = { name: "Nicholas" };
  let people = [{ name: "Nicholas" }];
  let morePeople = [person];
  console.log(people.indexOf(person));//-1
  console.log(morePeople.indexOf(person));  // 0
  console.log(people.includes(person));     // false
  console.log(morePeople.includes(person)); // true
  ```

- 断言函数搜索
  
  - find()，数组的最小索引开始，返回 第一个匹配的元素
  - findIndex()，第一个匹配元素的索引
  
  ```js
  const people = [
      {
        name: "Matt",
        age: 27 },
      {
        name: "Nicholas",
        age: 29
      } ];
      alert(people.find((element, index, array) => element.age < 28)); // {name: "Matt", age: 27}
      alert(people.findIndex((element, index, array) => element.age < 28)); // 0
  ```

### 迭代方法

  - every():用于检测数组所有元素是否都符合指定条件。
  
  ```js
  var ages = [32, 33, 16, 40];
  const ageState = ages.every(age=>age >= 18)
  console.log(ages)
  ```

  - filter():检测数值元素，并返回符合条件所有元素的数组。

  ```js
  let arr = [56, 15, 48, 3, 7];
  let newArr = arr.filter(function (value, index, array) {
      return value % 2 === 0;
  });
  console.log(newArr)
  // [56, 48]
  ```

  - forEach():对数组每一项都运行传入的函数，没有返回值。

  ```js
  var arr = [1, 2, 3, 4, 5];
  arr.forEach(function (item) {
      if (item === 3) {
          return;
      }
      console.log(item);
  });
  ```

  - map():对数组每一项都运行传入的函数，返回由每次函数调用的结果构成的数组。

  ```js
  array.map(function(currentValue,index,arr), thisValue)
  ```

  - some():对数组每一项都运行传入的函数，如果有一项函数返回 true，则这个方法返回 true。

  ```js
  function myFunction() {
      document.getElementById("demo").innerHTML = ages.some(v => {return v>document.getElementById("ageToCheck").value});
  }
  ```

### 归并方法

  - reduce()将数组元素计算为一个值（从左到右）。

  ```js
  /**
   * total必需。初始值, 或者计算结束后的返回值。
   * currentValue	必需。当前元素
   * currentIndex	可选。当前元素的索引
   * arr 可选。当前元素所属的数组对象。
   * initialValue 	可选。传递给函数的初始值
  */
  array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
  ```

  > reduce(),在日常工作中用的还是比较多的，面试中也是必考点
  例如：来实现一个find

  ```js
  function find(obj,str){
    const arr = str.split('.')
    return arr.reduce((pre,cur,index)=>{
        return pre?pre[cur]:undefined
    },obj)
  }
  var obj = {a:{b:{c:1}}}
  console.log(find(obj,'a.b.c'))
  ```

  - reduceRight(),将数组元素计算为一个值（从右到左）。