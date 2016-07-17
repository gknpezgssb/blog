每次遇到某个类型陌生的方法的时候，我通常会思考这样几个问题：
* 该方法需要的传入参数:
    1. 不需要传参数的：诸如Array的pop方法；
    2. 需要传参数，能传几个？例如[前文所诉的parseInt方法](https://segmentfault.com/a/1190000005956935);
    3. 可以不传？String的parseInt确实可以不传，但是没什么实际意义，会返回NaN；
    4. 但是某些方法如Number的toString方法，可接受一个参数即进制数，不传则按十进制解析。
* 该方法会改变调用该方法的对象？有返回值？
    1. 如Array的sort方法，会改变一个数组本身的顺序，并且返回一个按要求排列的数组；
    2. 像Array的join方法是不会改变原对象的；
* 方法是否是静态的：
    1. 例如Math的方法都是静态方法
* 最后一点纯属个人恶趣味，我会把一些明显不符合要求的值传入，看看是否出现异常。

#今天的故事从String的三个方法说起#
他们是slice，substring和subset（注意后面这两货都没有一个大写字母）
##方法的日常##

为什么单单说这三个？因为它们长得像，用途也像属于殊途同归的方法（其实Array，String的一些方法真心让人容易混长得像，用途迥异的如slice，splice，splite这三，以后找时间再说说他们仨的故事）

String的slice，substring和subsrt方法都能返回一个子字符串并且不改变原字符串，其中：
+ slice方法：接受两个参数，即起始位置与终点位置；
    ```javascript
"Hi Master Yoda".slice(1,3) //"i "
```
+ substring方法：接受两个参数，即起始位置与终点位置；
    ```javascript
"Hi Master Yoda".substring(1,3) //"i "
```
+ substr方法：接受两个参数，即起始位置与字符串长度；
    ```javascript
"Hi Master Yoda".substr(1,3) //"i M"
```
这样看起来slice和substring并没有区别，实则不然，这个我们闷骚后讨论，先来看看第一个变量起始位置这个概念，第一个参数三者都表示起始位置，需要注意的是：
1. 如果传入参数，则返回值为原字符串（若传入值为0效果相同）；
2. 若传入正数，则返回从该索引处**（包含该值）**到字符串结尾的子字符串
如上例子
3. 如果传入负数slice，与substr效果相同都返回该索引至字符串终点的子字符串，substring会得到与传入零一样的效果
    ```javascript
"May the Force be with you".slice(-10)  
//"e with you"

"May the Force be with you".substring(-10)   
//"May the Force be with you"

"May the Force be with you".substr(-10)   
//"e with you"
```
这样你就不会再认为slice和substring并没有区别，其实他们三个间的差异还挺多的，不过不急咱们先来看看所谓的负索引，你也可以有自己的一套理解不过一下是我觉得比较容易记得住的方式：
![image](https://sfault-image.b0.upaiyun.com/110/386/1103869080-578b6d5d86109)
结果返回数值区间内的字符串（ps：负零只是占位用，实际方法是不接受－0的只会得到和0一样的结果）：
##对每个函数的负数参数做一下说明##
+ slice
    1. slice方法可接受两个为负值的参数:
    ```javascript
"May the Force be with you".slice(-5,-1) //"aste"
 ```   
    2. 但无论有几个参数为负值，第一参数对应字符所在位置必须在第二参数对应字符所在位置的左边，否则返回空字符串（即不能逆向）：
    ```javascript
"May the Force be with you".slice(-1,-5)
"May the Force be with you".slice(-1,1)
"May the Force be with you".slice(4,1)
    ```
以上均返回 ""
+ substring

    1. substring方法对任何双负数返回""
    ```javascript
"May the Force be with you".substring(-1,-4)  //""
    ```
这里的任何至一切顺序的双负数都讲返回""；
    2. substring只返回数值区间内的字符串，没有slice中的逆向问题
    ```javascript
"May the Force be with you".substring(5,1)//"ay t"
    ```
    3. 一下返回相同结果 "May t"
    ```javascript
"May the Force be with you".substring(-5,5)
"May the Force be with you".substring(5,-5)
    ``` 
这里可以理解成出现负数时会被自动转成0，该理解对双负数同样适用。具体标准请参考ecma文档[15.5.4.10](http://www.ecma-international.org/publications/files/ECMA-ST-ARCH/ECMA-262,%201st%20edition,%20June%201997.pdf)
+ substring
    1. 从语意上理解第二参数不应传入负值，
   所以只要第二参数为负值，不论第一参数是什么，均返回""
    ```javascript
"May the Force be with you".substr(-5,-5)
"May the Force be with you".substr(0,-5)
"May the Force be with you".substr(5,-5)
    ```
+ 总结：
    1. slice接受双负数；
    2. substring不接受双负数（返回""），单负数会转换成0再解析；
    3. subset不接受双负数（返回""），单负数只能是第一个参数，否则返回""
##一些补充##
+ 当输入的索引数超出字符串长度时，会被解析为所能到达的最大值,长度亦是如此：
    ```javascript
"Master".slice(3,1024) //ter
"Master".substring(-1024,3)  //Mas
"Master".substr(3,1024)  //ter
```