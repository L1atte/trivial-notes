# 联合类型

表示多种类型之一，（它使用 `|` 作为标记，如 `string | number`）

TS 只允许适用于所有成员的操作

例如，以下行为是

```typescript
function printId(id: number | string) {
  console.log(id.toUpperCase());
  // Property 'toUpperCase' does not exist on type 'string | number'.
  // Property 'toUpperCase' does not exist on type 'number'.
}
```

# 交叉类型

# 元组`[type1,type2...]`

使用 `:[typeofmember1, typeofmember2]` 的形式，为元组添加类型注解

# 类型断言(`as` 或者 `<>`)

使用 `as` 或者尖括号语法 `<>` 来转换为更具体或者更不具体的类型

# 非空断言(后缀 `!`)

在任何表达式之后写入 `!` 实际上是一个类型断言，该值不是 `null` 和 `undefined`

# 类型谓词(`is`)

```typescript
function isStr(x: string | number) {
    return typeof x === "string";
}

function toUpperCase(x: string | number) {
    if (isStr(x)) {
        // 报错，x 的类型依旧为string | number，未被缩小
        x.toUpperCase(); 
    }
}
```

类型谓词的形式是 `parameterName is Type`，其中

* `parameterName` 必须是当前函数签名中的参数名称，
* `Type`是当前函数返回值为`true`时`parameterName`的类型，它必须包含于参数定义中的类型
* 所以类型谓词主要用于修饰函数返回`true`时的参数类型

```typescript
function isStr(x: string | number): x is string {
    return typeof x === "string";
}

function toUpperCase(x: string | number) {
    if (isStr(x)) {
        x.toUpperCase(); // x类型成功被缩小为string
    }
}
```

# 调用签名( `:` )

如果希望声明函数的属性的类型，可以在**对象类型**中写**调用签名**`:`

```typescript
type FnType = {
  age: number;
  (param: number): number;
};

function getFnAge(fn: FnType) {
  console.log(fn.age, fn(99));
}

function fn(a: number) {
  return a;
}
fn.age = '123'

// 报错，Types of property 'age' are incompatible.
getFnAge(fn)
```



# 构造签名(`new`)

JS 可以使用`new`运算符调用 JS 函数，TS 将其称之为构造函数，因为它们通常会创建一个新对象，我们可以在`调用签名`之前添加关键字来编写构造签名：`new`

```typescript
type SomeConstructor = {
  // 添加 new 后报错消失
  // new (s: string): object;
  (s: string): object;
};
function fn(ctor: SomeConstructor) {
  // 报错，'new' expression, whose target lacks a construct signature, implicitly has an 'any' type.
  return new ctor("hello");
}
```

# 泛型

把明确类型的工作推迟到创建对象或者调用方法的时候才去明确特殊的类型，简单来说可以将泛型理解为把类型当作参数来传递

```typescript
function identity<T>(arg: T): T {
  return arg;
}

// 调用identity时传入name，函数会自动推导出泛型T为string，自然arg类型为T，返回值类型也为T
const userName = identity('name');
// 同理，当然你也可以显示声明泛型
const id = identity<number>(1); 
```



# TS operator

## `keyof` 运算符

`keyof` 关键字代表它接受一个对象类型作为参数，并返回该对象所有 key 值组成的联合类型

```typescript
interface IProps {
  name: string;
  count: number;
}
type Ikea = keyof IProps; // Ikea = 'name' | 'count'
```



## `extends` 泛型约束

通过 `extends` 关键字可以约束泛型具有某些属性

> `extends`关键字表示约束的时候，就是表示要求泛型上必须实现包含约束的属性

```typescript
interface Lengthwise {
  length: number
}

// 表示传入的泛型T接受Lengthwise的约束
// T必须实现Lengthwise 换句话说 Lengthwise这个类型是完全可以赋给T
function loggingIdentity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length) // OK
  return arg
}
```



## `inner` 待推断类型

**我们类型定义时并不能立即确定某些类型，而是在使用类型时来根据条件来推断对应的类型**

需要注意的是 infer 关键字类型，必须结合 Conditional Types 条件判断来使用。

## `+` or `-` mapping modifiers

用于修饰 只读修饰符 `readonly` 和 可选修饰符 `?`

`+`: 表示添加修饰符

`-`: 表示移除修饰符

```typescript
// Removes 'readonly' attributes from a type's properties
type CreateMutable<Type> = {
  -readonly [Property in keyof Type]: Type[Property];
};
 
type LockedAccount = {
  readonly id: string;
  readonly name: string;
};
 
type UnlockedAccount = CreateMutable<LockedAccount>;
/** 
*	type UnlockedAccount = {
*   id: string;
*   name: string;
* }
*/
```

## `as const`

as const 用于将 `object` 的 key 收缩为更具体的字面量类型，而不是一般的 `string`, `number`

```typescript
const req = { url: "https://example.com", method: "GET" } as const;
typeof req
/* {
		 readonly url: "https://example.com";
		 readonly method: "GET";
   }*/
```



# TypeScript 高级概念

## distributive conditional types（分发）

分发指的是：使用联合类型做条件类型（conditional type）判断的时候，联合类型中的每一个成员都会参与条件类型判断

大多数联合类型的工具类型都是利用分发特性做的，举例：

```typescript
// Exclude<T, k>
type MyExclude<T, K extends T> = T extends K ? never : T

// test
type Bar = MyExclude<'a' | 'b' | 'c' | 'd', 'b'> 
// type Bar = "a" | "c" | "d"
```

分发的条件：

1. 必须是在 extends 的条件类型判断中才有分发
2. 只有联合类型才有分发

## 遍历

使用 `in` 操作符可以进行遍历操作，但遍历的对象必须是联合类型

```typescript
type Bar = ['a', 'b', 'c', 'd']
type Foo = 'a' | 'b' | 'c' | 'd'

type A = { [key in Foo]: string } // OK
type B = { [key in Bar]: string } // Error
```

遍历在构造对象类型的时候十分有效，比如

```typescript
// Partial<T>
type MyPartial = {
	[key in keyof T]?: T[p]
}
```



## 逆变（covariant）

如果有 A << B，而 F(A) >> F(B)，那么称 F 是逆变的（父子关系相反）

## 协变（contravariant）

如果有 A << B，而 F(A) << F(B)，那么称 F 是协变的（父子关系不变）

## 双向协变（bivariant）

既是协变又是逆变

## 不变（invariant）

不存在分配关系，或者说无法互相分配

## 函数参数类型是逆变的

在函数类型赋值兼容时，函数参数少的向下兼容函数参数多的

## 函数返回值类型是协变的

在函数类型赋值兼容时，返回值多的向下兼容返回值少的

## TypeScript 默认认为函数方法的入参是双向协变的

## 

# TypeScript 内置类型

## `Awaited<Type>`

## `Partial`

## `Required`

## `Readonly`

## `Record

## `Exclude<UnionType, ExcludedMembers>`

从联合类型 `UnionType` 中排除成员

```typescript
type T0 = Exclude<"a" | "b" | "c", "a">;
// type T0 = "b" | "c"
```



## `Extract<UnionType, Type>`

从联合类型 `UnionType` 中提取 `Type` 中的成员

```typescript
type T0 = Extract<"a" | "b" | "c", "a" | "f">;
// type T0 = "a"
```

## `Omit<Type, Keys>`

从对象类型中排除属性

```typescript
interface Todo {
  title: string;
  description: string;
}
 
type TodoPreview = Omit<Todo, "description">;
// type TodoPreview = { title: string; }
```



## `Pick<Type, Keys>`

从对象类型中选中属性

```typescript
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}
 
type TodoPreview = Pick<Todo, "title" | "completed">;
// type TodoPreview { title: string; completed: boolean; }
```



## NonNullable

## `Parameters<Type>`

将函数的参数类型组成的元组类型

```typescript
type F = (a: number, b: string, c: boolean) => void
type Params = Parameters<F>
// type Params = [a: number, b: string, c: boolean]
```



## `ConstructorParameters<Type>`

将构造函数的参数类型组成元组类型

```typescript
type T0 = ConstructorParameters<ErrorConstructor>;
// type T0 = [message?: string]
```



## `ReturnType<Type>`

返回一个由 `Type` 函数返回值组成的类型

```typescript
type T0 = ReturnType<() => string>;
// type T0 = string
```



## `InstanceType<Type>`

返回一个由`Type`构造函数的实例类型组成的类型

```typescript
class C {
  x = 0;
  y = 0;
}
type T0 = InstanceType<typeof C>;
// type T0 = c
```

