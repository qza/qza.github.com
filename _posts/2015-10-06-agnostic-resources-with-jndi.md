---
layout: post
title: "Agnostic resources with JNDI"
description: ""
category: [development, design]
tags: [java, jndi]
---
{% include JB/setup %}

Software applications rely on system and server resources. They are part of the deployment environment in which application operates. One of the good practices is to configure such resources via specific server configuration files and establish connections in agnostic way. If you are developing in Java, good starting point or reference can be found in [Official Oracle tutorial on JNDI](https://docs.oracle.com/javase/tutorial/jndi/).

The JNDI enables usage of naming services via it's own API and SPI that is implemented by the providers. Application containers like Tomcat and JBoss implement SPI and provide such services. This can be useful in application development with configuration of datasources and messaging queues. Instead configuring application with real URLs, better to give them name and reference resources by the name.

Java application can reference named datasources using JNDI in following ways:

#### Via initial context.

{% highlight java %}
Context context = new InitialContext(env);
DataSource ds = (DataSource) context.lookup("java:comp/env/jdbc/mydb");
{% endhighlight %}

#### Via @Resource annotation

{% highlight java %}
@Resource(name="java:comp/env/jdbc/mydb")
private javax.sql.DataSource datasource;
{% endhighlight %}

#### With Spring JNDI template

{% highlight java %}
JndiTemplate template = new JndiTemplate();
Datasource datasource = (Datasource) template.lookup("java:comp/env/jdbc/mydb");
{% endhighlight %}

#### With Spring JndiObjectFactoryBean

{% highlight xml %}
<beans:bean id="datasource" class="org.springframework.jndi.JndiObjectFactoryBean">
    <beans:property name="jndiName" value="java:comp/env/jdbc/mydb"/>
</beans:bean>
{% endhighlight %}

In similar way, Java application can reference messaging destination:

#### Via Initial context

{% highlight java %}
Context jndiContext = new InitialContext(); 
ConnectionFactory connectionFactory = (ConnectionFactory)    
	jndiContext.lookup("jms/myconnectionfactory"); 
Queue destination = (Queue) jndiContext.lookup("jms/myqueue"); 
{% endhighlight %}

#### With @Resource annotation

{% highlight java %}
@Resource(lookup = "jms/myqueue") Queue queue;
@Resource(lookup = "jms/mytopic") Topic topic;
{% endhighlight %}

#### With Spring JNDI template

{% highlight java %}
JndiTemplate template = new JndiTemplate();
Queue queue = (Queue) template.lookup("jms/myqueue");
{% endhighlight %}

By referencing resources with JNDI application stays loosely coupled with underlying provider and there is space for later change. Such applications are portable between environments as actual mapping is done during the deployment. This approach is often preferred as many environment are used for bringing application from development to production. The configuration of underlying system resources can be performed by other role, usually called: deploy-er which is usually the developer him self.