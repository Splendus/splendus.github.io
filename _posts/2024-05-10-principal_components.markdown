---
layout: post
title: 'Components'
date: 2024-05-10 22:32:26 +0100
categories: jekyll update
---

# Principal Components

What exactly is _PCA_, _Principal Component Analysis_? (Thanks to Dmitry)

Given some distribution of data, we want to find the characterstics which describe them best.
It turns out, that to find the components, that describe data best, we can search for
characteristics that differ among themselves the most.

As an example, consider describing a person to a friend, whose name escapes you.
"I've just met this guy at the supermarket and we got into a conversation. It turns out, he knows
you, too!"
'Ah, really! What is his name?'
"Oh... I just can't remember. He is rather tall"

Now imagine, that he was tall and was also wearing big shoes... This of course would not come as
a surprise. Specifically, because being tall and having large feet generally go together. They are
_correlated_. Telling your friend about characteristics that are correlated won't _add information_
to the puzzle.

"...He is rather tall."
'Hmm, I know a lot of tall people'
"And wore big shoes!"
'Well, that doesn't help.'

On the other hand, we would add a lot of information by supplying information that is uncorrelated
with his size.

'..., that doesn't help'
"Right. He had brownish hair and a full beard. He also talked about his career in particle physics."

These, then are a lot of informations that do not interplay.

PCA lets us find the directions with respect to which the data varies most. It at the same time
minimizes construction error and maximizes variance of projected points.

It turns out, that these two properties yield as directions the eigenvectors as directions chosen
in an order descending in eigenvalue magnitude.
