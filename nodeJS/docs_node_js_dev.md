# nodejs.dev

## Javascript Engines on browser

Chrome: V8

Firefox: [SpiderMonkey](https://spidermonkey.dev/)

Safari: [JavascriptCore](https://developer.apple.com/documentation/javascriptcore)

Edge: Chakra but recently rebuilt with Chromium and V8.

## Node with CLI

bash
#!/usr/bin/node

bash
#!/usr/bin/env node

## Exit a node app

`process.exit()` will force node to exit whatever amount of callback pending in the event loop.
By default the exit number is 0 which means success.

[exit code list](https://nodejs.org/api/process.html#process_exit_codes)

To Exit a process correctly

```js
const express = require('express');

const app = express();

app.get('/', (req, res) => {
	res.send('Hi!');
});

const server = app.listen(3000, () => console.log('Server ready'));

process.on('SIGTERM', () => {
	server.close(() => {
		console.log('Process terminated');
	});
});
```

`SIGKILL` is the signal that tells a process to immediately terminate, and would ideally act like `process.exit()`.

`SIGTERM` is the signal that tells a process to gracefully terminate. It is the signal that's sent from process managers like `upstart` or `supervisord` and many others.

## The process object

`USER_ID=239482 USER_KEY=foobar node app.js`

USER_ID env variable is only available in the execution of app.js scope. Once the execusion finish the env is lost.

## The REPL : Read Evaluate Print Loop

[REPL](https://nodejs.dev/learn/how-to-use-the-nodejs-repl)
run `node` in CLI to get the node console.

We can explore object in CLI

```node
> Number.
Number.__defineGetter__      Number.__defineSetter__      Number.__lookupGetter__      Number.__lookupSetter__      Number.__proto__
Number.hasOwnProperty        Number.isPrototypeOf         Number.propertyIsEnumerable  Number.toLocaleString        Number.valueOf

Number.apply                 Number.arguments             Number.bind                  Number.call                  Number.caller
Number.constructor           Number.toString

Number.EPSILON               Number.MAX_SAFE_INTEGER      Number.MAX_VALUE             Number.MIN_SAFE_INTEGER      Number.MIN_VALUE
Number.NEGATIVE_INFINITY     Number.NaN                   Number.POSITIVE_INFINITY     Number.isFinite              Number.isInteger
Number.isNaN                 Number.isSafeInteger         Number.length                Number.name                  Number.parseFloat
Number.parseInt              Number.prototype
```

We can explore global or process too !

```node
process.
process.__defineGetter__                     process.__defineSetter__                     process.__lookupGetter__
process.__lookupSetter__                     process.__proto__                            process.hasOwnProperty
process.isPrototypeOf                        process.propertyIsEnumerable                 process.toLocaleString
process.toString                             process.valueOf

process.addListener                          process.emit                                 process.eventNames
process.getMaxListeners                      process.listenerCount                        process.listeners
process.off                                  process.on                                   process.once
process.prependListener                      process.prependOnceListener                  process.rawListeners
process.removeAllListeners                   process.removeListener                       process.setMaxListeners

process.constructor

```

In REPL if you type \_ you will get the last result of the last cmd executed.

## Arguments from the CLI.

if we run this command

`node app.js Stephane Lucie Agata`

we can access the the arguments passed :

```js
process.argv.forEach((value, i) => {
	console.log(`value: ${i}: ${value}`);
});
```

output:

```console
$ value: 0: /usr/local/bin/node // full path of the node cmd
$ value: 1: /Users/steph08/Documents/Documents - Steph07 s MacBook Pro/Nodejs/Nodejs and Expressjs - Full course 8h16 - Freecodecamp/Tutorial/process_test.js // full path of the cmd
$ value: 2: Stephane
$ value: 3: Lucie
$ value: 4: Agata
```

## The console module

[console module documentation](https://nodejs.org/api/console.html)

```js
console.log('\x1b[33m%s\x1b[0m', 'hi!'); // will output `hi` with yellow color
```

The best convenient way to update the format of the output with console.log is by using [Chalk](https://github.com/chalk/chalk)

## Accept an input from the command line

This is possible with the help of the readline or inquirer module

```js
const readline = require('readline').createInterface({
	input: process.stdin,
	output: process.stdout,
});
readline.question(`What's your name?`, (name) => {
	console.log(`Hi ${name}!`);
	readline.close();
});
```

## Module.export

[The module object](https://nodejs.org/api/modules.html)

2 ways to expose/share variable object or function.

### 1st way

```js
car.js -------------------
const car ={
    brand:'Porsche',
    model: 911
}

module.exports = car

app.js --------------------
const car = require(./car.js);
```

### 2nd way

```js
car.js -----------------------
const car = {
    brand: 'Porsche'
    model: 911
}

module.exports.car = car;

app.js -----------------------
const { car } = require('./car.js');
```

## npm package manager

npm package manager handle the install of dependencies, update and versionning.

```console
$ --save-dev // installs and adds the entry to the package.json file devDependencies
$ --no-save // installs but does not add the entry to the package.json file dependencies
$ --save-optional // installs and adds the entry to the package.json file optionalDependencies
$ --no-optional // will prevent optional dependencies from being installed
```

Executable package are installed in `/node_modules/.bin/` folder.

```console
$ node_modules/.bin/cowthink haha
```

or

```console
$ npx cowthink node.js rocks !
```

## package.json file

```json
{
    "version": indicates the current version
    "name": sets the application/package name
    "description": is a brief description of the app/package
    "main": sets the entry point for the application
    "private": if set to true prevents the app/package to be accidentally published on npm
    "scripts": defines a set of node scripts you can run
    "dependencies": sets a list of npm packages installed as dependencies
    "devDependencies": sets a list of npm packages installed as development dependencies
    "engines": sets which versions of Node.js this package/app works on
    "browserslist": is used to tell which browsers (and their versions) you want to support

}
```

## package-lock.json

The goal of `package-lock.json` file is to keep track of the exact version of every package that is installed\
so that a product is 100% reproducible in the same way even if packages are updated by their maintainers.

## Find the installed version of an npm package

```console
$ npm ls -a`
```

```sh
-─┬ cowsay@1.5.0
│ ├── get-stdin@8.0.0
│ ├─┬ string-width@2.1.1
│ │ ├── is-fullwidth-code-point@2.0.0
│ │ └─┬ strip-ansi@4.0.0
│ │   └── ansi-regex@3.0.0
│ ├── strip-final-newline@2.0.0
│ └─┬ yargs@15.4.1
│   ├─┬ cliui@6.0.0
│   │ ├── string-width@4.2.3 deduped
│   │ ├── strip-ansi@6.0.1 deduped
│   │ └─┬ wrap-ansi@6.2.0
│   │   ├── ansi-styles@4.3.0 deduped
│   │   ├── string-width@4.2.3 deduped
│   │   └── strip-ansi@6.0.1 deduped
...
```

### Find a spcecific package version

```console
$ npm list get-stdin`
```

```
└─┬ cowsay@1.5.0
  └── get-stdin@8.0.0
```

### Get the latest version for a specific package

```console
$ npm view cowsay version`
```

```
1.5.0
```

```console
$ npm view cowsay versions`

[
  '1.0.0', '1.0.1', '1.0.2',
  '1.0.3', '1.1.0', '1.1.1',
  '1.1.2', '1.1.3', '1.1.4',
  '1.1.5', '1.1.6', '1.1.7',
  '1.1.8', '1.1.9', '1.2.0',
  '1.2.1', '1.3.0', '1.3.1',
  '1.4.0', '1.5.0'
]
```

### update packages to a newer version

```console
npm update
```

### update packages to a major version

```console
$ npm install -g npm-check-updates
$ ncu -u
$ npm update
$ npm install
```

this will upgrade all the version hints in the package.json file, to dependencies and devDependencies, so npm can install the new major version.

## Semantic versioning

- the first digit is the major version
- the second digit is the minor version
- the third digit is the patch version

When you make a new release, you don't just up a number as you please, but you have rules:

- you up the major version when you make incompatible API changes
- you up the minor version when you add functionality in a backward-compatible manner
- you up the patch version when you make backward-compatible bug fixes

Symbols that can be used in Semantic versioning

- ^: It will only do updates that do not change the leftmost non-zero number i.e - - - there can be changes in minor version or patch version but not in major version. If - you write ^13.1.0, when running npm update, it can update to 13.2.0, 13.3.0 even 13.- 3.1, 13.3.2 and so on, but not to 14.0.0 or above.
- ~: if you write ~0.13.0 when running npm update it can update to patch releases: 0.13.1 is ok, but 0.14.0 is not.
- gt: you accept any version higher than the one you specify
- gt=: you accept any version equal to or higher than the one you specify
- <=: you accept any version equal or lower to the one you specify
- <: you accept any version lower than the one you specify
- =: you accept that exact version
- -: you accept a range of versions. Example: 2.1.0 - 2.6.2
- ||: you combine sets. Example: < 2.1 || > 2.6

## Local and Global package

In general, all packages should be installed locally.

A package should be installed globally when it provides an executable command that you run from the shell (CLI), and it's reused across projects.
You can also install executable commands locally and run them using npx, but some packages are just better installed globally.

## Development and production dependencies

When you go in production, if you type npm install and the folder contains a package.json file, they are installed, as npm assumes this is a development deploy.

You need to set the --production flag (npm install --production) to avoid installing those development dependencies.

## The npx Node.js Package Runner

`npx` lets you run code built with Node.js and published through the npm registry.

`npx` can let you run executable code without having to installing it first.
`npx cowsay node rocks1`

`npx` can also run code hosted on GitHub gist:

```conso
npx https://gist.github.com/zkat/4bc19503fe9e9309e2bfaa2c58074d32
```

## THE EVENT LOOP

### Blocking the event loop

Any JavaScript code that takes too long to return back control to the event loop will block the execution of any JavaScript code in the page, even block the UI thread, and the user cannot click around, scroll the page, and so on.

Almost all the I/O primitives in JavaScript are non-blocking. Network requests, filesystem operations, and so on. Being blocking is the exception, and this is why JavaScript is based so much on callbacks, and more recently on promises and async/await.

### The call stack

The call stack is like a pile where function are executed and each instruction of a function is added on top of the call stack, once done it will be removed and the next instruction in the function is added at the top etc...

```js
const bar = () => console.log('bar');

const baz = () => console.log('baz');

const foo = () => {
	console.log('foo');
	setTimeout(bar, 0);
	new Promise((resolve, reject) =>
		resolve('should be right after baz, before bar')
	).then((resolve) => console.log(resolve));
	baz();
};

foo();
```

![image](https://nodejs.dev/static/4bb05f4d91852eb8c6b3de5371451315/f470a/call-stack-third-example.png 'call stack')

### The event module

[evnet module documentation](https://nodejs.org/api/events.html)

```js
const EventEmitter = require('events');
const eventEmitter = new EventEmitter();
```
