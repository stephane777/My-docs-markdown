# Table of content

<!-- TOC start -->

- [Typescript - Official documentation](#typescript-official-documentation)
  - [The Handbook](#the-handbook)
    - [The Basics](#the-basics)
      - [Emitting with errors](#emitting-with-errors)
      - [DOWNLEVELING](#downleveling)
      - [STRICTNESS](#strictness)
    - [Every day types](#every-day-types)
    - [Narrowing](#narrowing)
- [DEFINITIONS](#definitions)
  <!-- TOC end -->
  <!-- TOC --><a name="typescript-official-documentation"></a>

# Typescript - Official documentation

<!-- TOC --><a name="the-handbook"></a>

## The Handbook

<!-- TOC --><a name="the-basics"></a>

### The Basics

<!-- TOC --><a name="emitting-with-errors"></a>

#### Emitting with errors

```console
$ tsc --noEmitOnError hello.ts
```

The `--noEmitOnError` won't emit or generate the output file, that means if typescript found an error a new version of hello.ts won't be created.

<!-- TOC --><a name="downleveling"></a>

#### DOWNLEVELING

TypeScript has the ability to rewrite code from newer versions of ECMAScript to older ones such as ECMAScript 3 or ECMAScript 5
(a.k.a. ES3 and ES5). This process of moving from a newer or “higher” version of ECMAScript down to an older or “lower” one is sometimes called downleveling.

```console
$ tsc --target es2015 hello.ts
```

By default TypeScript targets ES3, an extremely old version of ECMAScript. We could have chosen something a little bit more recent
by using the `target` option. Running with `--target es2015` changes TypeScript to target ECMAScript 2015,
meaning code should be able to run wherever <mark>ECMAScript 2015<mark> is supported.

<!-- TOC --><a name="strictness"></a>

#### STRICTNESS

```console
$ tsc --noImplicitAny hello.tsc
```

When no type annotation are presend and typescript can't infer the type of a variable it will fallback to type any. If `--noImplicitAny` is ON typescript will throw an error.

<!-- TOC --><a name="every-day-types"></a>

### Every day types

```typescript
const name: string = 'Stephane';
const adult: boolean = true;
const age: number = 46;
const lotoNumber: Array<number> = [12, 43, 27, 12, 18, 23]; // T<U> see Generics chapter
const hobbies: string[] = ['guitar', 'tennis', 'running'];
let obj: any = { x: 0 }; // Using any disables all further type checking
```

No type annotations here, but TypeScript can spot the bug

```typescript
const names = ['Alice', 'Bob', 'Eve'];
```

Contextual typing for function

```typescript
names.forEach(function (s) {
	console.log(s.toUppercase());
	// Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?
});
```

Contextual typing also applies to arrow functions

```typescript
names.forEach((s) => {
  console.log(s.toUppercase());
}
// Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?
```

OBJECT

```typescript
const me: { name: string; adult: boolean; hobbies?: string[] } = {
	name: 'Stephane',
	adult: true,
}; // will pass type check as hobbies is optional
```

For optional parameter we need to test if this optional parameter is not undefined before we can use it

```typescript
if (me.hobbies !== undefined) {
	const { hobbies } = me;
	hobbies.forEach((hobby) => console.log(hobby));
}
```

UNION TYPE

```typescript
const printID = function (id: string | number) {
	console.log(id);
};
printID('3h5r84'); // OK;
printID(23); // OK;
printID({ name: 'stephane' }); // type error!
```

with union type we need to narrow the union code in order to use here string or number function

```typescript
const printID = function (id: string | number) {
	if (typeof id === 'string') {
		console.log(id.toUpperCase());
	} else {
		console.log(id);
	}
};
```

Another example with union type with an array of string and string

```typescript
function welcomePeople(x: string[] | string) {
	if (Array.isArray(x)) {
		// Here: 'x' is 'string[]'
		console.log('Hello, ' + x.join(' and '));
	} else {
		// Here: 'x' is 'string'
		console.log('Welcome lone traveler ' + x);
	}
}
```

When a method is available on all type declaration narrowing is not needed
Return type is inferred as number[] | string

```typescript
function getFirstThree(x: number[] | string) {
	return x.slice(0, 3);
}
// Array.prototype.slice;
// String.prototype.slice;
```

TYPES ALIASES

We can use type aliases when the type is used more than once

```typescript
type Point = {
	x: number;
	y: number;
};
function printCoord(pt: Point) {
	console.log(pt.x);
	console.log(pt.y);
}
type ID = string | number;
```

INTERFACE

```typescript
interface Point {
	x: number;
	y: number;
}

interface Point3D extends Point {
	z: number;
}
```

Almost all features of an interface are available in type, the key distinction is that a type cannot be re-opened to // add new properties vs an interface which is always extendable.

```typescript
type Point = {
	x: number;
	y: number;
};
```

extending a type with intersection

```typescript
type Point3D = Point & {
	z: number;
};
```

TYPE ASSERTION

They are used to tell typescript we know that we expect a more specific type suggested by type checker

```typescript
const myCanvas = document.getElementById('main_canvas') as HTMLCanvasElement; // instead of HTMLElement;

// or
const myCanvas = <HTMLCanvasElement>document.getElementById('main_canvas'); // with angle-bracket synthax;
```

Non-null Assertion Operator

TypeScript also has a special syntax for removing null and undefined from a type without doing any explicit checking. Writing ! after any expression is effectively a type assertion that the value isn’t null or undefined:

```typescript
function liveDangerously(x?: number | null) {
	// No error
	console.log(x!.toFixed());
}
```

<!-- TOC --><a name="narrowing"></a>

### Narrowing

```typescript
function padLeft(padding: number | string, input: string) {
	if (typeof padding === 'number') {
		return ' '.repeat(padding) + input;

		// (parameter) padding: number
	}
	return padding + input;

	// (parameter) padding: string
}
```

#### Equality narrowing

`x ===y` this condition will filter out number for x and boolean for y, meaning that the block code
will only be executed if both x and y are of type string

```typescript
function example(x: string | number, y: string | boolean) {
	if (x === y) {
		// We can now call any 'string' method on 'x' or 'y'.
		x.toUpperCase();

		// (method) String.toUpperCase(): string
		y.toLowerCase();

		// (method) String.toLowerCase(): string
	} else {
		console.log(x);
		// (parameter) x: string | number

		console.log(y);
		// (parameter) y: string | boolean
	}
}
```

The `in` operator narrowing

We can use the `in` operator to condition if an object has a specific property

```typescript
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function move(animal: Fish | Bird) {
	// if property 'swim' exist in animal object
	if ('swim' in animal) {
		return animal.swim();
	}

	return animal.fly();
}
```

`instanceOf` narrowing

```typescript
function logValue(x: Date | string) {
	if (x instanceof Date) {
		console.log(x.toUTCString());
		// (parameter) x: Date
	} else {
		console.log(x.toUpperCase());
		// (parameter) x: string
	}
}
```

Using type predicates

```typescript
function isFish(pet: Fish | Bird): pet is Fish {
	return (pet as Fish).swim !== undefined;
}
```

`pet is Fish` is our type predicate in this example. A predicate takes the form `parameterName is Type`, where `parameterName` must be the name of a parameter from the current function signature.

Discriminated unions

```typescript
interface Circle {
  kind: "circle";
  radius: number;
}

interface Square {
  kind: "square";
  sideLength: number;
}

type Shape = Circle | Square;

function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;

(parameter) shape: Circle
    case "square":
      return shape.sideLength ** 2;

(parameter) shape: Square
  }
}
```

The never type
When narrowing, you can reduce the options of a union to a point where you have removed all possibilities and have nothing left. In those cases, TypeScript will use a `never` type to represent a state which shouldn’t exist.

### More on Functions

#### Function Type Expression

```typescript
function greeter(callback: (a: string) => void) {
	callback('Stephane');
}

function printToConsole(text: string) {
	console.log(text);
}
greeter(printToConsole);

// or

type PrintToConsoleType = (text: string) => void;
function greeter(callback: PrintToConsoleType) {
	// ...
}
```

#### Call signatures: function callable with property

```typescript
type DescribableFunction = {
	description: string;
	(someArg: number): boolean;
};
function doSomething(fn: DescribableFunction) {
	console.log(fn.description + ' returned ' + fn(6));
}
```

#### Construct Signatures

```typescript
type SomeConstructor = {
	new (s: string): SomeObject;
};
function fn(ctor: SomeConstructor) {
	return new ctor('hello');
}
```

#### Generic Functions

In TypeScript, generics are used when we want to describe a correspondence between two values. We do this by declaring a type parameter in the function signature:

```typescript
function firstElement<Type>(arr: Type[]): Type | undefined {
	return arr[0];
}
```

#### Inference

```typescript
function map<Input, Output>(
	arr: Input[],
	func: (arg: Input) => Output
): Output[] {
	return arr.map(func);
}

// Parameter 'n' is of type 'string'
// 'parsed' is of type 'number[]'
const parsed = map(['1', '2', '3'], (n) => parseInt(n));
```

#### Constraints

```typescript
function longest<Type extends { length: number }>(a: Type, b: Type) {
	if (a.length >= b.length) {
		return a;
	} else {
		return b;
	}
}

// longerArray is of type 'number[]'
const longerArray = longest([1, 2], [1, 2, 3]);
// longerString is of type 'alice' | 'bob'
const longerString = longest('alice', 'bob');
// Error! Numbers don't have a 'length' property
const notOK = longest(10, 100);
// Argument of type 'number' is not assignable to parameter of type '{ length: number; }'.
```

#### Specifying Type Arguments

```typescript
function combine<Type>(arr1: Type[], arr2: Type[]): Type[] {
	return arr1.concat(arr2);
}
// or if you need more specific type
const arr = combine<string | number>([1, 2, 3], ['hello']);
```

#### Guidelines for Writing Good Generice Functions

```javascript
function firstElement1<Type>(arr: Type[]) {
  return arr[0];
}

function firstElement2<Type extends any[]>(arr: Type) {
  return arr[0];
}

// a: number (good)
const a = firstElement1([1, 2, 3]);
// b: any (bad)
const b = firstElement2([1, 2, 3]);
```

#### Optional parameter

```typescript
function f(x?: number) {
	// ...
}
f(); // OK
f(10); // OK
```

Although the parameter is specified as type number, the x parameter will actually have the type `number | undefined` because unspecified parameters in JavaScript get the value undefined.

#### Function Overloads

```typescript

```

<!-- TOC --><a name="definitions"></a>

# DEFINITIONS

**a discriminant property** is a common property shared by 2 object
type guard

**type predicates**

**non-null assertions**

**assignments**
