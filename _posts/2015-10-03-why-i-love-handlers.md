---
layout: post
title: "Why I love handlers"
description: ""
category: [technical]
tags: [design, patterns]
---
{% include JB/setup %}

Naming in software development is well known thing. We give names to the classes by combining roles, business and tech concepts. Classes that play roles of “Handler” are pretty common in software architecture. Here is why I love them so much.

First of all they are kind a simple. It's intuitively that they handle something and focus on that something is among the important things to keep in mind while developing handler class. 

They usually execute some concrete step or action, some business case like: “payment provider – payment service name”. This contributes to the understanding of business that makes the use of the application. Their implementation can be delegated to team making the initial idea of the class clear and simple.

From the technical view, they are also perfect for delegation. We can place delegate or router before them that will use predefined knowledge about which classes can handle which cases. In that way we can manage the cyclomatic complexity of the code and increase it's modularity. They are easily associated with something concrete and we can organize common flows for them in form of template method and reduce duplications. That part in architecture could be illustrated with following sketch: 

In such design, handler classes are valuable pieces of software with real business value. They can be right spot for understanding system and business behavior. 