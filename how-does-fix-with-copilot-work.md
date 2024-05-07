# How Does *Fix With Copilot* Work?
If you've [used Github Copilot in VS Code](https://docs.github.com/en/copilot/github-copilot-chat/using-github-copilot-chat-in-your-ide), you might have noticed that it offers a new option in the quick fix menu: *Fix With Copilot*. It's available for every error in every language.

![Error from ESLint](images/ai-no-unmodified-loop-condition-3.png)

This works well with the many excellent ESLint rules that don't have fixers for one reason or another. For example, take the ESLint rule [`no-unmodified-loop-condition`](https://eslint.org/docs/latest/rules/no-unmodified-loop-condition). It makes sure that a `while` loop body updates the variable it's checking.

![Error from ESLint](images/ai-no-unmodified-loop-condition-1.png)

There's no single way to fix this error, so ESLint doesn't have a fixer for it. But Copilot can figure out what's wrong: I should be checking `node` in the while loop, not `start`. It's the kind of silly mistake that's invisible to me until somebody else points it out.

![Error from ESLint](images/ai-no-unmodified-loop-condition-2.png)
	
Sometimes Copilot needs more information to fix errors. Here's another great ESLint rule: [`no-dupe-else-if`](https://eslint.org/docs/latest/rules/no-dupe-else-if). It catches if/else conditions that are duplicated by mistake. 

![Invoking AI](images/ai-no-dupe-if-1.png)

The rule points out that I've made a copy/paste error by forgetting to update the third condition of three very similar conditions. Copilot can get the right answer because, in a recent update to its VS Code extension, I added some additional instructions to the hidden Copilot prompt.

![Invoking AI](images/ai-no-dupe-if-2.png)

Without a nudge in the right direction, Copilot thinks that deletion is the best answer -- after all, duplicate code is bad, right?

Another place Copilot needs help is fixing a TypeScript error when I call an imported function with bad arguments:

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

At its core, *Fix With Copilot* is pretty simple: for any error in your code, it asks Copilot "There's an error ***\[\[Error Text Here\]\]*** in the code ***\[\[Code Here\]\]***. Can you fix it?"

This works great, in situations that LLMs are good at: a mistake that happened in the last few lines, plus an error message that explains how to fix it. But when the prompt doesn't imply a clear solution, Copilot will generate an answer based mostly on its training. Even worse, if the prompt implies the *wrong* solution, Copilot will always follow along.

The bottom line is that LLMs generate text from their training based on a prompt. If the prompt suggests a solution that's similar to some training, they're going to generate a good answer. To get good solutions to your specific problem, you have to make a prompt that *implies* a good solution.

Sometimes an error message does this: Did you tell Copilot "*'start' is not modified in this loop*"? It'll look around at 'start' in the `while` loop and compare it to patterns of `while` loops from training. Then it'll generate a fix that looks like patterns from those `while` loops.

In contrast, if you tell Copilot "*This branch can never execute.*", it'll probably find a lot of training that suggests that dead code should be deleted. But it doesn't consider intent or context. Sure, if you're writing an optimiser, dead code elimination is part of the job. But if you end up with a duplicate in an editor, it's more likely that you were copying a long series of cases and forgot to update one of them. (That's why I like the `no-dupe-else-if` rule so much. It's **always** paying attention.)

![Invoking AI](images/ai-no-dupe-if-3.png)

So, specifically for `no-dupe-else-if`, I nudge Copilot to update the copied code. I add "Fix the duplicate condition to be different from the first." to the hidden prompt. Not only does Copilot fix the duplicate case this way, it even gives my nudge as the reason it did so.

![Invoking AI](images/ai-no-dupe-if-2.png)

Sometimes error messages don't actively mislead so much as fail to imply a solution. When you call a function with bad argument types, TypeScript says "*Argument of type 'boolean' is not assignable to parameter of type 'number'.*" but doesn't tell you how to fix it. That's on purpose: TypeScript has no way to know whether the call or the function itself is wrong.

Copilot might be able to guess, but *Fix With Copilot* can't even see `connect`'s code because it's imported. There's no way for anybody to figure out that the arguments are swapped without seeing `connect`. So, with just the error message to go on, Copilot makes a very local change: it reasons that `0` is the closest `number` equivalent of `false`. That technically fixes the error but another immediately pops up, because it's not a correct fix.

![Invoking AI](images/ai-bad-call-3.png)

After I include the code for `connect` in the hidden prompt, Copilot can figure out that the arguments are swapped. Seeing that signature `function connect(address: string, timeout: number, loose: boolean)` strongly implies that the correct order is `timeout, loose`, not `loose, timeout`.

![Invoking AI](images/ai-bad-call-2.png)

## But really, how does *Fix With Copilot* work?

So that's why these additional instructions and context are needed.
Now let's talk about how they fit into the actual implementation of *Fix With Copilot*.

The first thing *Fix With Copilot* does is register as a fix provider for every kind of file. Every single one. Then, for each document you have open, it requests errors and adds *Fix With Copilot* to the list of available fixes for each.

When you click on *Fix With Copilot*, it invokes Copilot's inline chat with a hidden prompt constructed from JSX components. JSX&mdash;but not React. Copilot has its own renderer that generates plain text from JSX components, so *Fix With Copilot*'s prompt is structured as a React-like hierarchy of small components, which is useful for managing the different features of the prompt. As a bonus, it gives the code familiar visual ergonomics.

Here's an example of what the code looks like. `diagnostics` is the VS Code term for errors, warnings, etc. `cookbook` is the term for the per-error nudges in the form of extra instructions. `documentContext` has a reference to the current document, its language, and similar information.

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

By default *Fix With Copilot* includes the entire file around the error. Of course, most of this time the entire file won't fit in the prompt. Instead, it includes the code in current function and summarizes code outside. 

The algorithm to do this is complicated. It uses the parse tree to assign depths and distances to each block in the file. For example, the top-level `interface Node<T>` in the diagram is `distance: 2, depth: 1` from the error-containing function `errorInHere`. The body of `findRoot<T>` is `distance: 1, depth: 2`, since it's only one function above the error, but it's one down from the top level.

![ai-summarised-document.png](images/ai-summarised-document.png)

Once every block has a distance and depth, each one gets a score and everything above a certain score gets abbreviated. In the simplified example above, if you just sum `distance` and `depth`, you can see that anything above 3 is abbreviated. The body of `while (node)` was `distance:1 + depth:3` and the body of `interface Node<T>` was `distance:2 + depth:2`, so both get abbreviated.

Formatting the error starts with the error text itself, then adds a couple of sub-components: `DiagnosticRelatedInfo` and `DiagnosticSuggestedFix`. `DiagnosticRelatedInfo` itself adds two kinds of information: the first is any related code that the error itself returns. For example, TypeScript errors like "Duplicate declaration" include the duplicated declaration as related info. The second kind of information is the source of imported functions like I showed above. *Fix With Copilot* finds the source of a called function in a language-independent way, using VS Code's language service API, although right now the code only applies to TypeScript.

`DiagnosticSuggestedFix` is the 'nudge' I described earlier. Its uses a Javascript object to map error codes to additional instructions. Each language has a nested object. Here's a snippet of the object in the middle of the `"typescript:eslint"` object:

```ts
'no-dupe-else-if': [
    'Fix the duplicate condition to be different from the first.',
    'Remove the duplicate condition',
],
'no-duplicate-case': [
    {
        title: 'Change the duplicate condition to be different.',
        message: 'Do not delete the duplicate case, just fix it.',
        replaceError: true
    },
    'Remove the duplicate condition',
],
'no-duplicate-imports': 'Merge the duplicated import lines.',
```

In this object, errors can map to multiple instructions, like `no-dupe-else-if`, and the instructions can have a visible/hidden part, like `no-duplicate-case` does. Neither of those features are finished yet; currently only the first instruction gets added to the hidden prompt, and none of it is visible. Also, the instructions can request to replace the original error message, as `no-duplicate-case` does with `replaceError: true`. That's useful when the error message causes Copilot to do the wrong thing.

Before all of this, the prompt includes [few-shot learning](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/advanced-prompt-engineering?pivots=programming-language-chat-completions#few-shot-learning) to explain how to answer a code question, and how to format it so that the code and the explanation are clearly separated.

Then off to Copilot it goes. Once an answer comes back, we use [markdown-it](https://github.com/markdown-it/markdown-it) to parse the reply, making it easy to separate the explanation and the code. The explanation goes to the inline chat window. Meanwhile, we compare the code from Copilot's reply with the code surrounding the error using a custom diff algorithm. Finally, we show the diff using VS Code's typical diff display.

## Tasks

- [x] Learn about the missing pieces of my knowledge in the code.
- [x] Finish writing up the code part.
- [x] Daniel's comments
- [x] Polish the writing, especially near the end.
- [x] consistency: spelling of brand names (TypeScript, eslint, vscode, Fix With Copilot)
- [ ] consistency: verb tense and person. Separate I=example code, we=copilot code.
- [ ] more links to outside stuff? maybe. There aren't many dependencies.
- [x] Find a good example for base Fix With Copilot.
- [x] Take new pictures or gifs (with better cropping than the current set)
- [ ] Figure out how to mention my typescript extension work?
- [ ] add alt text
- [ ] rename file names to be in order/remove unused images

