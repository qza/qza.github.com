---
layout: post
title: "Agile with BDD"
description: ""
category: [design]
tags: [design, agile, bdd, gherkin]
---
{% include JB/setup %}

Behavior Driven Development, or simple BDD, is modern approach in applying knowledge about software behavior. BDD bridges the communication gap between different stakeholders gathered arround software requirements. This is achieved using language form that is understandable by the business and in the same moment detail enough to be considered a specification (known as: [Business Readable DSL](http://martinfowler.com/bliki/BusinessReadableDSL.html))

In agile approaches, business requirements are expressed in form of epics and user stories. Together with user stories, some sort of acceptance criteria is defined that will be used later on to assure proper system behavior. Agile approach that applies BDD places scenarios as acceptance criteria. Scenarios are written in DSL and come as the first refinement of user stories that includes the technical aspect of the requirement and in the same moment hides technical details. With that level of abstraction, developers are still able to implement actual steps and make them executable, meaning that we actually have executable, "human-readable" specification.

To demonstrate this on real-world case and check how it all looks, let's increase business value of our cool ticket service. Ticket service should have option that will allow customers to purchase ticket:

{% highlight gherkin %}

  As a Customer, I want to be able to purchase ticket, so that I can travel to my destination
	
{% endhighlight %}

Business has surprisingly decided to allow customers to purchase tickets. Discount period can be granted depending on the company policy. To be more precise, we can define expected behavior with the following scenarios:

{% highlight gherkin %}

Feature: Ticket purchase

Scenario: Ticket purchase with discount for regular customers

    Given tickets can be purchased

     When regular customer makes request to purchase the ticket
      And request is within the acceptable discount period defined by the policy

     Then price of the ticket should be reduced for amount defined by the policy
      And payment transaction should be initiated with payment provider
      And customer should be "notified" about the purchase


Scenario: Ticket purchase without discount for regular customers

    Given tickets can be purchased

     When regular customer makes request to purchase the ticket
      And discount period defined by the policy has expired
	
     Then ticket should have regular price
      And payment transaction should be initiated with payment provider
      And customer should be "notified" about the purchase

{% endhighlight %}

Similar, we could define scenarios for the premium customers.

BDD scenarios can be our complete acceptance criteria. As they are written in special DSL, we can make them executable and include in continuous intergation process. Gherkin is the language specially designed for expressing behavior. Gherkin comes with many features that can be used to achieve precise expressions. [Here](https://cucumber.io/docs/reference) is one of the excellent guides.

Having scenarios defined like this can have huge positive impact on project during all stages. Description of the exact business situations can be really helpful for new team members on existing projects. There are known situations when business rules and expected behavior can be figured out only from code. In such situations, creating and asserting scenarios with the business can be a big step forward in understanding unknown software systems, existing and new.
