# How Does *Fix With Copilot* Work?
If you've used Github Copilot in VS Code, you might have noticed that it offers a new option in the quick fix menu: *Fix With Copilot*. It's available for every error in every language.

![Error from ESLint](images/ai-no-unmodified-loop-condition-3.png)

This works well with the many excellent ESLint rules that don't have fixers for one reason or another. For example, take the ESLint rule `no-unmodified-loop-condition`. It makes sure that a `while` loop body updates the variable it's checking.

![Error from ESLint](images/ai-no-unmodified-loop-condition-1.png)

There's no single way to fix this error, so ESLint doesn't have a fixer for it. But Copilot can figure out what's wrong: I should be checking `node` in the while loop, not `start`. It's the kind of silly mistake that's invisible to me until somebody else points it out.

![Error from ESLint](images/ai-no-unmodified-loop-condition-2.png)
	
Sometimes Copilot needs more information to fix errors. Here's another great ESLint rule: `no-dupe-else-if`. It catches if/else conditions that are duplicated by mistake. 

![Invoking AI](images/ai-no-dupe-if-1.png)

The rule points out that I've made a copy/paste error by forgetting to update the third condition of three very similar conditions. Copilot can get the right answer because, in a recent update to its VS Code extension, I added some additional instructions to the hidden Copilot prompt.

![Invoking AI](images/ai-no-dupe-if-2.png)

Without a nudge in the right direction, Copilot thinks that deletion is the best answer -- after all, duplicate code is bad, right?

Another place Copilot needs help is fixing a Typescript error when I call an imported function with bad arguments:

![Invoking AI](images/ai-bad-call-6.png)

 In this code I mistakenly swapped the last two arguments to `connect`...but you have no way to know that unless you see the source of `connect`. Neither does Copilot.

```ts
// in database-mock.ts
export function connect(address: string, timeout: number, loose: boolean) {
    // TODO: Actually connect
}
```

Once I give Copilot the source of `connect` in the hidden prompt, the fix is easy:

![Invoking AI](images/ai-bad-call-4.png)

## Why Does Copilot Work This Way?

So why does Copilot need this help? What happens if it's not there? Let's start by looking at how *Fix With Copilot* works at a high level.
<!-- speculative -->

At its core, *Fix With Copilot* is pretty simple: for any error in your code, it asks Copilot "There's an error ***\[\[Error Text Here\]\]*** in the code ***\[\[Code Here\]\]***. Can you fix it?"

This works great, in situations that LLMs are good at: a mistake that happened in the last few lines, plus an error message that explains how to fix it. But when the prompt doesn't imply a clear solution, Copilot will generate an answer based mostly on its training. Even worse, if the prompt implies the *wrong* solution, Copilot will always follow along.

The bottom line is that LLMs generate text from their training based on a prompt. If the prompt suggests a solution that's similar to some training, they're going to generate a good answer. To get good solutions to your specific problem, you have to make a prompt that *implies* a good solution.

Sometimes an error message does this: Did you tell Copilot "*'start' is not modified in this loop*"? It'll look around at 'start' in the `while` loop and compare it to patterns of `while` loops from training. Then it'll generate a fix that looks like patterns from those `while` loops.

In contrast, if you tell Copilot "*This branch can never execute.*", it'll probably find a lot of training that suggests that dead code should be deleted. But it doesn't consider intent or context. Sure, if you're writing an optimiser, dead code elimination is part of the job. But if you end up with a duplicate in an editor, it's more likely that you were copying a long series of cases and forgot to update one of them. (That's why I like the `no-dupe-else-if` rule so much. It's **always** paying attention.)

![Invoking AI](images/ai-no-dupe-if-3.png)

So, specifically for `no-dupe-else-if`, I nudge Copilot to update the copied code. I add "Fix the duplicate condition to be different from the first." to the hidden prompt. Not only does Copilot fix the duplicate case this way, it even gives my nudge as the reason it did so.

![Invoking AI](images/ai-no-dupe-if-2.png)

Sometimes error messages don't actively mislead so much as fail to imply a solution. When you call a function with bad argument types, Typescript says "*Argument of type 'boolean' is not assignable to parameter of type 'number'.*" but doesn't tell you how to fix it. That's on purpose: Typescript has no way to know whether the call or the function itself is wrong.

Copilot might be able to guess, but *Fix With Copilot* can't even see `connect`'s code because it's imported. There's no way for anybody to figure out that the arguments are swapped. So, with just the error message to go on, Copilot makes a very local change: it reasons that `0` is the closest `number` equivalent of `false`. That technically fixes the error but another immediately pops up, because it's not a correct fix.

![Invoking AI](images/ai-bad-call-3.png)

When I look up and include the code for `connect` in the hidden prompt, Copilot can figure out that the arguments are swapped.

![Invoking AI](images/ai-bad-call-2.png)

## But really, how does Fix With Copilot work?

The first thing *Fix With Copilot* does is register as a fix provider for every kind of file. Every single one. Then, for each document you have open, it requests errors and adds *Fix With Copilot* to the list of available fixes for each.
<!-- see inlineChatCommand.ts:registerInlineChatCommands
and inlineChatCodeActions.ts:QuickFixesProvider -->

When you click on *Fix With Copilot*, it invokes Copilot's inline chat with a hidden prompt constructed from JSX components. JSX&mdash;but not React. Copilot has its own renderer that generates plain text from JSX components, so *Fix With Copilot* is structured as a React-like hierarchy of small components. In addition to looking familiar, one clever property on all these components is `priority`. The renderer can drop low-priority components if the prompt gets too big. Here's an example of what the code looks like. `diagnostics` is the list of errors to be fixed:
<!-- see inlineChatFix4Prompt.tsx (old version for now) -->

```tsx
diagnostics.map((d, idx) => {
    const cookbook = getFixCookbook(documentContext.language.languageId, d);
    return <>
        <DiagnosticDescription 
          diagnostic={d} 
          cookbook={cookbook} 
          showLineNumbers={this.props.showLineNumbers ?? false} 
          maxLength={LINE_CONTEXT_MAX_SIZE} 
          documentContext={documentContext} />
        <DiagnosticRelatedInfo diagnostic={d} cookbook={cookbook} document={documentContext.document} />
        <DiagnosticSuggestedFix cookbook={cookbook} />
    </>;
})
```

What components are there? The main ones are:

- Include code surrounding the error.
- Format the error nicely.
- Provide instructions to Copilot.

By default *Fix With Copilot* includes the entire file around the error. Of course, most of this time the entire file won't fit in the prompt. Instead, it includes the code in current function and summarizes code outside. The summary is constructed by X.

So next it tries to summarise the code outside the current function by selectively deleting code. By default it deletes all function bodies outside the current one.
(By default it divides the file into blocks and builds a tree out of it. Then each block gets assigned a weight based on distance from the error, and nesting depth. Blocks below a cutoff are omitted, so the prompt will include code near the error and less and less detail further away. 

    TODO: Find out from Martin how commonly this method is the one used, probably only describe the simple or complex one. This would be a good place for a diagram to explain the complex method.

Formatting the error starts with the error text itself, unless my cookbook offers a replacement. But it also includes a couple of sub-components. The first includes code from related error spans. Language services can attach related information to an error, and Copilot will include the function that contains each related location. The other subcomponent is example usages, which is what I showed earlier for fixing Typescript errors. TODO: Make this longer, better structured and more readable.

    Code snippet showing the cookbook data structure.

TODO: Talk about the cookbook a little more. Objecty, applies to all languages, might not replace the error entirely.

Before all of this, the prompt includes some [one-shot training](http://useful-link-here.com) to explain how to answer a code question, and how to format it so that the code and the explanation are clearly separated.

Then off to Copilot it goes. Once an answer comes back, we use markdown-it to parse the reply, making it easy to separate the explanation and the code. The explanation goes to the inline chat window. Meanwhile, we compare the code from Copilot's reply with the code surrounding the error. Typically, we use a custom diff algorithm to find the best place to make the edit, although there are a couple of fallbacks if that fails. Finally, we show the diff using VS Code's typical diff display. TODO: Make this more detailed and more readable.

5. The code gets matched with the original span of the error.
6. The range is passed in from outside. The diff is potentially the whole file, or just the summarised parts.
7. The code gets diffed and displayed in a new-biased diff format.
<!-- See editGeneration.ts:generateInlineEditWithUnknownBlock (and callers) -->

## Tasks

- [x] Learn about the missing pieces of my knowledge in the code.
- [x] Finish writing up the code part.
- [ ] Daniel's comments
- [ ] Polish the writing, especially near the end.
- [x] Find a good example for base Fix With Copilot.
- [x] Take new pictures or gifs (with better cropping than the current set)
- [ ] Figure out how to mention my typescript extension work?
- [ ] add alt text
- [ ] rename file names to be in order/remove unused images

