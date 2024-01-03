In a discussion on language design on the Typescript discord about types vs interfaces, I tried to give a historical perspective to the complaints that interfaces seem designed for OO but have weird extra features.

1. interfaces existed in the language from day 1 (2011), but type aliases weren't added until unions were in 2015 -- exactly the point when non-object types became expressible.
2. Typescript was extremely C#-inspired in its early days. Anders was working on C# previously, Luke was working on F# previously.
3. interface merging is a weird feature that disappears once ES modules happen. It was needed (and still is, a bit) to work in a global environment like the browser. It's not just interfaces, it's most pre-ES2015 features.
4. Type aliases need to exist so you don't have to write A | B | C every time. Over time this expanded to intersections and mapped types.
5. It was confusing that type aliases were so different from interfaces, especially the way they disappeared in quick info, etc. That, and wanting to write recursive conditional types, led to the convergence with interfaces over time.

The bottom line is that typescript has two nearly identical features for historical reasons, and there's not a principled way to separate them or provide clear usage guidelines.

As @Retsam19 said, if we started over today, we'd probably only have type, although it would maybe have some way to include extends and merging. We'd likely have first-class `extends` in the type system as an operator like intersection or spread (Typescript almost had first-class `spread` in the type system.)

Specific complaints from @micuseym:
> if all of this is intended, what are the benefits of this design? How do they outweigh the drawbacks?

Answer: it wasn't intended; the benefits are: backward compatibility.

> Interfaces seem like a feature designed specifically for OOP.

It's more accurate to say that they were designed in a C#-like environment, where classes and namespaces were the way of structuring your program, not modules and closures (or something else).
