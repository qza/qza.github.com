---
layout: post
title: "Enterprise Identity Management   zero to production"
description: ""
category: [technical]
tags: [enterprise, identity, security]
---
{% include JB/setup %}

Identity management (IM) systems bring us centralized authorization and authentication, many other features among with their most famous one: Single-sign-on (SSO). Best way to understand Identity management is by understanding what is ment by Identity providence.

One of the main roles of Identity management system is to act as Identity provider. Identity provider is the system that owns identity information and provides that information to many client (third-party) applications. Every authentication happens with Identity provider. Identity provider then notifies client applications when authentication events occur. This is known as Identity federation, describing the trust given to identity informations coming from different domains. Different domains relate to the fact that identity provider verifies credentials. Client applications have complete trust in Identity provider and perform authentication of users inside their security domain. Their security domain means that client application are still the ones that own and maintain client session. Events are used to synchronize session on the client side.

The exact protocol that enables Identity providence is usually pretty simillar with smaller differences between implementation. Considering the Web applications served over HTTP, when end-user makes access to the client application, request is redirected to IM system. IM system accepts end-user credentials, verifies this credentials against some sort of storage (ex. AD via LDAP) and starts session for end-user. After end-user request is enriched with information indicating that session has been started, request is redirected back to client application. Additional verification is performed from the client side, by asking IM system to verify received information. In case that IM system verifies existence of session for end-user, client application can trust IM system and initate session for the client that will grant him access to the application. In case that end-user makes access to some other client application, as end-user request is already enriched with security information, second client application only verifies this information with IM system and in case of successful verification initiate session in their security domain, without additional authentication step performed by the end-user (SSO). When logout request is received, there are many options available: to invalidate session in client application, invalidate session in IM system and option that will send event to each of the client applications notifying them to invalidate client session isntantlly. The third option is rarely used, as each access to the client application can make call to the IM system and check existance of session and deny access in case that session in IM system is missing.

In increased level of trust, IM systems can provide central place for all authorizations of end-users and act as Authorization provider. This means that client applications can relly on IM system to provide complete list of grants for user and use that list to grant or deny access to certain application resources. This authorization info is maintained in IM for each client application and sent during verification of end-user request. To perform this function, IM system should have some way of organizing authorizations for all users and application which demands for proper organization and definition of all roles and privileges for all applications and users.

Developers that take responsibility for implementation of IM system can have two types of users in this case, regular end-users and other developers that are responsible for integration of theirs applications to IM system. For developer this means developing client libraries (SDKs) for different languages and frameworks, writing integration guides and many security considirations. Existing open-source projects offer excellent starting point or even complete solution depending on company needs, so there is no actuall need to start from the zero. Many of those include prepared SDKs for client applications. Although, most of them need to be adapted to fit company needs, core support is there covering many of the current security standards and protocols used in Identity federation. 

There are many aspects against existing projects can be evaluated. Here are some of those:

+ storage support and pluggability (AD, DB etc.)
+ protocol support and connectivity ([SAML](http://saml.xml.org/), [OpenID](http://openid.net/) etc.)
+ application server support
+ easiness of main configuration (service, roles, privileges etc.)
+ password management features 
+ end-user self service features
+ extensibility and easiness of UI customization 
+ framework support and code base quality
+ audit trail, level, quality and options
+ availability of client SDKs

Implementation of enterprise IM requires many other actions and project roles. Data officers should ensure that credential data is collected, organized and stored in central way. Security experts should make proper test to verify that there is no exposure of sensitive data. Design team should prepare design for forms, emails and other graphical elements presented to the client. Project leads should understand that such project demands developers from all applications to be be included during integration and that client side of this project takes equally important part as IM system itself.
