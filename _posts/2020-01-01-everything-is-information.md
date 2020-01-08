---
layout: post
title: Everything is information
excerpt: "As it turns out, information is a fundamental principle of the universe."
categories: [information theory, musings, everything is]
---

# Introduction

As humans, we tend to see the world at our level. Which is to say, at the size,
mass, and energy scales common to our day-to-day lives. Tables and chairs, cars
and bikes, etc. However, a physicist would probably tell you that this
intuitive, anthropocentric conception of the universe is only a reasonable
*approximation* for the very small physical regime that we live in. What the
universe really looks like at small scales is the weirdness of quantum physics;
at very large scales, we encounter relativistic effects due to gravity; and in
some cases, like around black holes, some unknown combination of both.

This isn't an article about conventional physics, though - there is another
interesting way to view the universe. And that is in terms of *information*.

# What is information?

I don't want to get too philosophical, but we do have to start somewhere. When
we think of information, we probably think in terms of communicating it, via
speech or text. We also record and retrieve information, but for the purposes of
this post, we can just think of that as a form of communication between the
recorder and retriever (even if, say, they're the same person at different
points in time).

OK, but what exactly *is it* that we're communicating, and why? Fundamentally,
when two people communicate, there's something that one person knows that the
other doesn't, and the *knowledge* is what's communicated. Even if it's as
trivial as "what I ate for breakfast." So, putting aside the philosophical
questions about what "knowledge" is, we can think of information as "new
knowledge." If the knowledge is already available, then no information is
communicated.

When framed this way, it's natural for us to describe information in terms of a
*question* and an *answer*. In the simplest possible case, the space of
*possible* answers must have at least two elements, i.e. a yes/no question.
After all, if there's only one possible answer, then we would always know the
answer without any communication.

This describes the basic, implicit premise of a huge swath of computer science,
which we usually take for granted. We measure the storage of data in terms of
bits (bytes being 8 bits), most modern CPUs use either 32-bit or 64-bit memory
addresses, the rate of communication (bandwidth) of a network connection is
measured in bits per second, and so on and so forth.

Still, the title is "*everything* is information," not just data manipulated by
computers...

# The many forms of information

The most familiar example of quantifiable information, to most people, and most
especially computer scientists, will be digital data. However, information takes
many other forms. Every answer to every question is a specific amount of
information - as mentioned before, a single bit of information for a yes/no
question. We can think of any number as a certain amount of information, namely
the number of bits it takes to specify it in binary, and of course, pretty much
anything else we can think of can be converted to numbers and back.[^L]

This already seems fairly comprehensive. Every spoken word is some number of
bits of information, every piece of data touched by a computer, everything ever
written down. These are familiar enough, but the interesting part is we can
think of all of them in terms of bits.

## All things physical

But one might be tempted to think that, surely, there are some things which
*can't* be described as just a bunch of bits. For example, what something smells
like, or how it feels to laugh until your sides hurt. Certainly, we know of no
way to precisely describe these things on paper or a computer. We can describe
them approximately with words, but ultimately, language does not convey all the
complexities of a smell, or an emotion.[^I]

We'll skirt around more philosophical issues, by asserting that everything ever
experienced by any human corresponds exactly one-to-one with the state of a
physical system. In other words, the assertion that there is nothing special
about consciousness, and every subjective experience corresponds to some
configuration of neurons firing in our brains. This will of course be a rather
controversial assertion, but probably not an unfamiliar one.[^1]

Under this assumption that subjective experience really just reduces down to
some physical states, how do we proceed? Well, we already know how to talk about
physical systems, as alluded to in the introduction. It's the domain of
physicists, and the details aren't too important for us. What matters is that we
could, in theory, quantitatively describe physical states in some way, with a
bunch of numbers that we could write down.

Then, for some theoretical data format, we could describe the *entire universe*
in terms of this concept of information. If we play God just for the sake of
argument, we could describe the position and velocity of every particle in the
universe at a subatomic level, and that would capture everything physical,
including the brains of humans.[^2]

# What's the point?

"OK," you might say, "I get it, we can describe stuff with bits and bytes, but
that doesn't seem that interesting. Isn't it just theoretical wiffle-waffling?"

Well, actually, information theory has many applications.

## Compression

One interesting application is compression. By definition, compression is about
taking some information, and somehow making it smaller. It's applied everywhere,
for pictures, videos, programs, etc. With the information theoretic lens we can
make an observation about compression: the only way we can compress information
is to rely on the mutual assumptions of communicating parties. We must assume
certain kinds of redundancies in the data, and ways of encoding that redundancy.
Otherwise, if we reduce the amount of information communicated, we must always
lose something. This is intricately linked with a basic mathematical principle -
we can't make an invertible mapping between sets of different sizes. It also
demonstrates why there is no compression algorithm that will work well for all
possible data (e.g. random data) - for compression to work at all, we make
certain *biased* assumptions, and by definition, some data will violate those
assumptions.

## Cryptography

Cryptography, being the study of secure communication, is unsurprisingly all
about information. It's no coincidence that cryptographic algorithms tend to be
described with some extra metadata about how many bits they use (AES-256,
SHA-512, etc.). How much information do we need to authenticate, or verify the
author of a message? How many bits can be protected with a given number of
"secret" bits? Exactly how random do we need random numbers to be to ensure
security? These questions are at the heart of cryptographic methods used in
modern digital security.

## Physics, again

As it turns out, the physical interpretation of information has very real
consequences. A famous example is the second law of thermodynamics:

> The second law of thermodynamics states that the total entropy of an isolated
> system can never decrease over time

At first glance this doesn't say anything about information, but that's just
because I've avoided using the technical terminology of *entropy*. Those who
remember entropy from their chemistry or physics classes might think of it as a
measure of how "disordered" a system is, in terms of its indistinguishable
microstates. As it turns out, the thermodynamic definition of entropy is
precisely equivalent to the ideas of information outlined above. In fact,
properly speaking, entropy is measured in joules per kelvin per bit![^3]

Another interesting principle is the law of conservation of information. In
classical physics, we can seemingly create or destroy information at will. That
is, it is physically possible to copy any arbitrary sequence of bits perfectly,
at least in theory, and likewise we can take information and irreversibly
destroy it.

However, with quantum mechanics, information takes a different form, which is
quite different than classical bits. In particular, quantum information can
neither be created nor destroyed. The late Stephen Hawking once made a famous
bet *against* this law, based on a belief that black holes destroy information -
and he lost that bet. Why this should be the case is rather technical, and
anyways, to paraphrase Feynman, a technical explanation would not really be a
"why" so much as a "how."

The key point is that, much as physics forbids faster-than-light travel, it
appears to also forbid non-conservative operations on information at the lowest
scale. (And as an aside, we can rephrase the restriction of FTL as
"*information* cannot travel FTL.")

Based on similar ideas about black holes, there is a proven upper bound on how
much information can be contained in finite space with finite energy, known as
the Bekenstein bound. This places fundamental theoretical limits on how fast
computers can be, and how dense their storage can be.[^4] In fact, it is this
result which confirms the possibility of the previously mentioned idea of
describing any physical system - if a certain volume of space can only contain a
certain finite amount of information, then it is possible to describe that
volume using at most that much information.

There are whole theories of physics which are founded upon physical information.
So, I think, one should begin to suspect that information is a genuinely
powerful and pervasive concept.

## And much, much more

That I'm not going to list out for you! Wikipedia has plenty of information
(heh) about the topic.

# The conclusion that maybe should've been the introduction

In 1948, information theory was essentially invented by Claude Shannon, with the
paper "A Mathematical Theory of Communication". Of course, he wasn't the only
person to have these ideas or develop them, but he's the one known as the father
of information theory. At the time, he was mainly concerned with cryptography
and literal, electrical/mechanical communication. In the better part of a
century since then, the theory has found many applications, across too many
fields to count.

As far as we can tell, anything in the universe *is*, in a sense, the
information which describes it.[^U] Viewing things this way is no less correct
than, for example, viewing everything as particles. So, as it turns out,
information is a fundamental principle, not just of human activities, but,
apparently, of the very fabric of the universe.[^5]


[^1]: At any rate, it's certainly much harder to start talking about what quality subjective, conscious feelings (qualia) have which is beyond merely physical. For those who wish to disagree with the assertion, well, I will simply say that in your view of the world, not everything is information - but on the other hand, the stuff that's not information isn't a physically measurable thing, so you could never detect it in other people.

[^2]: You can nitpick that it's a little more complex thanks to quantum physics, and possibly other physics stuff I'm not familiar with. Or maybe physics we don't even know about yet. It's not relevant to the argument, so long as we can assume that physical states are completely quantifiable.

[^3]: Up to some conversion factors. SI units treat bits as dimensionless.

[^4]: We're nowhere close to those limits, and honestly, we might never get anywhere close. But the fact remains that such limits exist.

[^5]: Of course this is deliberately grandiose. The caveat is that you could really say this about a lot of things, like mathematics, or computers. In fact, this post is only the first of a series, hopefully, which covers many such ideas.

[^L]: The specific usage of binary isn't necessary, though it is fundamental in the sense of being the smallest discrete base. The important thing is that information is *logarithmic* in the number of states.

[^I]: One heuristic argument for this is that it's impossible to imagine what something you've never encountered smells or tastes like based purely on a verbal description.

[^U]: Again, philosophical arguments about definitionally unobservable state would evade this principle. And again, it is my personal opinion that such philosophical considerations are of no particular interest.
