---
layout: post
title: 'Symmetric Whitening'
date: 2025-04-22 12:51:44 +0100
categories: jekyll-update
---


### Comparing apples to ... apples 

Data comes in various scales. Measuring an apple's diameter
in millimeters and its weight in kilograms does not even seem odd as these two individual features, diameter and weight, characterize the data at different scales. 
Describing the data by these two features alone, without any notion of measuring units, will let the mass dimension appear much more important.
Already small changes in kilogram can make for a much different apple. On the other hand, comparably large changes
in millimeters are needed to have a similar effect. However, the units may be completely external 
to the program processing the data. We could either hope then that a model might learn the different scales, take into account that model coefficients
come with units or rescale the features by hand to work with similar scales, e.g. work with grams instead of kilograms. [^1]

Let's first look at some (fictitious) apple data in terms of their diameter and weight.

<p align="center">
    <img src="/figures/whitening/raw_2d.svg"/>
</p>

This figure does not convey the problem as its axes are conveniently scaled and shifted. Having no notion of the individual units,
presenting weight and diameter data on the same footing, the following figure is more honest.

<p align="center">
    <img src="/figures/whitening/raw_2d_honest.svg"/>
</p>

Judging from this plot alone, we cannot even see variations in weight.
Before we fix our apple data, let's formalize a bit. For the general case, we describe a data point
$$\mathbf{x}_i$$ in terms of $$d$$ features, $$\mathbf{x} \in \mathbb{R}^d$$,
where in our apple example $$d=2$$. We can regard the following sequence of steps as finding a transformation of the data,
as to make its honest depiction look like the nice first one which had conveniently scaled axis.
Transforming data such that it falls into a specific range, e.g. $$[0,1]$$, falls under the term *Normalization*.
In the following, we will transform the data somewhat more loosely in that we do not restrict its range in a hard way.
Rather than normalize we'll *standardize* the data. In doing so, we shift the its expected value to $$0$$ and
variance to $$1$$.

First, we *center* by subtracting the data's expectation value,
$$\mathbf{x} \mapsto \mathbf{x}_{\mathrm{c}} := \mathbf{x} - \mathrm{E}[x]$$.
Hoping for a somewhat well behaved distribution and assuming we are given
$$N$$ instances of data, we replace the expectation value of $$\mathbf{x}$$ by its sample estimate
$$\boldsymbol{\mu} := \mathrm{E}[\mathbf{x}] \approx \frac{1}{N} \sum_{i=1}^N \mathbf{x}_i$$.
After subtracting the mean from the data, any given feature dimension *centers* around $$0$$
instead of its individual mean $$\mu_j$$ where $$j=1,\dots, d$$. 

For ease of handling, we stack the data points $$\mathbf{x}_i$$ in a matrix of $$N$$ rows and $$d$$ columns,

$$\mathbf{X} = \begin{pmatrix} - \mathbf{x}_1 - \\ - \mathbf{x}_2 - \\ \vdots \\  - \mathbf{x}_{N} - \end{pmatrix},
\quad  \quad
\mathbf{X}_{\mathrm{c}} = \begin{pmatrix} - \mathbf{x}_{\mathrm{c},1} - \\ - \mathbf{x}_{\mathrm{c},2} - \\ \vdots \\  - \mathbf{x}_{\mathrm{c},N} - \end{pmatrix}.
$$

Then, feature $$j$$ of data point $$i$$ is $$X_{ij}$$.

The depiction of our 2D apple example after centering shows the shift to $$0$$.

<p align="center">
    <img src="/figures/whitening/c_2d_honest.svg"/>
</p>

Any given feature dimension, as in our example, still *spreads* differently
around $$0$$. To bring all within a similar range, we consider their variance, which we'll estimate
again by the sample statistics

$$\sigma^2_j := \mathrm{E} \left[(x_j - \mathrm{E}[x_j])^2 \right]  \approx \frac{1}{N-1} \sum_{i}^{N} (X_{ij} - \mu_j)^2.$$

This simplifies for centered data $$\mathbf{X}_{\mathrm{c}}$$ since

$$\frac{1}{N-1} \sum_{i}^{N} (X_{ij} - \mu_j)^2 = \frac{1}{N-1} \sum_{i}^{N} X^2_{\mathrm{c},ij} $$

Then, by *standardization* $$X_{\mathrm{s},ij} := \frac{X_{ij} - \mu_j}{\sigma_j}$$, we center all
features around the same point with unit variance which means  

$$\mu_{\mathrm{s},j} = 0,\ \sigma_{\mathrm{s},j} = 1 \quad \text{for} \quad j=1,\dots, d.$$

Taking a look at our apple data transformed like so

<p align="center">
    <img src="/figures/whitening/s_2d_honest.svg"/>
</p>

it feels justified to zoom in on the image.

<p align="center">
    <img src="/figures/whitening/s_2d.svg"/>
</p>

We have transformed the data in a way as to meaningfully depict it using the same scale for all dimensions.
More importantly, now anyone can interpret patterns in the data without need to refer to external scales.


### Decorrelation

We can reformulate the variance 

$$\sigma_j = \frac{1}{N-1} \sum_{i}^{N} X_{\mathrm{c},ij}^2 = \frac{1}{N-1} \sum_{i}^{N} X_{\mathrm{c},ij} X_{\mathrm{c},ij} =: \sigma_{jj}$$

and generalize the expression to the *co*variance

$$\sigma_{jk} = \frac{1}{N-1} \sum_{i}^{N} X_{\mathrm{c},ij} \, X_{\mathrm{c},ik}  = \frac{1}{N-1} \left( \mathbf{X}_{\mathrm{c}}^\top \mathbf{X}_{\mathrm{c}} \right)_{jk} =: \Sigma_{jk}$$

which defines the covariance matrix $$\boldsymbol{\Sigma} := \frac{1}{N-1} \mathbf{X}_{\mathrm{c}}^\top \mathbf{X}_{\mathrm{c}}.$$

With the transformation $$\mathbf{x} \mapsto \mathbf{x}_{\mathrm{s}}$$ we have achieved that the diagonal entries of 
$$\boldsymbol{\Sigma}$$ are $$1$$. All the off-diagonal entries, however, can still take any value. 

Returning to our example of apples, we now have a variable for weight and diameter that both center around $$0$$ 
and spread mostly between $$-1$$ and $$1$$. Now any weight that is, say, larger than $$0$$ will tell us that this apple
is heavier than the average apple but unless it exceeds $$1$$ we're not surprised by just how heavy it is. The
same holds for its diameter which we use as proxy for its volume. We find however, that these two feature are strongly
*correlated*. Apples with larger volumes are generally also heavier which makes sense when we assume
density variations in apples to not be off the roof[^2].

This means, that the apple's volume contains information about its weight and vice versa. Wouldn't it be more informative
if we had one feature that tells us everything about its volume *only* and one feature that tells us about its weight *only*?[^3]
Although hard to put a human understandable label on it, in formulae this is straight forward. In essence, we want
$$\sigma_{ij} = 0$$ for $$i \neq j$$ while retaining unit scale in the dimensions themselves, $$\sigma_{ii} = 1$$.
Unfortunately, this is not as simple as dividing just all features by all possible scales. 

In essence, we want to map the covariance matrix to the identity matrix, 
$$\boldsymbol{\Sigma} \mapsto \tilde{\boldsymbol{\Sigma}} \equiv \mathbb{I}$$ or 

$$
\begin{align} \tilde{\Sigma}_{ij} = \delta_{ij}
= \begin{cases}
    &1 \quad \text{if} \quad i=j \\
    &0 \quad \text{if} \quad i\neq j
    \end{cases}.
\end{align}
$$

Conceptually, we want so subtract from one feature all the information it has about other features.
That is, we have to "unmix" (better, "decorrelate") individual feature dimensions with a matrix $$\mathbf{M} \in \mathbb{R}^{d \times d}$$,
so that $$\tilde{X}_{ij} = \sum_{k=1}^d B_{jk} X_{ik}$$ or in matrix form, $$\tilde{\mathbf{X}} = \mathbf{X}\mathbf{B}^\top$$.

Recalling how the covariance matrix comes about, $$\boldsymbol{\Sigma} = \mathbf{X}_{\mathrm{c}}^\top \mathbf{X}_{\mathrm{c}}$$,
this system of linear equations in matrix from reads

$$ \mathbf{B}\mathbf{X}^\top \mathbf{X} \mathbf{B}^\top = \mathbb{I} $$

from which immediately follows

$$ \mathbf{B}^{-1} \mathbf{B}^{-\top} = \boldsymbol{\Sigma}. $$

The matrix $$ \mathbf{B} $$ takes an inverse square root character of $$\boldsymbol{\Sigma}$$
which means we transform the data by its covariance' inverse square root. Thinking about the single dimensional
case, this is not surprising at all. Here we simply rescale by the variance's inverse square root
$$x_{ij} \mapsto \frac{x_{ij} - \mu_j}{\sigma_j}$$.
To take into account the interdependence of features, we have to "rescale" the features by some matrix whose inverse square
is the covariance matrix.[^4]


### Matrix square roots

So far so good, but how do we find the (inverse) square root of $$\boldsymbol{\Sigma}$$ ? Various iterative methods exist 
(see e.g. [Higham, 1997](https://link.springer.com/article/10.1023/A:1019150005407) or a more recent account in [Song et al., 2022](https://openreview.net/forum?id=-AOEi-5VTU8))
and also the Cholesky decomposition, $$\mathbf{L}\mathbf{L}^\top = \boldsymbol{\Sigma}$$ with lower triangular $$\mathbf{L}$$, is a valid choice.
For reasons that will hopefully become clear later, we will use the eigendecomposition of $$\boldsymbol{\Sigma}$$.

Being symmetric, positive definite, we can find its eigenvalues and eigenvectors

$$ \boldsymbol{\Sigma} \boldsymbol{Q} = \boldsymbol{Q} \boldsymbol{\Lambda}, $$

where $$\boldsymbol{Q}$$ is the matrix with $$\boldsymbol{\Sigma}$$'s eigenvectors as columns and $$\boldsymbol{\Lambda}$$ the diagonal
matrix of its eigenvalues. Then

$$
\begin{align}
    \boldsymbol{\Sigma} \boldsymbol{Q} &= \boldsymbol{Q} \boldsymbol{\Lambda}  \nonumber \\
    \boldsymbol{\Sigma} &= \boldsymbol{Q} \boldsymbol{\Lambda} \boldsymbol{Q}^{-1} \nonumber \\
    \boldsymbol{\Sigma} &= \boldsymbol{Q} \boldsymbol{\Lambda} \boldsymbol{Q}^{\top},
\end{align}
$$

using that a symmetric matrix' eigenvectors are orthogonal,
$$\boldsymbol{\Sigma} = \boldsymbol{\Sigma}^\top \implies \boldsymbol{Q}^{-1} = \boldsymbol{Q}^{\top}$$.

As $$\boldsymbol{\Sigma}$$'s eigenvalues are real and positive we can safely assume their square root to also be real and write

$$
\begin{align}
    \boldsymbol{\Sigma} &= \boldsymbol{Q} \boldsymbol{\Lambda}^{^1\!/\!_2} \boldsymbol{\Lambda}^{^1\!/\!_2} \boldsymbol{Q}^{\top}
\end{align}
$$

where we have used the square root element-wise on $$\boldsymbol{\Lambda}$$. 

And there it is, _a_ square root of the covariance matrix

$$
\begin{align}
     \boldsymbol{B}^{-1} := \boldsymbol{Q} \boldsymbol{\Lambda}^{^1\!/\!_2}  \quad \boldsymbol{B}^{-1} \boldsymbol{B}^{-\top} = \boldsymbol{\Sigma}
\end{align}
$$

Returning to apples, let's look at the data transformed as $$\mathbf{X}_{\mathrm{c}} \mapsto \mathbf{X}_{\mathrm{c}} \mathbf{B}^{\top} $$

<p align="center">
    <img src="/figures/whitening/w_can_2d.svg"/>
</p>


On first sight, this may appear now with less structure than it had before but this is exactly the point. On the search for features that are decorrelated,
$$\sigma_{ij} = \delta_{ij}$$, we do not want any patterns between variables. I've put 'diameter' and 'weight' in quotation marks to emphasize,
that while we have identified the features that way before, the features now cannot be honestly labeled the same way.
We have achieved the goal of making
the scale at which individual feature dimensions are measured irrelevant. This, together with disentangling the feature dimensions from each other,
manifests in the covariance matrix being the identity matrix. This procedure, mapping the covariance to the identity, is called
*Whitening*, *Sphering* or sometimes the *Mahalanobis* transformation[^5]. 
That we have transformed the centered data $$\mathbf{X}_{\mathrm{c}}$$ is not essential. Also the uncentered data $$\mathbf{X}$$ would have the identity
as its covariance matrix after the whitening transformation, remaining uncentered though.

What shall be the main point is that this whitening transformation is *not* unique.

The covariance matrix in terms of the centered data matrix product is only defined up
to a rotation $$\mathbf{O}$$, i.e. 

$$ \mathbf{X}_{\mathrm{c}}^\top \mathbf{X}_{\mathrm{c}} = \mathbf{X}_{\mathrm{c}}^\top \mathbf{O}^\top \mathbf{O}\mathbf{X}_{\mathrm{c}} \quad \text{where} \quad \mathbf{O}^\top \mathbf{O} = \mathbb{I} .$$

This sounds reasonable, as a rotation of the data should not alter its covariance. 
Hereby, we also understand why the whitening transformation is called
_sphering_. Any rotation of the sphered data will be sphered again just like any rotation leaves a sphere invariant.

We can also show this rotational degree of freedom in the whitening transformation's defining equation

$$ \boldsymbol{\Sigma} = \mathbf{B}^{-1} \mathbf{B}^{-\top} = \mathbf{B}^{-1} \mathbf{O}^\top \mathbf{O} \mathbf{B}^{-\top}. $$

Any rotated whitening transformation is still a whitening transformation and specific whitening transformations 
are specified as a choice of orthogonal transformation $$\mathbf{O}$$ in 

$$ \mathbf{B}' := \mathbf{O} \boldsymbol{\Lambda}^{-^1\!/\!_2} \mathbf{Q}^\top .$$

Therefore, in our first definition of the whitening transformation we have simply chosen $$\mathbf{O} = \mathbb{I}$$. 

If all possible $$\mathbf{B}'$$ lead to whitened data, we may ask if it matters which choice of $$\mathbf{O}$$ we make. 
One simple choice, $$\mathbf{O} = \mathbb{I}$$ we have already taken. Another one also comes natural since $$\mathbf{Q}$$,
the matrix of $$\boldsymbol{\Sigma}$$'s eigenvectors, is orthogonal.
Aiming for symmetry of the whitening transformation, we choose $$\mathbf{O} = \mathbf{Q}^\top$$ and define
the *symmetric* whitening transformation

$$ \boldsymbol{B}_{\mathrm{S}} := \boldsymbol{Q} \boldsymbol{\Lambda}^{-^1\!/\!_2} \boldsymbol{Q}^{\top} = \boldsymbol{B}_{\mathrm{S}}^\top. $$

There is plenty to say about the differences between these two choices which will be the topic for another
time[^6]. For now, we want to visualize the differences returning to the 2D apple data.
For that, we plot the data whitened as previously, $$\mathbf{O} = \mathbb{I}$$, to the symmetrically
whitened one where $$\mathbf{O} = \mathbf{Q}^\top$$. 

![whitened-comparison-2D-data](/figures/whitening/w_comparison_2d.svg)

Sadly, this comparison does not seem to yield any exciting insights. In fact, the right-hand data
is, as expected, only a rotation of the left-hand data.

We can to better visualize the differences, color the data points by their position, i.e.
by their signed distance to the origin.

Coloring 'more positive' with brighter and 'more negative' with darker colors, the standardized data then looks like 

<p align="center">
    <img src="/figures/whitening/s_colored_2d.svg"/>
</p>

We then apply the whitening transformations maintaining this color coding. 

![whitened-colored-comparison-2D-data](/figures/whitening/w_colored_comparison_2d.svg)

Whereas the left side picture seems to be rotated randomly, the symmetric whitening transformation preserves 
the orientation much better.

Why the orientation is not maintained perfectly, what this whole procedure has to do with PCA and
how we can relate it to basis orthogonalization, we'll cover in the next parts of this series.



---

#### Footnotes

[^1]:  This is especially true for methods that depend explicitly on the euclidean distance between data, like kNN. Furthermore and empirically, gradient descent optimizations are faster and more accurate when working with data that is roughly normally distributed. There are exceptions, where similar scales across features are rather redundant, e.g., tree based methods that only work with comparisons "inside" one feature dimension. 

[^2]: Interestingly, [apple flesh density does vary and older apples are typically less dense](https://scijournals.onlinelibrary.wiley.com/doi/abs/10.1002/jsfa.2740470406).

[^3]: If this in some way reminds you of PCA, hold on to that thought.

[^4]: One might ask whether the inverse square root of the covariance matrix always exists. Interestingly, the inverse of the covariance matrix itself does not necessarily have to exist. It only does so if there does not exist a constant ($$\mathrm{cov}(\cdot, \cdot) = 0$$) linear combination of features. Let's forget about this degenerate case and assume the covariance matrix to be positive-definite (eigenvalues $$> 0$$), and hence invertible.

[^5]: The Mahalanobis transformation is of course linked to the [Mahalanobis *distance*](https://en.wikipedia.org/wiki/Mahalanobis_distance), where the distance between to points $$\mathbf{x_i}, \mathbf{x_j}$$ is calculated via $$ \sqrt{ \left( \mathbf{x}_i - \mathbf{x}_j \right)^\top \boldsymbol{\Sigma}^{-1} \left( \mathbf{x}_i - \mathbf{x}_j \right) }. $$ This is equivalent to first applying a whitening transformation and then taking the euclidean distance.

[^6]: A great and thorough explanation of different whitening transformations can already be found in [Kessy et al., 2015](https://www.tandfonline.com/doi/full/10.1080/00031305.2016.1277159).
