This transcript verifies that the pretty-printer produces code that can be successfully parsed, for a variety of examples. Terms or types that fail to round-trip can be added here as regression tests. Add tests at the bottom of this

## How to use this transcript: checking round-trip for inline definitions

```unison
x = 1 + 1
```

```ucm
.> add

  ⍟ I've added these definitions:
  
    x : Nat

.> edit x

  ☝️
  
  I added these definitions to the top of
  /Users/runar/work/unison/scratch.u
  
    x : Nat
    x =
      use Nat +
      1 + 1
  
  You can edit them there, then do `update` to replace the
  definitions currently in this namespace.

.> reflog

  Here is a log of the root namespace hashes, starting with the
  most recent, along with the command that got us there. Try:
  
    `fork 2 .old`             
    `fork #epnudil1fk .old`   to make an old namespace
                              accessible again,
                              
    `reset-root #epnudil1fk`  to reset the root namespace and
                              its history to that of the
                              specified namespace.
  
  1. #ugtr8mvop3 : add
  2. #epnudil1fk : builtins.mergeio
  3. #sjg2v58vn2 : (initial reflogged namespace)

.> reset-root 2

  Done.

```
Resetting the namespace after each example ensures they don't interact at all, which is probably what you want.

The `load` command which does parsing and typechecking of the `edit`'d definitions needs to be in a separate stanza from the `edit` command.

```ucm
.> load scratch.u

  I found and typechecked these definitions in scratch.u. If you
  do an `add` or `update`, here's how your codebase would
  change:
  
    ⍟ These new definitions are ok to `add`:
    
      x : Nat

```
## How to use this transcript: checking round-trip for definitions from a file

Examples can also be loaded from `.u` files:

```ucm
.> load unison-src/transcripts-round-trip/ex2.u

  I found and typechecked these definitions in
  unison-src/transcripts-round-trip/ex2.u. If you do an `add` or
  `update`, here's how your codebase would change:
  
    ⍟ These new definitions are ok to `add`:
    
      b : Nat

.> add

  ⍟ I've added these definitions:
  
    b : Nat

```
When loading definitions from a file, an empty stanza like this will ensure that this empty file is where the definitions being `edit`'d will get dumped.

```unison
-- empty scratch file, `edit` will target this
```

Without the above stanza, the `edit` will send the definition to the most recently loaded file, which would be `ex2.u`, making the transcript not idempotent.

```ucm
.> edit b

  ☝️
  
  I added these definitions to the top of
  /Users/runar/work/unison/scratch.u
  
    b : Nat
    b = 92384
  
  You can edit them there, then do `update` to replace the
  definitions currently in this namespace.

.> reflog

  Here is a log of the root namespace hashes, starting with the
  most recent, along with the command that got us there. Try:
  
    `fork 2 .old`             
    `fork #epnudil1fk .old`   to make an old namespace
                              accessible again,
                              
    `reset-root #epnudil1fk`  to reset the root namespace and
                              its history to that of the
                              specified namespace.
  
  1. #97dstg1ao2 : add
  2. #epnudil1fk : reset-root #epnudil1fk
  3. #ugtr8mvop3 : add
  4. #epnudil1fk : builtins.mergeio
  5. #sjg2v58vn2 : (initial reflogged namespace)

.> reset-root 2

  Done.

```
```ucm
.> load scratch.u

  I found and typechecked these definitions in scratch.u. If you
  do an `add` or `update`, here's how your codebase would
  change:
  
    ⍟ These new definitions are ok to `add`:
    
      b : Nat

```
No reason you can't load a bunch of definitions from a single `.u` file in one go, the only thing that's annoying is you'll have to `find` and then `edit 1-11` in the transcript to load all the definitions into the file.

## Destructuring binds

Regression test for https://github.com/unisonweb/unison/issues/2337

```unison
unique type Blah = Blah Boolean Boolean

f : Blah -> Boolean
f x = let
  (Blah.Blah a b) = x
  a
```

```ucm
.> add

  ⍟ I've added these definitions:
  
    unique type Blah
    f : Blah -> Boolean

.> edit Blah f

  ☝️
  
  I added these definitions to the top of
  /Users/runar/work/unison/scratch.u
  
    unique type Blah
      = Blah Boolean Boolean
    
    f : Blah -> Boolean
    f = cases Blah a b -> a
  
  You can edit them there, then do `update` to replace the
  definitions currently in this namespace.

.> reflog

  Here is a log of the root namespace hashes, starting with the
  most recent, along with the command that got us there. Try:
  
    `fork 2 .old`             
    `fork #epnudil1fk .old`   to make an old namespace
                              accessible again,
                              
    `reset-root #epnudil1fk`  to reset the root namespace and
                              its history to that of the
                              specified namespace.
  
  1. #hogb1vion0 : add
  2. #epnudil1fk : reset-root #epnudil1fk
  3. #97dstg1ao2 : add
  4. #epnudil1fk : reset-root #epnudil1fk
  5. #ugtr8mvop3 : add
  6. #epnudil1fk : builtins.mergeio
  7. #sjg2v58vn2 : (initial reflogged namespace)

.> reset-root 2

  Done.

```
```ucm
.> load scratch.u

  I found and typechecked these definitions in scratch.u. If you
  do an `add` or `update`, here's how your codebase would
  change:
  
    ⍟ These new definitions are ok to `add`:
    
      unique type Blah
      f : Blah -> Boolean

```
## Parens around infix patterns

Regression test for https://github.com/unisonweb/unison/issues/2224

```unison
f : [a] -> a
f xs = match xs with
  x +: (x' +: rest) -> x

g : [a] -> a
g xs = match xs with
  (rest :+ x') :+ x -> x

h : [[a]] -> a
h xs = match xs with
  (rest :+ (rest' :+ x)) -> x
```

```ucm
.> add

  ⍟ I've added these definitions:
  
    f : [a] -> a
    g : [a] -> a
    h : [[a]] -> a

.> edit f g

  ☝️
  
  I added these definitions to the top of
  /Users/runar/work/unison/scratch.u
  
    f : [a] -> a
    f = cases x +: (x' +: rest) -> x
    
    g : [a] -> a
    g = cases rest :+ x' :+ x -> x
  
  You can edit them there, then do `update` to replace the
  definitions currently in this namespace.

.> reflog

  Here is a log of the root namespace hashes, starting with the
  most recent, along with the command that got us there. Try:
  
    `fork 2 .old`             
    `fork #epnudil1fk .old`   to make an old namespace
                              accessible again,
                              
    `reset-root #epnudil1fk`  to reset the root namespace and
                              its history to that of the
                              specified namespace.
  
  1. #7rhiegjl3c : add
  2. #epnudil1fk : reset-root #epnudil1fk
  3. #hogb1vion0 : add
  4. #epnudil1fk : reset-root #epnudil1fk
  5. #97dstg1ao2 : add
  6. #epnudil1fk : reset-root #epnudil1fk
  7. #ugtr8mvop3 : add
  8. #epnudil1fk : builtins.mergeio
  9. #sjg2v58vn2 : (initial reflogged namespace)

.> reset-root 2

  Done.

```
```ucm
.> load scratch.u

  I found and typechecked these definitions in scratch.u. If you
  do an `add` or `update`, here's how your codebase would
  change:
  
    ⍟ These new definitions are ok to `add`:
    
      f : [a] -> a
      g : [a] -> a

```
## Type application inserts necessary parens

Regression test for https://github.com/unisonweb/unison/issues/2392

```unison
unique ability Zonk where zonk : Nat
unique type Foo x y =

foo : Nat -> Foo ('{Zonk} a) ('{Zonk} b) -> Nat
foo n _ = n
```

```ucm
.> add

  ⍟ I've added these definitions:
  
    unique type Foo x y
    unique ability Zonk
    foo : Nat -> Foo ('{Zonk} a) ('{Zonk} b) -> Nat

.> edit foo Zonk Foo

  ☝️
  
  I added these definitions to the top of
  /Users/runar/work/unison/scratch.u
  
    unique type Foo x y
      = 
    
    unique ability Zonk where zonk : {Zonk} Nat
    
    foo : Nat -> Foo ('{Zonk} a) ('{Zonk} b) -> Nat
    foo n _ = n
  
  You can edit them there, then do `update` to replace the
  definitions currently in this namespace.

.> reflog

  Here is a log of the root namespace hashes, starting with the
  most recent, along with the command that got us there. Try:
  
    `fork 2 .old`             
    `fork #epnudil1fk .old`   to make an old namespace
                              accessible again,
                              
    `reset-root #epnudil1fk`  to reset the root namespace and
                              its history to that of the
                              specified namespace.
  
  1.  #5bpdpn1048 : add
  2.  #epnudil1fk : reset-root #epnudil1fk
  3.  #7rhiegjl3c : add
  4.  #epnudil1fk : reset-root #epnudil1fk
  5.  #hogb1vion0 : add
  6.  #epnudil1fk : reset-root #epnudil1fk
  7.  #97dstg1ao2 : add
  8.  #epnudil1fk : reset-root #epnudil1fk
  9.  #ugtr8mvop3 : add
  10. #epnudil1fk : builtins.mergeio
  11. #sjg2v58vn2 : (initial reflogged namespace)

.> reset-root 2

  Done.

```
```ucm
.> load scratch.u

  I found and typechecked these definitions in scratch.u. If you
  do an `add` or `update`, here's how your codebase would
  change:
  
    ⍟ These new definitions are ok to `add`:
    
      unique type Foo x y
      unique ability Zonk
      foo : Nat -> Foo ('{Zonk} a) ('{Zonk} b) -> Nat

```
## Long lines with repeated operators

Regression test for https://github.com/unisonweb/unison/issues/1035

```unison
foo : Text
foo =
  "aaaaaaaaaaaaaaaaaaaaaa" ++ "bbbbbbbbbbbbbbbbbbbbbb" ++ "cccccccccccccccccccccc" ++ "dddddddddddddddddddddd"
```

```ucm
.> add

  ⍟ I've added these definitions:
  
    foo : Text

.> edit foo

  ☝️
  
  I added these definitions to the top of
  /Users/runar/work/unison/scratch.u
  
    foo : Text
    foo =
      use Text ++
      "aaaaaaaaaaaaaaaaaaaaaa"
        ++ "bbbbbbbbbbbbbbbbbbbbbb"
        ++ "cccccccccccccccccccccc"
        ++ "dddddddddddddddddddddd"
  
  You can edit them there, then do `update` to replace the
  definitions currently in this namespace.

.> reflog

  Here is a log of the root namespace hashes, starting with the
  most recent, along with the command that got us there. Try:
  
    `fork 2 .old`             
    `fork #epnudil1fk .old`   to make an old namespace
                              accessible again,
                              
    `reset-root #epnudil1fk`  to reset the root namespace and
                              its history to that of the
                              specified namespace.
  
  1.  #58g13u2vjv : add
  2.  #epnudil1fk : reset-root #epnudil1fk
  3.  #5bpdpn1048 : add
  4.  #epnudil1fk : reset-root #epnudil1fk
  5.  #7rhiegjl3c : add
  6.  #epnudil1fk : reset-root #epnudil1fk
  7.  #hogb1vion0 : add
  8.  #epnudil1fk : reset-root #epnudil1fk
  9.  #97dstg1ao2 : add
  10. #epnudil1fk : reset-root #epnudil1fk
  11. #ugtr8mvop3 : add
  12. #epnudil1fk : builtins.mergeio
  13. #sjg2v58vn2 : (initial reflogged namespace)

.> reset-root 2

  Done.

```
```ucm
.> load scratch.u

  I found and typechecked these definitions in scratch.u. If you
  do an `add` or `update`, here's how your codebase would
  change:
  
    ⍟ These new definitions are ok to `add`:
    
      foo : Text

```
## Emphasis in docs inserts the right number of underscores

Regression test for https://github.com/unisonweb/unison/issues/2408

```unison
myDoc = {{ **my text** __my text__ **MY_TEXT** ___MY__TEXT___ ~~MY~TEXT~~ **MY*TEXT** }}
```

```ucm
.> add

  ⍟ I've added these definitions:
  
    myDoc : Doc2

.> edit myDoc

  ☝️
  
  I added these definitions to the top of
  /Users/runar/work/unison/scratch.u
  
    myDoc : Doc2
    myDoc =
      {{
      **my text** __my text__ **MY_TEXT** ___MY__TEXT___
      ~~MY~TEXT~~ **MY*TEXT**
      }}
  
  You can edit them there, then do `update` to replace the
  definitions currently in this namespace.

.> undo

  Here are the changes I undid
  
  Added definitions:
  
    1. myDoc : Doc2

```
```ucm
.> load scratch.u

  I found and typechecked these definitions in scratch.u. If you
  do an `add` or `update`, here's how your codebase would
  change:
  
    ⍟ These new definitions are ok to `add`:
    
      myDoc : Doc2

```
## Parenthesized let-block with operator

Regression test for https://github.com/unisonweb/unison/issues/1778

```unison
structural ability base.Abort where
  abort : a

(|>) : a -> (a ->{e} b) -> {e} b
a |> f = f a

handler : a -> Request {Abort} a -> a
handler default = cases
  { a }        -> a
  {abort -> _} -> default

Abort.toOptional : '{g, Abort} a -> '{g} Optional a
Abort.toOptional thunk = '(toOptional! thunk)

Abort.toOptional! : '{g, Abort} a ->{g} (Optional a)
Abort.toOptional! thunk = toDefault! None '(Some !thunk)

Abort.toDefault! : a -> '{g, Abort} a ->{g} a
Abort.toDefault! default thunk =
  h x = Abort.toDefault! (handler default x) thunk
  handle (thunk ()) with h

x = '(let
  abort
  0) |> Abort.toOptional
```

```ucm
.> add

  ⍟ I've added these definitions:
  
    structural ability base.Abort
    Abort.toDefault!  : a -> '{g, Abort} a ->{g} a
    Abort.toOptional  : '{g, Abort} a -> '{g} Optional a
    Abort.toOptional! : '{g, Abort} a ->{g} Optional a
    handler           : a -> Request {Abort} a -> a
    x                 : 'Optional Nat
    |>                : a -> (a ->{e} b) ->{e} b

.> edit x base.Abort |> handler Abort.toOptional Abort.toOptional! Abort.toDefault!

  ☝️
  
  I added these definitions to the top of
  /Users/runar/work/unison/scratch.u
  
    structural ability base.Abort where abort : {base.Abort} a
    
    Abort.toDefault! : a -> '{g, Abort} a ->{g} a
    Abort.toDefault! default thunk =
      h x = Abort.toDefault! (handler default x) thunk
      handle !thunk with h
    
    Abort.toOptional : '{g, Abort} a -> '{g} Optional a
    Abort.toOptional thunk = '(toOptional! thunk)
    
    Abort.toOptional! : '{g, Abort} a ->{g} Optional a
    Abort.toOptional! thunk = toDefault! None '(Some !thunk)
    
    handler : a -> Request {Abort} a -> a
    handler default = cases
      { a }        -> a
      {abort -> _} -> default
    
    x : 'Optional Nat
    x =
      ('let
        abort
        0) |> toOptional
    
    (|>) : a -> (a ->{e} b) ->{e} b
    a |> f = f a
  
  You can edit them there, then do `update` to replace the
  definitions currently in this namespace.

.> undo

  Here are the changes I undid
  
  Added definitions:
  
    1. structural ability base.Abort
    2. base.Abort.abort  : {#oup50kgmqv} a
    3. handler           : a -> Request {#oup50kgmqv} a -> a
    4. Abort.toDefault!  : a -> '{g, #oup50kgmqv} a ->{g} a
    5. Abort.toOptional  : '{g, #oup50kgmqv} a
                         -> '{g} Optional a
    6. Abort.toOptional! : '{g, #oup50kgmqv} a ->{g} Optional a
    7. x                 : 'Optional Nat
    8. |>                : a -> (a ->{e} b) ->{e} b

```
```ucm
.> load scratch.u

  I found and typechecked these definitions in scratch.u. If you
  do an `add` or `update`, here's how your codebase would
  change:
  
    ⍟ These new definitions are ok to `add`:
    
      structural ability base.Abort
      Abort.toDefault!  : a -> '{g, Abort} a ->{g} a
      Abort.toOptional  : '{g, Abort} a -> '{g} Optional a
      Abort.toOptional! : '{g, Abort} a ->{g} Optional a
      handler           : a -> Request {Abort} a -> a
      x                 : 'Optional Nat
      |>                : a -> (a ->{e} b) ->{e} b

```
## Line breaks before 'let

Regression test for https://github.com/unisonweb/unison/issues/1536

```unison
r = 'let
 y = 0
 y
```

```ucm
.> add

  ⍟ I've added these definitions:
  
    r : 'Nat

.> edit r

  ☝️
  
  I added these definitions to the top of
  /Users/runar/work/unison/scratch.u
  
    r : 'Nat
    r = 'let
      y = 0
      y
  
  You can edit them there, then do `update` to replace the
  definitions currently in this namespace.

.> undo

  Here are the changes I undid
  
  Added definitions:
  
    1. r : 'Nat

```
```ucm
.> load scratch.u

  I found and typechecked these definitions in scratch.u. If you
  do an `add` or `update`, here's how your codebase would
  change:
  
    ⍟ These new definitions are ok to `add`:
    
      r : 'Nat

```
