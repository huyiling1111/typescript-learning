## typeScript学习
前言：javaScript是动态语言，数据类型由运行时决定。typeScript是静态语言，数据类型由编译时决定。引入ts的好处是当我们在写代码的时候会抛出异常信息。
### JS的八种内置类型
```
let str: string = "jimmy";
let num: number = 24;
let bool: boolean = false;
let u: undefined = undefined;
let n: null = null;
let obj: object = {x: 1};
let big: bigint = 100n;
let sym: symbol = Symbol("me"); 

``` 
#### null 和 undefined  

> 默认情况下 null 和 undefined 是所有类型的子类型。 就是说你可以把 null 和 undefined 赋值给其他类型。

```
// null和undefined赋值给string
let str:string = "666";
str = null
str= undefined

// null和undefined赋值给number
let num:number = 666;
num = null
num= undefined

// null和undefined赋值给object
let obj:object ={};
obj = null
obj= undefined

// null和undefined赋值给Symbol
let sym: symbol = Symbol("me"); 
sym = null
sym= undefined

// null和undefined赋值给boolean
let isDone: boolean = false;
isDone = null
isDone= undefined

// null和undefined赋值给bigint
let big: bigint =  100n;
它提供了一种方法来表示大于 2^53 - 1 的整数
big = null
big= undefined

```
#### number和bigint
虽然number和bigint都表示数字，但是这两个类型不兼容。


```
let big: bigint =  100n;
let num: number = 6;
big = num;
num = big;
```

会抛出一个类型不兼容的 ts(2322) 错误。

#### 数组（Array）
对数组类型的定义有两种方式：
```
let arr:string[] = ["1","2"];
let arr2:Array<string> = ["1","2"]；
```
定义联合类型数组
```
let arr:(number | string)[];
// 表示定义了一个名称叫做arr的数组, 
// 这个数组中将来既可以存储数值类型的数据, 也可以存储字符串类型的数据
arr3 = [1, 'b', 2, 'c'];
```
定义指定对象成员的数组
```
// interface是接口,后面会讲到
interface Arrobj{
    name:string,
    age:number
}
let arr3:Arrobj[]=[{name:'jimmy',age:22}]
```
#### 函数

##### 函数声明
```
function sum(x: number, y: number): number {
    return x + y;
}
```
##### 函数表达式
```
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```
##### 可选参数
```
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');

```
可选参数：可以是undefined

##### 函数重载
由于 JavaScript 是一个动态语言，我们通常会使用不同类型的参数来调用同一个函数，该函数会根据不同的参数而返回不同的类型的调用结果
```
type Combinable = string | number;
```
```
function add(a: Combinable, b: Combinable) {
    if (typeof a === 'string' || typeof b === 'string') {
     return a.toString() + b.toString();
    }
    return a + b;
}

```

```
//更多情况的函数重载
type Types = number | string
function add(a:number,b:number):number;
function add(a: string, b: string): string;
function add(a: string, b: number): string;
function add(a: number, b: string): string;
function add(a:Types, b:Types) {
  if (typeof a === 'string' || typeof b === 'string') {
    return a.toString() + b.toString();
  }
  return a + b;
}
const result = add('Semlinker', ' Kakuqo');
result.split(' ');
```

#### 元祖
元祖的特性是可以限制数组元素的个数和类型
```
let x: [string, number]; 
// 类型必须匹配且个数必须为2

x = ['hello', 10]; // OK 
x = ['hello', 10,10]; // Error 
x = [10, 'hello']; // Error

```

##### 元祖类型的可选元素
```
let optionalTuple: [string, boolean?];
optionalTuple = ["Semlinker", true];
console.log(`optionalTuple : ${optionalTuple}`);
optionalTuple = ["Kakuqo"];
console.log(`optionalTuple : ${optionalTuple}`);
```

##### 元祖类型的剩余元素
元组类型里最后一个元素可以是剩余元素，形式为 ...X，这里 X 是数组类型。剩余元素代表元组类型是开放的，可以有零个或多个额外的元素。
```
type RestTupleType = [number, ...string[]];
let restTuple: RestTupleType = [666, "Semlinker", "Kakuqo", "Lolo"];
console.log(restTuple[0]);
console.log(restTuple[1]);
```

##### 只读的元祖类型
我们可以为任何元组类型加上 readonly 关键字前缀，以使其成为只读元组。具体的示例如下： 
```
const point: readonly [number, number] = [10, 20];
``` 

#### void
void表示没有任何类型，和其他类型是平等关系，不能直接赋值:
```
let a: void; 
let b: number = a; // Error
```

值得注意的是，方法没有返回值将得到undefined，但是我们需要定义成void类型，而不是undefined类型。否则将报错: 
```
let unusable: void = undefined;

// OK
function fn(): void {
  return null
}

// OK
function fn(): void {
}
```

#### never 
never类型表示的是那些永不存在的值的类型 
1.如果一个函数执行时抛出了异常，那么这个函数永远不存在返回值（**因为抛出异常会直接中断程序运行，这使得程序运行不到返回值那一步，即具有不可达的终点，也就永不存在返回了**）；
2.函数中执行无限循环的代码（死循环），使得程序永远无法运行到函数返回值那一步，永不存在返回。

```
// OK
let bar: never = (() => {
  throw new Error('Throw my hands in the air like I just dont care');
})();

// ERROR
function fn(): never {
}
```

#### any
在any上访问任何属性都是允许的,也允许调用任何方法
```
let anyThing: any = 'hello';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName);
let anyThing: any = 'Tom';
anyThing.setName('Jerry');
anyThing.setName('Jerry').sayHello();
anyThing.myName.setFirstName('Cat');

```

#### unknown
unknown与any的最大区别是： 任何类型的值可以赋值给any，同时any类型的值也可以赋值给任何类型。unknown 任何类型的值都可以赋值给它，但它只能赋值给unknown和any
```
let notSure: unknown = 4;
let uncertain: any = notSure; // OK

let notSure: any = 4;
let uncertain: unknown = notSure; // OK

let notSure: unknown = 4;
let uncertain: number = notSure; // Error

```

### 类型推断
```
{
  let str = 'this is string'; // 等价
  let num = 1; // 等价
  let bool = true; // 等价
}
``` 
在 TypeScript 中，具有初始化值的变量、有默认值的函数参数、函数返回的类型都可以根据上下文推断出来。比如我们能根据 return 语句推断函数返回的类型，如下代码所示：

```
{
  /** 根据参数的类型，推断出返回值的类型也是 number */
  function add1(a: number, b: number) {
    return a + b;
  }
  const x1= add1(1, 1); // 推断出 x1 的类型也是 number
  
  /** 推断参数 b 的类型是数字或者 undefined，返回值的类型也是数字 */
  function add2(a: number, b = 1) {
    return a + b;
  }
  const x2 = add2(1);
  const x3 = add2(1, '1'); // ts(2345) Argument of type "1" is not assignable to parameter of type 'number | undefined
}

```  
如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 any 类型而完全不被类型检查：
```
let myFavoriteNumber;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```

### 类型断言
有时会碰到我们比 TypeScript 更清楚实际类型的情况，比如下面的例子：
```
const arrayNumber: number[] = [1, 2, 3, 4];
const greaterThan2: number = arrayNumber.find(num => num > 2); // 提示 ts(2322)
```

主观上要求typescript是什么类型，按照这个方式来检查 
```
const arrayNumber: number[] = [1, 2, 3, 4];
const greaterThan2: number = arrayNumber.find(num => num > 2) as number;
```

语法
```
// 尖括号 语法
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;

// as 语法
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;


``` 
以上两种方式虽然没有任何区别，但是尖括号格式会与react中JSX产生语法冲突，因此我们更推荐使用 as 语法。

#### 非空断言
在上下文中当类型检查器无法断定类型时，一个新的后缀表达式操作符 ! 可以用于断言操作对象是非 null 和非 undefined 类型 
`具体而言，x! 将从 x 值域中排除 null 和 undefined 。`

```
let mayNullOrUndefinedOrString: null | undefined | string;
mayNullOrUndefinedOrString!.toString(); // ok
mayNullOrUndefinedOrString.toString(); // ts(2531)
```
 
```
type NumGenerator = () => number;

function myFunc(numGenerator: NumGenerator | undefined) {
  // Object is possibly 'undefined'.(2532)
  // Cannot invoke an object which is possibly 'undefined'.(2722)
  const num1 = numGenerator(); // Error
  const num2 = numGenerator!(); //OK
}

``` 

#### 确定赋值断言
允许变量在赋值前使用
```
let x: number;
initialize();

// Variable 'x' is used before being assigned.(2454)
console.log(2 * x); // Error
function initialize() {
  x = 10;
}

```

```
let x!: number;
initialize();
console.log(2 * x); // Ok

function initialize() {
  x = 10;
}
```

### 字面量类型
在 TypeScript 中，字面量不仅可以表示值，还可以表示类型，即所谓的字面量类型。

目前，TypeScript 支持 3 种字面量类型：字符串字面量类型、数字字面量类型、布尔字面量类型，对应的字符串字面量、数字字面量、布尔字面量分别拥有与其值一样的字面量类型，具体示例如下： 
```
{
  let specifiedStr: 'this is string' = 'this is string';
  let specifiedNum: 1 = 1;
  let specifiedBoolean: true = true;
}
```

实际上，定义单个的字面量类型并没有太大的用处，它真正的应用场景是可以把多个字面量类型组合成一个联合类型
```
type Direction = 'up' | 'down';

function move(dir: Direction) {
  // ...
}
move('up'); // ok
move('right'); // ts(2345) Argument of type '"right"' is not assignable to parameter of type 'Direction'

```
```
interface Config {
    size: 'small' | 'big';
    isEnable:  true | false;
    margin: 0 | 2 | 4;
}
```

### 类型拓宽
如果满足指定了初始值且未显式添加类型注解的条件，那么它们推断出来的类型就是指定的初始值字面量类型拓宽后的类型，这就是字面量类型拓宽。
```
  let str = 'this is string'; // 类型是 string
  let strFun = (str = 'this is string') => str; // 类型是 (str?: string) => string;
  const specifiedStr = 'this is string'; // 类型是 'this is string'
  let str2 = specifiedStr; // 类型是 'string'
```

### 类型缩小
```
{
  type Goods = 'pen' | 'pencil' |'ruler';
  const getPenCost = (item: 'pen') => 2;
  const getPencilCost = (item: 'pencil') => 4;
  const getRulerCost = (item: 'ruler') => 6;
  const getCost = (item: Goods) =>  {
    if (item === 'pen') {
      return getPenCost(item); // item => 'pen'
    } else if (item === 'pencil') {
      return getPencilCost(item); // item => 'pencil'
    } else {
      return getRulerCost(item); // item => 'ruler'
    }
  }
}
```

### 联合类型
联合类型表示取值可以为多种类型中的一种，使用 | 分隔每个类型。 
```
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven'; // OK
myFavoriteNumber = 7; // OK
```

联合类型通常与 null 或 undefined 一起使用：

```
const sayHello = (name: string | undefined) => {
  /* ... */
};
```

### 类型别名
类型别名用来给一个类型起个新名字。类型别名常用于联合类型。 
```
type Message = string | string[];
let greet = (message: Message) => {
  // ...
};
```

### 交叉类型 
交叉类型是将多个类型合并为一个类型

```
  type IntersectionType = { id: number; name: string; } & { age: number };
  const mixed: IntersectionType = {
    id: 1,
    name: 'name',
    age: 18
  }

```

### 接口
用接口去声明类型
interface Person {
    name: string;
    age: number;
}
let tom: Person = {
    name: 'Tom',
    age: 25
};
赋值的时候，变量的形状必须和接口的形状保持一致，多一个属性或者少一个属性都不行

#### 可选｜只读属性 
```
interface Person {
  readonly name: string;
  age?: number;
} 
``` 
只读属性用于限制只能在对象刚刚创建的时候修改其值。此外 TypeScript 还提供了 ReadonlyArray<T> 类型，它与 Array<T> 相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改。
```
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error!

```


#### 接口与类型别名的区别
实际上，在大多数的情况下使用接口类型和类型别名的效果等价，但是在某些特定的场景下这两者还是存在很大区别

1.两者都可以用来描述对象或函数的类型，但是语法不同

```
interface Point {
  x: number;
  y: number;
}

interface SetPoint {
  (x: number, y: number): void;
}
```

```
type Point = {
  x: number;
  y: number;
};

type SetPoint = (x: number, y: number) => void;

```

2.interface只能描述对象或者函数类型,类型别名还可以用于其他类型，如基本类型（原始值）、联合类型、元组。

```
type Name = string;

// object
type PartialPointX = { x: number; };
type PartialPointY = { y: number; };

// union
type PartialPoint = PartialPointX | PartialPointY;

// tuple
type Data = [number, string];

// dom
let div = document.createElement('div');
type B = typeof div;
```

3.接口可以定义多次，类型别名不可以  
```
interface Point { y: number; }
const point: Point = { x: 1, y: 2 };
``` 

4.扩展 

两者的扩展方式不同，但并不互斥。接口可以扩展类型别名，同理，类型别名也可以扩展接口。

接口的扩展就是继承，通过 extends 来实现。类型别名的扩展就是交叉类型，通过 & 来实现。  
接口扩展类型别名

```
type PointX = {
    x: number
}
interface Point extends PointX {
    y: number
}
``` 
类型别名扩展接口 
```
interface PointX {
    x: number
}
type Point = PointX & {
    y: number
}
``` 

### 泛型 
泛型就像函数传参
![alt 属性文本](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ba87e7a8921144faa4e8dd7dc8e92916~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp?)

其实并不是只能定义一个类型变量，我们可以引入希望定义的任何数量的类型变量。比如我们引入一个新的类型变量 U，用于扩展我们定义的 identity 函数：
```
function identity <T, U>(value: T, message: U) : T {
  console.log(message);
  return value;
}
console.log(identity<Number, string>(68, "Semlinker"));

```