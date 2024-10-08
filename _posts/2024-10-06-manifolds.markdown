---
layout: post
title: 'Manifolds'
date: 2024-06-18 22:32:26 +0100
categories: jekyll update
---

# Manifolds

Manifolds are abundant nowadays. One does not have to delve into differential geometry to face
them - even in Machine Learning parlance they're present as, e.g. the training or data
manifolds.

What exactly are manifolds, those mathematical constructs we like to visualize by either a sphere or
some 2 dimensional "plane" that is not _flat_.
Manifolds are extremely general. A few things must be given,
first we need a _topological space_...

To get to the notion of a topological space we could even more fundamental.
Start with a _set_, the hopefully not so argurably, most fundamental
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
But at some point, for very large $n$, $1/n \approx 1/(n+1)$ and the sets will have
an intersection which is not necessarily in $O$. Admittedly, this proof is very unsatisfying
but for now we leave it as is.

(iii)

Let's consider a very simple example. Define the set $X$ as the collection of numbers
$\{1, 2, 3 \}$, then the _collection of sets_
$O_P:=\{ \{1\}, \{2\}, \{3\}, \{1, 2\}, \{1, 3\}, \{2, 3\}, \{1, 2, 3\}, \emptyset \}$
would be an honest to god topology of $X$, as (i) both the empty set $\emptyset$ but
also $X$ itself, $\{1, 2, 3\}$ are elements of $O_P$, (ii) any intersection of sets
that are element of $O$ are again an element of $O_P$ as well as any union is.
Indeed, $O_P$ is a very special topology of $X$, namely its _powerset_, that is,
the collection of all subsets of $X$. This is also called the chaotig topology of
$X$. Another valid topology of $X$ is $\{ \{1, 2, 3\}, \emptyset \}$ which again
fulfillfs the axioms. Obviously the set itself and the empty set are there.
Furthermore, any intersection $\{1, 2, 3\} \intersection \emptyset = \emptyset$
is again in the topology as is any union $\{1, 2, 3\} \union \emptyset = \{1, 2, 3\}$.
We call this topology also the trivial topology.

What now is the use of a topology? We can define _open_ sets as the sets which are elements
of $X$. We have the notion of open sets in terms of _open intervals_, which is solely the case
because we have defined the topology as the _standard topology_ of any _metric space_.
A metric space simply is a space equipped with a notion of _distance_ but we'll postpone
this discussion to another time.
Note simply, that the classification of sets being open, if they lie in the topology
has interesting properties if we continue with the standard procedure and define
_closed_ sets as those, that are complements of open sets. A complement is simply
the "opposite" of a set, which for sets means, that if $A$ is a subset of $X$,
$A \subset X$ then $A$'s complement $A^c$ is all of $X$ without $A$, $X \ A$.
Then, for any topology, we find that the underlying set itself, $X$, is both open
as it is by definition in the topology, but also closed, as its complement
$X \ X = \emptyset$ is of course by definition also in any toplogy $O$.
The same holds for the $\emptyset$ as $X \ \emptyset = X \element O$.
If we consider again the chaotic topology, i.e. the sets power set, we find
that any set in it is open and closed.

How, then, does this bring us any closer to the notion of _connectedness_?
It seems reasonable to define a connected space, as one that does not allow
its partitioning into two open subsets. That is, a connected topological space is
one that cannot be written as a union of two non overlapping open subsets.
For this definition to be of any use, we have to exclude the set itself and
the empty set as then any topological space would be connected.
Therefore, _a topological space (X, O) is called connected, if there exist
no two open nonempty subsets of $X$ whose union is $X$._
In formulae:
$(X, O) connected \equiv \not \exists A, B \in O: A \intersection B = \emptyset \and A \union B = X$.
