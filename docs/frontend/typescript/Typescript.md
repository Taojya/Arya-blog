## 一、安装Typescript

[Typescript 官网地址](https://www.typescriptlang.org/zh/)

安装Typescript:

```bash
npm install -g typescript
```

使用 tsc 全局命令：

```bash
// 查看 tsc 版本
tsc -v
// 编译 ts 文件
tsc fileName.ts
```

## 二、原始数据类型

- Boolean
- Null
- Undefined
- Number
- BigInt
- String
- Symbol

```typescript
let isDone: boolean = false

// 注意 es6 还支持2进制和8进制
let age: number = 10
let binaryNumber = 0b1111

// 注意 es6 新增的模板字符串也没有问题
let firstName: string = 'viking'
let message: string = `hello,${firstName}, age is ${age}`

let u: undefined = undefined
let n: null = null

// 注意 undefined 和 null 是所有类型的子类型。也就是说 undefined 类型的变量，可以赋值给 number 类型的变量：
let num: number = undefined
```

- any 类型

```typescript
let notSure: any = 4
notSure = 'maybe it is a string'
notSure = true
// 在任意值上访问任何属性都是允许的
notSure.myName
// 也允许调用任何方法
notSure.getName()
```

## 三、Array 和 Tuple

```typescript
// 最简单的方法是使用 [类型 + 方括号] 来表示数组：
let arrOfNumber: number[] = [1,2,3,4]
// 数组的项中不允许出现其他的类型
// 数组的一些方法的参数也会根据数组在定义时约定的类型进行限制
arrOfNumber.push(3)
arrOfNumber.push('abc')

// 元组将类型写在里面，对每一项进行限制
let user: [string, number] = ['viking',20]
// 但当我们 少写 或 多写 一项都会报错
user = ['molly',20,true]
```

## 四、interface接口

Duck Typing 概念：

> 如果某个东西长得像鸭子，像鸭子一样游泳，像鸭子一项嘎嘎叫，那他就可以被看成是一只鸭子。

```typescript
// 我们定义了一个接口
interface Person{
  name: string,
  age: number
}
// 接着定义一个变量 viking，它的类型是 Person。这样我们就约束了 vking 的形状必须和接口 Person 一致
let viking: Person ={
  name: 'viking',
  age: 20
}

// 有时我们希望不要完全匹配一个形状，那么可用可选属性：
interface Person {
  name: string,
  age?: number
}
let jack: Person = {
  name: 'jack'
}

// 只读属性，一些字段只能在创建时呗复制，可用 readonly 定义只读属性
interface Person {
  readonly id: number,
  name: string,
  age?: number
}
viking.id = 9527
```

## 五、函数

1. 定义一个函数，约定 参数类型、返回参数类型

```typescript
function add(x: number, y: number, z?: number): number {
  if (typeof z === 'number') {
    return x + y + z
  } else {
    return x + y
  }
// add(1, 2, 3) // 多加参数会报错
// add(1, '2') // 传参类型错误报错
let result = add(1, 2)
```

注意：可选参数后不可在加必填参数,引起传参混乱

```typescript
function add(x: number, y: number, z?: number, t: number): number {}
```

2. 函数表达式写法

```typescript
const add2 = (a: number, b: number): number => {
  return a + b
}
```

3. 赋值函数

==注意：此处箭头不是 ES6 中的箭头函数，而是 TS 中声明函数类型的返回方法==

==在 TS 中凡是在“：”后面的箭头都是在声明类型，和实际代码逻辑没有关系==

```typescript
let add3: (x: number, y: number, z?: number) => number = add2
```

4. 定义一个函数的 interface

==注意：声明返回值 'number' 此处用 ':'==

```typescript
interface ISum {
  // 声明参数, 注意：声明返回值 'number' 此处用 ':'
  (x: number, y: number, z?: number): number
}
let add4: ISum = add2
```

## 六、类型推断、联合类型和类型断言

类型断言 as 

```typescript
function getLength(input: string | number): number {
  // input.slice() // 没法确定类型，不能使用
  // 类型断言
  const str = input as string
  if (str.length) {
    return str.length
  } else {
    const number = input as number
    return number.toString().length
  }
}
```

```typescript
// type guard
function getLength2(input: string | number): number {
  if (typeof input === 'string') {
    return input.length
  } else {
    return input.toString().length
  }
}
```

## 七、class - 类

- 类（class）:定义了一切事物的抽象特点
- 对象（Object）:类的实例
- 面向对象（OOP）三大特性：封装、继承、多态

1. 封装

```javascript
// 封装
class Animal {
  constructor(name) {
    this.name = name
  }
  run() {
    return `${this.name} is running`
  }
}
const snake = new Animal('lily')
console.log(snake.run())
```

2. 继承

```javascript
// 继承
class Dog extends Animal {
  bark() {
    return `${this.bark.name} is barking`
  }
}

const xiaobao = new Dog('xiaobao')
console.log(xiaobao.run())
console.log(xiaobao.bark())
```

3. 多态

```javascript
// 多态
class Cat extends Animal {
  // 静态属性与实例的定义和状态无太大关系
  static categories = ['mammal'] // 静态属性定义
  constructor(name) {
    super(name) // 重写
    console.log(this.name)
  }
  run() {
    return 'Meow,' + super.run()
  }
}
const maomao = new Cat('maomao')
console.log(maomao.run())
console.log(Cat.categories) // 直接访问静态属性或方法
```

<img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/ts02.61swhbm8dpg0.webp" alt="image-20210817113035414" style="zoom:67%;" />

- Public：修饰的属性或方式是共有的，class定义默认是Public
- Private：修饰属性或方式是私有的，无法访问

```typescript
// 封装
class Animal {
  constructor(name) {
    this.name = name
  }
  private run() {
    return `${this.name} is running`
  }
}
const snake = new Animal('lily')
console.log(snake.run()) //  报错：属性“run”为私有属性，只能在类“Animal”中访问。
```

- Protected：修饰的属性或方法是受保护的，实例调用报错，但子类可以访问（只有本身和子类可以访问）

```typescript
// 封装
class Animal {
  constructor(name) {
    this.name = name
  }
  protected run() {
    return `${this.name} is running`
  }

}
const snake = new Animal('lily')
console.log(snake.run()) // 报错：属性“run”受保护，只能在类“Animal”及其子类中访问。

// 子类可以访问
class Cat extends Animal {
  // 静态属性与实例的定义和状态无太大关系
  static categories = ['mammal'] // 静态属性定义
  constructor(name) {
    super(name) // 重写
    console.log(this.name)
  }
  run() {
    return 'Meow,' + super.run() // 不报错
  }
}
const maomao = new Cat('maomao')
console.log(maomao.run()) 
```

- readonly 只读

```typescript
class Animal {
  readonly name: String
  constructor(name) {
    this.name = name
  }
  protected run() {
    return `${this.name} is running`
  }
}
const snake = new Animal('lily')
snake.name = '123' // 报错：无法分配到 "name" ，因为它是只读属性。
```

## 八、类和接口

 <img src="https://cdn.staticaly.com/gh/Taojya/Picx@master/blog/ts01.6r9el5axz940.webp" alt="image-20210819104121858" style="zoom:67%;" />

1. 通过interface完成功能的提取和验证

```typescript
interface Radio {
  switchRadio(trigger: boolean): void
}
class Car implements Radio { // 报错：类“Car”错误实现接口“Radio”。类型 "Car" 中缺少属性 "switchRadio"，但类型 "Radio" 中需要该属性。
  // switchRadio(trigger:boolean){
  // }
}
```

```typescript
interface Radio {
  switchRadio(trigger: boolean): void
}
class Car implements Radio {
  switchRadio(trigger: boolean) {}
}
```

2. 实现多个接口，用  “,”

```typescript
interface Radio {
  switchRadio(trigger: boolean): void
}
interface Battery {
  checkBatteryStatus(): void
}
class Car implements Radio {
  switchRadio(trigger: boolean) {}
}
// 实现多个接口，用 “,”
class Cellphone implements Radio, Battery {
  switchRadio(trigger: boolean) {}
  checkBatteryStatus() {}
}
```

3. 接口继承

```typescript
interface Radio {
  switchRadio(trigger: boolean): void
}
// 接口继承
interface RadioWithBattery extends Radio {
  checkBatteryStatus(): void
}
class Cellphone implements RadioWithBattery {
  switchRadio(trigger: boolean) {}
  checkBatteryStatus() {}
}

```

## 九、枚举

一系类常量，例如：周期、月份、色值等。

1. 数字枚举

```typescript
// 数字枚举
// 枚举成员会被自动赋值，从 0 开始,自动递增的数值
enum Direction {
  Up, // 0
  Down, // 1
  Left, // 2
  Right, // 3
}
console.log(Direction.Up) // 输出：0
// 枚举还做了反向映射
console.log(Direction[0]) // 输出：'Up'
```

 ```typescript
 // 枚举项可以手动赋值，其他项依次递增
 enum Direction1 {
   Up = 10, // 10
   Down, // 11
   Left, // 12
   Right, // 13
 }
 ```

源码

```javascript
var Direction;
(function (Direction) {
    Direction[Direction["Up"] = 0] = "Up";
    Direction[Direction["Down"] = 1] = "Down";
    Direction[Direction["Left"] = 2] = "Left";
    Direction[Direction["Right"] = 3] = "Right";
})(Direction || (Direction = {}));
console.log(Direction.Up); // 输出：0
// 枚举还做了反向映射
console.log(Direction[0]); // 输出：'Up'
```

2. 字符串枚举

```typescript
// 字符串枚举
enum Direction2 {
  Up = 'UP', // Up
  Down = 'DOWN', // Down
  Left = 'LEFT', // Left
  Right = 'RIGHT', // Right
}
const value = 'UP'
if (value === Direction2.Up) {
  console.log('go up!')
}
```

3. 常量枚举，使用常量枚举可以提升性能

   - 它会内联枚举的任何用法，并且不会把枚举编译成相关代码；

   - 但枚举的值有两种类型：1）常量值；2）计算值；只有常量值可以用常量枚举

```typescript
// 常量枚举
const enum Direction3 {
  Up = 'UP', // Up
  Down = 'DOWN', // Down
  Left = 'LEFT', // Left
  Right = 'RIGHT', // Right
}
const value3 = 'UP'
if (value === Direction3.Up) {
  console.log('go up!')
}
```

编译为源码

```javascript
var value3 = 'UP';
if (value === "UP" /* Up */) {
    console.log('go up!');
}
```

而非常量枚举编译源码为：

```javascript
// 字符串枚举
var Direction2;
(function (Direction2) {
    Direction2["Up"] = "UP";
    Direction2["Down"] = "DOWN";
    Direction2["Left"] = "LEFT";
    Direction2["Right"] = "RIGHT";
})(Direction2 || (Direction2 = {}));
var value = 'UP';
if (value === Direction2.Up) {
    console.log('go up!');
}
```

## 十、泛型（Generics）

1. 泛型：在定义函数、接口或类的时候不去指定类型，而是在使用的时候指定类型。

```typescript
function echo2<T>(arg: T): T {
  return arg
}
const str: string = 'str' 
const reslut2 = echo2(str) // function echo2<string>(arg: string): string
const reslut4:boolean = echo2(str) // 报错：不能将类型“string”分配给类型“boolean”
```

而不使用泛型

```typescript
function echo1(arg) {
  return arg
}
const result1 = echo1(123) // function echo1(arg: any): any
const result3: string = echo1(123) // 不会报错
```

2. 多项泛型

```typescript
function swap<T, U>(tuple: [T, U]): [U, T] {
  return [tuple[1], tuple[0]]
}
const result5= swap(['string',123])
result5[1].toLocaleUpperCase() // 可以使用string类型的方法及提示
```

3. 约束泛型

```typescript
function echoWithArr<T>(arg: T): T {
  console.log(arg.length) // 报错：由于不知道是那种类型，所以里面不能随意使用操作方法
  return arg
}
```

普通解法

```typescript
function echoWithArr<T>(arg: T[]): T[] {
  console.log(arg.length) // 报错：由于不知道是那种类型，所以里面不能随意使用操作方法
  return arg
}
const arrs = echoWithArr([1, 2, 3])
```

约束泛型

```typescript
interface IWithLength {
  length: number
}
function echoWithLength<T extends IWithLength>(arg: T): T {
  console.log(arg.length)
  return arg
}

const str1 = echoWithLength('str')
const obj = echoWithLength({ length: 10 })
const arr = echoWithLength([1, 2, 3])

echoWithLength(13) // 报错：类型“number”的参数不能赋给类型“IWithLength”的参数
```

4. 泛型在类中的使用

```typescript
// 泛型在类中的使用
class Queue {
  private data = []
  push(item) {
    return this.data.push(item)
  }
  pop() {
    return this.data.shift()
  }
}
const queue = new Queue()
queue.push(1)
queue.push('str')
console.log(queue.pop().toFixed()) // 报错：无法指定类型，使用数值类型方法，ts无法抛出错误
console.log(queue.pop().toFixed())
```

带泛型的类

```typescript
// 带有泛型的类
class Queue1<T> {
  private data = []
  push(item: T) {
    return this.data.push(item)
  }
  pop(): T {
    return this.data.shift()
  }
}
const queue1 = new Queue1<number>()
queue1.push(1)
queue1.push('str') // 报错：类型“string”的参数不能赋给类型“number”的参数。
```

5. 泛型在接口中的使用

```typescript
interface KeyPair<T, U> {
  key: T
  value: U
}
let kp1: KeyPair<number, string> = { key: 1, value: 'str' }
let kp2: KeyPair<string, number> = { key: 'str', value: 2 }
let arr3: number[] = [1, 2, 3]
let arrTwo: Array<number> = [1, 3, 5]
```

## 十一、类型别名、字面量、交叉类型

### 类型别名

1. 类型别名

```typescript
let sum: (x: number, y: number) => number
const reslut = sum(1, 2)
type PlusType = (x: number, y: number) => number

let sum2: PlusType // 创建类型，防止再重复编写的冗余
let result2 = sum(2, 3)
```

2. 创建联合类型

```typescript
// 创建联合类型
type StrOrNumber = string | number
let result4: StrOrNumber = 123
result4 = 1
```

### 字面量

```typescript
const str2: 'name' = 'name1' // 报错：不能将类型“"name1"”分配给类型“"name"”
const number: 1 = 2 // 报错：不能将类型“2”分配给类型“1”
const str3: 'name' = 'name' // 只能'name'等于'name'
```

```typescript
type Directions = 'Up' | 'Down' | 'Left' | 'Right'
let toWhere: Directions = 'Left'
```

### 交叉类型

```typescript
interface IName {
  name: string
}
type IPerson = IName & { age: number } // 类型 interface 的 extend
let person: IPerson = { name: '123', age: 123 }
```

- type 不是一种特殊类型，是别的类型的别名，像快捷方式；当使用交叉或组合的时候，可考虑使用 type

- interface是一种独特的类型；当使用extends或 implements 时，使用 interface

## 十二、声明文件

当使用第三方库的时候，很多库使用的不是 Typesript ，它们很多通过Javascript或浏览器或 nodeJs 提供的runtime 对象

- 声明：

```typescript
declare var jQuery: (selector: string) => any
jQuery('#foo')
```

或者在 jQuery.d.ts 文件中定义

- Type管理工具

```shell
npm install --save @type/jquery
```

## 十三、内置类型

内置对象：根据标准在全局global中使用的对象。

ctrl点击 ’Data‘ 可查看内置对象的定义

```typescript
const data=new Date()
data.getTime()
const reg= /abc/
Math.pow(2,2)
```

