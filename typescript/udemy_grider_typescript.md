# Setup Typscript and vscode

`npm i -g typescript ts-node` // install typescript and compiler

View command palette : install code
code / preferences / settings : format on save
code / preferences / settings : single quote
code / preferences / settings : Editor number of spaces a tabe is equal to: 2

`tsc index.ts` // tsc index.ts compile and check for error and generate a js file\
`node index.js` // execute index.js

`ts-node index.ts` // same as above it will do both in one command line.\

# Different types in typescript

## Primitives types

`number boolean void undefined string symbol null`

## Object types

`functions arrays classes objects`

# Types Annotation

`Type annotations` tell the compiler what type are those variable.
`Type inference` means typescript will guess what is the type of the variable ex: const name = "Stephane" name has a type of string.
