# Exercise 08 - Pagination in the SAP Ariba APIs

Now that we know how to authenticate against the SAP Ariba APIs, we'll be looking at how to retrieve large amounts of data by using a mechanism known as pagination. Pagination is commonly used by APIs to limit the amount of data returned in a response. Limiting the data in the response normally improves the APIs performance, ensuring that the request response doesn't take a long time or returning unnecessary data to the client.

## Steps

After completing the steps in this exercise you'll know what pagination in an API is, what a pagination token (`pageToken`) is used for and how pagination has been implemented in the SAP Ariba APIs.

To run the Python scripts included in this exercise, ensure that you checkout the prerequisites and recommendations documented in [prerequisites](prerequisites.md).

### 1. Get familiar with how pagination works

If you are not familiar with the concept of pagination in an API, I can bet you that you've used some form of pagination in the past. When interacting with a search engine, you've entered your search query, e.g. SAP Devtoberfest, click the :mag_right: button and the web page renders some results. It is possible that the search engine has thousands of results for your search query, but it will only display the first 10 results. If you want more results, you will need to click page :two:. Still unable to find what you are after, click page :three: and so on. Pagination in an API works pretty much the same but instead of clicking, we programatically specify which page we want to retrieve. The API response is batched and paginated, the results are returned one page at a time.

Unfortunately, there is no standard way APIs implement pagination but the process followed by APIs is very similar. The first request to the API, will only specify the data that the client is interested in. The API returns a subset of the data available and it informs the client that it has only returned part of the data. It does this by means of a "pagination token". This pagination token is returned in the response of the request, some APIs include it in the HTTP response headers, others in the body of the response. If the client is interested in retrieving more data, that matches its filtering criteria, it needs to include the pagination token in the request, to indicate to the API which page/batch of data it wants to retrieve.

:point_right: Navigate to the help documentation of the SAP Ariba [Analytical Reporting API](https://help.sap.com/viewer/bf0cde439a0142fbbaf511bfac5b594d/latest/en-US/1f8247e9e767438c8f18db7973779eaf.html) and get familiar with how we can invoke the Analytical Reporting [synchronous API](https://help.sap.com/viewer/bf0cde439a0142fbbaf511bfac5b594d/latest/en-US/e8f72dd7ea794de7b06417aa32e4524d.html). Ask yourself the following: how can I know the total number of records available for my request? How can I request additional batches of data? 

We'll use the diagram below to explain how pagination works in the SAP Ariba API that you'll be using in the exercise. In the case of the SAP Ariba API we will be using in this exercise, the pagination token is specified as a query parameter in the URL -> `?pageToken=[TOKEN_VALUE]`.

```
     +--------+                                                      +------------+
     |        |--(1)---     api.ariba.com/report?pageToken=       -->| Reporting  |
     |        |                                                      | Data (API) |
     |        |<-(2)----- Response includes PageToken: QmF0Y2gxCg ---|            |
     |        |                                                      +------------+
     |        |
     |        |                                                      +------------+
     |        |--(3)--- api.ariba.com/report?pageToken=QmF0Y2gxCg -->| Reporting  |
     | Client |                                                      | Data (API) |
     |        |<-(4)----- Response includes PageToken: MkJhdGNoCg ---|            |
     |        |                                                      +------------+
     |        |
     |        |                                                      +------------+
     |        |--(5)--- api.ariba.com/report?pageToken=MkJhdGNoCg -->| Reporting  |
     |        |                                                      | Data (API) |
     |        |<-(6)-----      No PageToken in response           ---|            |
     +--------+                                                      +------------+
```

Sequence:
1. The client sends a request to the API with no `pageToken`. Given that the `pageToken` is empty, the query parameter can be omitted.
2. The server response returns the first batch (page) of data and includes a PageToken (QmF0Y2gxCg) in the response body. The client will need to include this token in the next request if it wants to retrieve another batch of data.
3. The client sends the same request but it now includes the token sent by the server -> `pageToken=QmF0Y2gxCg`. 
4. The server response returns the second batch of data and includes a new PageToken (MkJhdGNoCg).
5. The client sends the same request but it now includes the second token retrieved -> `pageToken=MkJhdGNoCg`. 
6. The server response returns the third batch of data but this time there is no PageToken in the response, meaning that this is the last batch of data available for our request. No subsequent requests are needed to retrieve all the results.

By now, you should have a good understanding of the basic concepts around data pagination in an API and how you can paginate in the SAP Ariba API you'll be using in this exercise.

### 2. Set up the SAP Ariba API details in script

Enough of explanations, lets jump to some code. Before sending a request we will specify the application details previously shared by an SAP Ariba developer portal administrator in our environment configuration. In this step, we will use the [ariba_authentication.py](../07/scripts/ariba_authentication.py) script introduced in [exercise 07](../07/) to handle the authentication needed for this exercise.

The Python script uses [python-dotenv](https://pypi.org/project/python-dotenv/) to set some environment variable required by the script.

:point_right: Navigate to the scripts folder and create a file called `.env`. Copy and paste the sample config settings shown below and replace the values of `REALM`, `API_OAUTH_URL`, `API_KEY`, and `BASE64_AUTHSTRING` with the values provided by your administrator. 

> For a detailed explanation of the SAP Ariba developer portal, creating an application and its approval process, checkout the [SAP Ariba for developers YouTube playlist :tv:](https://www.youtube.com/watch?v=oXW3SBCadoI&list=PL6RpkC85SLQDXSLHrSPtu8wztzDs8kYPX)

### 3. Explore the [`ariba_pagination.py`](scripts/ariba_pagination.py) script

:point_right: Go to the scripts folder, open the [`ariba_pagination.py`](scripts/ariba_pagination.py) file and go through the code. 

The [`ariba_pagination.py`](scripts/ariba_pagination.py) script, included in this exercise, was created to facilitate completing the exercise. It retrieves the `access_token` from the credentials stored by the ariba_authentication.py script. The `access_token` will be used to retrieve data from the Analytical reporting API. To keep things simple, we will be using in this exercise the SourcingProjectSourcingSystemView template, which is available out of the box in SAP Ariba.

The script can be run in the following modes (by specifying the `--mode` parameter):
- `count`: Gets how many records will be included in the results set for a query using the SourcingProjectSourcingSystemView template.
- `paginate`: This is the default mode. Uses the previously retrieved access token to get a new access token from the OAuth server.


## Summary

You've made it to the end of this exercise :clap: :clap:. We've covered what pagination is and how it has been implemented in the SAP Ariba APIs. Also, we know where to look for a pagination token and how to include it in a request.


## Questions

1. Apart from a search engine, can you think of other application(s) where you've been presented with "pages" of data.
2. 