## TypeScript 教程 runoob 篇

这是基于 [runoob](https://www.runoob.com/typescript/ts-tutorial.html) 版的快速学习笔记

### 简介

TypeScript 是 JavaScript 的一个超集，支持 ECMAScript 6 标准。

TypeScript 由微软开发的自由和开源的编程语言。

TypeScript 设计目标是开发大型应用，它可以编译成纯 JavaScript，编译出来的 JavaScript 可以运行在任何浏览器上。

### TypeScript 安装

可以使用 npm 安装

```shell
## 全局安装
npm install -g typescript

## 查看版本
tsc -v
Version 3.2.2
```

通常我们使用 **.ts** 作为 TypeScript 代码文件的扩展名。

```typescript
// test.ts
var message:string = "Hello World" 
console.log(message)
```

然后通过下面命令将 TypeScript 转换为 JavaScript

```shell
tsc test.ts
```

转换成功后会在 test.ts 目录下生成同名的 test.js。

在大型项目开发中，我们当然应该利用 webpack 等工具结合相应的模块对 ts 文件批量处理转换。

### TypeScript 基础语法

TypeScript 程序由以下几个部分组成：

- 模块
- 函数
- 变量
- 语句和表达式
- 注释

ts 也有自己的关键字，基本跟 js 是一样的。

在 *空白/换行*、*分号*、*大小写*、*注释*、方面跟 js 是一样的。

关于类的写法，ts 采用了 ES6 的 class

```typescript
class Site { 
   name():void { 
      console.log("Runoob") 
   } 
} 
var obj = new Site(); 
obj.name();
```

### TypeScript 基础类型

js有的基础类型，ts 都有，ts多了几个：`any`、`enum`、`never`、`void`

ts 声明时带类型声明如下：

```typescript
// 关键字 变量名: 类型 = 赋值
let x: any = 1;
```

any 类型，任意值是 TypeScript 针对编程时类型不明确的变量使用的一种数据类型。任意值类型可以让这些变量跳过编译阶段的类型检查。

never 类型，never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值。声明为 never 类型的变量只能被 never 类型所赋值，在函数中它通常表现为抛出异常或无法执行到终止点（例如无限循环）。

```typescript
let x: never;
let y: number;

// 运行错误，数字类型不能转为 never 类型
x = 123;

// 运行正确，never 类型可以赋值给 never类型
x = (()=>{ throw new Error('exception')})();

// 运行正确，never 类型可以赋值给 数字类型
y = (()=>{ throw new Error('exception')})();
```

void 类型，用于标识方法返回值的类型，表示该方法没有返回值。

enum 类型，枚举类型用于定义数值集合。

```typescript
enum Color {Red, Green, Blue};
let c: Color = Color.Blue;
console.log(c);    // 输出 2
```

### TypeScript 变量声明

变量是一种使用方便的占位符，用于引用计算机内存地址。我们可以把变量看做存储数据的容器。

TypeScript 变量的命名规则（同 JS）：

- 变量名称可以包含数字和字母。
- 除了下划线 **_** 和美元 **$** 符号外，不能包含其他特殊字符，包括空格。
- 变量名不能以数字开头。

ts 变量使用前必须先声明，我们可以使用 var 来声明变量。（JS 引擎有声明提升，可以先使用变量后声明，但是原则上还是要在使用变量前声明变量，这样可以避免很多不必要的麻烦。）

声明变量的4种方式：

```typescript
// 声明变量的类型及初始值：
var [变量名] : [类型] = 值;

// 声明变量的类型及但没有初始值，变量值会设置为 undefined
var [变量名] : [类型];

// 声明变量并初始值，但不设置类型类型，该变量可以是任意类型
var [变量名] = 值;

// 声明变量没有设置类型和初始值，类型可以是任意类型，默认初始值为 undefined
var [变量名];
```

这种声明变量的方式跟 js 是一样的，只是比 js 多了个类型。

TypeScript 遵循强类型，如果将不同的类型赋值给变量会编译错误，如下实例：

```typescript
var num:number = "hello"     // 这个代码会编译错误
```

#### 类型断言

类型断言可以用来手动指定一个值的类型，即允许变量从一种类型更改为另一种类型。

语法格式用 `<类型>值` 或 `值 as 类型`

```typescript
var str = '1' 
var str2:number = <number> <any> str   //str、str2 是 string 类型
console.log(str2)
```

类型断言用的不好容易出问题，所以在不确定变量类型的情况下，最好用 `any`，稳的很。

> 它之所以不被称为**类型转换**，是因为转换通常意味着某种运行时的支持。但是，类型断言纯粹是一个编译时语法，同时，它也是一种为编译器提供关于如何分析代码的方法。

#### 类型推断

当类型没有给出时，TypeScript 编译器利用类型推断来推断类型。

如果由于缺乏声明而不能推断出类型，那么它的类型被视作默认的动态 any 类型。

```typescript
var num = 2;    // 类型推断为 number
console.log("num 变量的值为 " + num); 
num = "12";    // 编译错误
console.log(num);
```

#### 变量作用域

变量作用域指定了变量定义的位置。

程序中变量的可用性由变量作用域决定。

TypeScript 有以下几种作用域：

- **全局作用域** − 全局变量定义在程序结构的外部，它可以在你代码的任何位置使用。
- **类作用域** − 这个变量也可以称为 **字段**。类变量声明在一个类里头，但在类的方法外面。 该变量可以通过类的对象来访问。类变量也可以是静态的，静态的变量可以通过类名直接访问。
- **局部作用域** − 局部变量，局部变量只能在声明它的一个代码块（如：方法）中使用。

作用域方面的限制， ts 比 js 严格些。

### TypeScript 运算符

运算符用于执行程序代码运算，会针对一个以上操作数项目来进行运算。

ts 的运算符操作跟 js 的运算符是一样的。

### TypeScript 条件语句

条件语句用于基于不同的条件来执行不同的动作。

ts 的条件语句跟 js 的条件语句是一样的。

### TypeScript 循环

循环语句允许我们多次执行一个语句或语句组。

ts 的循环跟 js 的循环是一样的。

### TypeScript 函数

函数是一组一起执行一个任务的语句。

ts 的函数基本与 js 的函数一致，但 ts 的函数做了些扩展。

#### ts 函数区别之函数返回值

ts 函数定义了返回值类型的情况下，函数的返回值类型必须保持一致

```typescript
function greet():string { // 返回一个字符串
    return "Hello World" 
} 
```

#### ts 函数之可选参数和默认参数

ts 函数定义了参数就必须传入这些参数，除非将这些参数设置成可选，可选参数使用问号`?`标识。

```typescript
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}
 
let result1 = buildName("Bob");  // 正确
let result2 = buildName("Bob", "Adams", "Sr.");  // 错误，参数太多了
let result3 = buildName("Bob", "Adams");  // 正确
```

#### ts 函数之默认参数

ts 默认参数的设置等同于 ES6 的默认参数设置。

#### ts 函数之剩余参数

这个其实是用的 ES6 的解构，但是因为 ts 函数的参数设置特性，定义函数时，剩余参数必须给定义，否则会报错。

```typescript
function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
}
  
let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
```

#### ts 函数之重载

ts 函数的特性可以让它实现重载，但是 js 的函数特性又没法实现重载，实际上在 ts 转换成 js 后，函数重载就不存在了。

### TypeScript Number

TypeScript 与 JavaScript 类似，支持 Number 对象。

Number 对象是原始数值的包装对象。

ts 的 Number 对象跟 js 的 Number 对象是一样的。

### TypeScript String

String 对象用于处理文本（字符串）。

ts 的 String 对象跟 js 的 String 对象是一样的。

### TypeScript Array

数组对象是使用单独的变量名来存储一系列的值。

ts 的 Array 对象跟 js 的 Array 对象是一样的。

> 有一点点区别，因为 ts 变量类型，ts 的数组必须存储同类型的数据。
>
> ts 不同数据类型的数组另有个名字叫*元组*。

### TypeScript 元组

ts 数组中元素的数据类型都是相同的，如果存储的元素数据类型不同，则需要使用**元组**。

元组中允许存储不同类型的元素，元组可以作为参数传递给函数。

创建元组的语法如下：

```typescript
var tuple_name = [value1,value2,value3,…value n]
```

### TypeScript 联合类型

联合类型（Union Types）可以通过管道（|）将变量设置多种类型，赋值时可以根据设置的类型来赋值。

**注意**：只能赋值指定的类型，如果赋值其它类型就会报错。

创建联合类型的语法格式如下：

```typescript
Type1|Type2|Type3 
```

实例：

```typescript
var val:string|number 
val = 12 ;
val = "Runoob";
val = true;  // 报错
```

也可以定义联合类型的数组：

```typescript
// 联合类型的数组内的数据必须是同类型的，毕竟它不是元组
var arr:number[]|string[]; 
var i:number; 
arr = [1,2,4] 
console.log("**数字数组**")  
 
for(i = 0;i<arr.length;i++) { 
   console.log(arr[i]) 
}  
 
arr = ["Runoob","Google","Taobao"] 
console.log("**字符串数字**")  
 
for(i = 0;i<arr.length;i++) { 
   console.log(arr[i]) 
}
```

### TypeScript 接口

接口是一系列抽象方法的声明，是一些方法特征的集合，这些方法都应该是抽象的，需要由具体的类去实现，然后第三方就可以通过这组抽象方法调用，让具体的类执行具体的方法。

TypeScript 接口定义如下：

```typescript
interface interface_name { 
}
```

实例：

```typescript
// 定义接口 IPerson
interface IPerson { 
    firstName:string, 
    lastName:string, 
    sayHi: ()=>string 
} 
 
var customer:IPerson = { 
    firstName:"Tom",
    lastName:"Hanks", 
    sayHi: ():string =>{return "Hi there"} 
} 
 
var employee:IPerson = { 
    firstName:"Jim",
    lastName:"Blakes", 
    sayHi: ():string =>{return "Hello!!!"} 
} 
```

**注意：** 接口不能转换为 JavaScript。 它只是 TypeScript 的一部分。

上面的实例转换成 js 如下：

```js
var customer = {
    firstName: "Tom",
    lastName: "Hanks",
    sayHi: function () { return "Hi there"; }
};

var employee = {
    firstName: "Jim",
    lastName: "Blakes",
    sayHi: function () { return "Hello!!!"; }
};
```

#### 接口继承

接口继承就是说接口可以通过其他接口来扩展自己。

Typescript 允许接口继承多个接口。

继承使用关键字 **extends**。

单接口继承语法格式：

```js
Child_interface_name extends super_interface_name
```

多接口继承语法格式：

```js
Child_interface_name extends super_interface1_name, super_interface2_name,…,super_interfaceN_name
```

### TypeScript 类

TypeScript 是面向对象的 JavaScript。

类描述了所创建的对象共同的属性和方法。

TypeScript 支持面向对象的所有特性，比如 类、接口等。

TypeScript 类定义方式如下：

```js
class class_name { 
    // 类作用域
}
```

ts 的 class 跟 ES6 的 class 基本一致，包括构造函数、super、继承等。ts 多了一些东西：

- static 关键字，定义静态属性和方法。静态成员可以直接通过类名调用。
- 控制修饰符`public`、`protected`、`private`、。教程中没提转换成 js 后是什么样，推断应该是对象的属性和方法的数据属性做了修改，比如冻结对象这种。

### TypeScript 对象

对象是包含一组键值对的实例。 值可以是标量、函数、数组、对象等，如下实例：

```js
var object_name = { 
    key1: "value1", // 标量
    key2: "value",  
    key3: function() {
        // 函数
    }, 
    key4:["content1", "content2"] //集合
}
```

ts 的对象跟 js 的对象是一样的。

### TypeScript 命名空间

命名空间一个最明确的目的就是解决重名问题。

TypeScript 中命名空间使用 **namespace** 来定义，语法格式如下：

```typescript
namespace SomeNameSpaceName { 
   export interface ISomeInterfaceName {      }  
   export class SomeClassName {      }  
}
```

### TypeScript 模块

TypeScript 模块的设计理念是可以更换的组织代码。

ts 的模块导入/导出用到的是 export / import。

### TypeScript 声明文件

TypeScript 作为 JavaScript 的超集，在开发过程中不可避免要引用其他第三方的 JavaScript 的库。虽然通过直接引用可以调用库的类和方法，但是却无法使用TypeScript 诸如类型检查等特性功能。为了解决这个问题，需要将这些库里的函数和方法体去掉后只保留导出类型声明，而产生了一个描述 JavaScript 库和模块信息的声明文件。通过引用这个声明文件，就可以借用 TypeScript 的各种特性来使用库文件了。

比如用到 jQuery 时，ts 并不知道什么是 `$`或`jQuery`，这时需要用到声明。

ts 使用 declare 关键字来定义它的类型，帮助 TypeScript 判断我们传入的参数类型对不对：

```js
declare var $: (selector: string) => any;

$('#foo');
```

声明文件以 **.d.ts** 为后缀，例如：

```js
runoob.d.ts
```

声明文件或模块的语法格式如下：

```js
declare module Module_Name {
}
```

ts 引入声明文件语法格式：

```js
/// <reference path = " runoob.d.ts" />
```

声明文件不包含实现，它只是类型声明。



### 总结

runoob 的 TypeScript 教程讲的内容很基础，适合快速入门。从这里看 ts ，其实是一门强类型语言，刚好跟 js 相反，虽然基本的东西都一样。

其中最显眼的就是声明变量时引入了变量类型，赋值不同类型的值给变量就会报错。

ts 的后续很多内容都受到强类型的约束。