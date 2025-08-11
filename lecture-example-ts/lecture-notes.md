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
const items: string[] = ['apple', 'banana', 'orange'];
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

### Array methods

Array methods are methods that are avaialble on all created arrays out of the box.

#### push(element1, element2, ... ): number

- Purpose: Adds one or more lemeents ot the end of an array.
- Returns: The new length of the array

```ts
const items: string[] = ['apple', 'banana', 'orange'];

items.push('pear'); // Works
items.push(10); // Not allowed.
console.log(items); // ["apple", "banana", "orange", "pear"];
```

#### pop(): element

- Purpose: removes the last element of an array.
- Returns: The removed element.

```ts
const items: string[] = ['apple', 'banana', 'orange'];

const removedItem = items.pop();

console.log(items); // ['apple', 'banana']
console.log(removedItem); // 'orange'
```

#### filter(callback): newArray

- Purpose: Creates a new array with the result of calling the callback on every element. The callback must return true or false.
- Returns: A new array _( original is unchanged )_

```ts
const numbers: number[] = [1, 2, 3, 4, 5, 6];

// Let's filter all the uneven numbers

const evenNumbers = numbers.filter((n) => {
  return n % 2 === 0;
});

console.log(evenNumbers); // [2,4,6]
```

So it works by keeping the values that resultet in a true value in the callback functino, and removes the false ones.

#### map(callback): newArray

Use to modify each element and return those elements in a new array.

- Purpose: Creates a new array with the result of calling a callback on every element. The callback must return a new element of the same type in each iteration.
- Returns: A new array

```ts
const numbers: number[] = [1, 2, 3, 4, 5, 6];

const doubled = numbers.map((n) => {
  return n * 2;
});

console.log(dobled); // [2, 4, 6, 8, 10, 12]
```

This example above can be shortened if the code is simple.

```ts
const doubled = numbers.map((n) => n * 2);
```

This works the same way as in C#.

#### forEach(callback): undefined

- Purpose: Executes a provided callback once for each element.
- Returns: undefined. _( can not chain for array methods.)_

In short, this method is just a for loop but forEach is used more commonly in functional programming.

```ts
const numbers: number[] = [1, 2, 3, 4, 5, 6];

numbers.forEach((n) => {
  console.log(n);
});
```

There are many methods that you can use. Here is a link:

[All existing array methods in JS](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

## Objects

An object in JS is a data structure used to group related values together using key-value pairs. Each value is stored under a named propery. It is also called an object literal.

Used to represent all things like a user, a setting, a car, a house and so on. Very common in JS. This is an example on how to create one:

```ts
const user = {
  name: 'niklas',
  age: 34,
  country: 'sweden',
};
```

> In C#, this is similar ro creating an instance of a class, record or even annonymous object.

TS will automatically infer the structure of the object. This means, when you have created an object with properties, you can't changed that.

```ts
user.name = 'henrik'; // Works, since name is a string.
user.age = 35; // Works as well.
user.age = '35'; // Error: Type 'string' is not assignable to type 'number'
user.email = 'niklas@niklas.com'; // Error: Property 'email' does not exist on type '{ name: string; age: number; country: string; }'
```

You can also access properties on objects by using an array-like syntax.

```ts
const user = {
  name: 'niklas',
  age: 34,
  country: 'sweden',
};

const name = user['name'];
console.log(name); // niklas
```

This way demands some more work in order to get TS to understand what we are doing. But we are going to cover that now.

### Comparing TypeScript Objects to C# Constructs

| TypeScript Usage                                  | Closest C# Equivalent            | Notes                                              |
| ------------------------------------------------- | -------------------------------- | -------------------------------------------------- |
| Literal object with fixed keys and types          | `class` or `record`              | Like a simple class instance with named properties |
| Object where you modify values later              | `class` with mutable properties  | Same behavior — mutable by default                 |
| Readonly object                                   | `record` or `struct` with `init` | Can simulate immutability using `readonly` in TS   |
| Object with dynamic keys (`{ [key: string]: T }`) | `Dictionary<string, T>`          | Often used when keys are not known in advance      |

## Interface

An interface in TS defines the structure of an object. What properties it must have and what types those properties should be. Like a blueprint.

```ts
interface IUser {
  name: string;
  age: number;
  country: string;
}

const user: IUser = {
  name: 'niklas',
  age: 34,
  country: 'sweden',
};
```

- The User interface specifies that any object of this type must the given properties with the correct type.
- TS will give you an error if you try to leave out a property

```ts
const brokenUser: IUser {
  name: 'niklas',
  country: 'sweden',
} // Error: Property 'age' is missing
```

You can mark properties as optional using `?`.

```ts
interface ICar {
  color: string;
  make: string;
  model?: string;
}

const volvo: ICar = {
  color: 'red',
  make: 'volvo',
};
```

This code will make model undefined, which is fine according to the interface.

## Union Type

As with everything in JS, you are free to do a lot of things. In this case, I want to show that a variable, or property in an object can be of several types if needed. This is call union type.

```ts
let id: string | number;

id = 123; // OK
id = '123'; // Also ok
```

This "pipe" character is used to specify this union type. And we can use it everywhere!

```ts
interface ICar {
  model: string;
  color: string;
  regNr: string | number;
  isInsured: string | number | boolean;
}

function log(value: string | number): void | string {
  if (typeof value === 'string') {
    return `This is the value: ${value}`;
  }

  console.log(`This is the value ${value}`);
}
```

So, if you want crazy, you can have crazy! But it will not end good for you in that case...
