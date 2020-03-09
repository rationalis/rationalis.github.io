---
layout: post
title: On modern computation
excerpt: "A mildly disorganized collection of thoughts about computation"
categories: [computer science, history of computers, programming language theory, user interfaces, operating systems, processors]
---

# Introduction

Welcome to 2020. In this day and age, there are [64-core consumer
CPUs](https://www.anandtech.com/show/15483/),[smartphones that fold in
half](https://www.theverge.com/2019/10/30/20938653/), and [retail stores where
you just walk out with what you want to
buy.](https://www.theverge.com/2020/3/9/21171230/) There have been weak-AI
systems that can beat humans at [Go](), [StarCraft](), and [DotA](). We're just
a bit shy of cars that drive themselves (though, sadly, not ones that fly). In
short, 20 years into the 21st century, we are surrounded by technology which was
the stuff of dreams not so long ago.

And yet, it all runs on software and hardware which is, by comparison, quite
ancient. The trinity of operating systems, Windows, Linux, and macOS, originated
in the 80s and 90s. C, the predominant systems programming language, still used
as the foundation of each OS, not to mention embedded applications on pretty
much every electronic device manufactured in the last decade, was born in 1972.
C++? 1983. Python? 1990. Java, JavaScript, PHP, Ruby? 1995. Not only that, among
many experienced and skilled programmers, Emacs (1985) and Vim (1991) are the
text editors of choice.

Are these the *only* options? No, of course not, there's certainly newer
software being used. And every piece of software evolves over time. Modern C++
certainly looks almost nothing like its early days as "C with Classes." But on
the other hand, many of these examples were incremental improvements over their
predecessors, doing little to advance the state-of-the-art. No C++ programmer
would say that the invention of Java, for all the benefits it might have, was
like achieving enlightenment.

So, what gives? Let's take a whirlwind tour of computer history.

# What's inside a computer?

Once upon a time, computers were simple. They were just a collection of
glorified on/off switches with simple circuits to manipulate them. A marvel of
engineering at the time, but by today's standards, childish toys.

Then, the invention of the transistor. With the advent of microprocessors came a
complete revolution in the capabilities of computers. What's more, they
discovered they were consistently able to make smaller and smaller transistors,
enabling faster, more efficient processors. Soon they realized that a simple CPU
design that simply did one operation at a time was inefficient - the rate that
the CPU clock ticked at was fixed, and some operations are simply inherently
much faster than others. So they invented a trick called pipelining. And
out-of-order execution. Multiprocessors (multi-core).

And as a result, what happened on the software side? People invented
higher-level languages like C, so that they wouldn't have to be bogged down by
having to translate their thoughts into machine code. Compilers, software
designed to translate higher-level languages into machine code, grew ever more
complex. At one point, the system grew complex enough that the CPU architects
couldn't change the machine language of their CPUs without breaking the
compilers. So they had a bright idea: they would design their CPUs using
whatever they wanted internally, and add some extra circuitry to translate the
common machine language into the hardware-level instructions their CPUs could
understand.

This process continued in this manner for decades. It's still ongoing now. The
general trend can be described as continually increasing abstraction. The clear
benefit is that programmers can write programs more quickly and easily, not only
increasing the number of programmers and programs, but also enabling programs of
greater scale and sophistication. The downside is that there are a huge number
of inefficiencies due to a countless number of translations between what the
programmer intended to do, and what the hardware actually does.

# Operating systems and user interfaces

Before the development of microprocessors, computers had little in the way of
"user interfaces." They were basically complicated calculators where the state
of the machine was almost directly manipulated by the operator. Considering that
these were a tiny number of specialists, the designs had little in common with
the modern concept of UI.

But when computers became more widespread, professionals from many other walks
of life began using them. Anybody who wanted to store or communicate text or
numbers. Computers were now *general-purpose*. They had to be taught how to do
more than just one thing well and how to do more than one thing at a time. So
programmers invented operating systems, programs that had the sole purpose of
managing other programs. The beginnings of what we would call user interfaces
sprang into life: keyboards, printers, monitors. No longer were these
electromechanical devices to be operated by a handful of specialists; they were
now a proper appliance, to be used, even if not fully understood, by essentially
untrained individuals.

Computers became cheaper and more powerful, so suddenly there was a potential
for a much broader audience. Entrepreneurs like Bill Gates and Steve Jobs
capitalized on the opportunity by providing hardware and software that could be
marketed towards the average Joe, ushering in the age of the Personal Computer.
A key part of their early successes was their development of proper Graphical
User Interfaces (GUIs).[^M] The popularity of computers grew, as their
applicability expanded from an arcane incantation-accepting device into a
simple-to-understand interactive canvas.

# Text editors and programming languages

Alongside processors and operating systems, there was the development of text
editors. Text was originally, of course, the *only* human-accessible mode of
input and output for a computer. But even as graphics began to play a larger
role, most people focused on productivity found that text was simply
irreplaceable. While there was some experimentation, ultimately programmers also
found that the most effective way of representing their programs was in text.

The venerable vi and Emacs both originated in other programs of a bygone era. vi
embraced a system for maximum efficiency of basic text operations in an
extremely constrained environment. For its simplicity and minimalism, it became
nearly universal, in one form or another, on pretty much every non-Windows
computer. On the other hand, Emacs sought to provide the ultimate flexibility,
wanting to exploit their interface with the computer for what it was: the
ability to create and execute arbitrary programs.

## Free & open source software

The most popular open source operating system today is, of course, Linux, or, as
a certain someone would insist, GNU/Linux. And the only popular Emacs variant
today is GNU Emacs. That certain someone is Richard Stallman, who religiously
champions free-as-in-speech (libre) software. Regardless of one's personal
opinions on the matter, it's undeniable that this philosophy and the software
which came of it has profoundly influenced the modern landscape. It is this
philosophy which one might say inspired GNU Emacs to be what it is: a program
which can be modified in almost any way imaginable by the user.

## The Lisp machine

In those days, programming language research was quite fruitful, in the sense
that the space had room for exploration in almost every direction. Many
languages with very different ideas came to be, among them functional languages
(Haskell, Ocaml), truly dynamic languages (Lisp, Smalltalk), logical languages
(Prolog), and a wide variety of others, better and lesser known (APL, Forth,
etc.).

Lisp and Prolog, in particular, were subject to quite some study in the pursuit
of developing AI. In the 70s and 80s, processors were slower and compilers were
not particularly advanced. General-purpose computing hadn't really hit its
stride. Consequently specialized hardware was developed at the MIT AI Lab to
efficiently execute Lisp, namely Lisp machines. While they pioneered a variety
of extremely influential technologies, they were ultimately unsuccessful thanks
to general-purpose PCs coming of age and the AI winter.

As something of a twist of fate, Stallman encountered Lisp at MIT. As a dynamic,
high-level language with very advanced features for its time, and a lineage
already measured in decades, it was an unsurprising choice for an Emacs. It is
for this reason that Emacs uses Emacs Lisp.

## This was going somewhere after all

GNU Emacs has famously been referred to, a little tongue-in-cheek, as both "the
infinitely extensible editor" and "a great operating system, only in need of a
decent text editor." It's true, Emacs is a piece of software with a depth beyond
any single human being, one that comes with its own programming language and has
lasted nigh on 40 years. One can spend a decade using it and not come remotely
close to mastering it.[^E10] I personally can attest to at least a fraction of
that, having used it for several years.

Given the historical context, this is no coincidence. Emacs is perhaps the last
remaining descendant of the Lisp machines. Similarly to other Lisp
implementations in both software and hardware, and the other languages which
took inspiration from them, Emacs is a core runtime written in C supporting a
much larger labyrinth of Emacs Lisp, resembling something like the bytecode
interpreter of Python. It is a system which defines a large set of primitives
for displaying and manipulating text, interactions with the filesystem and
network, and an interface with the underlying operating system. Upon this
foundation, Emacs Lisp programs of all kinds have been written, including but
not limited to: music players, PDF viewers, web browsers, email clients,
terminal emulators, and Tetris.

What is the significance of this? It's a little hard to put into words, but I
will try nonetheless. Emacs is something of a monstrosity of a program. However,
it represents an unbroken line to a time when computers looked completely
different than their modern-day counterparts. Not only does it still function,
it is still evolving. Its language and its core are sufficiently flexible to
allow that, and as a result, the community is able to modify and extend it to
support features which might not have been dreamt of at its creation. In short -
while Linux, for example, might be a remarkably engineered program, Emacs is a
testament to organic growth that defies expectations.

# What the future holds

Many people have been remarkably pessimistic about the current state of software
development, remarking that it is crude, obsolete, inefficient, and just plain
dumb. [^D][^OS][^S]

However, in my view, in spite of all these shortcomings, we have so much to
appreciate, and so much to look forward to.

In the programming language space, I believe the likes of Rust will usher in a
new era of safer systems programming, without paying any penalty in performance.
As a side effect, many ideas, like algebraic data types, which were only popular
in more niche programming languages, will continue to gain traction. Looking out
even further, I'm interested in seeing much more powerful type systems being
developed, such as that of F^\*, which will allow programmers to write code
which can be rigorously reasoned about. Rather than being subject to the
limitations of our foresight in writing the right tests, we can confidently make
claims that our software will *never* encounter certain errors. We can state
with certainty that code adheres to the permissions it requests and is allowed.
And all this with only a minimal burden imposed on the programmer.

In the operating systems space, I am quite certain that innovation will continue
at a fairly rapid pace. Today, there are operating systems for smartphones which
consistently achieve a UI that renders at 60FPS. With better languages for
systems programming and the right goals in mind, I would not be surprised to see
operating systems which handle concurrency much more robustly, encounter none of
those seemingly arbitrary failures, run on multiple devices of different form
factors effortlessly and perhaps even in unison, and attain a low enough
interface latency at a high enough consistency as to be indistinguishable from
real physical interaction. When this goal is achieved we will have many forms of
ubiquitous computing to look forward to - I'm certainly excited for AR/VR
applications to become commonplace.[^N]

Finally, for lack of a better term, the Emacs-y space. This really has nothing
to do with text editors, it is simply that I do not have a better example in
mind. Today, many of us write programs which must be compiled before they are
executed, a process that can take between a few seconds and several hours. More
dynamic (i.e. interpreted and JIT compiled) languages have a lesser lag time,
but at a severe loss of performance. Programs are frequently *not*
cross-platform, compatible only with a specific OS or CPU architecture. And one
of the few languages which opposes these trends to an extent, I shudder to
admit, is JavaScript.

But I believe that this is only a growing pain. We will soon see a day when the
distinction between compiled and interpreted becomes blurred. Not only will
compilation get faster, other hybrid approaches will be developed. The trade-off
between performance and dynamism is not fundamental, it is an engineering
problem. And when we solve those problems, every program will be something of an
Emacs. Long before we achieve general AI (although I do believe that's coming
within the century), every person will be able to communicate with the programs
on any of their computing devices, instructing it as precisely as they wish.
Computers will be tools, not appliances.


[^E10]: http://edward.oconnor.cx/2009/07/learn-emacs-in-ten-years

[^D]: https://tonsky.me/blog/disenchantment/

[^M]: "The Mother of All Demos" demo'd (duh) these concepts in 1968, decades ahead of the first releases of Windows and the Mac OS.

[^N]: https://jnd.org/natural_user_interfaces_are_not_natural/

[^OS]: http://blog.rfox.eu/en/Programmer_s_critique_of_missing_structure_of_oper.html

[^S]: http://doc.cat-v.org/bell_labs/utah2000/utah2000.html
