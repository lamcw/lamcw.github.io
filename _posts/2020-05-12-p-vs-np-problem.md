---
layout: post
title: "ELI5: The P vs NP Problem"
categories:
- ELI5
mathjax: true
---

> _If a problem's correctness can be verified in polynomial time, does that mean the problem can be solved in polynomial time?_

If you ask me to guess a password of \\(8\\) English characters, it would take me at most \\(26^8\\) tries to get the correct answer. But it only takes you one try to verify if my answer is correct: it is either right or wrong.

# P vs NP

In computer science, problems are classified. There are problems that are easy to _solve_ for a computer, and there are problems in which the solutions can easily be _checked_ by computers. And they are denoted as \\(\mathbf{P}\\) and \\(\mathbf{NP}\\) respectively[^1], both are collections of different kinds of problems.

If \\(\mathbf{P} = \mathbf{NP}\\) (meaning both sets of problems are actually the same), solving a problem is as easy as verifying its solution. I can guess your password probably as fast as you checking if my answer is correct.

If \\(\mathbf{P} \neq \mathbf{NP}\\), it takes significantly more time to compute the solution of a problem than verifying it. It would take me days or even months to guess your password correctly. :scream:

Duh, isn't it easy to see that \\(\mathbf{P} \neq \\mathbf{NP}\\) then? It may be intuitive to come to this conclusion. But the reality is, no one is able to prove this rigorously yet.

# The Implication

Let's think about what is going to happen when a seemingly "hard" problem is as easy to solve as verifying its solution (if \\(\mathbf{P} = \mathbf{NP}\\)): I am going to be able to guess all of your passwords on all platforms (be it Facebook, Instagram, YouTube, ...), and so is anyone in this world. When cryptography does not work any more, there is no safe and private way for us to communicate with each other. Sooner or later someone will figure out a way to solve the [travelling salesman problem](https://en.wikipedia.org/wiki/Travelling_salesman_problem), companies will be able to make use of this to optimize their work flows (for e.g. logistics, surveying, etc.). The world will be changed.

If you happen to be a person who can provide a correct proof (whether \\(\mathbf{P} = \mathbf{NP}\\), or not), congratulations! You win a US$1,000,000 prize because it is one of the seven Millennium Prize Problems selected by the [Clay Mathematics Institute (CMI)](http://www.claymath.org/).


[^1]: For readers with CS background, \\(\mathbf{P}\\) are problems (or languages, in the context of Turing Machines) solvable in polynomial time, i.e., \\(\mathbf{P} = \bigcup_{k \in \mathbb{N}} \mathbf{TIME}(n^k)\\). \\(\mathbf{NP}\\) is the class of problems that have polynomial time verifiers. Define the _time complexity class_, \\(\mathbf{TIME}(t(n))\\) to be the collection of all languages that are decidable by an \\(O(t(n))\\) time Turing Machine, where \\(t : \mathbb{N} \longrightarrow \mathbb{R}_{\geq 0}\\).
