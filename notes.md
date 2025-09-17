#Talks

## Step Through Debugger (Rodrigo Mesquita (romes))

Demoing a Step Through Debugger, visualizing variables, expressions, stepping through into a thunk. 

Support for GHC 9.14 and later (so currently only alpha versions of the compiler)

(github)[https://well-typed.github.io/haskell-debugger/]

When designing a debugger for a lazy language, you can either pretend the language is strict, or trace it as it's actually lazily evaluated. The correct choice is "as it's actually evaluated": People will get used to following the lazy evaluation order 
 - Simon Marlow (Paraphrased)


## Hindsight: Type-safe, evolvable event sourcing (Gaël Deest)

What is event sourcing: 

* Every change to application state is captured in an event
* Application state can be recomputed as a fold over events

3 Different Backends

* In Memory
* FileSystem
* Postgres (the "real" one)

Payload separated into Data and Version information. 

Events identified by a (typelevel) string, versioning declared with type families. 

Implementation is tested, but not battle tested. 

- Advice, 
    * make business critical events synchronous
    * make very few things business critical. 
        * example: Ecommerce; you can always sell too many of an object, & then send an email to people if you wind out selling too many. 
    * expect a lot of duplication between the commands and the events

## Text Builder Linear (Andrew Lelechenko (Bodigrim))

For context, this is the text builder library we're using in the compiler service. 

In Haskell, the order of application of List Concatenation matters for performance (strings are concatenated in linear time).

(++) is right associative 

```haskell
type String = [Char]

concatRight :: String
concatRight = long ++ veryLong ++ extraLong

concatLeft :: String
concatLeft = (long ++ veryLong) ++ extraLong
```

You can consume from the start of a string without forcing the right hand side of `a ++ b`. 

So people used to write strings as "Difference Lists" 

```haskell 
type DList = String -> String

concatRight :: DList
concatRight = long ++ veryLong ++ extraLong

concatLeft :: DList
concatLeft = (long ++ veryLong) ++ extraLong
```

These are pretty much the same. 

"Modern" haskell doesn't (shouldn't) use Strings.

It uses `Text`.

Concatenation of `Text` is still linear (and slow, when you use it a lot).

You can do a "Java Style" text builder, but this requires mutable state. 

Haskell has ways to encapsulate this explicitly, but to do "hide" it implies hiding mutability. 

Linear Haskell, used to guarantee that the mutability is safe. 

Andrew recomends using the linear API (it has better performance)

(I don't think we should do this)



## PCRE 2 (Steven Shuck)

Regex not particularly common in Haskell

Most Haskell programmers prefer Parser Combinators
    * It's me, I' most haskell programmers

Steve thinks that parser combinators are _verbose_

Steve thinks part of the reason Regex is unpopular is due to the types in Regex Base. 

Presents a library with an `Alternative` api

Uses typelevel integers (I wrote the note "damn, that's a very nice error message")

template Haskell: reject invalid regex at compile time. 

Library: pcre2

Agree that this is nice, I'm not sure if I'm going to be using this, or `lens-regex-pcre`. 

## How I use Haskell for 3d Printing (Joe Warren)

You've all seen this at a previous techreation talks. 

## 6 years of FUNAR (Functional Architecture) : Teaching Software Architecture and Haskell to the Uninitiated (Mike Sperber)

Runs the Haskell Interlude podcast, and the FunArch conference.

Runs ISAQB certification for Software Architects. (Apparently quite well established in German BigCorps).

Teaches basics of Functional Programming, and Functional Modeling.

5 Days, Racket and Haskell (also have a version for Scala). 

Effect systems and MTL don't work well for teaching. 

Mike's settled on modeling with data types with explicit continuations.

"In functional programming, everything is data"

Describes this as "top down architecture", or "micro architecture" vs traditional "macro architecture". 

Suggests that you can push back on expensive decisions. 

Suggests that there's a dearth of literature on Functional Architecture. 

# Project Presentations

## MNIIP: cabal Matrix

Build your project in CI using a range of different configurations. 

Used by Library Authors.

Used to investigate what bounds you should claim to support. 

## Karl Filip Faxen: Specifying the Semantics of the Haskell Type Checker in lean

Implementing the Haskell 98' Spec

Mostly finished; constructors still not done, about 70-80% complete. 

If complete, would allow you to explore properties of the Haskell type checker. 

e.g. Can you prove that the Monomorphism Restriction removes the Principal Type Property, i.e the system without the Monomorphism Restriction would have this property

## Rodrigo Mesquita : HDB

Fixed several issues in HDB

* It has a version flag now

* Functions display with the function type, rather than as an unevaluated thunk

-- 

I released a demo of some Haskell Language server functionality back in May

![](https://www.youtube.com/watch?v=_6VHt0thoII)

I was able to chat with Rodrigo about this, and I'm not unblocked, but I think I know who to reach out to. 

## Théophile Choutri (Hecate): Flora.pm

Haskell Package Search (Alternative frontend to Hackage). 

* Fixed a content flash when loading in dark mode 
* Reduced amount of JS 
* Added a development Favicon, and development functionality
* Updated the search bar to be consistent with the rest of the site. 

## Someone (My notes don't say who)

Fixed a bug in GHC

There's a Language Extension:

`RequiredTypeArguments`

This was missing a check for when `DataKinds` (another extension) was not enabled. 

## Paul Brinkmeier: Waterfall-CAD

Found 2 Crashes. 

Redesigned a Bycycle Key from OpenSCAD. 

Designed a Lamp

Integrated With Jupyter Notebooks (Haskell Backend)

## Steven Shuck: MTL 

Steven recently took over stewardship of the MTL (Monad Transformer Library)

This has support for MicroHS (a non-ghc Haskell implementation), and this is tested in CI

Steve Cannot push directly to master

All Issues and PRs on the repo have been addressed. 

Some will be merged into 2.3.2. 

Roadmap for mtl-3.0

* Split MonadReader into MonadAsk + MonadLocal
* Split MonadWriter into MonadTell, MonadPass
* Split MonadError into MonadError, MonadThrow, and MonadCatch

This _should_ help with writing lawful instances. 

Relax the `Monad` constraint on some typeclasses to Applicative. 

Get rid of the implementation `Trans.Writer.Strict` 
    * There's no instance where it makes sense to use this
    * Always leaks space.
