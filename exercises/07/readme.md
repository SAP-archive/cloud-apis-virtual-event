# Exercise 07 - Authenticating against SAP Ariba APIs (OAuth 2.0) using Python

Now that we are familiar with the basic concepts of OAuth 2.0, we'll look at how to authenticate against the SAP Ariba APIs using Python. We will first retrieve an access token, look at the response of the authentication server and refresh an access token once the original token expires.

The SAP Ariba APIs allows creating integrations between applications/services/machines. The APIs are commonly used to extend the capabilities of SAP Ariba, e.g. extend approval process, interact with invoices, or it can just be to retrieve transactional/analytical data from the SAP Ariba realm (instance). 

To allow programmatic access to an SAP Ariba realm, an application will need to be created in the [SAP Ariba Developer Portal](https://developer.ariba.com) and go through the application approval process. Once the approval process completes, the developer portal administrator will be able to generate OAuth credentials for the application. These OAuth credentails are required to complete this exercise. 

> For this exercise, the OAuth credentials of any SAP Ariba API will work, as we will focus purely on authentication and we will not retrieve data from SAP Ariba.

## Steps

After completing the steps in this exercise you'll know how to authenticate agains the SAP Ariba APIs and have an understanding of an OAuth 2.0 response, what the different fields returned mean and how to use the `refresh_token` and why it is needed.

To run the Python scripts included in this exercise, ensure that you checkout the prerequisites and recommendations documented in [prerequisites](prerequisites.md).

### 1. Get familiar with an OAuth 2.0 successful response

As mentioned in previous exercises, OAuth 2.0 is an industry standard, based on work within the context of the Internet Engineering Task Force (IETF) and codified in [RFC 6749 - The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749). Before authenticating against the SAP Ariba APIs, lets remind ourselves what we should expects as a successful authentication response. For this, we can reference to [RFC 8693 - OAuth 2.0 Token Exchange](https://tools.ietf.org/html/rfc8693), which contains additional details and explanation on the different fields that we can expect in the response of an authentication request.

:point_right: Visit the [RFC 8693 - 2.2.1 Successful Response section](https://tools.ietf.org/html/rfc8693#section-2.2.1) and get familiar with the fields we should expect in the response from the SAP Ariba authorization server. 

> Pay special attention to the explanations of the `access_token`, `refresh_token`, `token_type` and `expires_in` fields.

### 2. Example of a response from a successful OAuth 2.0 request

Below an example of a response from a successful OAuth 2.0 request:
```json
{
    "access_token": "dc1aaf96-5987-4bcc-aeba-426e80f7e168",
    "refresh_token": "c6ef16aa-26d6-4296-9507-eded862d7e1b",
    "token_type": "bearer",
    "scope": null,
    "expires_in": 1440
}
```

Lets quickly see what each of the fields above are:
- `access_token`: This is the security token issued by the OAuth server. We will need to specify this value in the `Authorization` header when retrieving data from the API. Example of `Authorization` header value -> `Bearer dc1aaf96-5987-4bcc-aeba-426e80f7e168`
- `refresh_token`: Given that the SAP Ariba API access token expires, the OAuth server returns a `refresh_token`. This `refresh_token` will be used to get a new `access_token` when the original `access_token` retrieved expires. 
  *From RFC 8693 -> A refresh token can be issued in cases where the client of the token exchange needs the ability to access a resource even when the original credential is no longer valid (e.g., user-not-present or offline scenarios where there is no longer any user entertaining an active session with the client).* 
- `token_type`: Indicate the type of the `access_token` issued. As shown in an example above, `Bearer` is specified in the `Authorization` header.
    *From RFC 8693 -> The client can simply present it (the access token) as is without any additional proof of eligibility beyond the contents of the token itself.*
- `expires_in`: This field tells us how long before our `access_token` expires. If we are continuously calling the API, we will need to make sure that the access_token is refreshed before the


### 3. Set up our SAP Ariba API details

Enough of explanations, lets jump to some code. Before sending an authentication request we will specify the application details previously shared by an SAP Ariba developer portal administrator in our environment configuration. 

The Python script uses [python-dotenv](https://pypi.org/project/python-dotenv/) to set some environment variable required by the script.

:point_right: Navigate to the scripts folder, create a file called `.env`. Copy and paste the sample config settings shown below and replace the values of REALM, API_OAUTH_URL, API_KEY, and BASE64_AUTHSTRING with the values provided by your administrator. 

> For a detailed explanation of 

The contents of the new .env file should look similar to the one below:
```text
############
# Config settings
############

# Value in seconds
TOKEN_DELAY=5

############
# Ariba API
############

REALM=MyRealm-T
API_OAUTH_URL="https://api.ariba.com//v2/oauth/token"
API_KEY=ln9hwtJNgLX378oZd2TiCGqp7HPOOMoU
BASE64_AUTHSTRING=NWY5YzRlNWEtYThmZC00OGIyLWFkZTMtYWE1NjAzZDgzZDIwOlZoS0NaRUZwTng3TmhlU3dZRHNOc2NFMjlvdkhTMFZW
```

Now that you've set the API details, we can send an authentication request to the SAP Ariba API OAuth server.

### 4. Authenticate against the SAP Ariba API (Get an `access_token`)

```python
# Code from script
```

### 5. Refresh the `access_token` using the `refresh_token` mechanism

```python
# Code from script
```

> In the case of the SAP Ariba APIs, it is possible to refresh an access token 2 minutes before it expires.

## Summary

At this point you're ...


## Questions

1. Can we authenticate against the API more than once (step 4)? What happens to the previous access_token received?
1. What happens if we try to refresh a token that has not expired and that will not expire soon? *Hint: Send a refresh token request after acquiring a new access token.*
