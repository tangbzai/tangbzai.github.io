---
title: Utility Types
date: 2021-10-12 19:22:53
tags:
 - TypeScript
---
## Utility Types

TypeScript 提供了几种实用的类型以进行常见的类型转换。这些类型可在全局范围内使用。

> [Utility Types 官方文档](https://www.typescriptlang.org/docs/handbook/utility-types.html#returntypetype)

### `Partial<Type>`

> 发布版本: [2.1](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-1.html#partial-readonly-record-and-pick)

> 构造一个类型，`Type` 的所有属性都设置为可选。此实用类型返回一个表示给定类型的所有子集的类型。

```ts
interface Props {
  a: string;
  b: number;
}

type PartialProps = Partial<Props>;

/** 
 * type PartialProps {
 *  a?: string;
 *  b?: number;
 * }
 */
```

### `Required<Type>`
> 发布版本: [2.8](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-8.html#improved-control-over-mapped-type-modifiers)

> 构造一个类型，`Type` 的所有属性都设置为必选。此类型返回一个传入类型的所有子集类型。与[`Partial`](#Partial<T>)相对。

```ts
interface Props {
  a?: string;
  b?: number;
}

type RequiredProps = Required<Props>;

/** 
 * type RequiredProps {
 *  a: string;
 *  b: number;
 * }
 */
```

### `Readonly<Type>`

> 发布版本: [2.1](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-1.html#partial-readonly-record-and-pick)

> 构造类型时，`Type` 所有属性都设置为只读，这意味着无法重新分配构造类型的属性。

```ts
interface Todo {
  title: string;
}
 
const todo: Readonly<Todo> = {
  title: "Delete inactive users",
};

todo.title = "Hello";
/**
 * error
 * Cannot assign to 'title' because it is a read-only property.
 **/
```
> 这个类型主要用于在运行时赋值会失败的表达式（即，尝试重新分配冻结对象的属性时）

```ts
function freeze<Type>(obj: Type): Readonly<Type>;
```

### `Record<Key, Type>`

> 发布版本: [2.1](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-1.html#partial-readonly-record-and-pick)

> 构造一个对象类型，其属性为`Key`，属性值为`Type`。这个类型可用于将一个类型的属性映射到另一个类型。

```ts
interface CatInfo {
  age: number;
  breed: string;
}
 
type CatName = "miffy" | "boris" | "mordred";
 
const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: "Persian" },
  boris: { age: 5, breed: "Maine Coon" },
  mordred: { age: 16, breed: "British Shorthair" },
};
 
cats.boris;
/**
 * const cats: Record<CatName, CatInfo>
 * cats.boris: CatInfo
 */
```

### `Pick<Type, Keys>`

> 发布版本: [2.1](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-1.html#partial-readonly-record-and-pick)

> 通过从 `Type` 中拾取属性`Keys`(字符串类型或字符串文本的并集)来构造类型。

```ts
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}
 
type TodoPreview = Pick<Todo, "title" | "completed">;

/**
 * type TodoPreview {
 *  title: string;
 *  completed: boolean;      
 * }
 */
```

### `Omit<Type, Keys>`

> 发行版本：[3.5](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-5.html#the-omit-helper-type)

> 通过从类型中拾取所有属性，然后删除`Keys`(字符串类型或字符串文本的并集)来构造类型。

```ts
interface Todo {
  title: string;
  description: string;
  completed: boolean;
  createdAt: number;
}
 
type TodoPreview = Omit<Todo, "description" | "completed">;
/**
 * type TodoPreview {
 *   title: string;
 *   createdAt: number;
 * }
 */
```

### `Exclude<UnionType, ExcludedMembers>`

> 发行版本：[2.8](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-8.html#predefined-conditional-types)

> 通过从 `UnionType` 中排除可分配给 `ExcludedMembers` 的所有联合成员来构造类型。

```ts
type T0 = Exclude<"a" | "b" | "c", "a">;
/**
 * type T0 = "b" | "c"
 */
 
type T1 = Exclude<"a" | "b" | "c", "a" | "b">;
/**
 * type T1 = "c"
 */
 
type T2 = Exclude<string | number | (() => void), Function>;
/**
 * type T2 = string | number
 */
```

### `Extract<Type, Union>`

> 发行版本：[2.8](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-8.html#predefined-conditional-types)

> 从 `Type` 中提取可分配给 `Union` 的所有 `Union` 成员来构造类型。

```ts
type T0 = Extract<"a" | "b" | "c", "a" | "f">;
/**
 * type T0 = "a"
 */

type T1 = Extract<string | number | (() => void), Function>;
/**
 * type T1 = () => void
 */
```

### `NonNullable<Type>`

> 发行版本：[2.8](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-8.html#predefined-conditional-types)

> 通过从 `Type` 中排除 `null` 和 `undefined` 来构造类型。

```ts
type T0 = NonNullable<string | number | undefined>;
/**
 * type T0 = string | number
 */
type T1 = NonNullable<string[] | null | undefined>;
/**
 * type T1 = string[]
 */
```

### `Parameters<Type>`

> 发行版本：[3.1](https://github.com/microsoft/TypeScript/pull/26243)

> 从函数类型 `Type` 的参数中使用的类型构造元组类型

```ts
declare function f1(arg: { a: number; b: string }): void;
 
type T0 = Parameters<() => string>;
/**
 * type T0 = []
 */
type T1 = Parameters<(s: string) => void>;
/**
 * type T1 = [s: string]
 */
type T2 = Parameters<<T>(arg: T) => T>;
/**
 * type T2 = [arg: unknown]
 */
type T3 = Parameters<typeof f1>;
/**
 * type T3 = [arg: {
 *   a: number;
 *   b: string;
 * }]
 */
type T4 = Parameters<any>;
/**
 * type T4 = unknown[]
 */
type T5 = Parameters<never>;
/**
 * type T5 = never
 */
type T6 = Parameters<string>;
/** error
 * Type 'string' does not satisfy the constraint '(...args: any) => any'.
 *
 * type T6 = never
 */
type T7 = Parameters<Function>;
/** error
 * Type 'Function' does not satisfy the constraint '(...args: any) => any'.
 *   Type 'Function' provides no match for the signature '(...args: any): any'.
 *
 * type T7 = never
 */
```

### `ConstructorParameters<Type>`

> 发行版本：[3.1](https://github.com/microsoft/TypeScript/pull/26243)

> 从构造函数类型的类型构造元组或数组类型。它生成一个包含所有参数类型的元组类型（如果 `type` 不是函数，则类型为 `never` ）。

```ts
type T0 = ConstructorParameters<ErrorConstructor>;
/**
 * type T0 = [message?: string]
 */
type T1 = ConstructorParameters<FunctionConstructor>;
/**
 * type T1 = string[]
 */
type T2 = ConstructorParameters<RegExpConstructor>;
/**
 * type T2 = [pattern: string | RegExp, flags?: string]
 */
type T3 = ConstructorParameters<any>;
/**
 * type T3 = unknown[]
 */
 
type T4 = ConstructorParameters<Function>;
/** error
 * Type 'Function' does not satisfy the constraint 'abstract new (...args: any) => any'.
 *   Type 'Function' provides no match for the signature 'new (...args: any): any'.
 * 
 * type T4 = never
 */
```

### `ReturnType<Type>`

> 发行版本：[2.8](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-8.html#predefined-conditional-types)

> 构造由函数`Type`的返回类型组成的类型

```ts
declare function f1(): { a: number; b: string };
 
type T0 = ReturnType<() => string>;
/**
 * type T0 = string
 */
type T1 = ReturnType<(s: string) => void>;
/**
 * type T1 = void
 */
type T2 = ReturnType<<T>() => T>;
/**
 * type T2 = unknown
 */
type T3 = ReturnType<<T extends U, U extends number[]>() => T>;
/**
 * type T3 = number[]
 */
type T4 = ReturnType<typeof f1>;
/**
 * type T4 = {
 *   a: number;
 *   b: string;
 * }
 */
type T5 = ReturnType<any>;
/**
 * type T5 = any
 */
type T6 = ReturnType<never>;
/**
 * type T6 = never
 */
type T7 = ReturnType<string>;
/** error
 * Type 'string' does not satisfy the constraint '(...args: any) => any'.
 *
 * type T7 = any
 */
type T8 = ReturnType<Function>;
/**
 * Type 'Function' does not satisfy the constraint '(...args: any) => any'.
 *   Type 'Function' provides no match for the signature '(...args: any): any'.
 *
 * type T8 = any
 */
```

### `InstanceType<Type>`

> 发行版本：[2.8](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-8.html#predefined-conditional-types)

> 构造由 `Type` 中构造函数的实例类型组成的类型。

```ts
class C {
  x = 0;
  y = 0;
}
 
type T0 = InstanceType<typeof C>;
/**
 * type T0 = C
 */
type T1 = InstanceType<any>;
/**
 * type T1 = any
 */
type T2 = InstanceType<never>;
/**
 * type T2 = never
 */
type T3 = InstanceType<string>;
/** error
 * Type 'string' does not satisfy the constraint 'abstract new (...args: any) => any'.
 *
 * type T3 = any
 */
type T4 = InstanceType<Function>;
/**
 * Type 'Function' does not satisfy the constraint 'abstract new (...args: any) => any'.
 *   Type 'Function' provides no match for the signature 'new (...args: any): any'.
 *
 * type T4 = any
 */
```

### `ThisParameterType<Type>`

> 发行版本： [3.3](https://github.com/microsoft/TypeScript/pull/28920)

> 从函数 `Type` 内返回 `this` 参数的类型，若函数类型并没有 `this` 参数，则返回 `unknown`

```ts
function toHex(this: Number) {
  return this.toString(16);
}
 
function numberToString(n: ThisParameterType<typeof toHex>) {
  return toHex.apply(n);
}
```

### `OmitThisParameter<Type>`

> 发行版本：[3.3](https://github.com/microsoft/TypeScript/pull/28920)

> 从 `Type` 中删除 `this` 参数。如果 `Type` 没有显式声明 `this` 参数，则结果只是 `Type`。否则将从 `type` 创建不带 `this` 参数的新函数。泛型会被擦除，并且只有最后一个重载签名被传播到新的函数类型中。

```ts
function toHex(this: Number) {
  return this.toString(16);
}
 
const fiveToHex: OmitThisParameter<typeof toHex> = toHex.bind(5);
/**
 * const fiveToHex: () => string
 */
```

### `ThisType<Type>`

> 发行版本：[2.3](https://github.com/microsoft/TypeScript/pull/14141)

> 这个 Utility 并不返回已转换的类型，反而作为一个标记上下文中 `this` 的类型。<br />请注意，必须启用 [`noImplicitThis`](https://www.typescriptlang.org/tsconfig#noImplicitThis) 才能使用此 Utility。

```ts
type ObjectDescriptor<D, M> = {
  data?: D;
  methods?: M & ThisType<D & M>; // Type of 'this' in methods is D & M
};
 
function makeObject<D, M>(desc: ObjectDescriptor<D, M>): D & M {
  let data: object = desc.data || {};
  let methods: object = desc.methods || {};
  return { ...data, ...methods } as D & M;
}
 
let obj = makeObject({
  data: { x: 0, y: 0 },
  methods: {
    moveBy(dx: number, dy: number) {
      // 这里的 this 可以推断出来存在 x, y 属性
      this.x += dx;
      this.y += dy;
    },
  },
});
```

> 在上面的示例中，`makeObject`的参数中的`methods`对象有一个上下文类型，其中包含`ThisType<D&M>`，因此`methods`对象中的`ThisType`的类型是`{x:number，y:number} & {moveBy（dx:number，dy:number）：number}`。<br />
> 请注意，methods属性的类型如何同时是方法中此类型的推理目标和源。

> `ThisType<T>`标记接口只是在`lib.d.ts`中声明的空接口。 除了在对象文本的上下文类型中被识别之外，接口的行为就像空接口。