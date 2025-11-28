---
theme: seriph
background: https://cover.sli.dev
title: Four Vertex Theorem
info: |
  Presentation slides for Four Vertex Theorem and its application in CSS.
# https://sli.dev/features/drawing
drawings:
  persist: false
mdc: true
---

# Four Vertex Theorem

and its applications in object recognition.

Interactive slide (recommended): https://second-last.github.io/math1060-fvt

---
hide: true
---

# Volunteers?

We would like two volunteers to come up and we'll take a photo of
them.

The remaining presentation will show how the Four Vertex Theorem
helps us differentiate between an image of Gavin versus an image of
someone else purely based on boundaries.

---
src: ./pages/intro.md
---

---
layout: two-cols-header
layoutClass: gap-x-4
mdc: true
---

# Motivation

How do we distinguish objects?

::left::

![](./images/monitor.webp){width=300px}

::right::

![](./images/mouse.png){width=300px}

::bottom::

Human intuition: <span v-mark.underline.orange>boundaries -> vertices</span> based. A monitor has 4 vertices, while
a mouse has no sharp corners on its boundary.

---

# Challenge

<v-clicks>

Too many vertices!

Plus, a lot of them aren't actually meaningful. Even a tiny sharp
corner is counted as a vertex.

![](./images/panda-edge.png){width=200px}

Goal for simplification:
reduce the number of vertices while preserving the essential
shape.

</v-clicks>

---

# Key Question

What's the minimum structural complexity of a boundary?

Or: how many vertices can we throw away until we can't reliably
recognize anything?

---

# Four Vertex Theorem

The answer was revealed at the start...

<v-clicks>

## Terminology

- **Boundary:** a simple, closed, smooth plane curve $\alpha(t)$.

- **Vertex:** A certain $t$ where $\kappa'(t) = 0$

## Statement

A simple, closed, smooth plane curve has <span v-mark.underline.orange>at least four local extrema:</span>

- At least two local maxima.
- At least two local minima.

(assumes non-constant curvature, i.e. no circles)

</v-clicks>

---

# Intuition on the 4

Start with the simplest curve: a circle. To make the curve more
complex, let's turn it into an ellipse.

<div style="margin-top: 0rem; margin-bottom: -2rem;">
<CircleToEllipse :width="500" :height="300" />
</div>

<v-clicks>

- <span style="color: red">Red</span> and <span style="color: green">
  green</span> are the <span style="color: red">maxima</span> and <span
  style="color: green">minima</span> points.
- Every non-circular closed curve must have at least 4 such extrema!
- Any thing more complex will have more vertices.

</v-clicks>

---

# Proof Idea

<!-- "The essence of the proof may be distilled in a single phrase: -->
<!-- consider the circumscribed circle." - Robert Osserman, 1985 -->

* For the sake of contradiction, assume curve $\alpha$ has only **two vertices**, $p$ and $q$ (a maximum and a minimum for the curvature $k(s)$).
    * The curve is split into two arcs, $\beta$ and $\gamma$, by the points $p$ and $q$.
* A straight line $L$ is drawn through the two assumed vertices, $p$ and $q$.
    * The convexity of the curve implies that each of the arcs ($\beta$ and $\gamma$) must lie on a **definite side** of the line $L$.
* Contradiction: A specific integral (Eq. 5 in the text) involving $k'(s)$ is used to show that if $\alpha$ has only two vertices, $k'(s)$ must change sign twice on at least one of the arcs ($\beta$ or $\gamma$).
    * Two sign changes for $k'(s)$ on an arc, between the maximum and minimum, means there must be **two additional zeros** for $k'(s)$ on that arc.
    * This forces the existence of a **third and a fourth vertex**, contradicting the initial assumption of only two. 

TODO: draw a diagram that shows such $\beta$, $\gamma$, and $L$.

---

# What the 4 doing?

Real object has way more than four vertices!

![](./images/panda-edge.png){width=200px}

Now we know that the minimum we need is four vertices, we need a
systematic way to reduce many vertices down to four.

---

# Curvature Scale Space (CSS)

CSS Representation: multi-scale organization of the invariant
geometric features (either curvature extremas or zero-crossing
points).

![](./images/panda-curvature-graph.png)

The CSS graph is (implicityly) defined by $\kappa(\sigma, u) = 0$,
y-axis $\sigma$ as smoothing level, x-axis $u$ as arc length.

---

# The Full Object Comparison Algorithm

1. Extract contour (i.e. boundary).
2. Compute the CSS graph shown below by smoothing the curve until there are four extremas
   left.
3. Use some algorithm to compare the four extremas. There are many
   creative algorithms you can choose that we won't elaborate on here.

TODO: explain the CSS graph a bit more, since this example in the
paper uses
zero-crossing points and not extremas. Maybe we should create our own
graph.

![](./images/panda-curvature-graph.png){width=500px}

---

# CSS Demo

High level: keep smoothing until you have four vertices left!

\<show, best case sliders with smoothing parameters, worst case GIFs,
of the smoothing of the boundaries of different objects with all
vertices shown on the curves, so students can see in realtime how
smoothing removes extremas>

![](./images/css-sequence.png)

---

# How FVT helps CSS

FVT guarantees the fundamental structure that we'll eventually
approach 4 extremas before becoming circular.

---

# To Sum Up

- The Four Vertex Theorem tells us that shapes fundamentally have at
  least 4 curvature extrema.

- This is an intrinsic property of the shape.

- While FVT itself isn't used in the recognition algorithm, it
  motivates why curvature-based methods like CSS are effective at
  using a small number of data points to describe complex curves.

---

1. (5 mins) Core of (global) differential geometry: mapping from local to global.
    - Guass-Bonnet started **global** differential geometry; **global** diff geo studies how local <-> global.
     - But Guass-Bonnet is for surfaces. Go through some theorems we learned about _curves_ and note that none of them establish local <-> global relationship.
     - What's the first theorem in diff geo about _curves_ that's describing global results from purely local observations? FVT!
     - It's so old it's **almost** algorithmically useless, more on this later.
2. (10 mins) Book proof: FVT convex case.
3. (5 mins) Not _exactly_ useless: connects curvature extremas to shape properties. Intuitively, by knowing where and what are the curvature extremas, we can get some guarantees no what the shape would look like.
    - FVT is a simple guarantee, but it does provide the motivation to study curvature extremas since extremas are always available.
    - Further studies show that curvature extremas do matter.
    - FVT tells us any simple, ..., curve has those extremas, allowing us to apply the extrema results to any such curve.
    - e.g. object recognition, how do you know a computer is a computer? Note that object recognition is also mapping from local observations to global conclusions!


<!--
4. Circle back: how does FVT helps CSS?
    - Not alogrithmically useful.
    - FVT provides a lower bound and removes the trivial case of having no extremas.
-->

<!-- What is differential geometry? How do we map from local to global. Most of the stuff we have learned _prior to Gauss Bonnet_ are focusing on local stuff. List them. -->

<!--
1. Motivation: CSS representation, uses curvature extremas to compare boundaries under rigid motion and noise.
     - It takes curvature extremas, computationally heavy and noisy.
     - We want to reduce the number of extremas we need. What's the minimum?
     - Reuse our current introduction.
-->

<!--
1. Motivation: CSS representation, uses curvature extremas to compare boundaries under rigid motion and noise.
     - It takes curvature extremas, computationally heavy and noisy.
     - We want to reduce the number of extremas we need. What's the minimum?
     - Reuse our current introduction.
-->

---

Curvature extremas are useful!

- Mokhtarian et al. use the maxima of absolute curvature on the CSS image to detect and track corners
- Garcia & de Luna using extrema set for ship-silhouette recognition achieves SOTA stability when part of the outline is noisy.
- Our CSS paper!
- More...

Because of FVT, we know we can use these results on any simple, ..., curve!

FVT provides motivation to study curvature extremas for CV.

---
src: ./pages/outro.md
---
