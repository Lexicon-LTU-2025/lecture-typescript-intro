# Intro to TypeScript

## Comments

TS uses same comments as JS AND C#, no biggy!

Single-line comment

```js
// This is a single line comment
```

Multi-line comment

```js
/* 
This is a multi-line comment
that spans several lines.
*/
```

## Data Types

This types can be used for all our variables that we create in our applications.

### Number

JS uses a single number type that represents all numeric values. There is no distinction between differnet integers and floating numbers.

| C# Type   | JavaScript/TypeScript Equivalent |
| --------- | -------------------------------- |
| `byte`    | `number`                         |
| `short`   | `number`                         |
| `int`     | `number`                         |
| `long`    | `number`                         |
| `float`   | `number`                         |
| `double`  | `number`                         |
| `decimal` | `number`                         |

All of these values are treated in the same way internally: as a 64 bit floating point number.

### String

String represents texts, it used for both `char` and `string` in C#.

One big difference is that you are not restricted to use double quotation marks as you must in C#. You can use:

Single quotes:

```
'...'
```

Double quotes:

```
"..."
```

Backticks:

```
`...`
```

Backticks can be used for normal string but their power lies that we can inject variables in those strings. This is called a _template literal_

### Boolean

Works the same as in C#.

```js
true;

false;
```

### Null

Represents an intentional abseence of any value.

```js
null;
```

### Undefined

Represents a value that hasn't been assigned yet.

```js
undefined;
```

### Any

Disables type checking - can hold any type. This should be avoided at all costs, as it turns of TypeScript's safety features completely.

### Unknown

Like `Any` but is safer, you must check its type before using type specific functionality.

```ts
let value: unknown = 'hello';

if (typeof value === 'string') {
  console.log(value.toUpperCase());
}
```

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

## Variables

In JS, you declare variables using keywords, and they are:

- let
- const
- var

These keyword determine wheater a variabel can be changed or not and how it behaves in different scopes, like inside functions of code blocks.

### let

Use `let` when you need a variable whose value might be changed during the lifetime of an application. It is block-scoped, meaning it only exists within to code block where it is defined.

```ts
let count = 1;
```

Here TS will infer the type, and lock that variable in to that specific type, in this case a `number`.

```ts
count = 'one';
```

TS won't allow this, but you can still change the value to another number. _( You can still run the application though but you will have error lines in your code. )_

```ts
count = 2;
```

If we want to be specific with the type, we do this:

```ts
let name: string;
```

This will create a string specific variable that we can assign a string to later.

```ts
name = 'niklas';
name = 'henrik'; // Works
name = 10; // Won't be allowed.
```

We can also set type and give a variable a value straight away.

```ts
let cityName: string = "Stockholm";
```

This is best practis as it is clear/explicit what kind of variable and type you have.

When it comes to scope `let` variables only exist within its scope.

```ts
if (true) {
  let name: string = "niklas"
  console.log(name) // niklas
}

console.log(name) // undefined
```

Another example

```ts
let age: number = 10;

if (true) {
  age = 20;
  console.log(age); // 20
}

console.log(age); // 10
```

