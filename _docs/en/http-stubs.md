---
title: HTTP Stubs
permalink: /docs/en/http-stubs
key: docs-http-stubs
---
When your API under test has a dependency on an external HTTP API, and you want to test your API in isolation, HTTP stub can help. Repoint the dependency to an HTTP stub, and test your API with it.

There might be reasons for testing your API in isolation
- You want to control the response of the dependency, but you have no control over the external HTTP API.
- The external HTTP API is unavailable or unstable when you test your API.

The same technique is also called mock service or service virtualization in the testing space.

The HTTP Stubs feature in API Test Base is powered by [WireMock](http://wiremock.org/).

## A quick play
Let's play with an HTTP stub quickly.

### Create a stub and load it
Create a temp test case. Go to its `HTTP Stubs` tab and click `+ Stub` button. On the stub edit view, select `POST` method, enter URL `/some/thing`, select request body pattern `Equal to JSON`, enter request body `{ "a": "b" }`, and enter response body `Hello!`.

![Quick Play Stub Details](../../screenshots/http-stubs/quick-play-stub-details.png)

The meaning of the stub is that when an HTTP POST request is sent to the mock server with URL `http://<APITestBaseHost>:<MockServerHTTPPort>/some/thing` (like http://localhost:8092/some/thing) and request body `{ "a": "b" }`, the mock server will return an HTTP response with status code 200 and response body `Hello!`.

The mock server is embedded in the API Test Base instance and is started when API Test Base starts.

To use HTTPS with the mock server, you can just use URL `https://<APITestBaseHost>:<MockServerHTTPSPort>/some/thing`.

The mock server has a default HTTP port number 8092 and a default HTTPS port number 8093. Both can be changed in the `<ATB_DATA_DIR>/config.properties`.

To manually load the stub into the mock server, go to the test case edit view > HTTP Stubs tab, and click the `Load All` button.

![Quick Play Stub List](../../screenshots/http-stubs/quick-play-stub-list.png)

### Test the stub
Go to the `Test Steps` tab of the test case, and create an HTTP test step with method `POST`, URL `http://localhost:8092/some/thing` and request body `{ "a": "b" }`. Click the `Invoke` button to invoke the stub, and you'll see a response body `Hello!`.

![Quick Play Stub Invocation](../../screenshots/http-stubs/quick-play-stub-invocation.png)

## Mock server status
To know what stubs have been loaded into the mock server, or check stub request log, open the mock server page by clicking the `Mock Server` link under the `HTTP Stubs` tab of the test case.

![Mock Server Status Page](../../screenshots/http-stubs/mock-server-status-page.png)

## Use HTTP stubs in automated API testing
It is easy. Create stubs in your test case like above, but no need to load them into mock server manually (via the Load All button). Every time you run the test case, below things happen automatically
- On test case run start, the mock server is reset and the stubs defined on the test case are loaded into the mock server.
    - Mock server reset means all stubs and all stub request logs are cleared from the mock server.
- On test case run end, stub requests received by the mock server are checked by asserting
    - All stubs in the test case have been hit.
    - If the 'Check Hit Order' option is selected under the HTTP Stubs tab on the test case edit view, the stubs have been hit in ascending order by stub number.
    - All stub requests received by the mock server have been matched (i.e. each request has hit a stub in the test case).

## Other supported features
- Setting a delay for response
- [Response templating](http://wiremock.org/docs/response-templating/)  
    Use attributes of the request in the response.
- [Regex matching on path and query](http://wiremock.org/docs/request-matching/#regex-matching-on-path-and-query)  
    Use this when your client request URL contains dynamic parts.
- [Stateful Behaviour](http://wiremock.org/docs/stateful-behaviour/)  
    This is useful when you want to ensure the stubs are hit in specified order.