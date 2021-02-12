---
layout: page
permalink: /research/
title: Research
description: Some notes on my research.
nav: true
---

My main research interest is the so-called algebraic approach for analysing computational complexity. In short, many computational problems can be modelled as problems where the goal is to assign values to variables, subjected to certain constraints (*constraint satisfaction problems*). In the worst case, one might have to try all possible combinations of domain values to the variables in the instance, which yields an *exponential* running time (bad!). But it is known that the set of solutions to a constraint satisfaction problem can be described by certain symmetries, called *polymorphisms*, which makes it possible to reason about the solution space in a non-trivial way. In fact, it is even known that for finite domains, the presence of any non-trivial method to combine solutions, is sufficient for the existence of a *polynomial-time* algorithm (good!).

However, classical complexity fails to adequately adress why there is such a large difference in complexity even among problems with an expected superpolynomial running time. For example, why does there exist small instances containing only a handful of variables, which makes even the most sophisticated solvers crawl to a halt, when the same solvers manage to handle real-world instances with millions of variables?

To answer such questions the normal algebraic approach is not applicable, since the presence of a non-trivial polymorphism would result in a polynomial time solvable problem, and one alternative is to instead look at weaker symmetries than polymorphisms, so-called *partial polymorphisms*. Intuitively, while a partial polymorphism does not give a complete recipe for how solutions can be combined to solve a problem efficiently, even a partial description might be sufficient to avoid the need of trying all possible combinations. With the help of this approach I have for example studied the borderline between tractability and intracatbility, concentrating on hard problems suspected to be very close to this invisible barrier, which led to the identification of the *easiest NP-complete satisfiability problem*. More recently, this approach has also been applied to study kernelization properties of constraint satisfaction problems, and we have shown that the existence of certain types of partial polymorphisms are sufficient to obtain significantly faster, although still exponential, algorithms.


### Explain like I'm five summary ###
There are good and bad methods to solve problems. Some problems appear to be really hard to solve, and only bad methods are known. With the help of algebra I try to make bad methods slightly better. Essentially I'm trying to draw a picture which explains how the hard problems are related to each other.