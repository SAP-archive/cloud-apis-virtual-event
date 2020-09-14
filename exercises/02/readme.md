# Exercise 02 - Understand OAuth 2.0 at a high level

In this exercise you'll learn about OAuth, the open standard for access control, and specifically about how OAuth, in its current version of 2.0, is used to protect and control access to SAP Cloud Platform API resources.


## Steps

After completing the steps in this exercise you'll have an understanding of how OAuth 2.0 is used to protect and control access to resources, preparing you to be able to make calls to SAP Cloud Platform APIs that are so protected.

### 1. Get an overview of OAuth 2.0

OAuth 2.0 is an industry standard, based on work within the context of the Internet Engineering Task Force (IETF) and codified in [RFC 6749 - The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749). The abstract section in this RFC provides a decent overview, with which we can start:

_The OAuth 2.0 authorization framework enables a third-party
   application to obtain limited access to an HTTP service, either on
   behalf of a resource owner by orchestrating an approval interaction
   between the resource owner and the HTTP service, or by allowing the
   third-party application to obtain access on its own behalf._

> RFC stands for Request For Comments and is a type of formal document describing methods, behaviors, research and innovations that underpin the Internet and related systems and protocols.

Immediately we can see how OAuth 2.0 differs from, say, the use of HTTP Basic Authentication, which has been used in the past to protect and control access to API resources. How does this differ? Well, in many ways, but just to pick a couple:

- HTTP Basic Authentication requires the use of a username and password; once compromised, those credentials are hard to revoke cleanly and without causing issues for other systems with which they were shared

- Rather than just a resource and a username / password credential pair with HTTP Basic Authentication, the OAuth 2.0 approach identifies multiple actors in any given scenario - such as the resource owner, a third party application, and so on

At the beginning of the O'Reilly book [Getting Started with OAuth 2.0](https://www.oreilly.com/library/view/getting-started-with/9781449317843/) the author [Ryan Boyd](https://www.linkedin.com/in/ryguyrg/) describes this difference in a light-hearted manner by recounting a scene from the movie "Ferris Bueller's Day Off", where a valet parking attendant, given the keys to an expensive 1961 Ferrari to park, takes it out for a joyride instead of just parking it in the garage. Giving the valet the keys, and thereby full access to the car, is the equivalent of using a username and password based authentication mechanism. Ideally the car owner would have given the valet limited access preventing such a scenario, and preventing other access such as driving at high speed, going further than a few hundred meters, or opening the glove compartment or trunk.

The idea of OAuth 2.0 is to facilitate delegated access via tokens; tokens that have a limited scope, a limited lifespan, and that are granted either directly or via interaction with a resource owner.


## Summary

At this point you're ready to make your first SAP Cloud Platform API call to endpoints protected with OAuth 2.0.
