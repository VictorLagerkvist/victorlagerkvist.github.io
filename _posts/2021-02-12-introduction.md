---
layout: distill
title: Introduction to complexity theory
date: 2021-02-12 
description: A crash course in complexity

authors:
  - name: Victor Lagerkvist
    affiliations:
      name: TCSLAB, Linköping university
---

Deciding the topic of the first proper post turned out to be rather challenging. In the end I decided to give a crash course in complexity theory, since it is the subfield of theoretical computer science which underpins essentially all my research. The overall aim is actually rather easy to state. Our basic building blocks are computational problems, which are idealisations of "real-world problems" with a clearly specified input and output. For example, perhaps we want to multiply two numbers, or find the shortest path on a map. We then wish to characterise computational problems according to their complexity, which in practice means that we want to describe how easy or hard the problem is to solve, typically with the aim of either devising a method which is fast enough to solve large instances of the problem, or by providing evidence that such a method cannot exist (to the best of our knowledge). Let us now consider both these facets in greater detail.

### Computational problems

Before we delve deeper we need to define the concept of a "computational problem". Let us begin by giving a few examples before we attempt a general definition. First, consider the problem of sorting a list of numbers in ascending size, which is among the most common problem one faces when learning programming for the first time. Thus, we are given a sequence of numbers and want to order this sequence such that smaller numbers come before larger numbers. This problem can be formally defined as follows.

**Sorting**

Instance: A sequence $$x_1, \ldots, x_n$$ of natural numbers.

Goal: Find a permutation $$\pi : \{1, \ldots, n\} \rightarrow \{1, \ldots, n\}$$ such that $$x_{\pi(i)} \leq x_{\pi(i+1)}$$ for each $$i \in \{1, \ldots, n - 1\}$$.


This is a typical example of how one defines a computational problem in theoretical computer science. First, we specify exactly what constitutes an instance of the problem. In this case we e.g. decided to restrict the problem to natural numbers, since the word "number" is too ambiguous to be used when defining a problem.
Second, we specify the objective, the goal, of the problem. Note that the definition of the problem says absolutely nothing about how one should proceed to construct an algorithm for the problem; it merely states *what* should be computed, not *how* it should be computed. Clearly, there exist many different algorithms that produce the same answer, even if they may differ in complexity. For example, *insertion sort* can be used to solve the sorting problem, requiring roughly $$n^2$$ operations in the worst case. Other sorting algorithms, such as *merge sort*, require roughly $$n \cdot \log(n)$$ operations in the worst case.


The problem of sorting a sequence of numbers is an example of a *function problem* since we are asked to compute a concrete permutation. Let us consider another example of a function problem. In the *Travelling Salesman Problem* (TSP) one is given $$n$$ numbers $$1, \ldots, n$$, representing *cities*, and for each pair $$i,j$$ of cities, a natural number $$d_{i,j}$$ which represents the distance between the two cities $$i$$ and $$j$$. The objective is then to find a tour of all cities such that the total distance is minimised.

**The travelling salesman problem** (TSP)

Instance: A sequence $$1, \ldots, n$$ of natural numbers and a natural number, a *distance*, $$d_{i,j}$$ between every pair $$i,j$$ in this sequence.

Goal: Find a permutation $$\pi : \{1, \ldots, n\} \rightarrow \{1, \ldots, n\}$$ such that $$\Sigma^n_{i = 1} d_{\pi(i), \pi(i+1)}$$ is minimal.


In this problem definition we tacitly assume that $$\pi(n+1) = \pi(1)$$. What is the complexity of the TSP problem? First observe that we can solve this problem in a brute force way by simply calculating all possible permutations over $$n$$ and take the one that minimises the sum of the distances. Such an algorithm would run in roughly $$1 \cdot 2 \cdot \ldots \cdot (n-1) \cdot n = n!$$ time, which is clearly infeasible even for quite moderate values of $$n$$. Can we, similarly to the problem of sorting a sequence of numbers, construct a nice algorithm that solves the TSP problem in $$n^k$$ time for some reasonably small value of $$k$$? If this is not possible, can we at least find some $$k \geq 1$$ such that we can solve the problem in $$n^k$$ time, i.e., in polynomial time? Currently, no such algorithm is known, and it is widely believed that such algorithms are impossible to achieve. This phenomenon will be described in greater detail in future posts.

Sometimes, one is contempt to know whether there exists a solution or not. Such problems are called *decision problems* and will be the main focus of this and forthcoming posts. Note that decision problems can be seen as function problems where the output is always "yes" or "no". We could for example recast the *TSP* problem as a decision problem by instead asking if there exists a tour which is smaller than or equal to a given value.

**The decision version of TSP** (D-TSP)

Instance: A natural number $$k$$, a sequence $$1, \ldots, n$$ of natural numbers and a natural number, a *distance*, $$d_{i,j}$$ between every pair $$i,j$$ in this sequence.

Question: Does there exist a permutation $$\pi : \{1, \ldots, n\} \rightarrow \{1, \ldots, n\}$$ such that $$\Sigma^n_{i = 1} d_{\pi(i), \pi(i+1)} \leq k$$?


For a more rigorous definition of a computational problem one needs to be a bit more careful. First, fix a set $$\Sigma$$ of symbols (e.g., $$\{0,1\}$$ if we want a binary encoding). A decision problem is then simply a set of words over $$\Sigma$$, representing all possible inputs, together with a description of the yes-instances. This is not so different to how one would implement an instance of a problem in an actual computer, where one would need to fix a programming language and then decide how the various elements should be represented in terms of data structures. However, as long as there is no risk for confusion, a description using a formal language is typically not necessary, and only in rare cases does one need to specify a concrete representation. Consider, for example, the various methods to represent a natural number. The easiest method is to let a string of $$n$$ elements represent the number $$n$$, but if we use a binary encoding we can make do with only $$\mathrm{log}(n)$$ bits.

### Complexity classes and models of computation

We now know what a computational problem is, and the next natural step is to group problems together and attempt to find common denominators between them. Thus, we define a *complexity class* to be a set of problems. We could for example define a complexity class by letting it consist of all decision problems. In the literature this class is sometimes called ALL. Unfortunately, we do not really gain any insight by using the class ALL since it trivially includes all decision problems, and we therefore need to fix some interesting properties which we can use to describe complexity classes. Frequently studied properties are *time* and *space*, which in this context means the number of operations or the amount of memory required to solve a problem. Before we can define the complexity classes that will play a central role, we need to make a detour and explain our underlying model of computation.

Computing devices are certainly nothing new. Examples from ancient history include the Antikythera mechanism used for predicting astronomical observations, and abacuses, counting frames for performing arithmetical operations. These devices were constructed with a few very limited tasks in mind and do not fit the notion of universal computation that we normally associate with modern computers. The design of the first general purpose computer is normally attributed to Charles Babbage, who in 1837 described the so-called *analytical engine*. What made this computer different from the Antikythera mechanism and other primitive computing devices? This question was not answered until almost a hundred years later in Alan Turing's seminal paper *On computable numbers with an application to the Entscheidungsproblem*. What Alan Turing realised was that a seemingly extremely simple computer was sufficient to realise all imaginable forms of computation. A similar, and in fact equivalent, notion of universal computability was discovered independently by Alonzo Church, called the *Lambda calculus*.

For our purposes we see a *Turing machine* as a simple computer, equipped with a *tape*, that can be stretched arbitrarily long (so that we do not run out of space in the middle of a computation), and a *head*. Each cell in the tape can hold a symbol from a predetermined finite language. The program of the Turing machine consists of instructions of the form: if the machine is in a state and reads a symbol in the current cell, then write a new symbol in this cell and either (1) move the head to the left, (2) do not move the head, or (3) move the head to the right. These instructions have to be precise enough so that the Turing machine, for any possible state and symbol, always knows what the next step is. The Turing machine then patiently reads a single cell off the tape as input, writes a symbol on the cell, and moves the head according to its program. The machine halts if it ends up in either of the three special states <b> yes </b>, <b> no </b>, or <b> halt </b>. A Turing machine is said to *decide* a decision problem if, for every instance of the problem which is written on the tape in a suitable encoding, it correctly ends up in the <b> yes </b> state if and only if the instance is a yes-instance.

The reader who is familiar with modern computers but has never heard of Turing machines before might find this concept archaic. Why are we speaking of tapes and not memory, and how can we say anything about the time required to solve a problem using such a primitive and limited machine? The answer is that we wish to measure time with respect to the number of basic operations performed, not the time required to solve the problem on a specific computer, and this figure is essentially unchanged no matter if the problem is solved on a lowly Turing machine or a supercomputer. Furthermore, all rigorous models of computation, including the aforementioned Lambda calculus, have turned out to be equivalent in the sense that they can all "simulate" each other. This roughly means that if you come up with a reasonable alternative to Turing machines it is always possible to construct a Turing machine which accomplishes the same job (but potentially taking longer time). The phenomenon that Turing machines can compute anything that is reasonable computable is sometimes called the *Church-Turing thesis*.

### Summary

We have defined computational problems and seen how Turing machines can be used to give a formal semantics of computation. The next natural step is to define complexity classes via Turing machines and see how various complexity classes can be related to each other.

### Further reading

Parts of this post are based on the introductory chapter of [my PhD thesis](http://dx.doi.org/10.3384/diss.diva-122827){:target="\_blank"}. For further details on this topic and many others, I strongly recommend the textbook *Computational Complexity* by Christos Papadimitriou.