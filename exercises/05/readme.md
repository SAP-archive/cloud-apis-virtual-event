# Exercise 05 - Workflow API calls, scopes, access token contents & more

Thanks to the previous exercises, you have everything you need to make some Workflow API calls now. You have tools in your App Studio dev space that will enhance your experience, you have a workflow definition deployed to a Workflow service instance, and you have OAuth 2.0 authentication details in a service key that you will make use of now. In this exercise you'll make your first Workflow API call, and learn about scopes (aka "authorities") and the contents of access tokens. You'll make your first API call from the command line.

## Steps

After following the steps in this exercise, you'll have some familiarity with calling Workflow APIs, and will have gained some understanding of what's required to do so.

### 1. Prepare for your first Workflow API call

We have a workflow definition deployed. To start off, let's request a list of workflow definitions via the API, where we should see it.

:point_right: Jump over to the API Hub and look in there to see the resource information for the [Workflow API for Cloud Foundry](https://api.sap.com/api/SAP_CP_Workflow_CF/resource). In the Workflow Definitions group (select it on the left hand side of the page, in the list of groups that we first encountered in [exercise 01](../01#1-get-an-introduction-to-the-sap-api-business-hub)) we see this HTTP method and endpoint:

![GET /v1/workflow-definitions](get-workflow-definitions-endpoint.png)

Great, that seems to be what we want. We can make this API call by sending an HTTP GET request to the `/v1/workflow-definitions` endpoint. This of course is just the path info, which is going to be relative to the fully qualified hostname and base path of the Resource Server. What is this? Well, we can look for it in the API Hub, by switching to the [details](https://api.sap.com/api/SAP_CP_Workflow_CF/overview) section, and identifying the Production URL appropriate to our environment:

![Production URLs for Workflow API on CF](production-urls.png)

As we know now from [exercise 02](../02/), the Workflow APIs are protected with the OAuth 2.0 client credentials grant type. So in preparing for our first call, we need to gather what we need to request an access token. Because of how the client credentials grant type flow works, we'll be making the call in two stages:

Stage 1 is requesting an access token
Stage 2 is then using the access token in the actual call to the API endpoint

All the information we need is in our service key, which we have in the `workflow-lite-sk1.json` file. As you may remember, in order to obtain an access token, we need to send a request to the Authorization Server, specifically to the "token request" endpoint which is at the well-known path of `/oauth/token`.

:point_right: Make sure you have all the following information to hand:

**For stage 1 - requesting the access token**

|What|Where this value is|
|-|-|
|The Authorization Server base URL|`.uaa.url` (in the service key)|
|The client ID|`.uaa.clientid` (in the service key)|
|The client secret|`.uaa.clientsecret` (in the service key)|

**For stage 2 - making the actual call to the API endpoint**

|What|Where this value is|
|-|-|
|The Resource Server base URL|`.endpoints.workflow_rest_url` (in the service key)|
|The access token retrieved|In the response to the call in stage 1|

The App Studio supports the [VS Code REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) extension, which means that we can make HTTP calls easily by editing a file with an `.http` extension. That's how we're going to make our first API call.

:point_right: There's a file in the `workflowapi/` directory called `first-api-call.http`. Open that up in the App Studio editor, and you should see a couple of HTTP calls, one for each of the stages described above. There are placeholders denoted by the content in square brackets (`[ ... ]`) that you will have to fill in.

Thie is what the file contents look like:

```
# Request OAuth 2.0 access token via client credentials grant type
Send Request
POST [.uaa.url]/oauth/token
Authorization: Basic [.uaa.clientid] [.uaa.clientsecret]
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials

###

# Use access token to make API call to list workflow definitions
Send Request
GET [.endpoints.workflow_rest_url]/v1/workflow-definitions
Authorization: Bearer [the access token retrieved from the previous request]
```

> Individual HTTP calls are separated from each other by a line containing `###`.

:point_right: Replace each of the placeholders (including the actual square brackets) for the first HTTP call with the values you've gathered earlier in this step. Notice that there's a space between the `[.uaa.clientid]` and `[.uaa.clientsecret]` - make sure you preserve this space (the REST Client supports this format of username and password, and will [automatically perform the required Base 64 encoding](https://github.com/Huachao/vscode-restclient#basic-auth).

Here's an example of what the first HTTP call details will look like when you've performed the replacements (note that the client ID and secret have been shortened here for display reasons, and note also that your values will be different!):

```
# Request OAuth 2.0 access token via client credentials grant type
Send Request
POST https://xyz12345trial.authentication.eu10.hana.ondemand.com/oauth/token
Authorization: Basic sb-clone-b09d9...b10150 bc8b5...ad3DJtMLqjYuCo=
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials
```

:point_right: Now use the **Send Request** link that is part of the first HTTP call details to make the call to the Authorization Server to request an access token.

A response should appear in a new tab, that will look something like this (the value of the access token has been shortened for display reasons):

```
HTTP/1.1 200 OK
Cache-Control: no-store
Content-Type: application/json;charset=UTF-8
Date: Mon, 21 Sep 2020 07:20:41 GMT
Pragma: no-cache
Server: nginx
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-Vcap-Request-Id: 972b496c-fbe2-4a48-55f4-8660a009f23b
X-Xss-Protection: 1; mode=block
Connection: close
Transfer-Encoding: chunked
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload;

{
  "access_token": "eyJhMDlkOWZjZi1hNDE4LTQ0YzgtOTU4OS1lYmFiZWE2NTRjYjchYjU1ODg5fHdvcmtmbG93IWIxMDE1MCIsImdyYW50X3R5cGUiOiJjbGllbnRfY3JlZGVudGlhbHMiLCJyZXZfc2lnIjoiNzI5NmM4OTIiLCJpYXQiOjE2MDA2NzI4NDEsImV4cCI6MTYwMDcxNjA0MSwiaXNzIjoiaHR0cHM6Ly9hNTI1NDRkMXRyaWFsLmF1dGhlbnRpY2F0aW9uLmV1MTAuaGFuYS5vbmRlbWFuZC5jb20vb2F1dGgvdG9rZW4iLCJ6aWQiOiJmZDAzNDAyZS01OGM3LTRmYjgtOTQ0My01ZDBmYTJhNTMzZjQiLCJhdWQiOlsidWFhIiwid29ya2Zsb3chYjEwMTUwIiwic2ItY2xvbmUtYjA5ZDlmY2YtYTQxOC00NGM4LTk1ODktZWJhYmVhNjU0Y2I3IWI1NTg4OXx3b3JrZmxvdyFiMTAxNTAiXX0.N2hoBrcoG4eUZLBjGYzkjPlaxXd4BlcgOeinEzCtob3-XJTNK485_dGJ0MA6Qg0c8FayNfa3c9seK6QDlan_O3w3_Jd4l7jjO7tEm0GVPrxEKZuDu86fbQEPrSZWNaDjKrRDQxNEeyUQphh1zopdyyl9PjBTe-1MkQcEfFbB4udBCAEEnTX9kVPr53HtAaRk-HCD9VwHKvRzZ1Dlw3mEv7FLurqmZSV1F7jehhjeBVcDZZvuftJ1wThewlqu_K3NMUs09OzFkBmIXpbRRNR6YOlT37BkKeGI_v_-sZGtfjtSmzUas4AKh1zEjAC3u79M93T9fVY7WZJCY8Kzpp8G9w",
  "token_type": "bearer",
  "expires_in": 43199,
  "scope": "workflow!b10150.TASK_GET workflow!b10150.PROCESS_TEMPLATE_DEPLOY workflow!b10150.PROCESS_VARIANT_DEPLOY uaa.resource workflow!b10150.FORM_DEFINITION_DEPLOY workflow!b10150.TASK_DEFINITION_GET workflow!b10150.WORKFLOW_DEFINITION_DEPLOY",
  "jti": "ea3a8496e3c24c858384441c9619180e"
}
```

Great! You've successfully obtained an access token from the Authorization Server in a client credentials flow based call. Now it's time to actually make the API call. Ready?


### 2. Make your first Workflow API call


Now the moment of truth - stage 2, where we're going to make a GET request to the `/v1/workflow-definitions` API endpoint.

:point_right: Focus now on the second HTTP request in the `first-api-call.http` file, and replace the placeholder values as described earlier. Make sure you copy the whole value of the `access_token` property in the response to the call in stage 1.

After the placeholder replacements, the second HTTP request in the file should look something like this (again, the access token has been shortened here for display purposes):

```
# Use access token to make API call to list workflow definitions
Send Request
GET https://api.workflow-sap.cfapps.eu10.hana.ondemand.com/workflow-service/rest/v1/workflow-definitions
Authorization: Bearer NTAiXX0.N2hoBrcoG4eUZ...kKeGI_v_-sZGtfjtSmzUas4AKh1zEjAC3u79M93T9fVY7WZJCY8Kzpp8G9w
```

> Make sure you preserve the space between `Bearer` and the actual access token value in the `Authorization` header.

Ready?

:point_right: Use the **Send Request** link that is part of this second HTTP request's details to invoke the request.

You should get back a response, like this:

```
HTTP/1.1 403 Forbidden
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Content-Length: 65
Content-Type: application/json;charset=UTF-8
Date: Mon, 21 Sep 2020 07:34:58 GMT
Expires: 0
Pragma: no-cache
Server: SAP
X-Content-Type-Options: nosniff, nosniff
X-Frame-Options: DENY
X-Vcap-Request-Id: bb44cf53-e30f-407f-6740-8f94fd412e80
X-Xss-Protection: 1; mode=block
Connection: close
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload;

{
  "error": {
    "message": "User does not have sufficient privileges."
  }
}
```

Wait, what?

The HTTP response code returned (403), along with the message in the payload, might come initially as somewhat of a surprise. Why are we denied access? Were the client ID and client secret credentials incorrect? Well, no, because we were successfully granted an access code based upon them. So what's happened is that the identity that is represented by the access token is recognised, but that identity doesn't have the appropriate access for this particular API call.


