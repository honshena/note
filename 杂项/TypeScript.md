# TypeScript

# 声明

```js
//变量
let a:number=1;
let b:string;
//字面量
let c:10; //限制c只能是10
let d: 'male' | 'female' //限定变量值只能是male或female
//联合类型
let e: number | string | boolean //限定e的类型只能是三个中其中一个
//任意类型
let f:any;
let g; //不声明默认为any
//自动类型检测
let h = [1,2]; //类型: number[]
//未知类型
let i:unknown; //any可以任意赋值,但unknown之后i只能赋值不能被赋值给其他变量
i=1;
i='str';
if(typeof i === "number") a=i; //判断i的类型后可以对其他变量赋值
e=i as string//类型断言
e=<string>i//断言
//函数
function sum(a:number,b:number):number{ //参数类型限制,返回值类型限定,不写则自动推断
  return a+b
}
let func:(a:number,b:number)=>number //限定函数
//对象
let obj1: {name:string,age?:number}//问号表示该属性在该变量赋值时可选,name则必须赋值,但是不能有其它属性
let obj2:{[props:string]:any}//obj2只包含任意字符串类型的属性
let obj3: {a:number}  & {b:string};//obj3必须要求有这两属性
//ver类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型； 变量也可能是 never类型，当它们被永不为真的类型保护所约束时。
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
  throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
  return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
  while (true) {
  }
}
//Null 和 Undefined TypeScript里，undefined和null两者各自有自己的类型分别叫做undefined和null。
//和 void相似，它们的本身的类型用处不是很大：
let u: undefined = undefined;
let n: null = null;
//数组
let arr:number[];
let arr1:Array<string>
//元组:固定长度的数组Tuple
let t :[string,string]//t=['str1','str2']但是不可以t=['','','']也不可以t=[0,'']
let t2:[string,number]//t=['aaa',123]

//枚举
enum Gender{
  Male,
  Female
}
//原来定义对象
let stu1 :{
  name:string,
  gender: 0|1
}
//使用枚举
let stu2:{
  name:string,
  gender: Gender
}
stu1.gender =0;
stu2.gender=Gender.Female //此时gender的Female的值是啥已经不重要了
stu2.gender==Gender.Female; //这样判读就不需要知道female的值
//类型别名
type myType = 'male' | 'female'
let sex:myType; //相当于let sex:'male' | 'female'

```

# 编译选项

## 指令编译

```cmd
tsc xxx.ts
tsc xxx.ts -w #自动监视文件变化并编译但是只能对单个文件监视
```

## 自动编译

在根目录下新建`tsconfig.json`

[官网配置](https://www.tslang.cn/docs/handbook/tsconfig-json.html)

```cmd
#在含有tsconfig.json所在路径下执行
tsc 
#手动指定含有配置文件路径
tsc -p #路径
```

`tsconfig.json`

```json
{
  /*tsconfig.json*/
  "compilerOptions": { //编译器选项
    "module": "es2015", //指定模块化规范'none', 'commonjs', 'amd', 'system', 'umd', 'es6', 'es2015', 'es2020', 'esnext'.

    "target": "es5", //指定es版本must be: 'es3', 'es5', 'es6', 'es2015', 'es2016', 'es2017', 'es2018', 'es2019', 'es2020', 'es2021', 'esnext'.

    "sourceMap": true, //生成映射文件
    //"lib":[] ,//使用的第三方库
    //"outDir": "./dist",//指定编译完成后的文件
    //"outDir": "./index.js",//将编译后的文件整合为一个文件,如果使用了模块则模块必须选则AMD或system
    //"allowJs": false ,//是否编译js
    //"checkJs": false ,//不检查js文件中ts语法
    "strict": true, //启用严格检查
    "removeComments": true, //移除注释
    "noEmitOnError": true, //ts检查出错时不生成编译后的js
    "alwaysStrict": true, //编译后的文件使用严格模式
    "noImplicitAny": true, //是否运行隐式any
    "noImplicitThis": true, //是否运行隐式this
    "strictNullChecks": true //检查例如获取dom时可能产生的空值
  },
  // "include": ['src/**/*'],//表示只编译src路径下的文件,**表示任意文件夹*表示任意文件
  "exclude": [ //排除不包含的文件
    "node_modules"
  ]
}
```

# 类

```ts
class Person{
  name:string='xu'
  private age:number
  static props:boolean
  readonly read='a' //表示属性只可读修改改属性会报错
  constructor(name:string,age:number) {
    this.name=name
    this.age=age
  }
    /*等价于可以不用声明了
    constructor(public name:string,private age:number) {
    this.name=name
    this.age=age
  }
    */
}
let stu = new Person('xu',18)
console.log(stu.name);
console.log(Person.props);
console.log(stu.read);
```

# 存取器

TypeScript支持通过getters/setters来截取对对象成员的访问。 它能帮助你有效的控制对对象成员的访问。

```ts
let passcode = "secret passcode";

class Employee {
    private _fullName: string;

    get fullName(): string {
        return this._fullName;
    }

    set fullName(newName: string) {
        if (passcode && passcode == "secret passcode") {
            this._fullName = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}

let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    alert(employee.fullName);
}
```

# 抽象类

抽象类做为其它派生类的基类使用。 它们一般不会直接被实例化。 不同于接口，抽象类可以包含成员的实现细节。 `abstract`关键字是用于定义抽象类和在抽象类内部定义抽象方法。

```ts
abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log('roaming the earch...');
    }
}
```

抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。 抽象方法的语法与接口方法相似。 两者都是定义方法签名但不包含方法体。 然而，抽象方法必须包含 `abstract`关键字并且可以包含访问修饰符。

# 接口

**接口可以重复声明**

## 可选属性

接口里的属性不全都是必需的。 有些是只在某些条件下存在，或者根本不存在。 可选属性在应用“option bags”模式时很常用，即给函数传入的参数对象中只有部分属性赋值了。

下面是应用了“option bags”的例子：

```ts
interface SquareConfig {
  color?: string;
  width?: number;
}
```

## 只读属性

一些对象属性只能在对象刚刚创建的时候修改其值。 你可以在属性名前用 `readonly`来指定只读属性:

```ts
interface Point {
    readonly x: number;
    readonly y: number;
}
```

# 泛型

泛型函数的类型与非泛型函数的类型没什么不同，只是有一个类型参数在最前面，像函数声明一样：

```ts
function identity<T>(arg: T): T {
    return arg;
}
```

# 泛型约束

```ts
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);  // Now we know it has a .length property, so no more error
    return arg;
}
```

# 命名空间

> 请务必注意一点，TypeScript 1.5里术语名已经发生了变化。 “内部模块”现在称做“命名空间”。 “外部模块”现在则简称为“模块”，这是为了与 [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/)里的术语保持一致，(也就是说 `module X {` 相当于现在推荐的写法 `namespace X {`)。

```js
namespace Validation {
    export interface StringValidator {
        isAcceptable(s: string): boolean;
    }

    const lettersRegexp = /^[A-Za-z]+$/;
    const numberRegexp = /^[0-9]+$/;

    export class LettersOnlyValidator implements StringValidator {
        isAcceptable(s: string) {
            return lettersRegexp.test(s);
        }
    }
	//该命名空间下的变量或方法需要暴露出去才能使用命名.方法在外部访问
    export class ZipCodeValidator implements StringValidator {
        isAcceptable(s: string) {
            return s.length === 5 && numberRegexp.test(s);
        }
    }
}

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: Validation.StringValidator; } = {};
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        console.log(`"${ s }" - ${ validators[name].isAcceptable(s) ? "matches" : "does not match" } ${ name }`);
    }
}
```

# 装饰器

*装饰器*是一种特殊类型的声明，它能够被附加到[类声明](https://www.tslang.cn/docs/handbook/decorators.html#class-decorators)，[方法](https://www.tslang.cn/docs/handbook/decorators.html#method-decorators)， [访问符](https://www.tslang.cn/docs/handbook/decorators.html#accessor-decorators)，[属性](https://www.tslang.cn/docs/handbook/decorators.html#property-decorators)或[参数](https://www.tslang.cn/docs/handbook/decorators.html#parameter-decorators)上。 装饰器使用 `@expression`这种形式，`expression`求值后必须为一个函数，它会在运行时被调用，被装饰的声明信息做为参数传入。

```js
//类装饰器
function logClass(my:any){
  console.log(my)
}
@logClass
class my{
  name: string | undefined;
  constructor() {
    console.log(this)
  }
}
//带参数装饰器
function logClass2(params:string){
  return (my:any)=>{
    console.log(my)
  }
}
//修改类
function logClass(my:any){
  return class newMy extends my{
      //修改一一般属性和方法或构造函数
  }
}
//属性装饰器
function logAttr(my:any,attr:any){
  console.log(my,attr)
}
//带参数装饰器
function logClass2(params:string){
  return (my,attrName,desc)=>{ //desc下的value为装饰的属性
    console.log(my,attrName,desc)
  }
}
```

## 参数装饰器

*参数装饰器*声明在一个参数声明之前（紧靠着参数声明）。 参数装饰器应用于类构造函数或方法声明。 参数装饰器不能用在声明文件（.d.ts），重载或其它外部上下文（比如 `declare`的类）里。

参数装饰器表达式会在运行时当作函数被调用，传入下列3个参数：

1. 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
2. 成员的名字。
3. 参数在函数参数列表中的索引。

> 注意  参数装饰器只能用来监视一个方法的参数是否被传入。

参数装饰器的返回值会被忽略。

下例定义了参数装饰器（`@required`）并应用于`Greeter`类方法的一个参数：

```ts
class Greeter {
    greeting: string;

    constructor(message: string) {
        this.greeting = message;
    }

    @validate
    greet(@required name: string) {
        return "Hello " + name + ", " + this.greeting;
    }
}
```

**属性=>方法=>参数=>类 多个装饰器从后向前执行**