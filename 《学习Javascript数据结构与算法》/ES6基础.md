# es6基础

## let/const
```js
let a = 1
const b = 2
let a =2;//报错
b = 3; //报错
```
let和const都有暂时性死区,需要掌握let与const的作用域

## 模板字面量
```js
let a ='abc'
let b = `${a}`; //引用变量
```
模板字面量用两个<kbd>`<kbd>包裹,要插入变量的值使用`${变量名}`即可

## 箭头函数
```js
let func1 = function(){};//函数
let func2 = ()=>{} ;//箭头函数
```
**箭头函数没有自己的this**

函数默认赋值
```js
function func(x='1',y=2,z={}){
    console.log(x,y,z); //'1',2,{}
}
```
## 展开运算
```js
let a = [1,2,3,4,5]
console.log(a);//[1,2,3,4,5]
console.log(...a);//1,2,3,4,5
```
`...`可以当做剩余参数使用
```js
function func1(x,y,...z){};
func1(1,2,3,4,5,6);//x=1,y=2,z=[3,4,5,6]
```
**数组解构**
```js
//解构赋值
let [a,b] = ['a','b'];//数组解构赋值a='a',b='b'
//值互换
[a,b] = [b,a];//交换a,b的值
```
**对象解构**
```js
let {a,b} = {a:'a',b:'b'};//a='a',b='b'
```
**属性简写**
```js
//属性简写
let a='a',b='b'
let obj = {a,b};//相当于obj = {a:'a',b:'b'}

//简写方法名
let obj = {
    //在obj对象声明func方法,通过简写方法名
    func(){
        console.log('func')
    }
    //相当于
    func: function func(){
          console.log('func')
    }
    
}
```
## 类
```js
class Book(){}
```
**继承**
```js
class Book1 extends Book(){} ;//Book1继承自Book
```
**属性存取器**
```js
class Book(){
    //构造方法
    constructor(name){
        this._name = name 
    }
    //get和set进行属性存取
    get name(){
        return this._name
    }
    set name(name){
        this._name = name
    }
}
```
和其他语言不同,类的属性不是私有的但是还是应遵循命名模式以`this._xxx`来命名, get和set作为此属性的存取方法
## 乘方运算符

```js
let a = 3**2 ;//a=9
//2**2 = 4
//4**2 = 16
//5**3 = 125
```
**表示乘方运算即幂运算
## 模块
```js
//AMD
import xxx from '....'
import * from '...'
import xxx as xxx from '...'
import {xxx} from '...' //对象解构
export {xxx as xxx}

//CommonJS
let xxx = require('...')
let {xxx} = require('...')
```
nodejs使用`require`语句(CommonJS模块),AMD则使用`import`语句

## TypeScript
TypeScript是开源,渐进式包含类型的JavaScript超集
输入以下命令安装ts
`npm install -g typescript`
typescript文件后缀为`.ts`
**类型推断**
```ts
let age: number =20
let existFlag: boolean = true
let language: string = 'ts'
```
如果没有设置变量的类型它的类型会被自动设置为any
**接口**
```ts
interface Person{
    name: string,
    age:number,
    compareTo(b): number
}
func printName(person: Person){
    console.log(person.name)
}
class man implements Person(){
    age: number;
    name: string
    compareTo(b):{
    //具体实现
    }
}
```
**泛型**
```ts
interface Person<T>{
    //参数b被指定为泛型
    compareTo(b: T): number
}
class human{}
//指定泛型的类型
class man implements Person<human>{

}
```
**编译时检查**
全局安装TypeScript后,只需要在js文件开头写上`@ts-check`即可开启编译时检查