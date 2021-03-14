---
title: JS中的常用套路
image: /image/6.jpg  #设置本地图片
---
### 一、接收图片资源
```javascript
if(result.resultid == 0){
    var a = result && result.name && result.data.imgurl || 'sky.jpg'
}
```
    
当前端调用一个接口后，返回一个result对象，我们用变量a来接受result里的图片资源，如果，返回的结果中没有`result.data.imgurl`这个字段，就用'sky.jpg'作为兜底图片

### 二、短路运算的巧妙之处
```javascript
const a1 = 'pingchangxint';
//第一个值是true，会接着执行后面的内容
a1 && alert('是否出现a1');
//会弹出alert框

const a2 = undefined;
//第一个值是false，不会继续执行后面的内容
a2 && alert('是否出现a2');
//不会弹出alert框
```

js中的`&&`属于**短路**的与，如果第一个值是false，则不会执行后面的内容。如果第一个值是true，则继续执行第二条语句，并返回第二个值。

同理：`||`属于**短路**的或，如果第一个值是true，则不会执行后面的内容。如果第一个值是false，则会继续执行第二条语句，并返回第二个值。

### 三、比较字符串（大坑）
这是一种特殊情况：当符号两侧都是字符串时，**不会**转换成数字进行比较，而是比较**Unicode编码** 比如当你比较`"1234"`和`"567"`这两个字符串时，你会发现`"567"`更大（因为5比1大）

### 四、对象的属性值
常见的属性值这里就不说了，主要说一下，对象的属性值可以是一个函数。  
```javascript
var obj = new Object();
obj.name = function () {
    console.log('pingchangxin-t');
}
```
也可以是一个对象.
```javascript
var obj1 = new Object();
obj1.testname = undefined;
var obj2 = new Object();
obj2.name = "pingchangxin-t"
//将整个obj2对象，作为obj1的属性
obj1.testname = obj2

console.log(obj1.testname.name);//打印结果：pingchangxin-t
```
### 五、对象之间传值和传址的区别
这里要说明一点，对象的值是保存在**堆内存**中，而对象的引用（即变量）是保存在**栈内存**中的。    
js代码 ：

```javascript
var obj1 = new Object();
obj1.name = "李白";
var obj2 = obj1; // 将 obj1 的地址赋值给 obj2。从此， obj1 和 obj2 指向了同一个堆内存空间
obj2.name = "韩信";//修改obj2的name属性
console.log(obj1.name);//打印结果：韩信
```
所以如果你打算把引用类型 A 的值赋值给 B，让A和B相互不受影响的话，可以通过 Object.assign() 来复制对象,比如用下面的方法：
```javascript
var obj1 = {name: '韩信'};// 复制对象：把 obj1 赋值给 obj3。两者之间互不影响
var obj3 = Object.assign({}, obj1);
```
### 六、split()方法
解释：通过指定的分隔符，将一个字符串拆分成一个**数组**。不会改变原字符串。
```javascript
//split()方法：字符串变数组
var str3 = '平常心|pingchangxin|周杰伦';
console.log(str3.split()); // ["平常心|pingchangxin|周杰伦"]
console.log(str3.split('')); // ["平","常","心","|","p","i","n","g","c","h","a","n","g","x","i","n","|","周","杰","伦"]
console.log(str3.split('|')); // ["平常心","pingchangxin","周杰伦"]
console.log(str3.split('周')); // ["平常心|pingchangxin|","杰伦"]
```
### 七、生成两个整数之间的随机整数，并且要包含这两个整数

```javascript
function getRandom(min, max) {
        return Math.floor(Math.random() * (max - min + 1)) + min;
    }
    const arr = ['周杰伦', '陈奕迅', '隔壁老樊', '林俊杰'];
    const index = getRandom(0, arr.length - 1); // 生成随机的index
    console.log(arr[index]); // 随机点名
```
