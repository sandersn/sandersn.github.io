Here is my workflow to minify a repro for a Typescript bug. It assumes:

1. You know how to build Typescript locally and set VSCode to use that build.
2. You know the commit with the break and have a project that shows off the break.

## Start Up
 
1. Clone/install/open in vscode the broken project. Make sure the errors are the same as in the report. If not, cycle between step 2 and 1 until they are.
2. In a terminal, switch to `~/ts` and checkout the broken commit. Build it with `hereby`. Make sure that vscode is configured to use a local build of Typescript and then restart typescript server. Make sure the errors are the same as the report.
3. In another terminal, switch to the broken project. Learn how to compile it from the terminal. If you're lucky this is `tsc` or `tsc -b` (you'll need `tsc -b -f` if you build without changing the code in between). Even if the package.json claims to use webpack, esbuild, etc, it's worth trying `tsc` or `tsc -b`. Make sure the errors are the same as in the report.
4. Back in your typescript terminal, check out the commit before the broken one. Build it with `hereby tsc --no-typecheck`. Switch your project terminal and compile. Make sure the errors are gone.

Now you have a just-broken tsc in your editor and a just-before-broken tsc in your terminal. Use the editor to minify the repro and the terminal to make sure your changes don't mistakenly introduce the error in the unbroken tsc:

## Get Into

1. First, gather all the dependencies of the broken code into one place. I use an extension to highlight all the imports and external dependencies and then copy them into a single file one by one. You'll have to rename things that are already imported, but you'll want to do that eventually anyway.
2. Copy the now-independent code to a test project. I keep one around that has `"strict": true` and only a couple of empty .ts files. Dump the code into one of the files, and make sure the error is still there. Switch your terminal over to the test project and make sure there are no errors with the just-before-broken tsc you built. At this point you can switch to `tsc` instead of `tsc -b` if that's what the broken project used.
3. Start removing and simplifying code, keeping an eye on the part of the code that's supposed to have an error. After every change, run `tsc` from the terminal to make sure the pre-break tsc is still error-free.
4. Some ideas for simplifications: 
   - inline aliases
   - cut objects and unions down to one member each (or none!)
   - remove (type) parameter defaults and make everything required
   - remove (type) parameters.
5. Before you're done, rename and reformat the type names to be short and indicate what their function in the repro is. Sometimes the latter is too hard, but if so, still try to shorten the names. Also keep an eye on deduping names, eg having `T` in one place and `U` in another, or renaming `AllKeys/AllEventKeys/AllKeysMap/AllKeysMaps` to something distinct. It makes the repro easier to discuss with somebody else.

I actually interleave these 5 steps as I go, a little, although I generally follow this order.

----

[1] Track titles from Front Mission 3. Check out [the Front Mission soundtracks](https://www.youtube.com/results?search_query=front+mission+soundtracks); you might like them!