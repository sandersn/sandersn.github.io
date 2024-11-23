# Projects

## 1995&ndash;1999: High School

### Thingy32

[Thingy32](https://www.romhacking.net/utilities/218/) is a hex editor that lets you load custom coding schemes, not just ASCII. It is a port of the DOS-based tool [Thingy](https://www.romhacking.net/utilities/217/). Many games had simple, custom coding schemes based on indexing into a font stored in memory somewhere. Thingy lets you specify a coding scheme using a simple INI-like format. Then you can read or dump the data in a ROM interpreted according to that scheme.

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

Finally, for one of my two qualifying papers, I [implemented four algorithms for factorial typologies](https://github.com/sandersn/ot-factorial). Factorial typologies capture all possible outcomes from a language family. But to evaluate all of them, as the names says, takes factorial time.

### Statistical Dialectology: Syntax

For my second qualifying paper, I measured dialect distance statistically, using syntax. Mostly people use phonological methods, because they work a lot better. But academia requires novelty.
First on ICE-GB, then on Swediasyn or Nodalida or some other corpus I can't even remember anymore.

### Fing

Type search like Hoogle but not nearly as finished.

## 2010 - 2015

Microsoft [^2], SCOPE Data Processing Language on COSMOS.
First in Bing Infrastructure, then in the SQL group.

COSMOS is massively distributed execution; SCOPE is massively parallel SQL.
SCOPE mixes ugly all-caps SQL syntax with C# expressions.
I was part of the team that rewrote the SCOPE compiler from scratch.

1. 2010 - 2011: Help rewrite the COSMOS compiler.
1. 2012 - 2013: Integrate Roslyn to support C# expressions.
1. 2014 - 2015: Make COSMOS more SQL-like, preparing for public release.

- Finest achievement: convincing the optimiser team that it was OK to add the C# `&&`, which short-circuits. Previously, C# `&&` was forbidden and only a non-short-circuiting operator `AND` was allowed. This was great for the optimiser and extremely bad for anybody that needed to handle `null`. (This was before C# was null-safe and even before it had `?.`)
- Specialty: integrating C# expressions: parsing, binding, type-checking and emitting to the optimiser.
- I also spent lots of time making the language more like the SQL standard as part of the effort to sell the system as a language named U-SQL.
- I designed (poorly, and with help from experts) U-SQL's C# API for accessing untyped records at runtime.
- I contributed to design decisions throughout, including my favourite feature: pure `FUNCTION`s and impure `PROCEDURE`s.

Link to U-SQL documentation, I guess.

## 2015 - ???

Microsoft [^2], Typescript.

- Learning the code base by fixing complex bugs.
- Core language features like: `this` parameters, spread and rest typing.

- `this` parameters, spread and rest typing
- Definitely Typed (link to Definitely Typed, dt-tools or publisher CI, mergebot, and my measurements of usage)
- jsdoc support, as well as Closure and CommonJS support in JS (link to bugs where Google people are moving off of Closure)
- mini-typescript and youtube videos
- community PR triage
- AI

- Finest achievement: JSDoc typing good enough for the NYT Covid Tracking Page (via Rich Harris)

[^1] I do not recommend College of the Ozarks. They preemptively sued the government to discriminate against transgender individuals in campus housing.

[^2] Your text here.