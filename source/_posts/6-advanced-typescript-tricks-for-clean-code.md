---
title: 让代码更简洁的6个高级 TypeScript 技巧
date: 2024-04-16 14:41:28
updated: 2024-04-16 14:41:28
series: TypeScript 使用技巧
tags:
  - TypeScript
  - JavaScript
---

> 本篇将介绍六种高级 TypeScript 技巧，并通过示例逐步说明每种技巧的作用和优点。

# 高级类型

我们可以使用高级 TypeScript 类型（如映射类型和条件类型）在现有类型的基础上创建新类型。借助这些类型，可以以更强的方式更改和操作类型，从而提高代码的灵活性和可维护性。

## 1. 映射类型

映射类型是 TypeScript 中的一种高级类型，它可以根据现有类型创建新类型。通过映射类型，我们可以轻松地将现有类型的每个属性转换为新类型的属性。

```typescript
type Person = {
  name: string;
  age: number;
};

type ReadonlyPerson = {
  readonly [K in keyof Person]: Person[K];
};

const person: ReadonlyPerson = {
  name: 'Alice',
  age: 30,
};

// Error: Cannot assign to 'name' because it is a read-only property
person.name = 'Bob';
```

在上面的示例中，我们定义了一个 `Person` 类型，然后使用映射类型 `ReadonlyPerson` 将 `Person` 类型的所有属性转换为只读属性。这样一来，我们就无法修改 `ReadonlyPerson` 类型的属性。

## 2. 条件类型

条件类型是 TypeScript 中的一种高级类型，它可以根据条件表达式选择不同的类型。通过条件类型，我们可以根据不同的条件返回不同的类型。

```typescript
type IsString<T> = T extends string ? 'yes' : 'no';

type A = IsString<string>; // 'yes'

type B = IsString<number>; // 'no'
```

在上面的示例中，我们定义了一个条件类型 `IsString`，它根据泛型 `T` 是否为 `string` 类型返回不同的类型。通过条件类型，我们可以根据不同的条件返回不同的类型。

# 装饰器

TypeScript 中的装饰器是一项强大的功能，允许添加元数据，修改或扩展类、方法、属性和参数的行为。它们是高阶函数，可用于观察、修改或替换类定义、方法定义、访问器定义、属性定义或参数定义。

## 1. 类装饰器

类装饰器是一种装饰器，用于装饰类定义。**接收一个参数，该参数是类的构造函数**。类装饰器可以用于观察、修改或替换类定义。

```typescript
function LogClass(target: Function) {
  console.log('Class:', target.name);
}

@LogClass
class Person {
  name: string;

  constructor(name: string) {
    this.name = name;
  }
}

// Output: Class: Person
```

在上面的示例中，我们定义了一个类装饰器 `LogClass`，它用于观察类定义。通过 `@LogClass` 语法，我们将 `LogClass` 装饰器应用于 `Person` 类，从而观察 `Person` 类的名称。

## 2. 方法装饰器

方法装饰器是一种装饰器，用于装饰方法定义。它接收三个参数，分别是**目标对象、方法名称和方法描述符**。方法装饰器可以用于观察、修改或替换方法定义。

```typescript
function LogMethod(target: any, key: string, descriptor: PropertyDescriptor) {
  console.log('Method:', key);
}

class Person {
  @LogMethod
  greet() {
    console.log('Hello, world!');
  }
}

// Output: Method: greet
```

在上面的示例中，我们定义了一个方法装饰器 `LogMethod`，它用于观察方法定义。通过 `@LogMethod` 语法，我们将 `LogMethod` 装饰器应用于 `greet` 方法，从而观察 `greet` 方法的名称。

## 3. 属性装饰器

属性装饰器是一种装饰器，用于装饰属性定义。它接收两个参数，分别是**目标对象和属性名称**。属性装饰器可以用于观察、修改或替换属性定义。

```typescript
function LogProperty(target: any, key: string) {
  console.log('Property:', key);
}

class Person {
  @LogProperty
  name: string;
}

// Output: Property: name
```

在上面的示例中，我们定义了一个属性装饰器 `LogProperty`，它用于观察属性定义。通过 `@LogProperty` 语法，我们将 `LogProperty` 装饰器应用于 `name` 属性，从而观察 `name` 属性的名称。

## 4. 参数装饰器

参数装饰器是一种装饰器，用于装饰参数定义。它接收三个参数，分别是**目标对象、方法名称和参数索引**。参数装饰器可以用于观察、修改或替换参数定义。

```typescript
function LogParameter(target: any, key: string, index: number) {
  console.log('Parameter:', index);
}

class Person {
  greet(@LogParameter message: string) {
    console.log(message);
  }
}

// Output: Parameter: 0
```

在上面的示例中，我们定义了一个参数装饰器 `LogParameter`，它用于观察参数定义。通过 `@LogParameter` 语法，我们将 `LogParameter` 装饰器应用于 `message` 参数，从而观察 `message` 参数的索引。

# 命名空间

命名空间是 TypeScript 中的一种模块化机制，用于将代码组织到逻辑分组中。通过命名空间，我们可以避免全局作用域的污染，提高代码的可维护性和可重用性。

## 1. 声明命名空间

命名空间可以通过 `namespace` 关键字声明。命名空间中的代码可以通过 `export` 关键字导出，以便在其他文件中使用。

```typescript
namespace Math {
  export function add(a: number, b: number): number {
    return a + b;
  }
}

console.log(Math.add(1, 2)); // Output: 3
```

在上面的示例中，我们声明了一个命名空间 `Math`，并在其中定义了一个 `add` 函数。通过 `export` 关键字，我们将 `add` 函数导出，以便在其他文件中使用。

## 2. 引用命名空间

命名空间可以通过 `/// <reference path="..." />` 指令引用。通过引用命名空间，我们可以在当前文件中使用命名空间中的代码。

```typescript
/// <reference path="math.ts" />

console.log(Math.add(1, 2)); // Output: 3
```

在上面的示例中，我们使用 `/// <reference path="math.ts" />` 指令引用了 `math.ts` 文件中的命名空间 `Math`。通过引用命名空间，我们可以在当前文件中使用 `Math` 命名空间中的代码。

## 3. 嵌套命名空间

命名空间可以嵌套声明，以便更好地组织代码。通过嵌套命名空间，我们可以将相关代码组织到逻辑分组中，提高代码的可维护性和可重用性。

```typescript
namespace Math {
  export namespace Advanced {
    export function multiply(a: number, b: number): number {
      return a * b;
    }
  }
}

console.log(Math.Advanced.multiply(2, 3)); // Output: 6
```

在上面的示例中，我们声明了一个嵌套命名空间 `Math.Advanced`，并在其中定义了一个 `multiply` 函数。通过嵌套命名空间，我们可以更好地组织代码，提高代码的可维护性和可重用性。

# Mixins

TypeScript 中的混合类是由多个较小部分（称为混合类）组成类的一种方式。它们允许您在不同的类之间重用和共享行为，从而促进模块化和代码的可重用性。

## 1. 定义混合类

混合类是由多个较小部分（称为混合类）组成类的一种方式。通过混合类，我们可以在不同的类之间重用和共享行为。

```typescript
class Jumpable {
  jump() {
    console.log('Jumping');
  }
}

class Duckable {
  duck() {
    console.log('Ducking');
  }
}

class Character implements Jumpable, Duckable {
  jump: Jumpable['jump'];
  duck: Duckable['duck'];

  constructor() {
    this.jump = Jumpable.prototype.jump.bind(this);
    this.duck = Duckable.prototype.duck.bind(this);
  }
}

const character = new Character();
character.jump(); // Output: Jumping
character.duck(); // Output: Ducking
```

在上面的示例中，我们定义了两个混合类 `Jumpable` 和 `Duckable`，它们分别定义了 `jump` 和 `duck` 方法。然后，我们定义了一个 `Character` 类，它实现了 `Jumpable` 和 `Duckable` 接口，并在构造函数中绑定了 `jump` 和 `duck` 方法。

## 2. 使用混合类

混合类可以在不同的类之间重用和共享行为。通过混合类，我们可以将相同的行为添加到不同的类中，从而促进模块化和代码的可重用性。

```typescript
class Flyable {
  fly() {
    console.log('Flying');
  }
}

class Character implements Jumpable, Duckable, Flyable {
  jump: Jumpable['jump'];
  duck: Duckable['duck'];
  fly: Flyable['fly'];

  constructor() {
    this.jump = Jumpable.prototype.jump.bind(this);
    this.duck = Duckable.prototype.duck.bind(this);
    this.fly = Flyable.prototype.fly.bind(this);
  }
}

const character = new Character();
character.jump(); // Output: Jumping
character.duck(); // Output: Ducking
character.fly(); // Output: Flying
```

# 类型保护

类型保护是 TypeScript 中的一种机制，用于在运行时检查类型。通过类型保护，我们可以在运行时检查类型，并根据类型执行不同的操作。

要定义类型保护，需要创建一个接收变量或参数并返回类型谓词的函数。类型谓词是一个布尔表达式，用于缩小函数范围内参数的类型。

```typescript
function isString(value: any): value is string {
  return typeof value === 'string';
}

const value: any = 'Hello, world!';
if (isString(value)) {
  console.log(value.toUpperCase()); // Output: HELLO, WORLD!
}
```

在上面的示例中，我们定义了一个类型保护 `isString`，它用于检查变量是否为 `string` 类型。通过类型保护，我们可以在运行时检查类型，并根据类型执行不同的操作。

# utility-types

TypeScript 中的实用程序类型是一组内置类型，用于操作和转换其他类型。通过实用程序类型，我们可以轻松地操作和转换类型，从而提高代码的灵活性和可维护性。

```typescript
interface Person {
  name: string;
  age: number;
  email: string;
}

type PartialPerson = Partial<Person>;
type ReadonlyPerson = Readonly<Person>;
type NameAndAge = Pick<Person, "name" | "age">;
type WithoutEmail = Omit<Person, "email">;
```

在上面的示例中，我们使用实用程序类型 `Partial`、`Readonly`、`Pick` 和 `Omit` 分别创建了 `PartialPerson`、`ReadonlyPerson`、`NameAndAge` 和 `WithoutEmail` 类型。

## Partial

> `Partial` 类型将所有属性设置为可选属性。

```typescript
const person: PartialPerson = {
  name: 'Alice',
  age: 30,
};

// Output: { name: 'Alice', age: 30 }
console.log(person);
```

## Readonly

> `Readonly` 类型将所有属性设置为只读属性。

```typescript
const person: ReadonlyPerson = {
  name: 'Alice',
  age: 30,
  email: ''
};

// Error: Cannot assign to 'name' because it is a read-only property
person.name = 'Bob';
```

## Pick

> `Pick` 类型从现有类型中选择指定的属性。

```typescript
const person: NameAndAge = {
  name: 'Alice',
  age: 30,
};

// Output: { name: 'Alice', age: 30 }
console.log(person);
```

## Omit

> `Omit` 类型从现有类型中排除指定的属性。

```typescript
const person: WithoutEmail = {
  name: 'Alice',
  age: 30,
};

// Output: { name: 'Alice', age: 30 }
console.log(person);
```

# 总结

总之，本文探讨了各种高级 TypeScript 主题，如命名空间、高级类型、装饰器、混合体、类型保护和实用类型。通过了解和利用这些功能，您可以创建更多模块化、可重用和可维护的代码，从而遵守最佳实践并降低运行时出错的可能性。

通过利用这些高级 TypeScript 功能，您可以编写更简洁、更有条理和可维护的代码，充分利用 TypeScript 强大的类型系统和语言功能。
