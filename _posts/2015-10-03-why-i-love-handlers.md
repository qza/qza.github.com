---
layout: post
title: "Why I love handlers"
description: ""
category: [technical]
tags: [software design]
---
{% include JB/setup %}

Naming in software development is well known thing. We give names to the classes by combining roles, business and tech concepts. Classes that play roles of Handler or Processor are pretty common in software architecture. Here is why I find them useful.

First of all they are kind a simple. It's intuitively that they handle something and focus on that something is among the important things to keep in mind while developing handler class. 

They usually execute some concrete step or action, some business case like: “service-provider-name–service-action-name”. This contributes to the understanding of business that makes the use of the application. Their implementation can be delegated to team making the initial idea of the class clear and simple.

From the technical view, they are also perfect for delegation. We can place delegate or router before them that will use predefined knowledge about which classes can handle which cases. In that way we can manage the cyclomatic complexity of the code and increase it's modularity. They are easily associated with something concrete and we can organize common flows for them in form of template method and reduce duplications. That behaviour can be illustrated with following sketch:

<p class="img-box-center">
<img src="http://www.websequencediagrams.com/cgi-bin/cdraw?lz=dGl0bGUgUm91dGUgdG8gaGFuZGxlcgoKCkNsaWVudC0-ABYFcjoAEwcKbm90ZSByaWdodCBvZgAyBnI6IFVzaW5nIHByZWRlZmluZWQgcm91dGVzCgA3Bi0-SABRBgA9CQAJBwAQCVRlbXBsYXRlOiBwcmUAJwYAExEAGSRvc3QANwcKCg&s=napkin" alt="There should be diagram right here"/>
</p>
 
As illustrated on diagram, client event is passed to the router. Router correlates each event with dedicated handler class in form of routes. When event is received, router selects handler and delegates to the handler class. HandlerTemplate makes sure that right set of actions is executed before and after event is passed to the handler class. The only thing that is left is for handler class to perform it's function.  

 