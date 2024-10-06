---
layout: post
title: 'Symmetry'
date: 2024-05-09 22:32:26 +0100
categories: jekyll update
---

# Geometries

There are a few geometries roaming around. For me it is hard to comprehend, that one is not enough.
Specifically, what do we even mean by "geometry"?. I'd say, that geometry gives distances and angles
meaning. I mean, they do not seem arbitrary do they. Lifting something up is the hardest when levers
are at a right angle. That is, lifting a bottle of water in your hand becomes progressively tougher
when moving (elbow kept straight) upwards until the point where our arm is perpendicular to the
gravitational force, i.e. parallel to the floor.
Now thinking about it, it seems indeed a bit arbitrary to call this perpendicular, or rather to
identify the right angle with 90 degrees or $\pi/2$. It could have just been some other number,
we would not have to pick 360 degrees to be full circle (The number of $\pi$ of course has a more
rigid interpretation of course, as the area of the unit circle).

This aside, how do geometries arise? We mostly think of your Euclidean Geometry as being the
most natural one, but what even defines the Euclidean Geometry. It turns out, there is a number of
postulates being fulfilled by such a geometry. Parallel lines remaining parallel and so on (though
the parallel one is probably superfluous).

How does one get to a hyperbolic geometry? In the _conformal_ picture (formally, _representation_)
of hyperbolic geometry, angles coincede with euclidean ones whereas the notion of _straight lines_
do not. Indeed, in this picture, straight lines are line segments of a circle. The circle to which
the line segments belong itself meets another _bounding circle_ at both ends at an right angle.
There it is again, the right angle. The hyperblic space is all the space inside this bounding
circle.
Distances here can be related to the Euclidean distances by the (natural) logarithm. Let two
points $A, B$ be on a single such line segment, i.e. straight line with respect to the conformal
representation. This segment meets the bounding circle at the points $Q$ and $P$. The distance
between the points $A$ and $B$, i.e. the distance of the line $AB$, expressed by euclidean
distances amounts to

$$
AB_{\text{conformal}} = \log \frac{QA \, \cdot \, PB}{QB \, \cdot \, PA}

Another possible description is that of the *projective* representation. Then, straight lines
of euclidean and hyperbolic geometry coincede, whereas angles differ. Here, distances become
infinitely large, when one gets close to the bounding circle. The projective points $A, B$ are
found by *projecting* the conformal points out from the center of the bounding circle by
a distance related to the bounding circle radius $R$ and the distance $r_c$ out of the bounding
circle center to the conformal points, $\frac{R^2}{R^2 + r_c^2}$
$$
