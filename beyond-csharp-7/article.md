# Beyond C#7

[IMG:C# logo?]

It's been amazing to follow the development of the C# language over the last decades. When Microsoft unveiled C# 1.0 in the early 2000s, industry veterans were [skeptical](http://www.artima.com/weblogs/viewpost.jsp?thread=6543), seeing it as little more than a boring alternative to Java. James Gosling essentially called it [a stupid imitation](https://www.cnet.com/news/why-microsofts-c-isnt/) of his brainchild. Despite all these odds, the team behind the C# language, lead by one Anders Hejlsberg, who doesn't even [have a beard](https://www.wired.com/2012/06/beard-gallery/), has continued to impress us with their ingenuity and pragmatic direction.

[IMG:beard infographic]

Now at [version 7](https://blogs.msdn.microsoft.com/dotnet/2017/03/09/new-features-in-c-7-0/), C# has become a cross platform, multi-paradigm language with [many](https://msdn.microsoft.com/en-us/library/bb308959.aspx) [innovating](https://msdn.microsoft.com/en-us/library/bb397951(v=vs.110).aspx) [features](https://msdn.microsoft.com/en-us/library/mt674882.aspx). It's [well-loved](https://stackoverflow.com/insights/survey/2017#most-loved-dreaded-and-wanted) by the developers commnnity, and has become a source of inspiration for other languages like [Javascript](https://tc39.github.io/ecmascript-asyncawait/) and even [Java](https://jcp.org/en/jsr/detail?id=335).

[IMG:Survey chart]

Now it's worth mentioning that there are no modern programming languages that's developed in a vacuum. Programming language design is a fertile field with a lot of cross pollination between the academia and the industry. C#, being a language for programmers in the industry, is no exception. It draws inspiration from a myriad of sources, from classical languages like C++, Java and Pascal, to dynamic ones like Ruby, Python and Javascript, to functional like [Haskell](https://www.haskell.org/), [OCaml](https://ocaml.org/) and most prominiently its close cousin [F#](http://fsharp.org/).

With each new C# version, there's a clear trend toward functional programming. In fact, C#'s continued relevance over the years owes a lot to the ideas that it has adopted from the functional world. Imperative and OOP features still have their places in the language, but they're increasingly being retrofited, or glossed over by more funcional and declarative ones.

In this article, we will go over some important "pillars" of functional programming to see how the C# team had been incorporating them into their language in the past and take a look at what they might be planning for the future.

## Generics

Generics, or parametric polymorphism, is basically the ability for programmers to use one code base with multiple similar data types. A language can support generics by allowing types and functions to have parameters. 

There can be further development in this area, but they are most likely to be cosmetics

In F# and Haskell, there's an Automatic Generalization process where  https://docs.microsoft.com/en-us/dotnet/articles/fsharp/language-reference/generics/automatic-generalization

Generics, despite being relatively simple to use, is a huge feat of engineering to implement in any runtime.

In a somewhat amusing twist of fate, the people who has worked on a generics runtime often later went on to lead the development of a hybrid functional language. Don Syme, who worked on C# generics, went on to lead the development of F#. Martin Odersky, from the Java generics team, also designed Scala.

https://blogs.msdn.microsoft.com/dsyme/2011/03/15/netc-generics-history-some-photos-from-feb-1999/
## Type inference

One criticism that's often directed toward statically typed programming languages like C# and Java is their need to 

F# and Haskell programmers enjoy the benefit of a type system despite almost never having to specify.

https://en.wikipedia.org/wiki/Hindley%E2%80%93Milner_type_system

    var keyword, initially mistaken for dynamic
    Hilney Milner 
    Lambda is still a pain
    Expect minor, localized improment in this area, but not something drastic and game changing.

## First class functions

with automatic currying and function composition
    Extension method

## Everything is an expression 
In the LISP family of language (Scheme, Clojure)
    in stead of statements

## Immutable data structure

## Algebraic data type:
    Tuples, record, discriminated union

## Destructuring assignment

## Pattern matching

## Meta programming
    System.Linq.Expressions
    CallerInfo directives

## Trait

Technically this is not an intrinsic feature of a functional programming language (they have their union types). I'm including it here because it has interesting ramifications.

There's a proposal for [default interface methods](https://github.com/dotnet/csharplang/issues/52). Basically, it's to subvert the traditional notion of an interface. No longer will it has to be a pure contract. Now interfaces can provide concrete implementations too. In Scala this is known as [trait](http://docs.scala-lang.org/tutorials/tour/traits.html) and users seem to find it really useful.

```csharp
TODO: code sample
```

Many members of the C# community are vocally against this feature, claiming that it would bring about the mess that is multiple inheritance in C++. However, the core team have weigh the benefits and are adamant about this so this feature will most likely land in a coming version like C# 8.

Personally I'm optimistic about this feature, I see it as a fun way to mix and match types outside the confine of classical inheritance. Expect to see new frameworks to take advantage of this and liberate themselves from having maddeningly long chain of inheritance like the woe that happened to [WPF](https://msdn.microsoft.com/en-us/library/system.windows.controls.ribbon.ribbonbutton(v=vs.110).aspx)

[IMG: RibbonButton inheritance tree]

## Conclusion

Far has it evolved from its root and we as developers can learn a lot from its journey. 

Whether you're using C# or not, or is planning to make the leap to other "real" functional languages like F#, Scala, there are some take home lessons:

There's value in being a polyglot. You 