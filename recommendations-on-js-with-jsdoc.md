# Should I write JS with JSDoc instead of TS?

## Shortest
No.

## Shorter
No, because modern JS development makes using TS instead easy.

## Short
No. When JSDoc support was added to TS, 3 things were different.

1. Node didn't support Typescript type annotations.
2. Node didn't support ES modules.
3. Classes didn't support declaring their properties.

Now that you can put type annotations on types, you don't need JSDoc tags like `@type` and `@param`.
Now that you can use ES imports and class properties, you don't need JS inference rules to support expando assignments to `module.exports` and `this.property` in constructors.

## What about the Web
What, you're writing for the web but have no build, so you can't strip types?

Try again: you're either lying or you should be writing so little JS that you don't need types.
Seriously, try writing less JS on the web.

(If you have a build that doesn't strip types, you can almost certainly configure it to do so.)



