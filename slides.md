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

---
display: false
---

# Volunteers?

We would like two volunteers to come up and we'll take a photo of
them.

The remaining presentation will show how the Four Vertex Theorem
helps us differentiate between an image of Gavin versus an image of
someone else purely based on boundaries.

---
layout: two-cols-header
layoutClass: gap-x-4
mdc: true
---

# Motivation

How do we compare objects?

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

- **Vertex:** A certain $t$ where the curvature at this $t$ is a local
  extrema.

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

---

# What the 4 doing?

Real object has way more than four vertices!

![](./images/panda-edge.png){width=200px}

Now we know that the minimum we need is four vertices, we need a
systematic way to reduce many vertices down to four.

---

# Curvature Scale Space (CSS)

High level: keep smoothing until you have four vertices left!

![](./images/panda-curvature-graph.png)

Precisely: CSS tracks 

---

# CSS Demo

\<show, best case sliders with smoothing parameters, worst case GIFs,
of the smoothing of the boundaries of different objects with all
vertices shown on the curves, so students can see in realtime how
smoothing removes extremas>

![](./images/css-sequence.png)

---

# How FVT is involved

FVT guarantees the fundamental structure that we'll eventually
approach 4 extremas before becoming circular.

\<an animation, ideally a slider for smoothness,
worst case a gif, showing how
the minor extremas
merge and major extremas persist as we smooth the curve>

The four extremas that survive at the end are the persistent features
of the shape and become the "signature" of the shape.

---

# The Full Object Comparison Algorithm

1. Extract contour (i.e. boundary).
2. Compute CSS by smoothing the curve until there are four extremas
   left.
3. Use some algorithm to compare the four extremas. There are many
   creative algorithms you can choose that we won't elaborate on here.

---

# Tieing up

- The Four Vertex Theorem tells us that shapes fundamentally have at
  least 4 curvature extrema.

- This is an intrinsic property of the shape.

- While FVT itself isn't used in the recognition algorithm, it
  motivates why curvature-based methods like CSS are effective.

---


