---
layout: post
title: "Master of disaster"
description: ""
category: [development] 
tags: [tdd, dojo]
---
{% include JB/setup %}

One of the recent coding dojos was really an interesting experience. I personally call it 'Master of disaster”, coding game in which two developers write single feature in TDD style without talking. Developer A should write test and related code so that code compiles and test fails. Developer B should write code that makes the test green. On this move, developer can write code that works and make it smell bad with attention. The Developer A then performs refactoring of the code to make it fine. The roles are switched and cycle is repeated for predefined number of iterations, or until required functionality is done.

Many observations can be done in this type of challenge. Some developers intuitively start talking, guiding each other with explanations. In such cases, developers are guided to provide all the guidance by the code itself. In some occasions, implementation was completely different than the one imagined by developer that wrote the initial test.  Things are getting interesting after several iterations when there are additional functions. As each developer writes just enough code to make test green, code parts that are written for later can be misused by the other developer. This is specially exposed in defining mutable instance variables shared for manipulation. 

This coding dojo can be interesting when participants have different areas of expertise. Different coding ideas emerge together with personal coding styles. Practices like this can bring closer different mindsets. Big impression here is a difference, at least part that is considerable, or should be, so that there is no one.