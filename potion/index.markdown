---
layout: default
title: The Potion language
---
<div>
<pre><code style="margin: 0 auto; width: 224px;">      .ooo
     'OOOo
 ~ p ooOOOo tion ~
     .OOO
      oO      %% a little
        Oo    fast language.
       'O
        `
       (o)
   ___/ /          
  /`    \ 
 /v^  `  ,
(...v/v^/
 \../::/
  \/::/</code></pre>
</div>
## ~ Potion! ~
Potion is an object- and mixin-oriented (traits)
language.

Its exciting points are:

 * Just-in-time compilation to x86 and x86-64
   machine code function pointers. This means
   she's a speedy one. Who integrates very
   well with C extensions.

   The JIT is turned on by default and is
   considered the primary mode of operation.

 * Intermediate bytecode format and VM. Load
   and dump code. Decent speed and cross-
   architecture. Heavily based on Lua's VM.

 * A lightweight generational GC, based on
   Basile Starynkevitch's work on Qish.
   <http://starynkevitch.net/Basile/qishintro.html>

 * Bootstrapped "id" object model, based on
   Ian Piumarta's soda languages. This means
   everything in the language, including
   object allocation and interpreter state
   are part of the object model.
   (See COPYING for citations.)

 * Interpreter is thread-safe and reentrant.
   I hope this will facilitate coroutines,
   parallel interpreters and sandboxing.

 * Small. Under 10kloc. Right now we're like
   6,000 or something. Install sloccount
   and run: make sloc.

 * Reified AST and bytecode structures. This
   is very important to me. By giving access
   to the parser and compiler, it allows people
   to target other platforms, write code analysis
   tools and even fully bootstrapped VMs. I'm
   not as concerned about the Potion VM being
   fully bootstrapped, especially as it is tied
   into the JIT so closely.

 * Memory-efficient classes. Stored like C
   structs. (Although the method lookup table
   can be used like a hash for storing arbitrary
   data.)

 * The JIT is also used to speed up some other
   bottlenecks. For example, instance variable
   and method lookup tables are compiled into
   machine code.

However, some warnings:

 * Strings are immutable (like Lua) and byte
   arrays are used for I/O buffers.

 * No floating point support yet.

 * No error handling. I'm wary of just tossing
   in exceptions and feeling rather uninspired
   on the matter. Let's hear from you.

## ~ a whiff of potion ~

    5 times: "Odelay!" print.

Or,

    add = (x, y): x + y.
    add(2, 4) string print

Or,

    hello =
      "(x): ('hello ', x) print." eval
    hello ('world')

## ~ how it transpired ~

This isn't supposed to happen!

I started playing with Lua's internals and reading
stuff by Ian Piumarta and Nicolas Cannasse. And I,
well... I don't know how this happened!

Turns out making a language is a lovely old time,
you should try it. If you keep it small, fit the
VM and the parser and the stdlib all into 10k
lines, then it's no sweat.

To be fair, I'd been tinkering with the parser
for years, though.

## ~ the potion pledge ~

EVERYTHING IS AN OBJECT.
However, OBJECTS AREN'T EVERYTHING.

(And, incidentally, everything is a function.)

## ~ items to understand ~

1. A traditional object is a tuple of data
   and methods: (D, M).
   
   D is kept in the object itself.
   M is kept in classes.

2. In Potion, objects are just D.

3. Every object has an M.

4. But M can be altered, swapped,
   added to, removed from, whatever.

5. Objects do not have classes.
   The M is a mixin, a collection
   of methods.

   Example: all strings have a "length"
   method. This method comes with Potion.
   It's in the String mixin.

6. You can swap out mixins for the span
   of a single source file.

   Example: you could give all strings a
   "backwards" method. But just for the
   code inside your test.pn script.

7. You can re-mix for the span of a
   single closure.

To sum up:

EVERYTHING IS AN OBJECT.
EVEN MIXINS ARE OBJECTS.
AND, OF COURSE, CLOSURES ARE OBJECTS.

However, OBJECTS AREN'T EVERYTHING.
THEY ARE USELESS WITHOUT MIXINS.

## ~ feverish and fond thankyous ~

I am gravely indebted to Basile Starynkevitch, who fielded my
questions about his garbage collector. I favor French hackers
to an extreme (Xavier Leroy, Nicolas Cannasse, Guy Decoux,
Mathieu Bochard to name only a portion of those I admire) and
am very glad to represent their influence in Potion's garbage
collector.

Matz, for answering my questions about conservative GC and
for encouraging me so much. Potion's stack scanning code and
some of the object model come from Ruby.

Steve Dekorte for the Io language, libgarbagecollector and
libcoroutine -- I referred frequently to all of them in
sorting out what I wanted.

Of course, Mauricio Fernandez for his inspiring programming
journal housed at http://eigenclass.org/R2/ and for works
derived throughout the course of it -- extprot most of all.
Many of my thoughts about language internals (object repr,
GC, etc.) are informed by him.

Ian Piumarta for peg/leg. I use a re-entrant custom version
of it, but the original library is sheer minimalist parsing
amazement.

Final appreciations to Jonathan Wright and William Morgan
who pitched in, back in the wee hours of Potion's history.
Tanks.

## ~ license ~

See COPYING for legal information. It's an MIT license,
which lets you do anything you want with this. I'm hoping
that makes it very nice for folks who want to embed a little
Potion in their app!