---
css:
  - ./assets/css/variables.css
  - ./assets/css/styles.css
title: Highlights from MuniHac
subtitle:
introductory_notes: |
    I went to a conference last weekend, I'd like to talk through my notes
light: true
ratio43: true
embed-css: true
#display-notes: true
overlay: 
author:
  - name: Joe Warren
---

<br/><br/>

# Talks

:::notes
MuniHac is part "traditional conference, part Hack Event"

I'm going to run through the talks first, and then the Hack event
::: 

---

## Step Through Debugger 

### Rodrigo Mesquita (romes)

<br/>

* Demoing a Step Through Debugger, visualizing variables, expressions, stepping through into a thunk. 
* Support for GHC 9.14 and later (so currently only alpha versions of the compiler)
* [https://well-typed.github.io/haskell-debugger/](https://well-typed.github.io/haskell-debugger/)

<br/>

> When designing a debugger for a lazy language, you can either pretend the language is strict, or trace it as it's actually lazily evaluated. The correct choice is "as it's actually evaluated": People will get used to following the lazy evaluation order 

<div class="attribution">
Simon Marlow (Paraphrased)
</div>

---

<video autoplay muted loop src="assets/images/haskell-debugger.webm" class="bigimage"/>

---

## Hindsight: Type-safe, evolvable event sourcing 

### Gaël Deest

* What is event sourcing?

    * Every change to application state is captured in an event
    * Application state can be recomputed as a fold over events

:::notes
There was some discussion on the definition of Event sourcing

To the extent, that someone wound out running an ad-hoc "response" talk
:::

---

## Hindsight

### Type-safe, Evolvable Event Sourcing 

### Gaël Deest

* 3 Different Backends
    * In Memory
    * FileSystem
    * Postgres (the "real" one)
* Payload separated into Data and Version information. 
* Events identified by a (typelevel) string, versioning declared with type families. 
* Implementation is tested, but not battle tested. 

---

## Hindsight

### Gaël Deest

<br/>

### Advice

<br/>

* make business critical events synchronous
* make very few things business critical. 
    * example: E-Commerce
        * sell too many of an object, apologise later
* expect a lot of duplication between the commands and the events

---

## Text Builder Linear 
### Andrew Lelechenko (Bodigrim)

* For context, this is the text builder library we're using in the compiler service. 
* In Haskell, the order of application of List Concatenation matters for performance (strings are concatenated in linear time).
* `(++)` is right associative 

```haskell
type String = [Char]

concatRight :: String
concatRight = long ++ veryLong ++ extraLong

concatLeft :: String
concatLeft = (long ++ veryLong) ++ extraLong
```

* You can consume from the start of a string without forcing the right hand side of `a ++ b`. 

---

## Text Builder Linear 
### Andrew Lelechenko (Bodigrim)

<br/>

* So people used to write strings as "Difference Lists" 

```haskell 
type DList = String -> String

concatRight :: DList
concatRight = long ++ veryLong ++ extraLong

concatLeft :: DList
concatLeft = (long ++ veryLong) ++ extraLong
```

* These are pretty much the same. 

---

## Text Builder Linear 
### Andrew Lelechenko (Bodigrim)

<br/>

* "Modern" Haskell doesn't (shouldn't) use Strings.
* It uses `Text`.
* Concatenation of `Text` is still linear 
    * (and slow, when you use it a lot).
* You can do a "Java Style" text builder, but this requires mutable state. 
* Haskell has ways to encapsulate this explicitly
    * but to "hide" it implies hiding mutability
    * and promising you get this right 

---

## Text Builder Linear 
### Andrew Lelechenko (Bodigrim)

<br/>

* Linear Haskell, used to guarantee that the mutability is safe. 
* Andrew recommends using the linear API 
    * It has better performance
    * I don't think we should do this

---

## PCRE 2 

## Steven Shuck

* Regex not particularly common in Haskell
* Most Haskell programmers prefer Parser Combinators
    * It's me, I'm most Haskell programmers
* Steve thinks that parser combinators are _verbose_
* Steve thinks part of the reason Regex is unpopular is due to the types in Regex Base. 
* Presents a library with an `Alternative` api
* Uses typelevel integers (I wrote the note "damn, that's a very nice error message")
* template Haskell: reject invalid regex at compile time. 
* I agree that this is nice
    * I'm not sure if I'm going to be using this
    * `lens-regex-pcre` is also nice

---

## How I use Haskell for 3d Printing 

### Joe Warren

<br/>

* You've possibly seen this at a previous techreation talk. 

---

## 6 years of FUNAR 

### (Functional Architecture) 
<br/>

### Teaching Software Architecture
### and Haskell
### to the Uninitiated 

<br/>

### Mike Sperber

* Mike:
    * Runs the Haskell Interlude podcast
    * and the FunArch conference.
    * Runs ISAQB certification for Software Architects.
        * Apparently quite well established in German BigCorps.
    * Teaches basics of Functional Programming
        * and Functional Modeling.

---

## 6 years of FUNAR (Functional Architecture) 

### Mike Sperber

* 5 Days, Racket and Haskell (also have a version for Scala). 
* Effect systems and MTL don't work well for teaching. 
* Mike's settled on modeling with data types with explicit continuations.

<br/>
<br/>

> In functional programming, everything is data

<br/>
<br/>

* Describes this as "top down architecture"
    * or "micro architecture" 
    * vs traditional "macro architecture". 
* Suggests that you can push back on expensive decisions. 
* Suggests that there's a dearth of literature on Functional Architecture. 

---

<br/>

# Project Presentations

---

## MNIIP

### Cabal Matrix

<br/>
<br/>

* Build your project in CI using a range of different configurations. 
* Used by Library Authors.
* Used to investigate what bounds you should claim to support. 

---

## Karl Filip Faxen

### Specifying the Semantics 
### of the Haskell Type Checker 
### in lean

<br/>

* Implementing the Haskell 98' Spec
* Mostly finished: 
    * constructors still not done
    * about 70-80% complete. 
* If complete, would allow you to explore properties of the type checker. 

<br/>

> e.g. Can you prove that the Monomorphism Restriction removes the Principal Type Property
>
> i.e the system without the Monomorphism Restriction would have this property

---

## Rodrigo Mesquita (romes)
### HDB

<br/>

* Fixed several issues in HDB
    * It has a version flag now
    * Functions display with the function type
        * rather than as an unevaluated thunk

---

## Rodrigo Mesquita 

<br/>

* Back in May I wrote a prototype of a HLS Plugin
* [HLS Render-Plugin Demo on YouTube](https://www.youtube.com/watch?v=_6VHt0thoII)
* I was able to chat with Rodrigo about this
    * I'm not unblocked
    * but I think I know who to reach out to. 

---

## Théophile Choutri (Hécate)

### Flora.pm

<br/>
<br/>

* Haskell Package Search (Alternative frontend to Hackage). 
    * Fixed a content flash when loading in dark mode 
    * Reduced amount of JS 
    * Added a development Favicon, and development functionality
    * Updated the search bar to be consistent with the rest of the site. 

---

## Someone (My notes don't say who)

### GHC

<br/>
<br/>

* Fixed a bug in GHC
* There's a Language Extension:
    * `RequiredTypeArguments`
* This was missing a check for when `DataKinds` was not enabled

---

## Paul Brinkmeier

### Waterfall-CAD

* Found 2 Crashes. 
* Redesigned a Bycycle Key from OpenSCAD. 
* Designed a Lamp
* Integrated With Jupyter Notebooks (IHaskell Backend)

---

![](assets/images/lamp.jpg){.bigimage}

---

## Steven Shuck

### MTL 

* Steven recently took over stewardship of the MTL 
    * MTL: Monad Transformer Library
* This has support for MicroHS 
    * A non-ghc Haskell implementation
    * and this is now tested in CI
* Also Steve cannot push directly to `master`
* All Issues and PRs on the repo have been addressed. 
* Some will be merged into 2.3.2. 

---

## Steven Shuck

### MTL 

* Roadmap for mtl-3.0
    * Split `MonadReader` into `MonadAsk` + `MonadLocal`
    * Split `MonadWriter` into `MonadTell`, `MonadPass`
    * Split `MonadError` into `MonadError`, `MonadThrow`, and `MonadCatch`
* This _should_ help with writing lawful instances. 
* Relax the `Monad` constraint on some typeclasses to `Applicative` 
* Get rid of the implementation `Trans.Writer.Strict` 
    * There's no instance where it makes sense to use this
    * Always leaks space.

---
