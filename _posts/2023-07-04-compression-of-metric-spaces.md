---
layout: post
title: Metric Compression Part 1
excerpt: "A review of research on compression of metrics"
categories: [computer science, machine learning, curse of dimensionality, metric spaces, math]
usemathjax: true
useemoji: true
---

# Prologue: What is space?

In the field of machine learning, there's a lot of focus on dealing with
high-dimensional vector spaces. Usually, though not always, the standard
Euclidean space $$\mathbb{R}^d$$, and if we want to measure the distance between
two vectors in $$\mathbb{R}^d$$, we typically choose the familiar Euclidean
distance: $$d(x, y) = \sqrt{(x_1-y_1)^2 + ... + (x_d-y_d)^2}$$. The combination
of an ambient "space" and distance "metric" is aptly referred to as a *metric
space*.[^space]

I would be missing out on audience engagement if I didn't somehow mention Deep
Learning :tm:, so here's the tie-in: an *artificial neural network* is just a
bunch of "dumb" *differentiable* transformations composed together. Some are
linear, some are non-linear, and the "deep" part just means we're composing a
lot of 'em in a big chain like $$g(x) = f_1(f_2(f_3(...f_{1000}(x)...)))$$. The
way we train them is with *backpropagation*, which is just a technical term for
a way of efficiently calculating the derivative and optimizing a loss function
$$L(x, g(x))$$.

The requirement here is that our functions are all differentiable - which means
we need to be able to do calculus somehow. The bog-standard metric space to do
calculus in is, you guessed it, Euclidean space.

Now let's say we chop a trained neural network in half, perhaps one that
classifies images, to see what the intermediate function outputs are. We get
some points which don't resemble the inputs or the final outputs. The idea of
*representation learning* is that these intermediate outputs do mean something -
the neural network has learned to "see" things like different lines, shapes, and
colors patches in the image. Our intermediate vector might have coordinates
which are $$1$$ if it "sees" a vertical line in the image, and $$0$$ if not. But
when we try to interpret these intermediate network outputs in general, there is
a simple question which is very difficult to answer concretely. What does the
Euclidean distance between two points in this intermediate space
*mean*?[^embedding]

Even though we rely on a distance metric for the idea of backpropagation to even
make sense, the question is unanswered. Doing the math to outline the algorithm
is one thing. Doing the math (and experiments!) to figure out what are the
*structures* that result in the "representation space" when we work with
real-world data, and explaining the *meaning* of any such structures, appears to
be orders of magnitude more difficult. I'll leave it at this: the question "what
is space?" might seem like it's only for mathematicians or physicists, or maybe
even philosophers - but it can actually be a hard question even in applications
of machine learning.

That said, we'll sweep that under the rug and pretend that as long as we're
working with Euclidean space, everything is cozy and well-understood.

# Introduction

One commonly encountered fundamental problem is *nearest neighbor search* (NNS).
In an "Intro to Machine Learning" course the idea of nearest neighbors might be
discussed as part of a simple classifier called *k-nearest neighbors* (k-NN),
which is simple in concept: if you have a bunch of training data $$(x_i, y_i)$$,
and you see a new input $$x'$$ then you can classify $$x'$$ by looking at the
$$k$$ nearest $$x_i$$ points in your training data and seeing what their
corresponding labels $$y_i$$ are. Related applications are in recommendations
and information retrieval, where the goal is just to find some of the closest
$$x_i$$ without any particular labels required.

However, to actually implement these, well, you have to *find* those $$k$$
nearest neighbors. In other words, you need a solution to the NNS problem. The
naive solution is to do an exhaustive search every time - compute the distances
between $$x'$$ and *all* $$x_i$$. Problem is, if you have $$d$$-dimensional data
and $$n$$ training points, that's $$O(nd)$$ scaling. If you're looking at more
than a thousand dimensions and a billion points, things quickly get out of hand.

In this post, we won't be looking at direct solutions to the NNS problem.
Instead I'll explore some background and theory for a related problem,
compressing metric spaces, concluding with a powerful data structure and
algorithm with both practical effectiveness and strong theoretical
justification.[^original]

# Fundamentals

Let's outline our goal a bit more clearly: what we want is a data structure and
algorithm, AKA an index, that lets us query for the distance between any two
data points in given data set $$X$$, and scales up to thousands of dimensions
and billions or even trillions of points.

That means we have to be really efficient - both constructing and
querying the index have to be close to linear in computation and storage. In
terms of main memory (RAM) we would like it if, similar to classic
databases[^db], we could operate with much less RAM than the whole index.[^mem]
The storage size is of particular interest, as with all DBs, because reading
from storage and the network is slow but cheap, while RAM is fast but expensive.

To achieve this, we're generally willing to accept a reasonable *approximation*
to the true distances, as long as it's still practically useful.

## JL Lemma

A famous classical result in this space is the *Johnson-Lindenstrauss (JL)
lemma* first published in 1984. Since that first publishing, there have been
various different proofs of the lemma, as well as variants of it applicable to
different contexts. We'll just look at applying the original result in Euclidean
space.

Essentially, the JL lemma tells us that if we have $$n$$ points in
$$\mathbb{R}^d$$, we can project those points down to a lower dimensional space
$$\mathbb{R}^k$$, while preserving the distances between any pair of the $$n$$
points.

Specifically, we can choose any "tolerance" $$\epsilon$$ - normally referred to
in this context as the *distortion* - which is basically a percentage delta
between $$0\%$$ and $$50\%$$. Then there exists a $$k \times d$$ matrix $$A$$
that projects our $$d$$-dimensional data into $$k$$-dimensional space, while
preserving distances up to a relative error of $$\pm \epsilon$$. Crucially,
$$k$$ scales *logarithmically* with $$n$$, and is completely *independent* of
the original dimensionality $$d$$.[^tight]

Here's the mathematical statement:

Let $$\epsilon \in (0, 1/2)$$, and let $$X = \{x_1,...,x_n\} \subset \mathbb{R}^d$$ be
a set of high-dimensional points. For some positive integer $$k = O(\log
(n)/\epsilon^2)$$, there exists a matrix $$A \in \mathbb{R}^{k \times d}$$ such that for any
$$x, y \in X$$, we have

$$ (1 - \epsilon) \|x - y\|_2^2 \leq \|Ax - Ay\|_2^2 \leq (1 + \epsilon)\|x - y\|_2^2$$

### Intuition

To review some linear algebra, $$A$$ is a $$k \times d$$ matrix which is
projecting a higher-dimensional space into a lower-dimensional one. There are
two interesting views of this matrix: the column view and the row view.

The column vectors of $$A$$ are what the standard basis vectors of
$$\mathbb{R}^d$$ get mapped to, forming a new basis in $$\mathbb{R}^k$$. You can
see how that works by multiplying $$A$$ by the standard basis vectors, or in
aggregate, the $$d$$-dimensional identity $$I$$. Since we want $$d$$ to be much
larger than $$k$$, it's impossible for the column vectors to be linearly
independent. In other words, a projection to lower dimensions is inherently
*lossy* - some pairs of distinct points in the original higher-dimensional space
will get mapped to the same lower-dimensional points.

The row vectors of $$A$$ are $$k$$ different $$d$$-dimensional vectors that we
are projecting each data point $$x_i$$ onto. In this case, it's possible for
these row vectors to be linearly independent (geometrically,
orthogonal/perpendicular). In fact, the larger $$d$$ gets, the more likely that
a "small" set of random "almost"-unit vectors will be "nearly" linearly
independent. This is good for us - although the mapping is lossy, we are
extracting the maximum amount of information from the points as we can get for a
given $$k$$.

## JL Lemma Applied

OK, so conceptually we could maybe turn our 10,000-dimensional data, even with a
trillion data points, into 100-dimensional data. We haven't answered some
crucial questions, though. How do we actually compute this projection matrix
$$A$$? What are the constants involved here - will we actually end up with
10-dimensional data, or 1000-dimensional data? Does this projection require each
individual coordinate to be more precise (e.g. by going from 32-bit
single-precision to 64-bit double precision), nullifying some of the
compression?

I won't review the entire body of work here, but there is a paper published 2003
from Dimitris Achlioptas from Microsoft Research [^randomproj] which provides us
with a particularly nice result in applying the JL lemma. The basic idea, which
is common across most applications of the JL lemma, is to construct $$A$$ by
random sampling. The interesting contribution of this result is that we can
sample from very simple distributions, and with high probability get a "good"
$$A$$.

More formally:

Let $$\beta > 0$$ and

$$k_0 = \frac{4 + 2\beta}{\epsilon^2/2-\epsilon^3/3} \log n$$

Let $$k > k_0$$ and $$A$$ be constructed by randomly sampling from either one of
these following two distributions: $$a_{ij} = \{\pm 1\}$$ uniformly, or
$$a_{ij}=\pm 1$$ with $$1/6$$ probability each and $$a_{ij} = 0$$ with $$2/3$$
probability.

Then after some scaling by $$\sqrt{k}$$, and $$\sqrt{3}$$ for the latter
probability distribution, this $$A$$ projects $$X$$ with $$(1 \pm
\epsilon)$$-distortion with probability $$> 1 - n^{-\beta}$$.

Now we have a pretty well-specified algorithm with some nice properties, we can
take a look at the practical consequences.

### Computation

Since the distributions are pretty simple, we need very few random bits. In the
distribution with only $$\pm 1$$, we only need a single bit of randomness per
matrix entry. For the distribution that includes $$0$$, we only need $$\approx
2.6$$ bits per entry.

The original paper suggests an interesting possibility for specializing the
required computation: for each of the $$k$$ coordinates, we can randomly throw
away $$2/3$$ of the $$d$$ inputs. Then we randomly partition the $$d$$ inputs
into two halves, sum each half, and then subtract the difference. Originally
they intended this to exploit the performance of SQL databases, but in the
modern day we might consider whether this has efficient implementations on GPUs
or even further specialized hardware.

Just on the face of it, I find it incredibly interesting that we can get away
with avoiding the *multiplication* part of matrix multiplication, and have this
elegant construction of $$A$$ which takes as little as a single "coin flip" per
entry. You don't even need to do anything special to maintain the length of the
vectors.

### Choice of $$\beta$$

For any $$n$$ big enough that we don't want to do brute force search, $$\beta =
1$$ is good enough -- the chance of getting a bad $$A$$ would be $$1/n$$. In
fact, if $$n > 10^8$$, we're probably even fine with $$\beta = \frac{1}{2}$$. Of
course, the $$2 \beta$$ term only accounts for $$1/3$$ of $$k$$, so while going
down to $$\beta = \frac{1}{2}$$ reduces $$k$$ by $$\approx 17\%$$, trying to get
it down much further will reduce our probability of success for tiny marginal
savings.

### Values of $$k$$

Let's say we have $$n = 10^8$$, and we choose $$\beta = \epsilon =
\frac{1}{2}$$. Then $$k_0 \approx 1105$$. OK, that might be interesting if the
original data is 10,000-dimensional, but if it was under 1,000 dimensions to
begin with, we're not achieving much. This is already with a fairly small
$$\beta$$ and large $$\epsilon$$, so it seems like this is a limitation of the
JL transform...

### Unluckiness

One trade-off we have to consider is whether it's necessary to maintain $$(1 \pm
\epsilon)$$-distortion across all points simultaneously. In other words, a
single pair of points which gets mapped to a too-wrong distance is considered a
violation of the JL lemma. If we're willing to accept that *some* pairs of
points will have distances that are slightly outside the $$\epsilon$$ error
range, what exactly is that trade-off? That is, if we intentionally choose a
too-small $$k$$, how often do we get bad pairs of points, and how off is the
mapped distance between them?

If the answer is not too often and not too far off, then maybe we could do a lot
better (smaller $$k$$), if we're willing to accept that, say, $$1\%$$ of the
time, a pair of points would have an error between $$\epsilon$$ and
$$2\epsilon$$. More generally, can we quantify the probability distribution of
the relative error between randomly chosen points?

I'm not aware of any published results in this context, but I'm fairly certain
that it's a straightforward exercise[^exercise] to compute this distribution, so
it's likely been done.

An advantage of this construction compared to other methods for nearest neighbor
indices is that we get a smooth fall-off of the error, so in that sense the
output (probabilistically) gets gradually worse with smaller $$k$$. However,
using the JL lemma has a marked disadvantage in that, if many of the distances
in the metric are close to each other, $$\epsilon$$ relative error could give
wrong neighbors frequently.

### Precision

Since this approach only involves a handful of multiplications by constants,
it's pretty clear that the coordinates we end up with have the same precision as
the ones we started with. Plus with only additions and subtractions (and scaling
by relatively small constants) we don't have to worry about numerical stability
or anything like that.

# Conclusion

In this post, we covered the idea of *metric spaces* and *nearest neighbor
search*, particularly in a high-dimensional context, to motivate the need for a
compressed index that does a good job of approximately preserving distances.
Next time, we'll dig deeper into a concrete analysis of the storage
requirements, and a more sophisticated data structure which is provably
near-optimal as a compressed index.

[^space]: Many of the results referenced in this post apply generally to all sorts of metric spaces. For simplicity we'll only cover the "standard" Euclidean $$\mathbb{R}^n$$.

[^embedding]: This also applies for various models which produce *embeddings* or any form of *latent representation*.

[^original]: Most of the content of this post series I originally wrote for [one of my graduate topics reports](https://github.com/rationalis/cse-291-final-report/blob/master/report_postsubmit.pdf)

[^db]: Nowadays databases solving this particular problem are referred to as *vector databases*.

[^mem]: If this system is part of a major feature which makes a ton of money, like say Amazon's recommendation engine, you can probably justify keeping everything in RAM. The rest of us probably can't.

[^randomproj]: Beyond just the result, [this paper](https://www.sciencedirect.com/science/article/pii/S0022000003000254) is also quite readable, outlines the motivations and intuitions quite well, and reviews specific instances of prior work. Technically it was published in 2001 but officially in a journal only in 2003.

[^exercise]: The original proof of the JL lemma involves starting from the *distributional JL lemma*, which is a consequence of the concentration of measure (AKA [the soap bubble](https://www.inference.vc/high-dimensional-gaussian-distributions-are-soap-bubble/)), and taking a union bound over pairs of points.

[^tight]: One important note here is that the JL bound is tight in the sense that, for any set size $$n$$, there's some pathological cases where you can't use any fewer than $$k$$ dimensions and still keep a low distortion. In fact, [the bound even applies to *nonlinear* embeddings](https://arxiv.org/abs/1609.02094), not just the linear ones guaranteed by the JL lemma. To do any better in terms of compression, we have to give up the full distortion guarantee, or we have to look outside of pure dimensionality reduction.

*[JL]: Johnson-Lindenstrauss
