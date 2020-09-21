# Exercise 05 - Workflow API calls, scopes, access token contents & more

Thanks to the previous exercises, you have everything you need to make some Workflow API calls now. You have tools in your App Studio dev space that will enhance your experience, you have a workflow definition deployed to a Workflow service instance, and you have OAuth 2.0 authentication details in a service key that you will make use of now. In this exercise you'll make your first Workflow API call, and learn about scopes (aka "authorities") and the contents of access tokens. You'll make your first API call from the command line.

## Steps

[1. Prepare for your first Workflow API call](#1-prepare-for-your-first-workflow-api-call)<br>
[2. Make your first Workflow API call](#2-make-your-first-workflow-api-call)<br>
[3. Inspect the contents of the access token](#3-inspect-the-contents-of-the-access-token)<br>

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

:point_right: There's a file in the `workflowapi/` directory called [`first-api-call.http`](../../workspaces/workflowapi/first-api-call.http). Open that up in the App Studio editor, and you should see a couple of HTTP calls, one for each of the stages described above. There are placeholders denoted by the content in square brackets (`[ ... ]`) that you will have to fill in.

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
POST https://a52544d1trial.authentication.eu10.hana.ondemand.com/oauth/token
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

Err, wait a minute...

The HTTP response code returned (403), along with the message in the payload, might come initially as somewhat of a surprise. Why are we denied access? Were the client ID and client secret credentials incorrect? Well, no, because we were successfully granted an access code based upon them. So what's happened is that the identity that is represented by the access token is recognized, but that identity doesn't have the appropriate access for this particular API call.


### 3. Inspect the contents of the access token

Let's briefly take stock of where we are. We've got an access token that we successfully retrieved from the Authorization Server. But that access token, as it stands, is not enough to allow us to make the API call to the `/v1/workflow-definitions` endpoint. Can we dig in a little bit to see what's going on? Well, we have a terminal at our disposal, and some useful tools. So the answer is "of course we can"!

Before we do, though - consider that making the two-stage call was quite cumbersome. Using the REST Client is very useful in many circumstances, but when OAuth 2.0 flows are involved, there are other options. One is to use the environments facility within the API Hub, another is to automate some of the process ourselves. That's what we'll do, so that we can, for this exercise at least, remain in the App Studio, while staying close to the flow so that we don't lose sight or understanding of what's going on.

:point_right: Look in the `workflowapi/` directory and open the [`workflow`](../../workspaces/workflowapi/workflow) file that's in there. It's a shell script, that we can run in the terminal. Take a brief look at it; don't worry if you don't understand everything, just get a general impression of it.

Here's a summary of what's in there, going from top to bottom:

- "sourcing shared": the [`shared`](../../workspaces/workflowapi/shared) file is sourced, bringing in the values (such as the instance name, service key name, and so on) that we've used earlier
- "function definitions": a number of functions are defined, such as `get_access_token` and `list_workflow_definitions` - these two exactly reflect the two HTTP requests we executed in the two-stage process earlier
- "command variable": all the functions are listed in a `command` variable
- "service key value retrieval": the OAuth 2.0 values and the Resource Server API root URL are retrieved from the service key file
- "list possible commands": if no command is given when the script is invoked, the possible commands are listed
- "get access token and invoke command": otherwise an access token is retrieved (using the `get_access_token` function) and then the requested function is executed

> The script is deliberately simple and ripe for improvements, but has been kept like this to strike a balance between simplicity and practicality. Also, the astute amongst you will point out that we don't need to retrieve a fresh access token for every API call - they have a lifetime that lasts for quite a while - but again, for simplicity's sake, that's what we do here.

:point_right: Try the script out by running it in the terminal:

```shell
> ./workflow
```

Because you didn't specify any command, it lists what's possible (or more accurately, what's in the `commands` variable):

```
add_authorities
get_access_token
list_workflow_definitions
list_workflow_instances
start_workflow_instance
```

> The `commands` variable is used in a more complex version of this script to provide command completion; if you're interested in learning more, check out the `management` and `management.completion` script pair in the [cloud-messaging-handsonsapdev](https://github.com/SAP-samples/cloud-messaging-handsonsapdev) repo.

Let's generate a new access token, with a view to inspecting it. Rather than use the first HTTP call in the REST Client `first-api-call.http` file, let's use this `workflow` script to generate ourselves an access token on the command line. First though, let's make sure we understand what will happen when we make the invocation.

:point_right: Look inside the `workflow` script again, and take a look at the "service key value retrieval" section; you'll see that it retrieves values for `clientid`, `clientsecret` and `authserver` variables. Now look at the `get_access_token` function definition, where you'll see these three variables referenced in a call to [`curl`](https://curl.haxx.se/), the powerful command line HTTP client (that's available in the App Studio's terminal shell environment).

This is what the invocation looks like:

```bash
  curl \
    --$output \
    --user "$clientid:$clientsecret" \
    --data "grant_type=client_credentials" \
    "$authserver/oauth/token" \
  | jq -r .access_token
```

You can check out the options in the `curl` documentation, but here's a summary of what we see:

- given that the value of `$output` is set to `silent` (see near the top of the script), the call will be executed quietly with no extraneous logging
- with the `--user` option we can supply a username and password which will be Base 64 encoded into a Basic Authentication header
- the `--data` option gives us the opportunity to supply data that will be passed in the payload (body) of the request
- the actual URL is made up from the value of the `authserver` variable, with `/oauth/token` appended
- the default content type that is sent in a `curl` request is `application/x-www-form-urlencoded` which is exactly appropriate for what we want to send

The output of the response, specifically the payload (body), an example of which we saw at the end of step 1 in this exercise, is JSON, where one of the properties is `access_token`. So we can use `jq` to parse out exactly that value, and give it to us in raw form with the `-r` switch (basically this means "don't present the value in double quotes").

Now we understand what we're running, let's generate a new access token.

:point_right: Generate an access token with the script, like this:

```bash
> ./workflow get_access_token
```

The response should look something like this - an access token in raw form (it's been shortened here for display purposes):

```
eyJhVQTE9ZIl0sImNsaWVudF9pZCI6InNiLWNsb25lLWIwOWQ5ZmNmLWE0MTgtNDRjOC05NTg5LWViYWJlYTY1NGNiNyFiNTU4ODl8d29ya2Zsb3chYjEwMTUwIiwiY2lkIjoic2ItY2xvbmUtYjA5ZDlmY2YtYTQxOC00NGM4LTk1ODktZWJhYmVhNjU0Y2I3IWI1NTg4OXx3b3JrZmxvdyFiMTAxNTAiLCJhenAiOiJzYi1jbG9uZS1iMDlkOWZjZi1hNDE4LTQ0YzgtOTU4OS1lYmFiZWE2NTRjYjchYjU1ODg5fHdvcmtmbG93IWIxMDE1MCIsImdyYW50X3R5cGUiOiJjbGllbnRfY3JlZGVudGlhbHMiLCJyZXZfc2lnIjoiNzI5NmM4OTIiLCJpYXQiOjE2MDA2NzY4MjQsImV4cCI6MTYwMDcyMDAyNCwiaXNzIjoiaHR0cHM6Ly9hNTI1NDRkMXRyaWFsLmF1dGhlbnRpY2F0aW9uLmV1MTAuaGFuYS5vbmRlbWFuZC5jb20vb2F1dGgvdG9rZW4iLCJ6aWQiOiJmZDAzNDAyZS01OGM3LTRmYjgtOTQ0My01ZDBmYTJhNTMzZjQiLCJhdWQiOlsidWFhIiwid29ya2Zsb3chYjEwMTUwIiwic2ItY2xvbmUtYjA5ZDlmY2YtYTQxOC00NGM4LTk1ODktZWJhYmVhNjU0Y2I3IWI1NTg4OXx3b3JrZmxvdyFiMTAxNTAiXX0.kvK72llXiXigBSjAvyAPBG1BqwxAf_2RasOfyZ7Utz5yH7vcqk6argtXQVSqSdyp8VvV2iPM6RhlBxkeduh27o74QGEUZhZ3oWVRMukEllMAZCxngllmdnK-oRty-NeF04l-Y6jCcLZXWQkujusS6WaoKYLFfJ0v7BvzFylmf0WBuqyz1JILgE2b9uxndl19cCvv6-oGiHMgqev7zALmhXcSZlqz9SezAanbGdr5h_eOkiRaDTMmNM9jx8vzO_K_cYWaAre7gVGBVHm1In2l44WXDhoEy2WMlmZOFk0th3aoykdS5gOIOgP68gqeHVx_UOtzO2pCbfGITXnLD1Htdw
```

This access token doesn't look much different to what we've seen already, and it certainly is pretty impenetrable as far as detail is concerned.

That's where `jwt`, the JSON Web Token tool we installed in [exercise 01](../../01) comes in.

:point_right: Run the command again, and this time, extend it so that the output (the raw access token) is piped into `jwt`, like this:

```bash
> ./workflow get_access_token | jwt
```

This will reveal the glorious details embedded in that access token. There's lots of information, but what's relevant for us is the payload section, which will look something like this:

```json
{
  "jti": "e585cdb02adc466c90bac5cb399b2962",
  "ext_attr": {
    "enhancer": "XSUAA",
    "subaccountid": "fd03402e-58c7-4fb8-9443-5d0fa2a533f4",
    "zdn": "a52544d1trial",
    "serviceinstanceid": "b09d9fcf-a418-44c8-9589-ebabea654cb7"
  },
  "sub": "sb-clone-b09d9fcf-a418-44c8-9589-ebabea654cb7!b55889|workflow!b10150",
  "authorities": [
    "workflow!b10150.TASK_GET",
    "workflow!b10150.PROCESS_TEMPLATE_DEPLOY",
    "workflow!b10150.PROCESS_VARIANT_DEPLOY",
    "uaa.resource",
    "workflow!b10150.FORM_DEFINITION_DEPLOY",
    "workflow!b10150.TASK_DEFINITION_GET",
    "workflow!b10150.WORKFLOW_DEFINITION_DEPLOY"
  ],
  "scope": [
    "workflow!b10150.TASK_GET",
    "workflow!b10150.PROCESS_TEMPLATE_DEPLOY",
    "workflow!b10150.PROCESS_VARIANT_DEPLOY",
    "uaa.resource",
    "workflow!b10150.FORM_DEFINITION_DEPLOY",
    "workflow!b10150.TASK_DEFINITION_GET",
    "workflow!b10150.WORKFLOW_DEFINITION_DEPLOY"
  ],
  "client_id": "sb-clone-b09d9fcf-a418-44c8-9589-ebabea654cb7!b55889|workflow!b10150",
  "cid": "sb-clone-b09d9fcf-a418-44c8-9589-ebabea654cb7!b55889|workflow!b10150",
  "azp": "sb-clone-b09d9fcf-a418-44c8-9589-ebabea654cb7!b55889|workflow!b10150",
  "grant_type": "client_credentials",
  "rev_sig": "7296c892",
  "iat": 1600677038,
  "exp": 1600720238,
  "iss": "https://a52544d1trial.authentication.eu10.hana.ondemand.com/oauth/token",
  "zid": "fd03402e-58c7-4fb8-9443-5d0fa2a533f4",
  "aud": [
    "uaa",
    "workflow!b10150",
    "sb-clone-b09d9fcf-a418-44c8-9589-ebabea654cb7!b55889|workflow!b10150"
  ]
}
```

:point_right: Look closely at the values in the `scope` section - they include scopes such as "TASK_GET", "PROCESS_TEMPLATE_DEPLOY", and "WORKFLOW_DEFINITION_DEPLOY".

These are the scopes that the access token includes, describing essentially what the bearer of the access token is permitted to do.

:point_right: Now jump back right to the start of this exercise, and gaze upon the details of the `GET /v1/workflow-definitions` API endpoint. Note in the description, we see this:

```
Scope: WORKFLOW_DEFINITION_GET
```

Is that scope in the list of scopes in our current access token?

No.

So we need to address that if we want to make a call to that API endpoint.


