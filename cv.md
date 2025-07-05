# Projects

## 1995&ndash;1999: High School

### Thingy32

[Thingy32](https://www.romhacking.net/utilities/218/) is a hex editor that lets you interpret bytes with custom coding schemes, not just ASCII. It is a port of the DOS-based tool [Thingy](https://www.romhacking.net/utilities/217/). Many games had simple, custom coding schemes based on indexing into a font stored in memory somewhere. Thingy lets you specify a coding scheme using a simple INI-like format. Then you can read or dump the data in a ROM interpreted according to that scheme.

After the Japanese font is replaced with an English font, it also let you write to the file in a custom coding scheme, converting from ASCII.

Thingy was written in BASIC for DOS; Thingy32 was written in Visual Basic.

I uploaded [the source](https://github.com/sandersn/thingy32) a few years ago. Good luck building a VB5 program!


### Translation Credits

I helped to translate a number of Japanese games to English. This despite not really knowing Japanese! I was a teenager, and the people playing the games were also teenagers. The quality bar was low. But also, the professional quality bar was low at the time.

- Ganpuru: Gunman's Proof
- Herakles no Eikou II: Titan no Metsubo
- Monstania
- Little Magic
- FEDA: The Emblem of Justice
- Live a Live
- Ys
- Chuka Taisen

[I got this list is from ROMHacking.net](https://www.romhacking.net/community/231/) because I had forgotten a lot of these. I've ordered them by the percentage that I contributed -- but it's a dimly remembered guess.

## 2000 - 2004: College

Bachelor of Arts at College of the Ozarks [^1]
- Major in Computer Science
- Minors in French and Spanish

### Martial

Letter frequency analysis to determine the most efficient variable-width coding scheme for a given text. Some RPGs used variable-width coding schemes to store extremely large scripts on a small ROM in the 1990s. Assuming you could decode the scheme, [Martial](https://www.romhacking.net/utilities/230/) let you generate a new, optimal variable-width scheme for your replacement language.

I don't have any idea if anybody ever used this tool -- surely deciphering a variable-width coding scheme and changing the executable code to work with a different dictionary is a harder task than generating an optimal dictionary. But I wrote Martial because I learned about Huffman coding in class and wanted to apply it to my work.

I uploaded [the source](https://github.com/sandersn/martial) a few years ago. Good luck building a VB5 program!

### Pham Nuwen 

[Pham Nuwen](https://github.com/sandersn/nuwen) is the Vernor Vinge-inspired name for my senior project. It's an error-correcting parser intended for simple natural language&mdash;the kind you'd find in a first-year foreign language class. There's a parser based on Earley parsing, and an LALR parser. Although they parse 'natural language', the parsing is still compiler-esque.

The code is Python 2, so it should still be easy enough to run (though I haven't tried it). The UI wrappers are a mix of cgi-bin and wxWindows. I'd stick to the command-line versions.

There is a bunch of unnecessary stuff I threw in from reading [Jurafsky and Martin's natural language processing textbook](https://web.stanford.edu/~jurafsky/slp3/), like some kind of part-of-speech tagger. This was the biggest program I had ever written, and shows the limits of my design experience at the time. Programming in untyped Python certainly didn't help, although this is probably less obvious from the source code. (Notably, typed Python didn't exist at the time.)

### Highly Impressive Quenya Parser

A repackaging of the parser work for a different senior project, this one a general education class on Tolkien, right before the release of _The Return of the King_ movie. I found a few sentences of Quenya, hand-tagged them, and changed the heading on the cgi-bin page.

Notably, [diemquenya.py](https://github.com/sandersn/nuwen/blob/main/diemquenya.py) was one of the last files I wrote in that repo, and you can see that I had already started putting type annotations in the docstrings. The type checking decorator would come later (and I'm not sure that the version of Python I had at the time even supported decorators).

## 2004 - 2010: Graduate School
Ph.D. in Linguistics at Indiana University
- Minor in Computer Science (IU Ph.D.s are required to have a minor.)

### Factorial Typologies in OT: Four Algorithms

I [started writing code](https://github.com/sandersn/ot-misc) to implement Optimality Theory as soon as I started learning it&mdash;and I think this is probably what its creators did. I started by trying to evaluate OT candidates and moved to learning algorithms. Along the way I created a feature database for the Internal Phonetic Alphabet and 3-4 implementations of Levenshtein distance.

Finally, for one of my two qualifying papers, I [implemented four algorithms for factorial typologies](https://github.com/sandersn/ot-factorial). Factorial typologies capture all possible outcomes from a language family. But to evaluate all of them, as the name says, takes factorial time.

### Statistical Dialectology: Syntax

For my second qualifying paper and then for my dissertation, [I measured dialect distance statistically, using syntax](https://github.com/sandersn/dialect). Mostly people use phonological methods, because they work a lot better. But academia requires novelty, so I developed a way to first slice sentences into multiple pieces and then measure the difference between large collections of those pieces. 

For my qualifying paper, I used ICE-GB, the British section of a well-known corpus of English. For my dissertation, I expanded on the methods and used Swediasyn, an unparsed corpus of Swedish interviews. The second was much more successful because the interviews were with a variety of people in their own villages, instead of college students in London.

Looking at the code, you can see me switch from Python to Haskell for the majority of my work. The ICE-handling code is in Python, using my type-checking decorator anywhere the data structures got complicated&mdash;which was most places. There's also a core of C++, needed because I initially misread the paper whose method I based mine on and ran one million iterations instead of one thousand. (I don't think I kept the Caml version that finished extremely slowly because of garbage collection.)

There's still a C++ core in the Swediasyn code, but the Python has simplified to be a runner for all the other pieces of the project. There's no complicated processing except in one unused file that starts with the comment, "Types are invading this code. It's stupid but I can't help it. I don't want to port this code to Haskell so it's going into the comments."

I ported it to Haskell. The rest of the code is Haskell. Still very Lispy Haskell&mdash;I over-relied on deep nesting of built-in data structures&mdash;but at least I would get an error at compile time instead of at the end of a 15-minute run.

Overall, the project code is much more mature than my earlier work, which makes sense given the date and the amount of time I spent working on it. It's multi-lingual (Python for running, C++ for the efficient core, Haskell for tree processing, R for statistics and figure generation) and repeatable (mostly).

### Fing

[Fing](https://github.com/sandersn/fing) is a type search I wrote after I finished my dissertation but before I started work at Microsoft. It's a copy of Hoogle, but based on Microsoft technology, which is how I got the name Fing. It was, in fact, the reason I created a github account. At that time, Microsoft had an anti-open source reputation, so I wanted to make sure that I got it into the open before I started work there full time, so that Microoft couldn't claim it in some way.

The code mostly worked at the time, but never had a polished interface. But it turns out type-based search is mostly useful when you're learning a new language and neeed to learn the new name for the utility you used in the old language. After that, it's not very helpful.

## 2010 - 2015

Microsoft [^2], SCOPE Data Processing Language on COSMOS. <br/>
First in Bing Infrastructure, then in the SQL group.

COSMOS is massively distributed execution; [SCOPE](https://www.goland.org/Scope-VLDB-final.pdf) is massively parallel SQL.
SCOPE mixes ugly all-caps SQL syntax with C# expressions.
I was part of the team that rewrote the SCOPE compiler from scratch.

1. 2010 - 2011: Help rewrite the COSMOS compiler.
1. 2012 - 2013: Integrate Roslyn (C# compiler) to support C# expressions.
1. 2014 - 2015: Make COSMOS more SQL-like, preparing for public release as U-SQL.

- Finest achievement: convincing the optimiser team that it was OK to add the C# `&&`, which short-circuits. Previously, C# `&&` was forbidden and only a non-short-circuiting operator `AND` was allowed. This was great for the optimiser and extremely bad for anybody that needed to handle `null`. This was before C# was null-safe and even before it had `?.`, so SCOPE scripts frequently crashed because of null pointer exceptions.
- Specialty: integrating C# expressions: parsing, binding, type-checking and emitting to the optimiser. Parsing C# in the middle of SQL is an interesting problem, as is embedding those expressions into an artificial C# program and retrieving the types with the compiler.
- I also spent lots of time making the language more like the SQL standard as part of the effort to sell the system as a language named U-SQL.
- I designed (poorly, and with help from experts) U-SQL's C# API for accessing untyped records at runtime.
- I contributed to design decisions throughout, including my favourite feature: pure `FUNCTION`s and impure `PROCEDURE`s.

SCOPE never quite made it out to the public, but there are some related things in public:
- [paper describing early SCOPE](https://www.goland.org/Scope-VLDB-final.pdf)
- [SCOPE Studio for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-ssdevteam.scope-vscode-ext)
- [U-SQL introduction](https://devblogs.microsoft.com/visualstudio/introducing-u-sql-a-language-that-makes-big-data-processing-easy/)

Link to U-SQL documentation, I guess.

## 2015 - Present

Microsoft [^2], Typescript.

When I started working on Typescript, I hadn't used Javascript in 10 years, and barely remembered the language. So I did the usual thing of learning the code base by fixing complex bugs.

Later  I worked on a few core language features: 
- `this` parameters, spread and rest typing
- Definitely Typed (link to Definitely Typed, dt-tools or publisher CI, mergebot, and my measurements of usage)
- jsdoc support, as well as Closure and CommonJS support in JS (link to bugs where Google people are moving off of Closure)
- mini-typescript and youtube videos
- community PR triage
- AI (some of this is even public now!)

- Finest achievement: JSDoc typing good enough for the NYT Covid Tracking Page (via Rich Harris)

[^1] I do not recommend College of the Ozarks. They preemptively sued the government to discriminate against transgender individuals in campus housing.

[^2] Your text here.
