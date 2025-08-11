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
let cityName: string = 'Stockholm';
```

This is best practis as it is clear/explicit what kind of variable and type you have.

When it comes to scope `let` variables only exist within its scope.

```ts
if (true) {
  let name: string = 'niklas';
  console.log(name); // niklas
}

console.log(name); // undefined
```

### const

Use `const` when you don't want the variable to be reassigned during the lifetime of the application. It is also block-scoped.

```ts
const name: string = 'Niklas';
name = 'Erik'; // Won't be allowed.
```

This works as expected. But we can declare a `const` and NOT giving it a value. `const` always need to be assigned straight away.

```ts
const country: string;
```

Typescript won't like this and will complain. You need to assigned a value here.

Use `const` whenever possible - it makes your code more predictable.

> You can still change properties of objects declared with `const`, or elements inside an array - only the variables itself can't be reassigned. This is because objects and array are reference types.

### var

In JS, `var` is the old way of declaring variables and should be avoided. It has a function-scope instead of block-scope which can lead to confusing bahavior. Otherwise it works like a `let`, you can redeclare it and reassign the value.

```ts
if (true) {
  var greeting = 'hello'; // TS will infer this as a string.
}

console.log(greeting);
```

This works, greeting is 'hosited' to the upper function scope. In this case we don't have a enclosing function, which means the file itself will be the scope of this variable.The entire files acts as the function scope - so this variable becomes accessible throughout the entire file. _( like a global scope )_

> Important differenct from C#. In C# `var` means "let the compiler infer the type", but the variable is still strongly typed and block-scoped.

## Functions

Functions in JS can live on their own which is a very nice feature of course. But in order to create a function, you will need a keyword `function` _( in most cases )_.

Here is an example:

```ts
function greet() {
  console.log('Hello');
}

greet(); // Hello
```

This works fine, but where is the typing? If we are not explicit, TS will infer everything for us. But we can be explicit as well.

```ts
function greet(): void {
  console.log('Hello');
}
```

C# equivalent

```csharp
void Greet()
{
   Console.WriteLine("Hello");
}
```

With arguments and return value:

TypeScript

```ts
function greet(name: string): string {
  return `Hello ${name}! How are you doing?`;
}
```

C#

```csharp
string Greet(string name)
{
  return $"Hello {name}! How are you doing?"
}
```

To summaraize

- The function keyword introduces a named function declaration.

- Parameter types and return value SHOULD be declared for clarity.

- Closely resembles a method in C#

### Anonymous Functions

An anonymous function is a function without a name. It's often used as a **callback** or assined to a variable.

```ts
const log = function (message: string): void {
  console.log(message);
};

log('Hello!'); // Hello!
```

If you are going to pass a function as an argument in another funcion, you don't have to put it in a variable first, you can just pass it 'inline' in to the parenthesis of the function.

> Note: In TS, the function keyword is used to deine a function, and it can be stored in a variable. This is sometimes called a function expression.

### Arrow Functions

Arrow functions are a shorter syntax for writing anonymous functions. They are especially useful for **callbacks** and simple operations.

```ts
const log = (message: string): void => {
  console.log(message);
};
```

Arrow function syntax is very common in modern JS and TS, especially in frameworks like React and Vue. It improves readability and is the preferred style in most modern codebases.

## Arrays

An array is a list that holds multiples values of the same type. TypeScript ensures type safety, so all elements in an array must match teh declared type.

In TS, arrays behave more like a List in C#, you can grow or shrink them easily using methods like `.push()`, `.pop()`. There methods are called array methodss and there are many more to play around with. We will cover them as we go.

There are two common ways to type arrays:

```ts
// #1
const scores: number[] = [90, 85, 70];

// #2
const names: Array<string> = ['Niklas', 'Henrik'];
```

Of these two, number 1 is the most common one, and the one I am going to use.

Always use const when declaring arraus since arrays are reference types -> they point to a memory location where the data is stored.

Using const prevents reassignment of the array variable itself, but does not make the array immutable. You can still modify the content.

But first, let us try and access the elements in the array.

```ts
const items: string[] = ["apple", "banana", "orange"];
```

Two ways to access elements. First is using array brackets to access a specific index.

```ts
const item = items[0];
console.log(item); // apple
```

Other way is to use a method called `.at(index: number)`.

```ts
const item = items.at(0);
console.log(item); // apple
```

The `.at()` method is quite new and your application might need to specify a specific version of JS. The `.at()` accepts negative values as well which can be nice to have in some cases.


