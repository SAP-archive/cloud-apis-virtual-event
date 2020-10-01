# Using APIs on SAP Cloud Platform

[![REUSE status](https://api.reuse.software/badge/github.com/SAP-samples/cloud-apis-virtual-event)](https://api.reuse.software/info/github.com/SAP-samples/cloud-apis-virtual-event)

## Description

This repository contains the material for a virtual event all about using APIs on SAP Cloud Platform.

Prerequisites and recommendations are documented in the [prerequisites](prerequisites.md) file.

### Virtual event overview

In this virtual event, you'll get to know how to find, understand, authenticate for and use APIs on SAP Cloud Platform. It covers discovery via the SAP API Business Hub (and other locations), authentication with different OAuth 2.0 grant types and practical advice, through direct hands-on experience calling APIs from various endpoints.

### Material organization

The material consists of a series of exercises that are to be done in order (each one building on the previous one). Each exercise is contained in a directory, with a main 'readme' file containing the core exercise instructions, with optional supporting files, such as screenshots and sample files.

### Following the exercises

During the virtual event you will complete each exercise one at a time. At the end of each exercise there are questions; these are designed to help you think about the content just covered, and are to be discussed with the entire class, led by the instructors, when everyone has finished that exercise.

If you finish an exercise early, please resist the temptation to continue with the next one. Instead, explore what you've just done and see if you can find out more about the subject that was covered. That way we all stay on track together and can benefit from some reflection via the questions (and answers).

:point_right: Where there's an action for you to perform, it will be prefixed with this pointing symbol, to help you focus on where you are in each exercise.

### The exercises

Here's an overview of the exercises.

**Some basics**

In these first two exercises, you'll get a basic understanding of what APIs on SAP Cloud Platform look like, and also get an introduction to important OAuth 2.0 concepts which are fundamental to using the APIs.

- [Exercise 01 - Get an overview of API resources](exercises/01/)
- [Exercise 02 - Understand OAuth 2.0 at a high level](exercises/02/)

**Learning with the Workflow API**

In these next few exercises you'll get your feet wet with the Workflow APIs. First, you'll set up a dev space in the SAP Business Application Studio (App Studio) with some extra tools, then you'll create a Workflow service instance and deploy a simple workflow definition. Through trial and error, you'll learn about scopes, the contents of access tokens, and become familiar with a few different Workflow API resources.

- [Exercise 03 - Setting up your dev space in the SAP Business Application Studio](exercises/03/)
- [Exercise 04 - Creating a Workflow service instance & deploying a definition](exercises/04/)
- [Exercise 05 - Workflow API calls, authorities, access token contents & more](exercises/05/)
- [Exercise 06 - Calling the Workflow API from within the SAP API Business Hub](exercises/06/)

> Subsequent exercise groups in this Virtual Event are in "planned" status only and do not yet exist.

**Authenticate and refresh tokens against the SAP Ariba APIs using Python**

In this exercise, you'll learn how you can use Python to authenticate against the SAP Ariba APIs and the usage of refresh tokens and in which scenarios they are commonly used.

- [Exercise 07 - Authenticate :heavy_plus_sign: refresh tokens in the SAP Ariba APIs using :snake:](exercises/07/)

**Calling Enterprise Messaging APIs with Postman**

In this group of exercises you'll learn about the management and messaging groups of APIs for Enterprise Messaging, and interact with them using Postman (planned).

**Exploring the Password grant type with the Core Service APIs**

In this group of exercises you'll take a brief look at, and get your hands dirty with a different grant type, one that is used currently to protect the Core Service APIs of SAP Cloud Platform (planned).

**Consuming OAuth 2.0 protected APIs from within Node.js**

To round out this virtual event, you'll learn how you can use Node.js packages to help you manage calls to Cloud APIs from JavaScript (planned).


## Requirements


The requirements to follow the exercises in this repository, including hardware and software, are detailed in the [prerequisites](prerequisites.md) file.


## Download and installation

You do not need to download this repository nor install anything from it. You can just follow the exercises by visiting each of them as listed in the [exercises](#the-exercises) section.


## How to obtain support

Support for the content in this repository is available during virtual events, for which this content has been designed. Otherwise, this content is provided "as-is" with no other support.

## Contributing

If you wish to contribute code, offer fixes or improvements, please send a pull request (PR). Due to legal reasons, contributors will be asked to accept a [Developer Certificate of Origin (DCO)](https://en.wikipedia.org/wiki/Developer_Certificate_of_Origin) on submitting their first PR to this project. This DCO acceptance can be done in the PR itself - look out for the CLA assistant that will guide you through the simple process. SAP uses [the standard DCO text of the Linux Foundation](https://developercertificate.org/).

## License

Copyright (c) 2020 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License version 2.0.
