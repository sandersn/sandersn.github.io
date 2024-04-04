# Adding AI to Language Services

This is a summary of my AI work on the Typescript team.
I've been looking for useful applications of AI to make it easier to write Typescript code.
Along the way I've tried a lot of things and come up with a language-centric picture of the value of current-generation AIs.

## Value

One of my basic assumptions, backed up by some experience, is that a chat interface is not the best interface for coding.
It works well enough for high-level, discrete tasks, but is too much work for simple things -- the things that an LLM is most likely to get right.
Today, all programmers are new to working with AI.
They're going to do best with tools they're familiar with at first: in this case I'm thinking of completions and refactors, which the IDE suggests instead of being queried for.

Even though they're packaged in a chat interface currently, LLMs are good at a number of tasks.
Specifically, they're good at "creative" tasks (generative, actually), from simple to complex. But they're just generating code with no way to check it, and no reference to an explicit semantics.
Fortunately, the tools we already have complement this perfectly: they're not creative, but they encode and check language semantics.

TODO: I don't even consider augmenting our existing tools' *internals* by consulting an LLM. The modes of reasoning are too different and LLMs are the complex thing -- they'd have to bridge to the logical reasoning of our tools.

We have an immediate opportunity to augment our existing tools with LLMs, re-using their interface.
LLMs can provide the appearance of creativity that our tools are missing.
(Or they can choose between possible solutions with far more context than our existing tools can.)
And, especially for simple tasks, their output is easy to check with our existing tools.

That's why the features and history I describe below focus on augmenting existing tools and ways of working.

## Shipped Features

The first feature I implemented AI followups to refactors and codefixes, based on a prototype by Johannes Rieken.
When Typescript runs a refactor, it can generate code that would benefit from AI editing: giving good names to things, filling in default implementations, etc.

### Implement interface

For many interfaces, implementation is boilerplate that's easy for the AI to generate.

| Unimplemented interface | |
|----|----|
| ![Invoking AI](images/ai-quickfix-implements-2.png) |
| Typescript output | Copilot output |
| ![Before AI](images/ai-quickfix-implements-4.png) | ![After AI](images/ai-quickfix-implements-3.png) |


### Suggest meaningful names

This is an easy task as long as the names are accessible in the context window:

| Missing parameter names | |
|----|----|
| ![Invoking AI](images/ai-add-names-1.png) &rArr; |  |
| Typescript output | Copilot output |
| ![Before AI](images/ai-add-names-3.png) | ![After AI](images/ai-add-names-2.png) |


### Infer types in untyped Typescript

Couple of things to note here:

1. This codefix does not invoke the typescript language service.
2. On any of these AI followups, you can request changes, just like a chat interaction.
3. This works in Javascript too; the LLM produces JSDoc types.

| Implicit `any` on parameters | |
|----|----|
| ![Invoking AI](images/ai-infer-types-1.png) |  |
| Typescript output | Copilot output |
| ![Before AI](images/ai-infer-types-4.png) | ![After AI](images/ai-infer-types-2.png) |
| | Improved Copilot output |
| | ![After AI](images/ai-infer-types-3.png) |

### Augment "Fix with Copilot" with better errors

eslint rule no-dupe-if. The error misleads the AI into thinking that deletion is the best option.
In reality, it's more likely that a duplicate is a copy/paste that was mistakenly not updated.

| Duplicate `if` predicates |
|----|
| ![Invoking AI](images/ai-no-dupe-if-1.png) | 
| AI output when only given error | 
| ![Before AI](images/ai-no-dupe-if-3.png) |
| AI output when given additional prompt |
| ![After AI](images/ai-no-dupe-if-2.png) |

### Augment "Fix with Copilot" with context

Fix with Copilot is not normally able to include context outside the current file.
I added context of the currently-called function, even from imports.
In the example below, the AI is then able to correctly swap out-of-order arguments.

| Swapped function call arguments |
|----|
| ![Invoking AI](images/ai-bad-call-1.png) |
| AI output when only given error | 
| ![Before AI](images/ai-bad-call-3.png) |
| AI output when given context |
| ![After AI](images/ai-bad-call-2.png) |

## Distractions, Duplications and Paths Not Taken

The road to the shipped features, and my philosophy, had a lot of detours and wrong turns.
Let me tell you about them.
It starts with type inference in untyped Typescript code.
I wrote the deterministic inference for the typescript language service some years ago, never mind how long precisely.
The type inference codefix is nice but limited.
In particular, it can only infer primitives and object types.
It can't infer a new interface or class type based on usage, even consistent usage throughout the program.

My first experiment with copilot's inline chat showed me that LLMs could do this task wonderfully.
I went looking for how to prototype this and ended up in vscode's typescript extension.
There, I saw Johannes Rieken's prototype which invoked Copilot inline chat after a Typescript refactor.

I quickly expanded the prototype to cover the rest of Typescript's refactors and code fixes.
These shipped in VSCode Insiders but were behind a flag for a long time so few people used them.
The most useful to me were those that suggested meaningful names after extracting a local or function, although that's now obsoleted by AI-added suggestions in all renames, which came to VS first and VS Code later.

In the meantime, I experimented with improving type inference by adding context to the prompt.
I abandoned this because my initial results weren't great and I thought people didn't use it much anyway.
This turned out to be incorrect, as we discovered later when Eric Cornelson began experimenting with whole-file JS type inference.
Typescript's deterministic type inference is actually 4th most-used code fix after auto-imports and spelling correction.

Another reason I switched is that I started thinking about other code fixes -- there are quite a lot of errors that are simple, but code fixes are so difficult to write.
The problem is particularly widespread in eslint, because eslint rules often have simple semantics and contributors without enough time or expertise to write a code fix.
Again, the AI turned out to be very good at this.

So good, in fact, that vscode's copilot already has a "Fix with Copilot" feature that puts the error in the prompt and asks the AI to fix it.
However, some errors are misleading or unhelpful, since error span plus error text really isn't much context.
I added the prompts I'd developed to Fix with Copilot. 
But plenty of fixes I tested still didn't improve.
Replacing misleaing errors helped.

At this point I caught the attention of the vscode-copilot team, and they shared some telemetry with me.
This showed that the most used "Fix with Copilot" error was "Argument type does not match parameter type in call" -- and the rest of the top 10 were mostly variations on "This type can't be assigned to that type".
This is a considerably harder problem, one that an LLM isn't necessarily able to help with.
But I suspected that some context could help, so I added the source of the called function to the prompt, which improves results even when it doesn't prompt the AI to correct fix.

## Future Work

Currently I'm adding more kinds of context to the LLM's prompt, trying to make it better at fixing bad-type-assignment errors of all kinds.
The current step is to add other calls to the function whose call is bad.
There are other kinds of errors that will benefit from other kinds of context, so I'll look there afterwards.
It's really simple to add improved prompts, so I want to help people who own other language services improve the performance of "Fix with Copilot" for their language too.
And I have a long list of experiments to try.