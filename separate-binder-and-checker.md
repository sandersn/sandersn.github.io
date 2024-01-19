In a discussion about compiler internals on the Typescript discord, I tried to give reasons why you might want to have a separate binder and checker.

In compilers for old languages, the binder and checker could be one unit. Practically the whole thing could be one unit for a simple enough language&mdash;parser, binder, checker, emitter. This gives you a fast, streaming compiler, almost like the JS emit from today's bundlers. The downside is that everything has to be checked in the order that it's written. For example:

```js
function sayHello() {
  console.log('hello', world())
}
function world() {
  return 'world'
}
```

Is not legal in a language that (1) needs to be compiled top-to-bottom (2) needs enough information to check and emit all code. Some languages delay compilation, or interpretation, until the function actually runs, meaning that the above program is legal in Python or PHP, but this one is not:

```js
world()
function world() {
  return 'world'
}
```

That's because the program is executed top-to-bottom, but function bodies aren't checked until they're called. (Thanks to discord user @jean_michelet for this example translated from PHP.)

One way around the top-to-bottom problem is forward declaration:

```ts
declare function world(): void;
function sayHello() {
  console.log('hello', world())
}
function world() {
  return 'world'
}
```

You can even put your forward declarations in a re-usable file and copy/paste it everywhere, like the C pre-processor does.

The modern solution, however, is to have multiple passes: specifically, a binder to gather declaration information and a checker to check the code. This solves the problem. And, over time, the binder and checker can start to look different. In Typescript, the binder is a fast, top-down walk over the tree that adds pointers to declaration nodes into tables in container nodes. The checker is a lazy, self-recursive walk over the type graph, which you can enter from basically any node you want to type check. When code changes in the editor, only the current file is re-bound. But the entire checker is thrown away. That's because declarations have fairly simple dependencies, but types have complex, multi-file dependencies.

Note, from the above discussion:

- A symbol is the "pointer to declaration".
- A symbol table is the "table of pointers" that's added to a container node.

So the basic operation of the binder is attaching a structure like this to a container:

```js
{
  world: { line: 3, pos: 0, kind: "function" },
  sayHello: { line: 0, pos: 0, kind: "function" },
}
```