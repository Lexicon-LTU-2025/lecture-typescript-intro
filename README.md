# Introduction to Typescript

<details open>
<summary>Table of content</summary>

- [Comments](#comments)

- [Variables](#variables)

  - [let](#let)
  - [const](#const)
  - [var](#var)

- [Data Types](#data-types)

  - [Number](#number)
  - [String](#string)
  - [Boolean](#boolean)
  - [Null](#null)
  - [Undefined](#undefined)
  - [Any](#any)
  - [Unknown](#unknown)
  - [Summary of data types](#summary)

- [Functions](#functions)

  - [Anonymous Functions](#anonymous-functions)
  - [Arrow Functions](#arrow-functions)

- [Objects](#objects)
  - [Comparing TypeScript Objects to C# Constructs](#comparing-typescript-objects-to-c-constructs)
- [Interface](#interface)
- [Types](#types)
- [Union Types](#union-types)

</details>

## Comments

TypeScript uses the same comment syntax as JavaScript.

Single-line comment

```ts
// This is a single-line comment
```

Multi-line comment

```ts
/*
 This is a multi-line comment
 that spans several lines
*/
```

[Back to top](#introduction-to-typescript)

## Variables

In JavaScript, you declare variables using `let`, `const`, or `var`. These keywords determine whether a variable can be changed, and how it behaves in different scopes (like inside functions or blocks).

### let

Use `let` when you need a variable whose value may change later. It is block-scoped, meaning it only exists within the block `{ } `where it's defined.

```ts
let count = 1;
count = 2; // ✅ OK
```

Here is an example on how it works within and outside a defined scope.

```ts
if (true) {
  let message = 'inside block';
  console.log(message); // OK
}
console.log(message); // ❌ Error: message is not defined
```

✅ Use `let` by default for variables that need to change.

[Back to top](#introduction-to-typescript)

---

### const

Use `const` when you don’t want the variable to be reassigned. It's also block-scoped like `let`.

```ts
const name: string = 'Alice';
name = 'Bob'; // ❌ Error: Cannot assign to 'name' because it is a constant
```

> 💡 You can still change properties of objects declared with `const`, or elements inside an array — only the variable itself can’t be reassigned. This is because objects and arrays are a reference type, but more on that later.

✅ Use `const` whenever possible — it makes your code more predictable.

[Back to top](#introduction-to-typescript)

---

### var

In JavaScript, var is the old way of declaring variables and should be avoided. It is function-scoped, not block-scoped, which can lead to confusing behavior.

```ts
if (true) {
  var greeting = 'hello';
}
console.log(greeting); // ✅ This works — greeting is hoisted to the function scope
```

> 📌 Note: If `var` is used outside any function, then the entire file acts as the function scope — so the variable becomes accessible throughout the file _( or globally in scripts without modules )_.

> ⚠️ Important difference from C#:
> In C#, `var` means "let the compiler infer the type", but the variable is still strongly typed and block-scoped.

In TypeScript, `var` means "old-style variable declaration" — it does not trigger type inference _( TypeScript infers types regardless of `let` or `var` )_ and uses function scope, not block scope.

✅ Use `let` or `const` in TypeScript instead. They behave more like what you're used to in C#.

[Back to top](#introduction-to-typescript)

```ts
// TypeScript: avoid this
var total = 100;

// C#: this is fine and typesafe
var total = 100;
```

---

## Data Types

TypeScript is statically typed, which means you must define (or infer) the type of each variable.

### Number

JavaScript uses a single number type to represent all numeric values — there’s no distinction between integers and floating-point numbers like in C#.

| C# Type   | TypeScript Equivalent |
| --------- | --------------------- |
| `byte`    | `number`              |
| `short`   | `number`              |
| `int`     | `number`              |
| `long`    | `number`              |
| `float`   | `number`              |
| `double`  | `number`              |
| `decimal` | `number`              |

```ts
let byteValue: number = 255;
let shortValue: number = -32000;
let intValue: number = 123456;
let longValue: number = 9007199254740991; // max safe integer in JS
let floatValue: number = 3.14;
let doubleValue: number = 2.718281828;
let decimalValue: number = 99.99;
```

All of these values are treated the same way internally: as 64-bit floating point numbers. Be cautious with precision when working with large integers or financial values because JavaScript (and TypeScript) uses IEEE 754 double-precision floats under the hood for all number values, which can cause:

1. **Precision loss with large integers** — numbers above `Number.MAX_SAFE_INTEGER` (≈ 9 quadrillion) may be rounded incorrectly.

   ```ts
   console.log(9007199254740991 + 1); // 9007199254740992 ✅
   console.log(9007199254740991 + 2); // 9007199254740992 ❌
   ```

2. **Precision issues with decimals** — common in financial calculations:

   ```ts
   console.log(0.1 + 0.2); // 0.30000000000000004 ❌
   ```

In those cases, it's safer to use libraries like [`decimal.js`](https://mikemcl.github.io/decimal.js/) or [`big.js`](https://github.com/MikeMcl/big.js) for accurate math, especially in money-related code.

[Back to top](#introduction-to-typescript)

---

### String

In TypeScript, string is used to represent text. It replaces C# types like char, string, and string interpolation syntax.

You can use:

Single quotes '...'

Double quotes "..."

Backticks `...` for template literals _( preferred when inserting variables, just like interpolated string using the $ symbol in C# )_

```ts
const firstName: string = 'Alice';
const lastName: string = 'Smith';
const fullName: string = `${firstName} ${lastName}`; // template literal
```

You can also perform string operations:

```ts
let message: string = 'Hello';
message = message + ', world!'; // concatenation

// String manipulation examples
const length: number = message.length; // property: get string length
const upper: string = message.toUpperCase(); // method: convert to uppercase
```

JavaScript strings come with many built-in properties and methods for string manipulation — like .length, .toUpperCase(), .toLowerCase(), .includes(), .replace(), and more.

These operations are commonly referred to as string manipulation — changing, analyzing, or working with string data in various ways.

> 💡 **Note:** There's no separate `char` type in TypeScript — a single character is just a `string` of length `1`.

[Back to top](#introduction-to-typescript)

---

### Boolean

The boolean type in JavaScript represents a logical value: true or false.

```ts
let isLoggedIn: boolean = true;
let hasPermission: boolean = false;
```

You often use booleans in conditional logic:

```ts
if (isLoggedIn) {
  console.log('Welcome back!');
} else {
  console.log('Please log in.');
}
```

> 💡 Note: Unlike C#, you can't use 0 or 1 in place of false or true in strict TypeScript — you must use actual boolean values.

---

### Null

Represents an intentional absence of any value.

```ts
let empty: null = null;
```

---

### Undefined

Represents a value that hasn't been assigned yet.

```ts
let notAssigned: undefined = undefined;
```

This variable will also be given the value `undefined`:

```ts
let name;
```

---

### Any

Disables type checking — can hold any type. This should be avoided at all costs, as it turns off TypeScript's safety features completely.

```ts
let anything: any = 'text';
anything = 42;
anything = true;
```

---

### Unknown

Like `any`, but safer — you must check its type before using it.

```ts
let value: unknown = 'hello';

if (typeof value === 'string') {
  console.log(value.toUpperCase());
}
```

---

### Summary

| Type        | Category           | Description                             |
| ----------- | ------------------ | --------------------------------------- |
| `number`    | **Primitive type** | All numbers (integer, float, etc.)      |
| `string`    | **Primitive type** | Textual data                            |
| `boolean`   | **Primitive type** | `true` or `false`                       |
| `null`      | **Primitive type** | Intentional absence of value            |
| `undefined` | **Primitive type** | Uninitialized value                     |
| `any`       | **Special type**   | Turns off type checking — avoid         |
| `unknown`   | **Special type**   | Like `any`, but safer — must be checked |

#### Advanced types _( not going to cover )_

| Type     | Category           | Description                                                           |
| -------- | ------------------ | --------------------------------------------------------------------- |
| `bigint` | **Primitive type** | Very large integers (e.g. beyond `Number.MAX_SAFE_INTEGER`)           |
| `symbol` | **Primitive type** | Unique identifiers, mostly used in advanced object scenarios          |
| `never`  | **Special type**   | Represents values that never occur (e.g. function that always throws) |

[Back to top](#introduction-to-typescript)

## Arrays

An array is a list that holds multiple values of the same type. TypeScript ensures type safety, so all elements in an array must match the declared type.

In TypeScript, arrays behave more like List<T> in C# — you can grow or shrink them easily using methods like .push() or .pop(). These are called array methods and there exist a lot of them and it is something we will cover as we go.

There are two common ways to declare arrays:

```ts
const scores: number[] = [90, 85, 70]; // square bracket syntax
const names: Array<string> = ['Alice', 'Bob']; // generic syntax, not very common.
```

In this course, we will use the square bracket syntax consistently. It's straightforward to use — simply specify the type, followed by a pair of square brackets.

Always use const when declaring arrays, since arrays are reference types — they point to a memory location where the actual data is stored.

Using const prevents reassignment of the array variable itself, but does not make the array immutable. You can still modify its contents (e.g., add or remove items) because the reference stays the same — only the internal data changes.

```ts
const items: string[] = ['apple', 'banana'];
items.push('orange'); // ✅ allowed
items = []; // ❌ Error: assignment to constant variable
```

Just to showcase the type safety:

```ts
const numbers: number[] = [1, 2, 3];

numbers.push(4); // ✅ OK
numbers.push('five'); // ❌ Error: Argument of type 'string' is not assignable to parameter of type 'number'
```

> 💡 TypeScript enforces type safety, so if you declare number[], only numbers are allowed. Trying to push a string _( or any other type )_ will result in a compile-time error.

[Back to top](#introduction-to-typescript)

## Functions

Functions allow you to group and reuse logic. In TypeScript, you define both the parameter types and the return type, much like in C#.

You can declare a function using the function keyword. This is the most common and explicit way to define a named function in TypeScript.

```ts
function greet(name: string): string {
  return `Hello, ${name}`;
}
```

C# e quivalent

```csharp
string Greet(string name) {
    return $"Hello, {name}";
}
```

- The function keyword introduces a named function declaration.

- Parameter types and return type must be declared for clarity and safety.

- This structure closely resembles a method in C#.

> 📌 Note: Unlike C#, the function keyword is required in TypeScript for declaring a regular function.

[Back to top](#introduction-to-typescript)

---

### Anonymous Functions

An anonymous function is a function without a name. It's often used as a **callback** or assigned to a variable.

```ts
const log = function (message: string): void {
  console.log(message);
};

log('Hello');
```

> 📌 Note: In TypeScript, the function keyword is used to define the function, and it's stored in a variable. This is sometimes called a function expression.

[Back to top](#introduction-to-typescript)

---

### Arrow Functions

Arrow functions are a shorter syntax for writing anonymous functions. They’re especially useful for **callbacks** and simple operations.

```ts
const log = (message: string): void => {
  console.log(message);
};
```

> 📌 Arrow function syntax is very common in modern JavaScript and TypeScript, especially in frameworks like React, Vue, and Angular. It improves readability and is the preferred style in most modern codebases.

## Objects

An **object** in TypeScript is a data structure used to group related values together using key–value pairs. Each value is stored under a named property.

Objects are used to represent things like users, settings, or items — and are one of the most common building blocks in TypeScript.

```ts
const user = {
  name: 'Alice',
  age: 30,
};
```

> 🆚 In C#, this is similar to creating an instance of a class, record, or even an anonymous object.

TypeScript will automatically infer the structure _( shape )_ of the object. Once that's done, you're not allowed to add new properties or change the type of existing ones.

```ts
const user = {
  name: 'Alice',
  age: 30,
};

user.name = 'Bob'; // ✅ OK – still a string
user.age = 31; // ✅ OK – still a number
user.age = '31'; // ❌ Error: Type 'string' is not assignable to type 'number'
user.email = 'a@b.com'; // ❌ Error: Property 'email' does not exist on type '{ name: string; age: number; }'
```

### Comparing TypeScript Objects to C# Constructs

| TypeScript Usage                                  | Closest C# Equivalent            | Notes                                              |
| ------------------------------------------------- | -------------------------------- | -------------------------------------------------- |
| Literal object with fixed keys and types          | `class` or `record`              | Like a simple class instance with named properties |
| Object where you modify values later              | `class` with mutable properties  | Same behavior — mutable by default                 |
| Readonly object                                   | `record` or `struct` with `init` | Can simulate immutability using `readonly` in TS   |
| Object with dynamic keys (`{ [key: string]: T }`) | `Dictionary<string, T>`          | Often used when keys are not known in advance      |

[Back to top](#introduction-to-typescript)

## Interface

An interface in TypeScript defines the structure of an object — what properties it must have and what types those properties should be.

You can think of it like a blueprint or contract that objects must follow.

```ts
interface User {
  name: string;
  age: number;
}

const user: User = {
  name: 'Alice',
  age: 30,
};
```

- The User interface specifies that any object of this type must have a name _(string)_ and an age _(number)_.

- TypeScript will give you an error if you try to leave out a property or use the wrong type.

```ts
const brokenUser: User = {
  name: 'Bob',
}; // ❌ Error: Property 'age' is missing
```

You can mark properties as optional using ?:

```ts
interface User {
  name: string;
  age?: number;
}

const user: User = {
  name: 'Charlie',
}; // ✅ OK – age is optional
```

> 💡 Tip: Interfaces make your code more readable, reusable, and consistent — especially when working with functions, APIs, or large applications.

[Back to top](#introduction-to-typescript)

## Types

A type alias lets you create a custom name for a specific type or combination of types. This is useful when you want to simplify complex types or give a name to a structure.

```ts
type Product = {
  id: number;
  name: string;
  price: number;
};

const item: Product = {
  id: 1,
  name: 'Book',
  price: 99.99,
};
```

> 📌 This works just like an interface — in most cases, type and interface can be used interchangeably for object shapes.

[Back to top](#introduction-to-typescript)

## Union Types

A union type lets a variable hold more than one possible type. You define it using the | (pipe) symbol.

```ts
let id: string | number;

id = 123; // ✅ OK
id = 'abc123'; // ✅ OK
id = true; // ❌ Error: boolean not allowed
```

You can use union types in function parameters too:

```ts
function log(value: string | number): void {
  console.log('Value:', value);
}
```

Inside the function, you often need to check the type before using it:

```ts
function handle(input: string | number) {
  if (typeof input === 'string') {
    console.log(input.toUpperCase());
  } else {
    console.log(input.toFixed(2));
  }
}
```

You can also use union typs with arrays an objects.

[Back to top](#introduction-to-typescript)
