---
layout: post
title: "BDD in Java with JUnit, Spring and Cucumber JVM"
description: ""
category: [tutorial]
tags: [java, spring, junit, cucumber, bdd]
---
{% include JB/setup %}

Cucumber is well known framework for practical BDD. Project Cucumber-JVM brings support for most of the JVM languages and frameworks, including Java 6/7/8, Spring and JUnit. 

This tutorial is example for the user story given in previous post and shows how to organize Cucumber features and run them in standard Maven project setup. Some of the steps will be skipped to keep focus on elements that are required to integrate Cucumber, Spring and JUnit. Exact steps can be seen in [commit history on github](https://github.com/qza/bdd-cucumber-jvm-spring/commits/master).

First step is to add required Cucumber dependencies to maven pom:

{% highlight xml %}

<dependency>
	<groupId>info.cukes</groupId>
	<artifactId>cucumber-java</artifactId>
	<version>${cucumber.version}</version>
	<scope>test</scope>
</dependency>

<dependency>
	<groupId>info.cukes</groupId>
	<artifactId>cucumber-spring</artifactId>
	<version>${cucumber.version}</version>
	<scope>test</scope>
</dependency>

<dependency>
	<groupId>info.cukes</groupId>
	<artifactId>cucumber-junit</artifactId>
	<version>${cucumber.version}</version>
	<scope>test</scope>
</dependency>

{% endhighlight %}

Before adding other elements, just a note about artifacts that are included in feature execution:

 * step definitions: regular .java files, placed in src/test/java
 * features: have extension .feature, placed in src/test/resources
 * feature runners: regular .java files, place in src/test/java

Step definitions are used to define processing for each statement of the feature. They are so called "glue" part that connects feature statements to processing logic. To be able to define processing we need our spring beans. Cucumber-spring enables us to wire and invoke spring beans from the step definitions. Two xml files are used: appcontext.xml and cucumber.xml. Here are the relevant parts:

appcontext.xml

{% highlight xml %}

<context:component-scan base-package="bdd" />

{% endhighlight %}

cucumber.xml

{% highlight xml %}

<import resource="classpath:appcontext.xml" />

{% endhighlight %}

Now test execution context can be enriched with Spring context via @ContextConfiguration annotation. StepsBase class is serves this purpose:

{% highlight java %}

package bdd;

import org.springframework.test.context.ContextConfiguration;

@ContextConfiguration("/cucumber.xml")
public class StepsBase {

}

{% endhighlight %}

Feature runners are the ones that execute features. Cucumber comes with his own JUnit runner implementation that is used to configure environment and the output of feature execution. In this example FeaturesBase class is defined:

{% highlight java %}

package bdd;

import org.springframework.test.context.ActiveProfiles;

import cucumber.api.CucumberOptions;

@ActiveProfiles("test")
@CucumberOptions(plugin = { "pretty", "html:target/cucumber" })
public class FeaturesBase {

}

{% endhighlight %}

By default, Cucumber will look for the files with extension .feature in src/test/resources folder that matches package structure in which runner class is located. In case that fetures are located by the runner and there are missing steps definitions, cucumber-junit generates code for missing steps in the console log. This is cool feature and can be used to quickly get required annotations.

Here is the implementation of the steps in PurchaseTicketSteps class.

{% highlight java %}

@Given("^tickets can be purchased$")
public void assertAllServicesActive() throws Throwable {
	assertNotNull(customerService);
	assertNotNull(ticketService);
}

@When("^\"([^\"]*)\" customer makes request to purchase the ticket$")
public void makePurchase(String customerType) throws Throwable {
	Customer customer = customerType.equals("regular") ? new Customer() : new PremiumCustomer();
	Flight flight = new Flight(Calendar.getInstance());
	ticket = ticketService.purchase(flight, customer);
}

@And("^request is within the acceptable discount period defined by the policy$")
public void configureDiscountOn() throws Throwable {
	Calendar in48Hours = Calendar.getInstance();
	in48Hours.add(Calendar.HOUR, 48);
	ticket.getFlight().setDatetime(in48Hours);
}

@Then("^price of the ticket should be reduced for amount defined by the policy$")
public void assertTicketWithDiscount() throws Throwable {
	assertNotNull(ticket.getDiscount());
}

@And("^payment transaction should be initiated with payment provider$")
public void assertPaymentTransactionMade() throws Throwable {
	assertTrue(ticket.isPaymentCompleted());
}

@And("^customer should be notified about the \"([^\"]*)\"$")
public void assertCustomerNotified(String message) throws Throwable {
	assertTrue(ticket.getMessages().contains(message));
}

@And("^discount period defined by the policy has expired$")
public void configureDiscountOff() throws Throwable {
	Calendar in23Hours = Calendar.getInstance();
	in23Hours.add(Calendar.HOUR, 23);
	ticket.getFlight().setDatetime(in23Hours);
}

@Then("^ticket should have regular price$")
public void assertTicketWithoutDiscount() throws Throwable {
	assertEquals(BigDecimal.ZERO, ticket.getDiscount());
}

{% endhighlight %}

Notice that muable object attribute "ticket" is used as test context of this feature. Each scenario resets this ticket to null. This is done with special cucumber annotation @After that is different that JUnit annotation with same name. Difference is that cucumber @After method is executed after each scenario. Last element is PurchaseTicketFeature that is started with cucumber JUnit runner by the Maven Surefire plugin.

{% highlight java %}

package bdd.example.ticket;

import org.junit.runner.RunWith;

import bdd.FeaturesBase;
import cucumber.api.junit.Cucumber;

@RunWith(Cucumber.class)
public class PurchaseTicketFeature extends FeaturesBase {

}

{% endhighlight %}

Features can be run with command:

mvn clean test

This should produce output simillar to this:

<p class="img-box-center">
<img src="/assets/posts/bdd-cucumber-jvm-spring-report.png" alt="There should be image right here"/>
</p>

And thats all!

Console output shows which step definitions were used for which statements of scenario. HTML report is generated in target/cucumber folder by default.
The complete source code of this tutorial is available as [public github repository](https://github.com/qza/bdd-cucumber-jvm-spring)