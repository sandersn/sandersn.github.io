I replied to a new suggestion on [an old issue suggesting a jsdoc tag for passing type arguments](https://github.com/microsoft/TypeScript/issues/27387):

Our approach to JSDoc has evolved since [this issue](https://github.com/microsoft/TypeScript/issues/27387) was created. At that time I didn't want to add anything that didn't have precedent in [the jsdoc documentation generator](https://jsdoc.app/), or at least in [Closure compiler](https://developers.google.com/closure/compiler/). However, Typescript is the largest processor of JSDoc at this point, and TS users who want typing are probably the most prolific authors of it. So we've decided on a few new principles:

1. If you're exposing existing TS syntax and semantics to JS, the semantics should be identical and the syntax should be as close as possible. `@import` is a good example, as is `@overload`. `@enum` is the worst counter-example I can think of, since it has Closure semantics.
2. However, new tags should be normal tags as much as possible. That means:
   a. Starts with `@alphanumeric`.
   b. Isn't nested -- starts at `@` and continues till the next `@`.
   c. Is used in a `/**` comment directly above a declaration. Both `@import` and `@overload` violate this, for good reasons.

(1) means that TS users will be able to use the new JS syntax with a minimum of surprise. (2) means that jsdoc processors (including TS itself!) will be able to understand the new tag with a minimum of new parsing.

Re (2c), the rough descending order of goodness, or familiarity, in my opinion is:

1. In a `/**` comment directly preceding a declaration -- like `@type`.
2. A `/**` comment not connected to any JS syntax (which is to say, preceding some non-declaration) -- like `@import`.
3. A `/**` comment preceding an expression -- like `@type` as a cast.
4. A `/**` comment preceding another `/**` comment that eventually precedes a declaration -- like `@overload`.
5. A `/**` comment preceding some other, non-top-level node in the parse tree.
6. A `/*::` comment or some other new syntax.
7. A `// @ts-alphanumeric` comment preceding a declaration.
