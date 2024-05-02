# How Does *Fix With Copilot* Work?
In VS Code, Copilot has a feature called *Fix With Copilot* that offers to fix any error in the editor with AI. It's like a code fix for every error imaginable, with the caveat that Copilot is just trying its best.

	Show an example here.
	
The Copilot extension intercepts every single error (diagnostic) that vscode gets from a language service
(TODO: Omit or find out whether it's actually subscribed to every one or if there is a generic subscription).
Then it builds a prompt that, at its core, says "There is an error with ***Error text here*** in this code ***Code here***. Please fix it."

This works well for many errors, because the error implies a fix, and the fix is simple enough for Copilot to figure out. 

## Targeted Fixes For Specific Errors
Fix With Copilot fails when the context or the error are misleading. Most errors in Typescript and ESLint, which I work with day-to-day, actually try to avoid prescribing a fix in cases where it's not obvious. That's the opposite of what Copilot needs -- if the error isn't helpful, then all it has to use is the context of surrounding code. For example, ESLint has a great rule that detects duplicate if/else predicates:

![Invoking AI](images/ai-no-dupe-if-1.png)

The error message suggests that the duplicate is extraneous. Which it is, from a strictly control-flow perspective. But if we stop and look at *intent*, in my experience a duplicated `if/else` or `switch case` is the result of copy/paste/modifying cases until you get enough--and then forgetting to change the last one. That's why this ESLint rule is so great, because it catches attentional mistakes. Still, the fix is to make the duplicate case *different* from the others, not to delete.

So I ask Copilot to do that, specifically for this error. I add "Fix the duplicate condition to be different from the first." to the prompt, which is enough for it to do the right thing:
	
![After AI](images/ai-no-dupe-if-2.png)

Sometimes the error is fine, at least not actively misleading, but it just doesn't have enough context:
	
![Invoking AI](images/ai-bad-call-1.png)

In this case there's *no way* (except blind trial and error) for you **or** Copilot to know how to fix this without looking at `connect`:

	connect source here
Once you do, the problem is obvious. The last two parameters are swapped.

-Add source (talk briefly about implementation)
-Add other calls (talk briefly about implementation)
-That's enough for anybody, even an AI that's not paying attention, to fix the problem:

![After AI](images/ai-bad-call-2.png)


## But really, how does Fix With Copilot work?

The first thing *Fix With Copilot* does is register as a fix provider for every kind of file. Every one. Then, for each document you have open, it requests errors and adds *Fix With Copilot* to the list of available fixes.
<!-- see inlineChatCommand.ts:registerInlineChatCommands
and inlineChatCodeActions.ts:QuickFixesProvider -->

<!-- TODO: cut these two paragraphs -->
Aside: technically, it starts inline chat with the starting user prompt `/fix ERROR TEXT HERE`, and the inline chat has the ability to recognise slash commands and map `/fix` to the Fix intent, which is the same between inline chat and panel chat. I'm not sure the additional detail is worth talking about.

When you click on *Fix With Copilot*, it invokes inline chat, which uses InlineFixPromptCrafter to build a prompt with, InlineFix4Prompt or InlineFix3Prompt. (3 is what you get normally but 4 is in testing.)
InlineFixPromptCrafter delegates the actual prompt crafting to PromptRenderer, which is two levels of indirection more than need to show up in this article.
<!-- InlineFixPromptCrafter, PromptRenderer and *more* actually render the components and interact with the inline chat, but I am writing this to elide the non-prompt interfacing for starters.  -->

When you click on *Fix With Copilot*, it renders a prompt using JSX components. Not with React though! It uses its own renderer that generates plain text. So you can write a React-like hierarchy of small components. And one of the properties of the PromptElement is `priority`, so the renderer can drop low-priority components  if the total prompt size gets too big. Overall, it's a clever approach to building text that works about as well for generating a prompt as it does for generating a web page.
<!-- see inlineChatFix4Prompt.tsx (old version for now) -->

    Show example snippet here, priming the next paragraphs

What components are there? The main ones are:

- Include code surrounding the error.
- Format the error nicely.
- Provide instructions to Copilot.

By default Copilot tries to include the entire file around the error. Of course, most of this time this won't fit in the prompt. So next it tries to summarise the code outside the current function by selectively deleting code. By default it deletes all function bodies outside the current one.
(By default it divides the file into blocks and builds a tree out of it. Then each block gets assigned a weight based on distance from the error, and nesting depth. Blocks below a cutoff are omitted, so the prompt will include code near the error and less and less detail further away. 

    TODO: Find out from Martin how commonly this method is the one used, probably only describe the simple or complex one. This would be a good place for a diagram to explain the complex method.

Formatting the error starts with the error text itself, unless my cookbook offers a replacement. But it also includes a couple of sub-components. The first includes code from related error spans. Language services can attach related information to an error, and Copilot will include the function that contains each related location. The other subcomponent is example usages, which is what I showed earlier for fixing Typescript errors. TODO: Make this longer, better structured and more readable.

    Code snippet showing the cookbook data structure.

TODO: Talk about the cookbook a little more. Objecty, applies to all languages, might not replace the error entirely.

Before all of this, the prompt includes some [one-shot training](http://useful-link-here.com) to explain how to answer a code question, and how to format it so that the code is easy to parse.

Then off to Copilot it goes. Once an answer comes back, we parse the reply into two parts: explanation and code. The explanation goes to the inline chat window. The code gets matched with code surrounding the original error. Once there's a good match, it's displayed as a diff. TODO: Make this more detailed and more readable.

5. The code gets matched with the original span of the error.
6. TODO: Find out how.
7. The code gets diffed and displayed in a new-biased diff format.
8. TODO: Find out how.


#### Tasks
- Learn about the missing pieces of my knowledge in the code.
- Finish writing up the code part.
- Find a good example for base Fix With Copilot.
- Take new pictures or gifs (with better cropping than the current set)
- Figure out how to mention my typescript extension work?

