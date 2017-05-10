# Beyond C#7

[TODO: IMG:C# logo?]

It's been amazing to follow the development of the C# language over the last decades. When Microsoft unveiled C# 1.0 in the early 2000s, industry veterans were [skeptical](http://www.artima.com/weblogs/viewpost.jsp?thread=6543), seeing it as little more than a boring alternative to Java. James Gosling essentially called it [a stupid imitation](https://www.cnet.com/news/why-microsofts-c-isnt/) of his brainchild. Despite all these odds, the team behind the C# language, lead by one Anders Hejlsberg, who doesn't even [have a beard](https://www.wired.com/2012/06/beard-gallery/), has continued to impress us with their ingenuity and pragmatic direction.

[TODO: IMG:beard infographic]

Now at [version 7](https://blogs.msdn.microsoft.com/dotnet/2017/03/09/new-features-in-c-7-0/), C# has become a cross platform, multi-paradigm language with [many](https://msdn.microsoft.com/en-us/library/bb308959.aspx) [innovating](https://msdn.microsoft.com/en-us/library/bb397951(v=vs.110).aspx) [features](https://msdn.microsoft.com/en-us/library/mt674882.aspx). It's [well-loved](https://stackoverflow.com/insights/survey/2017#most-loved-dreaded-and-wanted) by the developers commnnity, and has become a source of inspiration for other languages like [Javascript](https://tc39.github.io/ecmascript-asyncawait/) and even [Java](https://jcp.org/en/jsr/detail?id=335).

[TODO: IMG:Survey chart]

Now it's worth mentioning that there are no modern programming languages that's developed in a vacuum. Programming language design is a fertile field with a lot of cross pollination between the academia and the industry. C#, being a language for programmers in the industry, is no exception. It draws inspiration from a myriad of sources, from classical languages like C++, Java and Pascal, to dynamic ones like Ruby, Python and Javascript, to functional like [Haskell](https://www.haskell.org/), [OCaml](https://ocaml.org/) and most prominiently its close cousin [F#](http://fsharp.org/).

With each new C# version, there's a clear trend toward functional programming. In fact, C#'s continued relevance over the years owes a lot to the ideas that it has adopted from the functional world. Imperative and OOP features still have their places in the language, but they're increasingly being retrofited, or glossed over by funcional and declarative constructs.

In this article, we will go over some important "pillars" of functional programming to see how the C# team had been incorporating them into their language in the past and take a look at what they might be planning for the future.

## Generics and type inference

Generics, or [parametric polymorphism](https://en.wikipedia.org/wiki/Parametric_polymorphism), is basically the ability for programmers to handle similar kinds of data with one code base. And doing more with less is always a good thing. Generics is the foundation upon which many advance features are implemented. It is the bread and butter of any self-respecting language with a type system.

Despite being relatively simple to use, generics is a huge feat of engineering to implement in any language. Java has generics a bit earlier than C#, but their implementation suffer from the problem of type erasure, which means the type parameter is known only at the compiler level, and it is lost (erased) when compiled to JVM byte code. C#'s generics was released with C# 2.0 in 2005, the result of the collaboration with Microsoft Research in Cambridge, UK, a place famous for functional researchs. The team decided to make the effort to implemented reified generics, which means type information is preserved all the way down to the CLR. It is said that, without that great undertaking to implement reified generics, later development of C# would be [severely limited](https://blogs.msdn.microsoft.com/dsyme/2011/03/15/netc-generics-history-some-photos-from-feb-1999/): there would be no LINQ in C#3, no Task Parallel Library in C#4, and no async/await in C#5. Amusingly, Don Syme, who worked on C# generics went on to lead the design of F#.

C#3 in 2007 introduced the `var` keyword and better generic type inference in method invocation, where the compiler can infer the type base on what's being used. While not specific to generics, this feature alleviate a lot of verbosity when declaring generic variables. Amusingly, many developers mistook this for C# having a dynamic type system, which it totaly doesn't.
C#4 in 2010 added [covariance and contravariance](https://docs.microsoft.com/en-us/dotnet/articles/csharp/programming-guide/concepts/covariance-contravariance/index), which allowed greater reuse of generic types.

These features helps a great deal with brevity, but it still doesn't go all the way up to the level of languages like F# yet. In F# and Haskell, there's an [automatic generalization](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/language-reference/generics/automatic-generalization) process where any function declared is generic by default. They also have the [Hindley Milner type inference](https://en.wikipedia.org/wiki/Hindley%E2%80%93Milner_type_system) system, which can infer the type of expressions base on usage so programmers don't have to write down the type in most situations. These 2 features combined make it really ergonomic to use generic code in those languages.

There are many in the C# [community](https://github.com/dotnet/roslyn/issues/12694) who [wish](https://github.com/dotnet/roslyn/issues/17) to have this duo of features in their language, too. That's because in C#, generic type parameters have to be specified explicitly, which can make the declaration and usage of generic code really verbose and unwieldly at times. For example, declaring generic members and standalone lambda expression is still a pain.

Due to its C++ and OOP lineage, C# will most likely never get automatic generalization and H&M type inference. Future features in this area are expected to be small enhancement, like [default value for generic type parameter](https://github.com/dotnet/csharplang/blob/master/proposals/target-typed-default.md), and better type inference in [specific cases](https://github.com/dotnet/csharplang/blob/master/proposals/nullable-enhanced-common-type.md). Nothing that will fundamentally change the way C# programmers think and code.

## Functions as first class citizens

Have you ever wondered, what if we can declare functions as easily as declaring a string, or combine functions as naturally as adding 2 integers? Haskell users have been enjoying functions as a first class citizen like that for a long time. C# got the first part, declaring functions, with its lamba expressions at version 3, and it was insrumental to the success of LINQ. There was also method group conversion and static using in C#6, which helps passing functions around as delegates easier.

Haskell and F# also have dedicated operators to compose and chain functions together. They also have an automatic currying process (named after [Haskell Curry](https://wiki.haskell.org/Currying)), which can turn functions with any number of parameters into functions with one parameter for composing. Because of this duo of features, we can often see a style in functional code where the programmer prepares a bunch of small, testable functions, then weave them into a chain of computation, and finally pipe in the data at the last step. This coding style is highly testable and tremendously reduces the need for elaborate mocking. This is in contrast to the OOP approach, where, in the [word](https://www.amazon.com/gp/product/1430219483) of Joe Armstrong of Erlang fame: "You wanted a banana but what you got was a gorilla holding the banana and the entire jungle."

```
[TODO: code sample]
```

Extension methods in C#3 helps a bit in this regard so we can have code that resemble a chain of computation. Unfortunately, once again, due to its OOP lineage with so many methods in the BCL having overloads, C# will probably never see anything like native currying and built-in functions composition. We will have to look to user-land libraries like the excellent [lang-ext](https://github.com/louthy/language-ext) for that need.

## Everything is an expression

Most constructs in a programming language can be divided into 2 kinds: statements and expressions. Statements are all about side effects, while expressions are all about values. Expressions are typically shorter and more composable than statements, which depends on specific ordering to make sense. In the far end of the functional spectrum, there is the LISP family of languages (Scheme, Clojure) where everything is an expression. Any program, however complex, can be composed from a tree of small,reasonable and testable expressions. A high degree of composability is also a good thing. 

Obviously, C# will never go that far, and it will never becomes a full-fledge functional language. However, historically, the designers have been paying attention to making the language more expressive. There are a lot of features that enabled developers to replace statements with expressions where it makes sense:

- C#3 introduced [lambda expression](https://docs.microsoft.com/en-us/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions), which allowed functions to be passed around freely, eliminating the need for boilerplates in the form of intemediary objects. This revolutionized the way APIs in libraries and frameworks were written for C#.
- C#3 also added the object and collection [initialization expressions](https://docs.microsoft.com/en-us/dotnet/articles/csharp/programming-guide/classes-and-structs/object-and-collection-initializers), making it very succint to declare complex data structure.
- In C#6, we can start replacing [method bodies](https://github.com/dotnet/roslyn/wiki/New-Language-Features-in-C%23-6) with lambda expressions, making class declarations really terse.
- C#7 introduced the [is expression](https://github.com/dotnet/roslyn/blob/master/docs/features/patterns.md). This is an important foundation for powerful pattern matching down the road.
- The `out` keyword, widely regarded as an ugly hack for multiple returns, can now have an [assignment expression right inside](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.0/out-var.md), reducing the need for a clunky variable declaration in front of it.
- For better or worse, [throw](https://github.com/dotnet/csharplang/blob/master/proposals/csharp-7.0/throw-expression.md) is now also an expression in C#7.

For future versions, there are many proposals in the same vein:

- As part of the pattern matching proposal, there will be a [match](https://github.com/dotnet/csharplang/blob/master/proposals/patterns.md) expression, which will be able to replace `if` and `switch` statements in many cases.
- For easy cloning and modifying of existing data structure, there will be a `with` expression as part of the [record](https://github.com/dotnet/csharplang/blob/master/proposals/records.md)  proposal.

All in all, we're looking at a very "[expressive](https://github.com/dotnet/csharplang/issues/63)" C# in a near future.

## Powerful type system

In functional languages, there is a trio of features that blend together in harmony to make working with data a really pleasant experience:

- **Algebraic type system** allows users to succintly compose a new type from existing ones, then creating instances of that type just as easily.
- **Pattern matching** for testing if a piece of data matches a certain shape and size of a type
- **Destructuring** makes extracting pieces of data from a container just as natural as putting them in.

Looking at the activities on [github](https://github.com/dotnet/csharplang), we can see that the designers of C# are definitely working toward this goal.

### Algebraic data type:

One criticism toward classical OOP and their type system is that it is too rigid and inflexible, where the only natural way to re-use types is to extend them. So often do we see the textbook OOP hierachy of cats, dogs, birds and human inheriting from animals breaking spectacularly in a real world projects.

Some folk, probably when faced with existential crisis about types, came up with questions similar to one that were asked about functions: just as we can add or multiply integers, can we do the same with types? It turns out that we can, and the results are product type and sum type.

#### Product type

Product type is the result of multiplying types together. Classes in traditional OOP languages are product types. However, they were originally designed to house many things aside from data like methods, events, indexers... so they can be unecessarily verbose to be use as data type. The designers of C# are adding tuples and records to the language to help in this regard.

A **tuple** is basically an ordered sequence of values of different types. The concept of Tuples were introduced to C# back in version 4, but it was clunky and almost nobody uses it. [Tuples in C#7](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md) gets a lot of compiler love, making it really easy to put data in a tuple, passes it around and extract data from it. Expect lots of code where multiple returning values or intermediary classes can be eliminated and replaced with tuples in the future.

A **Record** is a simple container of named values. It'l like classes, but really succint and typically don't have any methods. There's a really promising [proposal for records]((https://github.com/dotnet/csharplang/blob/master/proposals/records.md)) in future C#.

```
[TODO: code sample]
```

#### Sum type

Sum type is the result of adding types together. It's also known as tagged union or [discriminated union](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/language-reference/discriminated-unions) in F#, or [case class](http://docs.scala-lang.org/tutorials/tour/case-classes.html) in Scala. It is useful for expressing a lot of tricky data situation, for example:

- A function returning some data or none
- An operation that either evaluate to a value successfully or encountered an error
- An API that returns a 301 redirect or JSON content

```
TODO: code sample
```

Traditional OOP languages also have a form of sum type in the form of `enum` but they are often underused and underpowered. C#'s [current enum implementation](https://docs.microsoft.com/en-us/dotnet/articles/csharp/language-reference/keywords/enum) is severely crippled, as it can only hold a few numeric data types. In contrast, new languages like [Swift](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html) and [Rust](https://doc.rust-lang.org/book/enums.html) got their enum right and they can be used in place of tagged union.

There's a [proposal](https://github.com/dotnet/csharplang/issues/113) for proper discriminated union in C# and there's clear intent from the team to start work on it some time in the future.

#### No Null

Another aspect where functional type system really shine is the banishment of the [billion-dollar mistake](https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare): null reference. Null is dangerous in Java and C# because the type system allows it to be a member of all reference types. Null claims to support the contract of a type but that claim is a lie. Null is like a timed bomb that blows up when we try to use it, often at the most embarassing moment. In contrast, functional languages use the sum type [Maybe](http://package.elm-lang.org/packages/elm-lang/core/latest/Maybe) or [Option](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/language-reference/options) to handle missing values. An Option represent a value that can be either **something** or **none**. Unlike null, none isn't part of any other type, it doesn't claim to support any contract and will [never blows up](https://blogs.msdn.microsoft.com/dsyme/2013/03/25/quote-of-the-week-what-can-c-do-that-f-cannot/), as the compiler forces you to check for it whenever it encounters an Option.

C# can never go back on null without breaking backward compatibility. However, that doesn't mean the designers can't find ways to make handling of nulls easier. In C#6, there was the [conditional operator](https://docs.microsoft.com/en-us/dotnet/articles/csharp/language-reference/operators/null-conditional-operators) `?.` for safely accessing nullable members. We're starting to see developers abusing this feature to great success in the wild. Future version of the C# compiler will likely have the ability to [track usage of nulls](
https://github.com/dotnet/csharplang/blob/master/proposals/nullable-reference-types.md) and warn users where appropriate.

### Pattern matching

F# has it and C#'s gonna have this [nice thing](https://github.com/dotnet/csharplang/blob/master/proposals/patterns.md) soon. When this feature lands, we would expect the gradual fading away of `if` and `switch` statements, as the coming `match` expression is a much more powerful and succint construct:

```
TODO: code sample
```

### Destructuring assignment

While only marginally useful when used on its own, this feature becomes much more powerful when paired with pattern matching. After we have tested that the incoming data match a certain shape and size, we can immediately break it a part and do useful things with its members. [Deconstruction protocol](https://github.com/dotnet/roslyn/blob/master/docs/features/deconstruction.md) landed in C#7, and any type can be deconstructed.

```
TODO: code sample
```

## Trait

Technically this is not an intrinsic feature of a functional programming language. I'm including it here because it has very interesting ramifications.

There's a proposal for [default interface methods](https://github.com/dotnet/csharplang/issues/52). Basically, it's to subvert the traditional notion of an interface. No longer will it has to be a pure contract. Now interfaces can provide concrete implementations too. In Scala this is known as [trait](http://docs.scala-lang.org/tutorials/tour/traits.html) and it can be used to implement some thing similar to the mixins as found in dynamic languages like Ruby and Python.

```csharp
TODO: code sample
```

Many members of the C# community are vocal against this feature, claiming that it would bring about the mess that is multiple inheritance in C++. However, the core team are adamant about including it. Apparently, this feature will scratch a lot of long running itches in the .NET framework and BCL. Hence we will most likely see this feature landing in a coming version like C# 8.

Personally I'm optimistic about this feature, I see it as a fun way to mix and match types outside the confine of classical inheritance. Expect to see new frameworks to take advantage of this and liberate themselves from maddeningly long chains of inheritance like the woe that happened to [WPF](https://msdn.microsoft.com/en-us/library/system.windows.controls.ribbon.ribbonbutton(v=vs.110).aspx)

[IMG: RibbonButton inheritance tree]

## Conclusion

For programmers looking to enter the field, C# is an good starting point in term of its practicality and paradigms it has adopted. Far has C# evolved from its root and we can learn a lot from its journey. It's beneficial for your long-term development as a programmer, whether you're using C# or not, or is planning to make the leap to other "real" functional languages like F#, Scala or Elm....