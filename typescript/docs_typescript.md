# Typescript - Official documentation

## The Handbook

### The Basics

#### Emitting with errors

```console
$ tsc --noEmitOnError hello.ts
```

The `--noEmitOnError` won't emit or generate the output file, that means if typescript found an error a new version of hello.ts won't be created.

#### DOWNLEVELING

TypeScript has the ability to rewrite code from newer versions of ECMAScript to older ones such as ECMAScript 3 or ECMAScript 5
(a.k.a. ES3 and ES5). This process of moving from a newer or “higher” version of ECMAScript down to an older or “lower” one is sometimes called downleveling.

```console
$ tsc --target es2015 hello.ts
```

By default TypeScript targets ES3, an extremely old version of ECMAScript. We could have chosen something a little bit more recent
by using the `target` option. Running with `--target es2015` changes TypeScript to target ECMAScript 2015,
meaning code should be able to run wherever <mark>ECMAScript 2015<mark> is supported.

#### STRICTNESS

```console
$ tsc --noImplicitAny hello.tsc
```

When no type annotation are presend and typescript can't infer the type of a variable it will fallback to type any. If `--noImplicitAny` is ON typescript will throw an error.

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

Equality narrowing

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
