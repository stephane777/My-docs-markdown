# Typescript - Official documentation

## The Handbook

### The Basics

#### Emitting with errors

```properties
tsc --noEmitOnError hello.ts
```

The `--noEmitOnError` won't emit or generate the output file, that means if typescript found an error a new version of hello.ts won't be created.

#### DOWNLEVELING

TypeScript has the ability to rewrite code from newer versions of ECMAScript to older ones such as ECMAScript 3 or ECMAScript 5
(a.k.a. ES3 and ES5). This process of moving from a newer or “higher” version of ECMAScript down to an older or “lower” one is sometimes called downleveling.

```properties
tsc --target es2015 hello.ts
```

By default TypeScript targets ES3, an extremely old version of ECMAScript. We could have chosen something a little bit more recent
by using the `target` option. Running with `--target es2015` changes TypeScript to target ECMAScript 2015,
meaning code should be able to run wherever <mark>ECMAScript 2015<mark> is supported.

#### STRICTNESS

```properties
tsc --noImplicitAny hello.tsc
```

When no type annotation are presend and typescript can't infer the type of a variable it will fallback to type any. If `--noImplicitAny` is ON typescript will throw an error.

### Every day types
