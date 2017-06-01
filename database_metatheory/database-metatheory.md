# Database Theory

# Table of Contents
1. [Introduction](#introduction)
2. [Theory and its Function](#theory-and-its-function)
3. [On Negative Results](#on-negative-results)
4. [What is "Good Theory"?](#what-is-\"Good Theory\"?)
5. [On Paradigms and Revolutions](#on-paradigms-and-revolutions)
6. [A PODS Retrospective](#a-pods-retrospective)
7. [A Brief History of Practice](#a-brief-history-of-practice)
8. [On Applicability](#on-applicability)
9. [Theory in the Time of Crisis](#theory-in-the-time-of-crisis)

The paper poses a series of questions of Database theory which presumably are addressed in the paper.

## Introduction

Makes point that this paper hopes to address what Database theory is.

## Theory and its Function

> Virtually all computer research, including experimental research, is "theroetical" in this weak sense.
This is even more true in database research, because abstraction is the essense and raison d^etre of databases.

Raison D^etre means `the most important reason or purpose for someone or something's existence.`

> Theory is needed to cure the complexity that plagues the artifact being studied or, more typically, designed.

Key point which I think points to databases try to address the fact that databases try to model a particular environment.

Theoreticians attack complexity in several ways:

* They develop mathematical models of the artifact.
* They propose complexity-reducing solutions such as algorithims and representational schemes
* They analyze the mathematical models to predict the outcome of the experiments
  * Interesting note that the need for normalization was seen but that the difficulty of optimizing queries was not obvious at first
* They explore
  * Makes note that taste and basically opinion plays factor here

  3 important reasons defending exploration:

  1. It has benefitted computer science.
  2. It promotes connection and healthy outlook to computer science.
  3. Exploration is natural and attractive to people.

  3 criticisms of exploration:

  1. Can disorient and lead to crisis if not balanced with model building and analysis.
  2. Needs to be balanced with practice.
  3. Requires discipline and honesty so that research doesn't make unsubstantiated claims.

## On Negative Results

> whether the result advances the complexity-reducing program of theoretical computer science in the specific application, or points out a setback in this regard.

Interesting statement made that I think points to abstractions being both a benefit and a possible downfall

Positive results of research should be followed up with experimentation.

The limitations of a model or knowing the boundaries of a subject helps further understanding of a subject.

Negative results can also be a matter of opinion, one man's trash is another man's treasure.

## What is "Good Theory"?

> Science is an essentially anarchic enterprise. There is not idea that is not capable of improving our knowledge. The only principle that does not inhibit progress is anything goes.

Interesting point made that new ideas are usually spawned from an old idea that is dire straits.

Scientific ideas usually invade other areas just like in microbiology a virus might invade a host and spread like wild fire.

Paper makes the argument that you must convince others of the worth of your ideas as a good idea is not in of itself enough to convince others.

Interesting chart from Thomas Kuhn depicting that as science matures and grows into normal science that crisis creates a revolutions which then drives normal science and this occurs in a cycle.

## On Paradigms and Revolutions

Broadly accepted scientific assumptions and theories become a paradigm.

A side effect of this is that scientists defend such paradigms fiercely and when they encounter points of contention these get swept under the table so to speak, thus leading to a crisis.

Crisis drives new competing ideas and from this battle a new successor emerges which becomes the new paradigm.

Thomas Kuhn's theory stem from natural sciences but computer science is a different beast.

Computer science promotes a rapid feedback loop and its nature is predicated on models changing as you study them.
Conversely natural sciences have models that deal with mostly static objects.

Interesting point made that natural sciences have crises that arise from unsolved problems and as such theories become falsifiable,
but computer science has no real life crises in the model like natural sciences and so this question is posed.

**What is the operational analog of falsifiability in computer science?**

Applied Research is described as a directed graph. Most theory is at most a couple of hops from practice.

Interestingly the author says that research becomes stale and practictioners have to model their own environments and come up with their own metrics.
This consequence feeds researchers to do research in the small and conduct meaningful exploration which then helps promote new paradigms.

The idea of database research spawning from computer science crisis arises.

## A PODS Retrospective

A chart is shown looking at the number of PODS (the Symposium on Principles of Database Systems) from 1983 to 1995

5 different Research Areas:

1. Relational Theory
  1. Dependencies, normalization, views, query optimization, universal relation assumption, acyclicity.
2. Logic Databases
3. Access Methods
4. Complex Objects
5. Transaction Processing
  1. Concurrency Controllers and Schedulers, Reliability and Recovery, Distributed Concurrency Control and Systems.
  2. Transaction Theory and concurrency specialized to data structures and to transactions with known semantics.

Talk about DataLog taking database research by storm in this time.

*Datalog is a declarative logic programming language that syntactically is a subset of Prolog.*
*It is often used as a query language for deductive databases.*

Discussion about the chart being like an ecosystem of predator and prey behavior.

Pose question whether computer science holds onto traditions for too long but then asserts that are industry is full of fashion and fad.

The influence of normalization and dependency theory is illustrated by the fact that there is more than 20 database design tools.

Concurrency control techniques are mentioned such as two-phase locking and tree-based locking

A major disappointment is emphasized mainly the lack of development of recursive queries.

## A Brief History of Practice

During the time of Aristotle the pursuit of knowledge for its own sake was highly valued over practical application.
The industrial revolution is cited as a happy marriage between theory and application.
During the 80s and 90s theory comes under ideological attack.

Computer science has its roots in mathematics which is theoretical in nature.
During the 1960s with advances in multitasking operating systems then did practical uses of computer science arise.
Starting in the 1980s theory came into critisism.

## On Applicability

The concept of applicability is important according to the paper to stimulate research and can serve as a wakeup call.

Phony Applicability:

* Recursive Applicability: Just because an idea is practically important doesn't mean it is. Its applicability is as important according to author.
* Remote Applicability: Applications of computer science in other fields must walk a fine line.
  * I think the author is talking about really meaningful work not just computer science theory for its own sake.
* Applicability by Association: Author says you must have a practical interest in a paper not just show a semblance of it
* Applicability by Pun:
  * I think the concept here is that the intersection of 2 seemingly important ideas is not necessarily important as ideas in independence.

*Theoreticians must check the validity of their claims and their proofs as well as reviewers.*
*Checking validity of claims and proofs will make ts
## Theory in the Time of Crisis

3 Applicability pressure issues:

1. The social milieu.
2. The artifact
3. The science

Author makes the point that our field is stil young as compared to fields such as economics and physics but I am not sure what he is referring to.

Interesting note made by author "that the easy observations have all been made", hinting at a crisis here.

Author suggest researchers must get radical in their research and challenge beliefs and trends and try to their own experimentation during a crisis.s

Author makes point is that crisis lead to revolution and innovation.
