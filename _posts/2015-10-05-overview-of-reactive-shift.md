---
layout: post
title: "Overview of Reactive shift"
description: ""
category: [technical]
tags: [design]
---
{% include JB/setup %}

Software development produced many paradigms, approaches and styles for constructing and expressing elements of the code. Paradigm can be described as global way of thinking and approach as way of acting in some domain. Sequential, concurrent, parallel, imperative, declarative, functional, reactive and many other ways. Lets name it a way and take an overview of principles and practical implications that they carry in everyday work.

Sequential programming is a way of structuring and processing the code. It is built around concepts like mutual state and sequence with effects. Textual order of statements defines order of execution and they have to be executed in successive order. This is not applicable for concurrent programs which can produce different order of instructions when two operations are concurrent. Concurrency is there to manage non-deterministic situations like user events. Parallelism is concerned with parts that have deterministic behavior. Parallel programming beside concurrent execution includes communication between the sub-processes and exchange of signals during execution. Parallel programming exposes the demand for other way then sequential, as mutable state and different ordering of instructions are solid introductions to horror of race conditions.

Imperative way is closely related to Sequential. Imperative is kinda way of expressing the code, telling the computer what to do. In imperative programming commands are expressed in form of actions that change the state of object. Daily work on business logic is usually done in imperative way as process specific concepts and actions lacks abstractions of higher level. Declarative programming is contrast to imperative as it defines computational logic without specification of concrete operation flow other that nested functional order. Declared functions are pure, depending only on supplied arguments and have no side effects by producing only single observable output which is value returned by the function. Examples of declarative way can be found in Excel spreadsheets, regular expressions and SQL.

Functional programming is related to declarative in domain of pure functions. It threats functions as first class objects and use them as parameters for further processing. Functional programming in some way extends declarative programming by achieving separations of concerns with sub-typing and functional compositions. Functional code is stateless and immutable. Ignorance of the past enables isolated executions of functions and this characteristics is suitable for parallel processing.

Reactive paradigm gives central place to data flows and propagation of the change. Applied approach is on higher level. Such compositions are part of event driven architectures and involve principles of functional programming. Reactive applications are usually based on messaging, they are responsive, resilient and elastic. In direct relation with such application goes the Actor model. Actors are components that operate on isolated state with encapsulated routines, send and receive messages from other actors.

There are other approaches and ways to express semantic of the code. Experiences and new characteristics of environment are trend setters. Application languages follow these trends and introduce elements taken from other approaches. This is huge topic and given overview should at least help catching up with basics in this domain.