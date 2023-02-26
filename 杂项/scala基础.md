---
title: 第1章--Scala 函数式编程
date: 2021/5/4 18:10:45
categories:
- [scala]
tags:
- [大数据]
- [scala]
- [笔记]
---

# Scala 函数式编程

# 注释

- 注释和Java一样

```scala
//单行注释
/*
多行注释
*/
/**
  * 文档注释 
  */
```

# 变量声明

```scala
//命名规范
/*
1. 驼峰命名,变量函数方法驼峰命名
2. 类名大写
*/
val str = "scala" //自动推断类型,行尾不需要加分号
var num = 123 //自动推断类型
//var 和 val 区别: var修饰的变量引用是可变的,val修饰的变量引用不可变,即指针的指向不可变
//实际开发过程中推荐使用val,引用不可变,var常用于值经常改变的地方
val num: Int = 123 //自定义类型
var (num1,num2,num3) = (1,2,3) //元组 定义多变量
val num1,num2 =10 //赋值为同一个值
var Array(x,y,z) = Array(1,2,3) //x=1,y=2,z=3

//字符串的拼接
val name = "a"
val age = 0
//方法一
val info = "name"+name+"age="+age
//方法二
val info = s"name= $name age= $age"
val info = s"name=${name}age=${age}"

//字符串的切分
val str="key1=a,key2=b,key3=c" //str.split(",")不推荐
var str=
      """
        |key1=a
        |key2=b
        |key3=c
        |""".stripMargin //语法糖--(key,value)键值对

```

**数据类型**

![数据类型](https://images-1300732204.cos.ap-chengdu.myqcloud.com/MarkDown/Snipaste_2021-05-04_23-57-04.png)

> 一切类的基类是Any类型
>
> AnyRef: 引用类型的基类
>
> AnyVal: 值类型的基类 -- 10个子类,Unit:代表void无返回值类型 def func():Unit = {}

# 操作符

`+ - * / % > < >= <= == && || !` 

**操作符是方法的重载: `2+3 == 2.+3`**

<b style="color:red">scala中无`++ --`运算符</b>

# 表达式

```scala
val y = if(x>0) 1 else 0
val y = if(x>0) 1 else if (x<0) 0 else "error"//混合表达式
```

<p style="color:blue">
val res = {
    //代码块
    }</p>

# Scala的输入输出

`scala.io.StdIn.readLine` 读取行

`scala.io.StdIn.readInt` 读取int

# 循环

for(i <-表达式/集合/range) 和 while

> 条件表达式
>
> i to j  包含i和j,等价于`i.to(j)`
>
> i until j 包含i但不包含j
>
> ```scala
> for(item <- i to j){//item为循环的元素,i和j为循环条件}
> 
> val arr = Array(1,2,3,4)
> for(item <- arr){//item为数组元素}
> 
> //嵌套for循环
>     for(i<-1 to 3;j<-1 to 3 if i!=j){/*函数体*/}
>     //等价于
>     for(i<-1 to 3){
>         for(j<-1 to 3){
>             if(i!=j){
>                 //函数体
>             }
>         }
>     }
>     
> //数组循环
> arr.map(item=>{/*函数体*/}) //返回值为新的数组
> arr.foreach(item=>{/*函数体*/}) //无返回值
> arr.filter(item=>{/*函数体*/})
> ```
>
> 终止循环: 
>
> ```scala
> import util.control.Breaks._
> //break
> def func(): Unit {
>     breakable( //参数为函数,包含了一个循环体,参数放于try中
>     	for(i<-1 to 3){
>             if(i==2)
>             break() //这里throw一个错误,catch去捕获用于跳出循环
>         }
>     )
> }
> //continue
> def func(): Unit {
>     breakable(
>     	for(i<-1 to 3){
>             if(i==2)
>             continue()
>         }
>     )
> }
> ```

# 声明函数和方法

```scala
def 方法名(参数1:类型,参数2:类型...):返回值 = {/*方法体*/}

def func():Unit = {} //无返回值
def func() = {} //返回值,可以进行类型推断,当方法为递归方法时必须加返回值

def func(x:Int,y:Int):Int = {/*statement*/}
```

## 匿名函数

```scala
val f1 = () => {}
val f2 :Int => Int x => x*x //简写
arr.map(f1(_)) //传参1
arr.map(f1) //传参2,自动
```

## 方法转函数

```scala
val f1 = m1 _
```

## 返回值

`scala中讲最后一行函数或方法的代码作为返回值,最后一行不能时赋值语句`

# 集合

```scala
scala.collection.immutable //默认不可变集合可并发访问
scala.collection.mutable 
```

![image-20210514205305208](https://images-1300732204.cos.ap-chengdu.myqcloud.com/image-20210514205305208.png)

## 数组

### 定长数组

```scala
//默认定长数组
val arr = Array(1,2,3)
val arr = new Array(5) //长度5 arr(0)表示第一个元素
var arr = new Array[Int](9) //定义长度为9的数组且全部初始化为0
```

### 变长数组

```scala
import  scala.collection.mutable.ArrayBuffer

//代码声明
val arr = ArrayBuffer[Int]() //声明可变数组
arr.append(1) //追加数字
arr += 2 //追加数字,语法糖
arr += (1,2,3) //追加多个值
arr ++= Array(1,2,3)  //追加多个值
arr.insert(1,(1,2,3)) //第一个参数位置,第二个多多参数
arr.remove() //参数1为下标,第二个参数为个数
arr -= 5 //移除数组元素5第一个出现的位置,如果没有改元素返回原数组
arr.reverse() //反转数组
for (i <- arr if i%2==0)yield i //返回偶数i
```

### 方法

```scala
arr.sorted() //排序
arr.sortWith((x,y)=>x>y) //函数自定义排序
arr.sortWith(_>_)//大小排序
val arr = Array(("a",1),("b",2))
arr.sortBy(_._2) //参数为函数,以第二个值排序
arr.sortBy(x=>x) //arr为(1,2,3.4)数组这样的排序按x排序
```

## 映射

### 不可变map

```scala
//不可变map是有序的
val stu = Map(("a",1),("b",2)) //声明方式1
val stu = Map("a"->1,"b"->2) //声明方式2
stu("a") //获取值a
stu.contains("a") //寻找键值a返回值bool
stu.getorElse("键名","默认值") //获取键值对
```

### 可变map

```scala
//可变map是无序的
import scala.collection.mutable.Map
stu += Map("a",1,"b",2) //添加元素,如果键名一致则覆盖原来的值
stu = stu + ("a",1)
stu -= "键名" //移除键值对
stu.remove("键名") //移除键值对
```

## map遍历

```scala
for ((key,value) <- stu){}
for (value <- stu.values){}
import scala.collection.mutable.LinkedHashMap//记录插入顺序
import scala.collection.mutable.SortedMap //按key排序map
```

## 元组

```scala
//小括号括起来的多个值
//两个值默认是key,value
val y = (1,2,3)
y._2 //去第2个元素
val y =new Tuple1(1)
val y =new Tuple3(1,2,3) //Tuple后的数字决定元组长度,最多22个元素

```

## zip拉链

```scala
arr1 zip arr2 //将两个数组拉到一起成key,value的元组
```

## 列表

```scala
//不可变列表
val l = Seq(1,2,3) //声明
数字::列表//新的list,数字在头
列表.::(数字) //放到list最后
数字+:列表 //数字加到list前
列表+:数字 //数字加到后,如果数字是一个数组则会被作为整体加入
list1 ++ list2 //两个列表相加
list1 ::: list2 或 list1.:::(list2)

//可变列表 scala.collection.mutable.ListBuffer
val l = ListBuffer[Int]()
l.append(多个数字)
list1 ++= list2 //追加list2使用 ++也行
//压平
list.flatten
//map+flatten
list.flatMap()
```

# set

```scala
val set1 =Set(1,2,3)
set1+数字 //追加
set1 + set2 //追加
set1.getClass

//可变set,重复数据自动去重
val set = new HashSet[Int]
set1 ++ set2 //++=
set1-=数字
```

# 懒加载

```scala
//关键字lazy
lazy val prop = init()//使用时才加载lazy
```

# 多线程

```scala
//并行计算 par
arr.par.reduce()
arr.aggregate(初始值,局部聚合函数,q)
```









