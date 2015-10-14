---
layout: post
title: "Agile with BDD"
description: ""
category: [technical]
tags: [agile, bdd, gherkin]
---
{% include JB/setup %}

Behavior Driven Development, or simple BDD, is modern approach in applying knowledge about software behavior. BDD bridges the communication gap between different stakeholders gathered arround software requirements. This is achieved using language form that is understandable by the business and in the same moment detail enough to be considered a specification (known as: [Business Readable DSL](http://martinfowler.com/bliki/BusinessReadableDSL.html))

In agile approaches, business requirements are expressed in form of epics and user stories. Together with user stories, some sort of acceptance criteria is defined that will be used later on to assure proper system behavior. Agile approach that applies BDD places scenarios as acceptance criteria. Scenarios are written in DSL and come as the first refinement of user stories that includes the technical aspect of the requirement and in the same moment hides technical details. With that level of abstraction, developers are still able to implement actual steps and make them executable, meaning that we actually have executable, "human-readable" specification.

To demonstrate this on real-world case and check how it all looks, let's increase business value of our cool ticket service. Ticket service should have option that will allow customers to cancel ticket and refund money:

{% highlight gherkin %}

  As a Customer, I want to be able to cancel ticket, so that I can refund money
	
{% endhighlight %}

Business has decided to allow customers to cancel their tickets. Amount of money for refund is dependent on cancelation policy and status of the customer. To be more precise, we can define expected behavior with the following scenarios:

{% highlight gherkin %}

Feature: Ticket cancelation

Scenario: Cancelation for regular customers withing cancelation period

    Given regular customer has bought a ticket

     When customer makes request to cancel the ticket
      And request is within the acceptable cancelation period defined by the policy

     Then payment transaction should be "canceled"
      And new transaction should be "created" with amount defined by the policy
      And ticket should be "canceled"
      And customer should be "notified" about the cancelation


Scenario: Cancelation for regular customers after cancelation period

    Given regular customer has bought a ticket

     When customer makes request to cancel the ticket
      And cancelation period defied by the policy has expired
	
     Then customer should be "notified" about expired period
      And ticket should be "active"

{% endhighlight %}

Similar, we could define scenarios for the premium customers.

BDD scenarios can be our complete acceptance criteria. As they are written in special DSL, we can make them executable and include in continuous intergation process. Gherkin is the language specially designed for expressing behavior. Gherkin comes with many features that can be used to achieve precise expressions. [Here](https://cucumber.io/docs/reference) is one of the excellent guides.

Having scenarios defined like this can have huge positive impact on project during all stages. Description of the exact business situations can be really helpful for new team members on existing projects. There are known situations when business rules and expected behavior can be figured out only from code. In such situations, creating and asserting scenarios with the business can be a big step forward in understanding unknown software systems, existing and new.
