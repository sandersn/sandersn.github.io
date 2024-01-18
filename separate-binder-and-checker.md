In a discussion about compiler internals on the Typescript discord, I tried to give reasons why you might want to have a separate binder and checker.

In compilers for old languages, the binder and checker could be one program.

Thanks for this repo: https://github.com/sandersn/mini-typescript
It's minimalist, which I think is a good thing, because it really helps to understand the big picture.
I guess the binding is a separate phase from type-checking to comply with hoisting in JS? Are there other reasons?
Does the term Symbol refer only to the notion of table of symbols (variables, functions, custom types, etc.) in the sense of most compilers/interpreters implementation, or are there other reasons?
GitHub
GitHub - sandersn/mini-typescript: A miniature model of the Typescr...
A miniature model of the Typescript compiler, intended to teach the structure of the real Typescript compiler - GitHub - sandersn/mini-typescript: A miniature model of the Typescript compiler, inte...


1

Jake Bailey — Today at 12:49
Binding is how you can track that one identifier refers to the same variable as another identifier; it's not a step specific to JS in general, though the name may be different elsewhere

jean_michelet — Today at 12:58
Yes, I get it, but I is necessary to know that this function exists before execute it:
sayHello()

function sayHello() {
  console.log('hello')
}


In PHP, this code fail ^^ (edited)

@jean_michelet
Yes, I get it, but I is necessary to know that this function exists before execute it: sayHello()  function sayHello() {   console.log('hello') } In PHP, this code fail ^^ (edited)

Nathan Shively-Sanders — Today at 12:59
you're getting at the basic reason, yes
[12:59]
(also true for type and interface)
[13:00]
I think the main reason is that the binder is simple, fast tree walk, but the checker is a lazy and self-recursive -- you can start it with a tree walk, or start it with a call into any check function, but you'll quickly end up following type dependencies instead

Jake Bailey — Today at 13:00
Yeah. Many (most?) other languages are compiled, and so don't have this sort of order dependence, e.g. in C I can write:
int get_number() { return 1234; }
void main() {
    printf("the number is %d\n", get_number())
}
 (edited)
[13:00]
(so, this kind of analysis is not uncommon)

Nathan Shively-Sanders — Today at 13:01
The previous compiler I worked on was for a SQL dialect, and it indeed put most type checking the binder, but after a certain point, we had to add a check phase for complex checks that depended on whole-program information.
[13:01]
So I think it's something that happens naturally as languages become more complex.

1
[13:02]
@jean_michelet in PHP can you say
function sayHello() {
  console.log('hello', sayWorld())
}
function sayWorld() {
  return 'world'
}

1
[13:03]
You can in Python, even though technically it's evaluated top-to-bottom -- it's just that function bodies are not evaluated immediately.

jean_michelet — Today at 13:03
Yes you can

Nathan Shively-Sanders — Today at 13:04
If you want to check that hello/world program, you'll need a separate bind and check phase, because checking is sort of partial interpretation.

jean_michelet
Yes you can

jean_michelet — Today at 13:05
Hum, sorry, I am not sure
[13:06]
Yes, I confirm

Nathan Shively-Sanders — Today at 13:07
In that case you will want a separate binder and checker.
[13:08]
The SQL dialect I worked on technically had separate functions, but they were compiled into one giant program via inlining, so it did actually make sense to disallow forward references, and to have the binder and checker combined.

1
[13:08]
(It was https://devblogs.microsoft.com/visualstudio/introducing-u-sql-a-language-that-makes-big-data-processing-easy/ -- I can't believe this page is still up)
[13:13]
Well, hack would have to check the program up-front I believe
[13:13]
(though I haven't used PHP or Hack before)

jean_michelet — Today at 13:14
I know that the language has come a long way, even the original opensource engine, thanks in particular to Nikita Popov, an LLVM contributor. (edited)
[13:16]
But I'm migrating to Node because I'm fed up with maintaining knowledge of two ecosystems for the same web problem.
Time that I could invest in an other language like C++ Rust.
[13:16]
Thanks for your time and feedbacks

jean_michelet
Yes, I get it, but I is necessary to know that this function exists before execute it: sayHello()  function sayHello() {   console.log('hello') } In PHP, this code fail ^^ (edited)

jean_michelet — Today at 13:21
Ooops, sorry.

I think it's late and I am a bit tired, this code is completely valid in PHP...

@Jake Bailey
Yeah. Many (most?) other languages are compiled, and so don't have this sort of order dependence, e.g. in C I can write: int get_number() { return 1234; } void main() {     printf("the number is %d\n", get_number()) } (edited)

jean_michelet — Today at 13:39
Yes, even PHP is compiled in bytecodes and has JIT.
Sorry, I lost my way tonight ^

Message #compiler-internals-and-api