# Projects

This is a project-focussed view of my career, from the first program I made for others to use all the way to present day. You might want to skip ahead to at least graduate school for things to get interesting.

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

The code is Python 2, so it should still be easy enough to run (I haven't tried it). The UI wrappers are a mix of cgi-bin and wxWindows. I'd stick to the command-line versions.

There is a bunch of unnecessary stuff I threw in from reading [Jurafsky and Martin's natural language processing textbook](https://web.stanford.edu/~jurafsky/slp3/), like some kind of part-of-speech tagger. This was the biggest program I had ever written, and shows the limits of my design experience at the time. Programming in untyped Python certainly didn't help, although this is probably less obvious from the source code. (typed Python didn't exist at the time.)

### Highly Impressive Quenya Parser

A repackaging of the parser work for a different senior project, this one a general education class on Tolkien, right before the release of _The Return of the King_ movie. I found a few sentences of Quenya, hand-tagged them, and changed the heading on the cgi-bin page.

[diemquenya.py](https://github.com/sandersn/nuwen/blob/main/diemquenya.py) was one of the last files I wrote in that repo, and you can see that I had already started putting type annotations in the docstrings. The type checking decorator would come later (and I'm not sure that the version of Python I had at the time even supported decorators).

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

## 2010 - 2015: SCOPE

Microsoft, SCOPE Data Processing Language on COSMOS. <br/>
First in Bing Infrastructure, then in the SQL group.

COSMOS is massively distributed execution; [SCOPE](https://www.goland.org/Scope-VLDB-final.pdf) is massively parallel SQL. The language is still a dialect of SQL&mdash;look for ugly all-caps SQL syntax&mdash;but it compiles to a graph of .NET executables passing rows of data of data between computers. The .NET executables lead to one of the distinctive things about SCOPE: its scalar expressions are C# expressions.

### 2011 - 2012: Help rewrite the COSMOS compiler.
SCOPE had just transitioned from research project to foundation of Bing's offline training infrastructure. But its compiler was still barely a prototype, written without any resemblance to a normal compiler.  I was part of the team that rewrote the SCOPE compiler from scratch.

### 2012 - 2013: Integrate Roslyn (C# compiler) to support C# expressions.
The biggest hole near the end of the rewrite was the ersatz parsing and checking of scalar expressions. It was a small, badly specified copy of C# expressions. Fortunately, at the time, Roslyn (the C# compiler rewrite) was nearly done. So we delegated parsing and checking to Roslyn. I figured out how to generate a synthetic program from the scalars of each SCOPE statement and then retrieve the types from it. Plus, SCOPE supports a little more expression-level syntax than C#, so I had to pre-parse and rewrite that.

### 2014 - 2015: Make COSMOS more SQL-like, preparing for public release as U-SQL.
When Microsoft decided to sell COSMOS on Azure, with SCOPE renamed U-SQL, we decided that it needed to become more similar to the SQL standard. It would still be quite non-standard, but the idea was to be less surprising to existing SQL programmers. It helped that in preceding years, SCOPE style had become more and more declarative, drifting further away from the low-level MapReduce graph style.

I spent my time reworking existing statements, but also adding new SQL features that weren't in SCOPE.
- I designed (poorly, and with help from experts) U-SQL's C# API for accessing untyped records at runtime.

- Finest achievement: convincing the optimiser team that it was OK to add the C# `&&`, which short-circuits. Previously, C# `&&` was forbidden and only a non-short-circuiting operator `AND` was allowed. This was great for the optimiser and extremely bad for anybody that needed to handle `null`. This was before C# was null-safe and even before it had `?.`, so SCOPE scripts frequently crashed because of null pointer exceptions.
- Specialty: integrating C# expressions: parsing, binding, type-checking and emitting to the optimiser. Parsing C# in the middle of SQL is an interesting problem, as is embedding those expressions into a synthetic C# program and retrieving the types with the Roslyn compiler.
- Favourite feature: I contributed to design decisions throughout, including my favourite feature: pure `FUNCTION`s and impure `PROCEDURE`s.

SCOPE never quite made it out to the public, but there are some related things in public:
- [paper describing early SCOPE](https://www.goland.org/Scope-VLDB-final.pdf)
- [SCOPE Studio for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-ssdevteam.scope-vscode-ext)
- [U-SQL introduction](https://devblogs.microsoft.com/visualstudio/introducing-u-sql-a-language-that-makes-big-data-processing-easy/)

Search engines still have *links* to U-SQL documentation, but the pages are gone. [Here's a link to the page for `&&` et al](https://learn.microsoft.com/en-nz/u-sql/operators/logical-operators), for example.

## 2015 - Present: Typescript

Microsoft, Typescript.

In 2015, as I started work on Typescript, ES2015 had (finally) been released, and the team had finished implementing the now-standard Javascript features like classes and modules that had been missing from Typescript. Typescript was shifting from being "the future of JavaScript" to being a type checker for standard Javascript. 

That meant that we spent more and more time typing all of the weird patterns that JS programmers bring to TS, rather than TS having its own, insular style.

- Finest achievement: JSDoc typing was good enough to help maintain the New York Times' Covid Tracking Page, their longest-running web project at the time. (via Rich Harris)
- My favourite feature was JSDoc typing until node added Typescript annotation stripping.

### 2015 - 2016: Language features

During this phase I worked on 3 different language features:

- [Variadic type parameters](https://github.com/Microsoft/TypeScript/issues/5453)
- Object [spread](https://github.com/microsoft/TypeScript/pull/11150) and [rest](https://github.com/microsoft/TypeScript/pull/12028), plus types to for precise checking
- [`this` parameter types](https://github.com/microsoft/TypeScript/pull/6739) (and [proposal](https://github.com/microsoft/TypeScript/issues/6018))

None of the new type kinds shipped: new type kinds are complicated and need a really good reason to be in the language to justify their complexity.

My proposal for variadic type parameters got stuck when I realised that a full solution would handle multiple variadic types in a row, and also that the new type kind should share a lot of features with tuples, except that, at the time, tuples didn't have any of those features either. So I'd need to propose and implement improvements to tuple types first. I gave up and switched tasks, and Anders implemented variadic type parameters years later after we had gradually added nearly all of those tuple features.

Object spread and rest syntax were a feature of ES2018 that had just reached stage 3 when I joined the team. I implemented the parsing, binding, checking and emit. 
Checking was the hard part: spread needs a new kind of subtype operator in addition to the 2 Typescript already has (`extends` and `&`) and rest needs a new kind of supertype operator in addition to `|`.
Object spread and rest types have way more quirks and features than `|` and `&`, making them less generally useful. So we decided not to ship them, and instead produce normal object types where we could calculate them and `any` elsewhere. After a couple of years, we decided to type spread with `&`, which is *close* to correct. Welcome to Typescript!

`this` parameter types are a niche feature that remains useful even now that constructor functions are obsolete, because they type callback patterns. But we decided to make them optional to avoid breaking code and forcing people to break their existing code. They still remain useful as documentation and in the cases when both a library and a consumer are pedantic enough to use them. Which is more common than you might think! Welcome to Typescript!

All 3 of these features capture the spirit of developing Typescript. We ship a lot of features that could be correct, but are either (1) too expensive or (2) too breaky to ship that way. Even the more complex ones often have a long history with false starts and delays for other features to be finished. Take a look at [Typescript's early design notes](https://github.com/sandersn/typescript-history) to see how surprisingly early features were discussed.

### 2017 - 2018: Javascript

In 2017, I shifted toward bringing Typescript to Javascript programmers. Before 2017, Typescript strictly required .ts files and produced .js files. Nowadays, you can include .js files and the compiler will do its best to understand them. At the time, I thought that this would be the most-used feature of Typescript; at the time, the number of Javascript authors outnumbered Typescript authors maybe 10 to 1. I also thought that legacy support was important, again because of the huge amount of JS written without the Typescript compiler, or indeed any modern version of Javascript, in mind.

- [Tags](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html) for specifying types and other metadata.
- [Expandos](https://www.typescriptlang.org/docs/handbook/type-checking-javascript-files.html), especially for constructor functions.
- Other legacy features: [CommonJS](https://www.typescriptlang.org/docs/handbook/type-checking-javascript-files.html#commonjs-modules-are-supported), many other weird kinds of expandos, and a lot of [small](https://www.typescriptlang.org/docs/handbook/type-checking-javascript-files.html#var-args-parameter-declaration-inferred-from-use-of-arguments) [legacy](https://www.typescriptlang.org/docs/handbook/type-checking-javascript-files.html#function-parameters-are-optional-by-default) [rules](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html#legacy-type-synonyms).

These days, 67% of Javascript programmers actually write more Typescript, and it's been years since people have seen anything but `class` and `import`. That's why Typescript 7 cuts out a lot of legacy JS features that people don't use much anymore.

For me, it started when I heard Ryan behind me, swearing at his computer. I went over to find out about the problem. Turns out it was a parser, and I happened to have spent a lot of time writing parsers. I took over the JSDoc parser overhaul and continued on to add many more features.

Along the way I found a lot of large JS projects that I used to test. This was useful &mdash; I was always working from real code &mdash; but it did lead me to overfit to a particular time and place: large JS projects written in the mid-early teens. That lead me to put lots of support for Closure compatibility that probably didn't need to be there&mdash;Closure codebases were maintained by exactly the kind of professional teams that would translate to Typescript instead of relying on backward-compatibility in the editor.

I still have a soft spot for JS support even though I don't use it much anymore myself (thanks Node type stripping!). It's exactly the kind of almost-formal, barely intractable language problem that I love to solve. Partly, and imperfectly, but usefully solve.

### 2018 - 2024: Definitely Typed

After spending a lot of time in bringing Typescript to JS programmers, I also helped out making sure that JS packages were available to Typescript programmers.

- In 2018, I took over the [publication part of Definitely Typed](https://github.com/microsoft/types-publisher): the part that periodically looks at commits on the community repo for updated packages and publishes them to npm under the `@types` scope. At the time it was a full-fledged web service that was fairly unreliable. I used what I had seen in COSMOS to put together [watchdogs](https://github.com/DefinitelyTyped/types-publisher-watchdog) and shake out the bugs.
- On the Typescript team, we're on a weekly rotation for approving complex PRs. Over time I became the person who [trained new team members](https://docs.google.com/presentation/d/1_F05Q5-SSJvxDkfHJWMP2rDKoRFigGPkV6zjrmNjRug/edit?slide=id.p#slide=id.p).
- I used Definitely Typed [to answer questions about real-world usage of Typescript](https://github.com/sandersn/dt-perf). Definitely Typed consists of all the "missing packages" that are still written in JS, so they exhibit exactly the kind of *interesting* patterns that Typescript tries to capture. That's also why we use it as a test suite.

In the 2020s, others on the team took over the infrastructure parts, including simplifying the publisher all the way down to a scheduled Github Action (Actions didn't exist at the time we created the web service!). They also created a mergebot that lets people merge their own simple PRs.

In 2016, I noted from my measurements that a random installed package was 49% likely to be typed--"an astounding number, especially since only 10% of packages were typed". That astounding number crept up over the years to nearly 90%, while Definitely Typed's contribution to that number fell from 40% to just 10%. When I stopped measuring at the end of 2023, Definitely Typed largely contributed a few core packages that still don't support Typescript, plus many odd and obsolete packages that only one or two people need.

### 2020 - 2023: Community Support

- PR triage
- education: videos
- education: mini-typescript
- history

In between my other duties (a stream of JS features and bug fixes), I took over PR triage and review. By this time, Typescript was popular enough to attract lots of PR, including those from highly skilled repeat contributors. I tried to sort the unready from ready PRs and assign them to the best team member to review them.

https://github.com/sandersn/pr-triage

I also created [a number of training videos](https://www.youtube.com/@shively-sanders). This was a lot of work and got pre-empted by the AI boom, so there aren't as many as I'd like. They are very very low-fi nerd, both in their content (compiler internals) and form.

I also created [a miniature training compiler called mini-typescript](https://github.com/sandersn/mini-typescript). There are two sizes:  micro- and centi-. micro-typescript is the smallest possible compiler for a Typescript-esque language, barely bigger than a textbook compiler frontend. centi-typescript is a miniature Typescript, still with only support for one of each kind of thing in Typescript, but significantly more realistic. It even has super simple type argument inference.

Even better, I also came up with exercises. There are 10 ideas for what to add to the compiler next.

All of the pre-github design notes for Typescript were stored in an internal Onenote. They stretched from very early post-demo notes in early 2011 to the September 2014. I got permission from Microsoft legal to [save them all to PDF in public](https://github.com/sandersn/typescript-history).


### 2022: Typescript on the web

I got the web version of the tsc compiler working with a WASI filesystem provided by the vscode team. This allowed the real compiler to run on github.dev and vscode.dev, with full access to files that were not open in tabs, but only on the (virtual) filesystem. It also left me despising web development.

https://vscode.dev/
https://github.com/microsoft/vscode/pull/169311

### 2023 - Present: AI Developer Tools

When the AI boom started, I got pulled in early because of my degree in computational linguistics. Computational linguistics would have been called AI, if that name weren't so unpopular at the time. Now that AI has taken over the code-writing part of software engineering, I have switched to it basically full time. Still on the Typescript team, but finding ways to make AI developer tools better for Typescript.

- I added copilot-powered refactors and rename to the Typescript extension, although these are now obsoleted by copilot chat in all but a couple cases. https://github.com/microsoft/vscode/pull/192602
- I added a general mechanism for improving copilot-powered code fixes in vscode-copilot-chat, which is the extension that now provides most AI support in vscode. I [have linked to the source code I wrote](https://github.com/microsoft/vscode-copilot-chat/blob/03c40ebe6ca2d4782d8e85a0a9fe1b914bdf97d8/src/extension/prompts/node/inline/fixCookbookService.ts#L132); the PRs I made are on the original repo, which is still closed source.
- I prototyped lots of Typescript and Definitely-Typed adjacent ideas, [such as a PR triager](https://github.com/sandersn/pr-triage/blob/main/close-pr.ts). This work is closed source.
- I helped add Typescript support to [the Github App Moderniser](https://learn.microsoft.com/en-us/azure/developer/github-copilot-app-modernization/).
- I worked on measurement--of our own tools, of vscode, Copilot completions, and other products I can't talk about. A lot of this revolved around [SWEBench](https://www.swebench.com/) and similar benchmarks.

It turns out one of the things you learn in grad school is rigour. This surprised me because I thought of myself as playing fast, loose and shallow with concepts in school (this was how you know I was doing AI in school: no rigour compared to real academic disciplines). But in the world of developer tools, I'm unusual in knowing how to gain knowledge from stochastic, unreliable processes.

### 2024 - 2025: Typescript native port

For the native port of the Typescript compiler, I went back to Javascript for a little while. This was an opportunity to revisit code that was ugly and inconsistent.
But at a higher level, lots and lots of the code existed to support features that never actually got used very much. I wasn't very good at planning features 7-8 (!) years ago.
So it was really an opportunity to find features that weren't worth supporting and remove the code to support them. This is really what makes the native code simpler.

- Most of the compiler is a straight port. The only part I ported straight over was [the JSDoc scanner and parser](https://github.com/microsoft/typescript-go/pull/225).
- I wrote [a "reparser"](https://github.com/microsoft/typescript-go/pull/610) that copies simple constructs with exact Typescript equivalents from JSDoc into the correct place in the tree. `@type` becomes a synthetic Typescript type annotation, for example.
- Expandos use the same technique as the old compiler, but with rewritten code. I didn't write ([much](https://github.com/microsoft/typescript-go/pull/860)) of that code, though.

[^1] I do not recommend College of the Ozarks. They preemptively sued the government to discriminate against transgender individuals in campus housing.