In a discussion about language design on the Typescript discord, I tried to give reasons why Typescript has distinct language constructs for manipulating types and values.
Why have distinct syntax for looping over the properties of a type versus looping over the properties of an object?

Instead of: 

```ts
type Extract<T, K> = { [KT in T]: KT extends K ? T[KT] : never }
```

Why not have:

```ts
function extract(o, ks) {
    const p = {}
    for (const k in o) {
        if (ks.includes(k)) {
            p[k] = o[k]
        }
    }
    return p
}
```


The main reason is intended usage. Our ideal TS program is simple to type: it's not very different from a C# program or an ML program (your choice, please don't mix the two). The complex types are intended for limited usage so that library authors, hopefully mostly legacy authors, can type their dynamic code. Typescript doesn't actually have dependent types, so we should not mislead people into thinking that we can evaluate arbitrary Javascript at compile time. Bottom line: it's OK for complicated types to look complicated. We're not trying to have them used in every program.

Confusingly, a lot of Typescript types do have similar syntax to their values: object literals, tuples and arrow functions especially. The two reasons I can think of for that: these types were added early and we continue to want people to use them frequently.