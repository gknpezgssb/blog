```javascript
    ["1","2","3"].map(parseInt)
```
    
这道JS题目，相信大家并不会陌生。也给当初出入JS迷宫的我不小考验，一道题目可以引发许多思考，今天写下的只是今时今日的想法，到未来也许还有别样的看法。

## parseInt ##

得到正确答案，我们先来看看parseInt这个函数

- 名称：parseInt
- 功能：将字符串转化为数字

函数可以接受两个参数，一般来说第一个参数是字符串，第二个参数是该字符串采用的进制数（接受2-36）。

### 如果不传参数？###
```javascript
    parseInt()  //NaN
```
### 传入一个参数 ###
这个时候会默认将字符串为十进制进行解析
```javascript
    parseInt("1111")  //1111
```
不是所有字符串都可以正确解析的，遇到非数字的字符串会解析为NaN
```javascript
    parseInt("Yoda")  //NaN
```
还要说明的一点是，字符串的解析从首位开始，如果是数字就继续直到解析到非数字项
```javascript
    parseInt("2233Yoda")  //2233
```
### 做一些奇怪的事 ###
但是如果传入的不是字符串？
来看下栗子🌰
```javascript
    parseInt(2233)  //2233
```
如果是数字转换为相应字符串
```javascript
    parseInt(null)  //NaN
```
```javascript
    parseInt(undefined)  //NaN
```
```javascript
    parseInt(false)  //NaN
```
```javascript
    parseInt(true)  //NaN
```
布尔值，undefined，null一律返回NaN，其实NaN也返回NaN
```javascript
    parseInt(NaN)  //NaN
```
### 第二参数是第一个参数字符串采用的进制数 ###
```javascript
parseInt("F", 16);
parseInt("17", 8);
parseInt("15", 10);
parseInt(15.99, 10);
parseInt("FXX123", 16);
parseInt("1111", 2);
parseInt("15*3", 10);
parseInt("12", 13);
```
以上都返回15。

关于第二参数有几点点需要说一下。

1.第二参数取值范围是2-36，如果超出范围会返回NaN；
```javascript
    parseInt("2233",37)  //NaN
```
如果传入0，则忽略，和不传效果一样
```javascript
    parseInt("2233",0)  //2233
```
2.如果字符串超出进制的显示范围也会返回NaN；
```javascript
    parseInt("2233",2)  //NaN
```
3.如果传入值不是number类型，会出现一些奇怪的事
```javascript
    parseInt("11","2")  //3  字符串被转换成数字
    parseInt("2233",false)  //2233  布尔型被转换成数字
    parseInt("2233",037)  //61600  看到这个我有点方
    parseInt("2233",true+true+true+true)  //175   看到这个我更方
    var arr = [1,2,3]
    parseInt("2233",arr)
    parseInt("2233","Yoda")  //2233
    
```
第二参数支持基本类型加减转换，还支持字符串或其它位进制值，如果解析不成数字该参数会被忽略。

## map函数 ##
map函数是数组迭代操作中一个常用的方法，可以按照特定的函数来处理数组，返回值是新的数组，原数组不会改变：

```javascript
    var arr = [1,2,3]
    var newArr = a.map(function(para1){return a+2});
    console.log(arr)  // [1,2,3]
    console.log(newArr)  //[3,4,5]
    
```

这里要清楚map可以传入两个参数，第一个是一个回调函数，第二个是this（篇幅有限这里就不展开这块了）。对于回调函数，map可以对其传入三个参数分别是当前元素，元素索引，调用map的数组。

是不是有点晕？！让我们 ~~撸一撸~~ **啊呸！** 捋一捋：

- map函数接受一个回调函数callback
- callback可以传入三个参数（前元素，元素索引，调用map的数组）
- 结合前面说的parseInt，parseInt可以传入两个参数（字符串，进制数）

是不是有一种豁然开朗的赶脚？为什么返回值是[1,NaN,NaN]?

对于数组["1","2","3"],执行["1","2","3"].map(parseInt)时，对于第一个元素"1"她的index为0：
```javascript
parseInt("1",0)   // 1
```
第二参数会直接被忽略从而得到结果，对于第二个元素"2"她的index为1:
```javascript
parseInt("2",1)   // NaN
```
翻看前问即可知道，结果；

同样对于第三个参数"3"，由于二进制值任何一位不可能有3，所以结果也是NaN
```javascript
parseInt("3",2)   // NaN
```

Ps：查阅了网上的许多资料，也看了不少书，加了一些自己的思考这道题就解到这里，如果错误还请指正。

PPs：这题中参数index就是所传入元素的index，可以传入index的还有数组的另一个方法reduce，形如：
```javascript
array.reduce(function(preValue,currentValue,index,array){
    return //.... 
})
```
这里既然可以传入两个值，那么index究竟是谁的index？这个就留到下回继续讨论。