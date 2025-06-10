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
- [Objects](#objects)
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

[Back to top](#introduction-to-typescript)

## Functions

[Back to top](#introduction-to-typescript)

## Objects

[Back to top](#introduction-to-typescript)

## Interface

[Back to top](#introduction-to-typescript)

## Types

[Back to top](#introduction-to-typescript)

## Union Types

[Back to top](#introduction-to-typescript)
