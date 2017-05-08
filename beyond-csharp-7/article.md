# Beyond C#7

[IMG:C# logo?]

It's been amazing to follow the development of the C# language over the last decades. When Microsoft unveiled C# 1.0 in the early 2000s, industry veterans were [skeptical](http://www.artima.com/weblogs/viewpost.jsp?thread=6543), seeing it as little more than a boring alternative to Java. James Gosling essentially called it [a stupid imitation](https://www.cnet.com/news/why-microsofts-c-isnt/) of his brainchild. Despite all these odds, the team behind the C# language, lead by one Anders Hejlsberg, who doesn't even [have a beard](https://www.wired.com/2012/06/beard-gallery/), has continued to impress us with their ingenuity and pragmatic direction.

[IMG:beard infographic]

Now at [version 7](https://blogs.msdn.microsoft.com/dotnet/2017/03/09/new-features-in-c-7-0/), C# has become a cross platform, multi-paradigm language with [many](https://msdn.microsoft.com/en-us/library/bb308959.aspx) [innovating](https://msdn.microsoft.com/en-us/library/bb397951(v=vs.110).aspx) [features](https://msdn.microsoft.com/en-us/library/mt674882.aspx). It's [well-loved](https://stackoverflow.com/insights/survey/2017#most-loved-dreaded-and-wanted) by the developers commnnity, and has become a source of inspiration for other languages like [Javascript](https://tc39.github.io/ecmascript-asyncawait/) and even [Java](https://jcp.org/en/jsr/detail?id=335).

[IMG:Survey chart]

Now it's worth mentioning that there are no modern programming languages that's developed in a vacuum. Programming language design is a fertile field with a lot of cross pollination between the academia and the industry. C#, being a language for programmers in the industry, is no exception. It draws inspiration from a myriad of sources, from classical languages like C++, Java and Pascal, to dynamic ones like Ruby, Python and Javascript, to functional like [Haskell](https://www.haskell.org/), [OCaml](https://ocaml.org/) and most prominiently its close cousin [F#](http://fsharp.org/).

With each new C# version, there's a clear trend toward functional programming. In fact, C#'s continued relevance over the years owes a lot to the ideas that it has adopted from the functional world. Imperative and OOP features still have their places in the language, but they're increasingly being retrofited, or glossed over by funcional and declarative constructs.

In this article, we will go over some important "pillars" of functional programming to see how the C# team had been incorporating them into their language in the past and take a look at what they might be planning for the future.

## Generics and type inference

**Generics**, or [parametric polymorphism](https://en.wikipedia.org/wiki/Parametric_polymorphism), is basically the ability for programmers to handle similar kinds of data with one code base. And doing more with less is always a good thing(tm). Generics is the foundation upon which many advance features are implemented. It is the bread and butter of any self-respecting language with a type system.

Despite being relatively simple to use, generics is a huge feat of engineering to implement in any language. Java has generics a bit earlier than C#, but their implementation suffer from the problem of type erasure, which means the type parameter is known only at the compiler level, and it is lost (erased) when compiled to JVM byte code. C#'s generics was released with C# 2.0 in 2005, the result of the collaboration with Microsoft Research in Cambridge, UK, a place famous for functional researchs. The team decided to make the effort to implemented reified generics, which means type information is preserved all the way down to the CLR. It is said that, without that great undertaking to implement reified generics, later development of C# would be [severely limited](https://blogs.msdn.microsoft.com/dsyme/2011/03/15/netc-generics-history-some-photos-from-feb-1999/): there would be no LINQ in C#3, no Task Parallel Library in C#4, and no async/await in C#5. Amusingly, Don Syme, who worked on C# generics went on to lead the design of F#.

In static functional languages like F# and Haskell, there's an **[automatic generalization](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/language-reference/generics/automatic-generalization)** process where any function declared is generic by default. They also have the [Hindley Milner **type inference**](https://en.wikipedia.org/wiki/Hindley%E2%80%93Milner_type_system), which can infer the type of expressions base on usage so programmers don't have to write down the type in most situations. These 2 features combined make it really ergonomic to use generic code in those languages.

There are many in the C# [community](https://github.com/dotnet/roslyn/issues/12694) who [wish](https://github.com/dotnet/roslyn/issues/17) to have this duo of features in their language, too. That's because in C#, generic type parameters have to be specified explicitly, which can make the declaration and usage of generic code really verbose and unwieldly at times.

C#3 in 2009 (TODO: confirm year?) introduced a few features to help with this:

- The `var` keyword, where the compiler can infer the type of the left side of an assignment by looking at what's on the right side. Many developers mistook this for C# having a dynamic type system, which it totaly doesn't.
- Inference of generic type parameter on methods

These features helps a great deal with brevity, but it still doesn't go all the way up to the level of languages like F# yet. For example, declaring generic members and lambda expression is still a pain.

Due to its C++ and OOP lineage, C# will most likely never get automatic generalization and H&M type inference. Future features in this area are expected to be small enhancement, like [default value for generic type parameter](TODO: link), and better type inference in [specific cases](TODO: link). Nothing that will fundamentally change the way C# programmers think and code.

## First class functions

Have you ever wondered, what if we can declare functions as easily as declaring a string, or combine functions as naturally as adding 2 integers? Haskell users have been enjoying functions as a first class citizen like that for a long time. C# got the first part, declaring functions, with its lamba expressions at version 3, and it was insrumental to the success of LINQ. There was also method group conversion and static using in C#6, which helps passing functions around as delegates easier. Recently, Java 8 also start to introduce lambda expressions.

Haskell and F# also have dedicated operators to compose and chain functions together. They also have an automatic currying process (named after Haskell Curry), which can turn functions with any number of parameters into functions with one parameter. Because of this duo of features, we can often see functional code where the programmer declares a bunch of small, testable functions, weave them up into a chain of computation and only pour in data at the last step. This style is highly testable and doesn't require any mocking.

[TODO: code sample]

Extension methods in C#3 helps a bit in this regard so we can have code that resemble a chain of computation. Unfortunately, once again, due to its OOP lineage with so many methods in the BCL having overloads, C# will probably never see anything like native currying and composing of functions. We will have to look to user-land libraries like the excellent [lang-ext](TBD: link) for that need.

## Everything as an expression 

Most constructs in a programming language can be divided into 2 kinds: statements and expressions. Statements are all about side effects, while expressions are all about values. In the far end of the functional spectrum, there is the LISP family of languages (Scheme, Clojure) where everything is an expression. Obviously, C# will never go that far, but there are a couple of features that enabled developers to replace statements with expressions where it makes sense:

- In C#6, we can replace the body of a method with a lambda expression.
- As of C#7, The `out` keyword, widely regarded as an ugly hack for multiple returns,can now have an assignment expression right inside, reducing the need for a clunky variable declaration in front of it.
- There is now also a throw expression in C#7.

For future versions, there are many proposals in the same vein:

- We would be able to use lambda expressions in individual property getters/setters as well as indexers.
- As part of the pattern matching proposal, there will be a `match` expression, which will be able to replace `if` and `switch` statements in many cases.
- For cloning and modifying existing data structure, there will be a `with` expression.

All in all, we're looking at a very expressive C# in a near future.

## Algebraic data type:

One criticism toward classical OOP and their type system is that it is too rigid and inflexible, where the only way to re-use types is to extend them. So often do we see the textbook OOP hierachy of cats, dogs, birds inheriting from animals breaking spectacularly in a real world projects.

Some folk, probably when faced with existential questions about types, come up with questions similar to one that were asked about functions: just as we can add or multiply integers, can we do the same with types? It turns out that we can, and it's called algebraic data type.

Tuples, record, discriminated union

## Destructuring assignment

## Pattern matching

## Trait

Technically this is not an intrinsic feature of a functional programming language (they have their union types). I'm including it here because it has very interesting ramifications.

There's a proposal for [default interface methods](https://github.com/dotnet/csharplang/issues/52). Basically, it's to subvert the traditional notion of an interface. No longer will it has to be a pure contract. Now interfaces can provide concrete implementations too. In Scala this is known as [trait](http://docs.scala-lang.org/tutorials/tour/traits.html) and it can be used to implement some thing similar to the mixins as found in dynamic languages like Ruby and Python(TDB: verify?).

```csharp
TODO: code sample
```

Many members of the C# community are vocal against this feature, claiming that it would bring about the mess that is multiple inheritance in C++. However, the core team are adamant about including it. Apparently, this feature will scratch a lot of long running itches in the .NET framework and BCL (TODO: what itches?). Hence we will most likely see this feature landing in a coming version like C# 8.

Personally I'm optimistic about this feature, I see it as a fun way to mix and match types outside the confine of classical inheritance. Expect to see new frameworks to take advantage of this and liberate themselves from maddeningly long chains of inheritance like the woe that happened to [WPF](https://msdn.microsoft.com/en-us/library/system.windows.controls.ribbon.ribbonbutton(v=vs.110).aspx)

[IMG: RibbonButton inheritance tree]

## Conclusion

Far has it evolved from its root and we as developers can learn a lot from its journey. Whether you're using C# or not, or is planning to make the leap to other "real" functional languages like F#, Scala...