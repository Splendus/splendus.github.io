---
layout: post
title: 'Manifolds'
date: 2024-06-18 22:32:26 +0100
categories: jekyll update
---

# Manifolds

Manifolds are abundant nowadays. One does not have to delve into differential geometry to face
manifolds - even in Machine Learning parlance they're present as, e.g. the training or data
manifolds.

What exactly are manifolds, those mathematical constructs we like to visualize by either a sphere or
some 2 dimensional "plane" that is not _flat_. Manifolds are extremely general. A few things must be given,
first we need a _topological space_...

Let's get even more fundamental. Start with a _set_, the hopefully not so argurably, most fundamental
building block in maths. For our and most other purposes, a set is merely a collection of unique
things (Maybe, we'll dive into ZF-set theory and its axioms at a later point in time).
To obtain a _space_ from a set, we shall equip it with _structure_. A set $X$ together
with a topology $O$, $(X,O)$ yields a topological space.

What now is a toplogy? Intuitively speaking, a topology tells us about the _connectedness_ of
the given set. To render a collection of subsets $A \subset X$ a topology it must afford three
properties.
(i) The empty set $\emptyset$ and the whole underlying set $X$ must be in the topology,
$X, \emptyset \in O$.

Note, that we have written $X, \emptyset \in O$ and not $X, \emptyset \subset O$, as
$O$ truly is a _collection_ or _set of sets_, that is, its elements are sets.

(ii) Any finite number of intersections of sets in $O$ and any infinite number of
unions of sets in $O$ must, again, be in $O$.

Why is this the case, and why not the other way around. I think we can use the example
of the _standard topolgy_ on a vector space. Then let $(1/(n+1), 1/n)$ be a sequence
of sets in $O$. That is, for $n=1$ simply the interval $(1/2, 1)$. Then take the next number
$n=3$ with the corresponding set $(1/3, 1/2)$ and take their intersection,
$$(1/2, 1) \intersect (1/3, 1) = \emptyset \in O$$.
But at some point, for very large $n$, $1/n \approx 1/(n+1)$ and the sets will be point sets
that is no _open_ sets, and therefore not in $O$.
