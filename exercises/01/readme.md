# Exercise 01 - ...

In this exercise you'll get an overview of some of the Application Programming Interfaces (APIs) available on SAP Cloud Platform, and of the OAuth 2.0 based authentication approach used to protect them.


## Steps

After completing the steps in this exercise you'll be famililar with places where SAP Cloud Platform APIs are documented, and understand the basics of OAuth 2.0 authentication.

### 1. Get an introduction to the SAP API Business Hub

There are many services available on SAP Cloud Platform, and most of these have APIs that can be used to access those services' facilities programmatically. The canonical "home" of API information for SAP services generally is the [SAP API Business Hub](https://api.sap.com/), or API Hub for short.

:point_right: Go to the [API Hub](https://api.sap.com) and log in using your SAP Cloud Platform trial account (a trial account is a [prerequisite](../prerequisites.md) for this virtual event).

At this point you can have a look around and get a feel for what the API Hub has to offer. Note that the API Hub has different content types, such as Business Processes, APIs, Events, Integration and more.

:point_right: To dig in a little deeper, let's pick a service - Workflow. Search for APIs for Workflow by entering the term in the search box. You should see results that look something like this:

![Workflow search results](workflow-search-results.png)

There are a number of important things to note in this list of results:

- you can refine the search results by type (see the list in the left hand column)
- there are APIs (denoted by the three circles symbol) and API packages (denoted by the symbol containing the globe)
- within the context of the APIs, there are REST APIs and OData APIs
- there are currently Workflow APIs for different environments - in this example we see APIs for Neo & for Cloud Foundry

:point_right: Select the "[SAP Cloud Platform Workflow](https://api.sap.com/package/SAPCPWorkflowAPIs?section=Artifacts)" API Package and you're taken to the list of artifacts. At the time of writing, there are four presented, as can be seen in the screenshot; again, note the details across each of them - versions, API type, and environment:

![Workflow API artifacts](workflow-api-artifacts.png)

:point_right: Finally in this step, dig one level deeper by selecting a specific API artifact. Choose the Workflow API for Cloud Foundry (the REST API) and have a brief look at the details shown.

Note that there are many API endpoints, and they are grouped together logically. You can see these groups in the left hand column; for the [Workflow API for Cloud Foundry](https://api.sap.com/api/SAP_CP_Workflow_CF/resource) that you've selected, there are the following groups of endpoints:

- User Task Instances
- Task Definitions
- Workflow Definitions
- Workflow Instances
- Forms
- Data Export
- Purge
- Jobs
- Messages



## Summary

At this point you're ...


## Questions

1. ...
