# Adding AI to Language Services

This is a summary of my AI work on the Typescript team.
I've been looking for useful applications of AI to make it easier to write Typescript code.
Along the way I've tried a lot of things and come up with an opinion of how best to use current-generation AIs for language services.

## Value

One of my basic assumptions, informed by experience with copilot and chatgpt, is that a chat interface is not the best interface for coding.
It's a decent starting point for sizable, discrete tasks, but most of the time people don't break down tasks discretely in their head.
They just want help with the next step, and the next step is usually "writing code".
Fortunately, our tools are designed to make writing code easy.
It's a matter of re-packaging LLMs from their chat interface into a form that our editors can use, and that our users are familiar with: completions, refactors and code fixes, for example.

Packaged this way, LLMs complement Devdiv's existing tools quite well.
Specifically, they're good at "creative" tasks (well, generative, at least), where our tools are good at tasks with a single right answer.
(This is true all the way down to single-line fixes, and the AI has a good chance of guessing right in tasks this small.)
But they're just generating code with no explicit reference to semantics, and no way to check its correctness.
Good news! Devdiv is full of checkers: language services, linters, test harnesses, etc.
We can automatically check LLM output in number of ways as soon as it's generated.

So my approach has been to augment our existing tools with LLMs, re-using the existing interface.
This gives our tools the appearance of creativity that they've been missing.
Then all the usual checks can be run on the output of the LLM.

## Shipped Features

The first feature I implemented AI followups to refactors and codefixes, based on a prototype by Johannes Rieken.
When Typescript runs a refactor, it can generate code that would benefit from AI editing: giving good names to things, filling in default implementations, etc.

### Implement interface

For many interfaces, implementation is boilerplate that's easy for the AI to generate.
For this fix, the AI works on the output of the typescript language service.

| Unimplemented interface | |
|----|----|
| ![Invoking AI](images/ai-quickfix-implements-2.png) |
| Typescript output | Copilot output |
| ![Before AI](images/ai-quickfix-implements-4.png) | ![After AI](images/ai-quickfix-implements-3.png) |


<!--
### Suggest meaningful names

This is an easy task as long as the names are accessible in the context window:

| Missing parameter names | |
|----|----|
| ![Invoking AI](images/ai-add-names-1.png) &rArr; |  |
| Typescript output | Copilot output |
| ![Before AI](images/ai-add-names-3.png) | ![After AI](images/ai-add-names-2.png) |

-->

### Infer types in untyped Typescript

This fix does not invoke the Typescript language service.
Like the language-provided fix, the LLM can fix Javascript too; it generates types in JSDoc instead of Typescript annotations.
On any of these AI followups, you can request changes, since the interface is an inline chat.

| Implicit `any` on parameters | |
|----|----|
| ![Invoking AI](images/ai-infer-types-1.png) |  |
| Typescript output | Copilot output |
| ![Before AI](images/ai-infer-types-4.png) | ![After AI](images/ai-infer-types-2.png) |
| | Improved Copilot output |
| | ![After AI](images/ai-infer-types-3.png) |

### Augment "Fix with Copilot" with better errors

The eslint rule `no-dupe-if` has a misleading error that prompts the AI to delete code.
It acts as if the only reason for duplicate `if` conditions is that the entire code was copied twice.
In reality, it's more likely that a duplicate is a an intentional duplication that was mistakenly not updated.


| Duplicate `if` predicates |
|----|
| ![Invoking AI](images/ai-no-dupe-if-1.png) | 
| AI output when only given error | 
| ![Before AI](images/ai-no-dupe-if-3.png) |
| AI output when prompted to fix instead of delete |
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

Both of these augmentations are powered by a JSON object literal that supports any language that VSCode does &mdash; it's as simple as adding a new entry for a language and an object literal with entries for error codes.

<!--
## Distractions, Duplications and Paths Not Taken

The road to the shipped features, and my philosophy, had a lot of detours and wrong turns.
Here are a couple.

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
-->

## Future Work

Currently I'm adding more kinds of context to the LLM's prompt, trying to make it better at fixing bad-type-assignment errors of all kinds.
The future is to augment the prompt with correct context so that Copilot can fix any error that requires non-local information&mdash;even if the fix is not local.

It's really simple to add improved prompts, so I want to help people who own other language services improve the performance of "Fix with Copilot" for their language too.

And I have a long list of experiments to try.