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

- Rather than just a resource and a username & password credential pair with HTTP Basic Authentication, the OAuth 2.0 approach identifies multiple actors in any given scenario - such as the resource owner, a third party application, and so on

At the beginning of the O'Reilly book [Getting Started with OAuth 2.0](https://www.oreilly.com/library/view/getting-started-with/9781449317843/) the author [Ryan Boyd](https://www.linkedin.com/in/ryguyrg/) describes this difference in a light-hearted manner by recounting a scene from the movie "Ferris Bueller's Day Off", where a valet parking attendant, given the keys to an expensive 1961 Ferrari to park, takes it out for a joyride instead of just parking it in the garage. Giving the valet the keys, and thereby full access to the car, is the equivalent of using a username & password based authentication mechanism. Ideally the car owner would have given the valet limited access preventing such a scenario, by providing scope limited in time and features - not allowing access to driving at high speed, going further than a few hundred meters, or opening the glove compartment or trunk.

The idea of OAuth 2.0 is to facilitate delegated access via tokens; tokens that have a limited scope, a limited lifespan, and that are granted either directly or via interaction with a resource owner.


### 2. Understand the roles in OAuth 2.0

As mentioned in the previous step, there are multiple actors at play in an OAuth 2.0 scenario. These actors have different roles, and they can be summarized as follows:

- **Client**: the application that requires access to resources

- **Resource Server**: where the resources are, i.e. where the endpoints are that the Client wishes to consume

- **Resource Owner**: the owner of the resources to which access is required; ultimately, this might be a system, or a human

- **Authorization Server**: A server that (in the case where approval or denial of delegated access requires a decision by a human Resource Owner) presents an interface to approve or deny access requests, and that also issues and renews access tokens

In some circumstances the Authorization Server and the Resource Server might be the same and combined into one; in the context of SAP Cloud Platform however they are usually separate.


### 3. Understand OAuth 2.0 grant types

OAuth 2.0 has been designed to protect and control access to resources in different scenarios, where some or all of the roles described in the previous step are involved.

In the context of OAuth 2.0, protection of SAP Cloud Platform APIs is achieved in the context of different grant types. These grant types are specific scenarios that each describe what type of approach, or flow, is used that results in the granting of an access token with the appropriate authorizations.

The OAuth 2.0 grant types (also known as "flows") are summarized here. It's worth having this simple and deliberately abstract (not specific to any particular grant type) protocol flow diagram in mind, which is taken from the RFC ([section 1.2](https://tools.ietf.org/html/rfc6749#section-1.2)) and shows the different actors and exchanges:

```
     +--------+                               +---------------+
     |        |--(A)- Authorization Request ->|   Resource    |
     |        |                               |     Owner     |
     |        |<-(B)-- Authorization Grant ---|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(C)-- Authorization Grant -->| Authorization |
     | Client |                               |     Server    |
     |        |<-(D)----- Access Token -------|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(E)----- Access Token ------>|    Resource   |
     |        |                               |     Server    |
     |        |<-(F)--- Protected Resource ---|               |
     +--------+                               +---------------+
```

**Authorization Code**

This normally involved a request, initiated by the Client, to a human Resource Owner, asking them to grant access. If the access request is approved, an authorization code is returned to the Client, which then contacts the Authorization Server to ask for the code to be exchanged for an access token, which can then be used to authenticate calls to the appropriate endpoints on the Resource Server. Note that at no time in this grant type does the Client see or use the Resource Owner's (username & password) credentials.

If you've ever been asked, on a web page, by e.g. Google or GitHub, to allow access by a third party app to some of your resources, then you've participated as a Resource Owner in this flow.

**Client Credentials**

One can almost see this as the other side of the coin to the Authorization Code grant type, in that the attainment of an access token is still the key goal, but this time the flow is outside the context of any human. In other words, this is where the Client (the script or program that is to make calls to the APIs) is acting on its own behalf, rather than on behalf of any Resource Owner (think of it as if the Client in this case is also the Resource Owner).

This is a very common grant type used for access to SAP Cloud Platform APIs, especially for automated and "headless" activities.

**Implicit**

This grant type was designed as a simple authorization code flow optimized for browser-based scripts (written typically in JavaScript), where the number of round trips between actors are minimized. Clients are granted access tokens directly, where no intermediate authorization codes are issued. This is not used in the context of SAP Cloud Platform APIs and is considered legacy and not recommended any more.

**Resource Owner Password Credentials**

Also known simply as the "Password" grant type, this is a flow designed for use in the situation where there is strong trust between the Client and the Resource Owner - more specifically, when the Resource Owner trusts the Client so much that they are willing to give their username & password credentials to the Client, which can then use them to request an access token. One redeeming feature of this grant type is that the Client does not have to store the credentials, as the access token granted can be long-lived, and / or the lifetime of the token can be extended by use of a refresh token.

While this grant type is also considered legacy, it is used currently to protect the [Cloud Management APIs on SAP Cloud Platform](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/3670474a58c24ac2b082e76cbbd9dc19.html), so worth understanding, at least for the moment.




## Summary

At this point you're ready to make your first SAP Cloud Platform API call to endpoints protected with OAuth 2.0.
