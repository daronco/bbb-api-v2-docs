# Overview

<aside class="warning">
  This is the specification of BigBlueButton's API version 2. This document is still not finished and might be largely modified before the API is released.
</aside>

## Goals and requirements

* It should be friendly to the developer and be explorable via a browser address bar.
* It should be simple, intuitive and consistent to make adoption not only easy but pleasant.
* It should be easy enough to add external modules that extend the API.

## API design rules

(Use this also as guidelines when extending the API.)

* Follow [http://jsonapi.org](jsonapi.org).
* JSON is used in all requests and responses.
* All responses and body of requests are in JSON and use the content type `application/vnd.api+json`.
* The authentication method uses HMAC-SHA256 to calculate the signature.
* The authentication method is based on OAuth 2.0. It supports a simple method when you have HTTPS, and a more complicated method which signs the request that can be used for both HTTP and HTTPS.
* API calls return JSON values instead of redirects. The body of the response indicates whenever it should redirect the user to another URL.
* The major version of the API is included in the URL, as in “/api/v2/”.
* API calls that update or create a resource return the resource in their response.
* CamelCase is used on URLs and returned parameters.
* API calls return pretty formatted data by default. gzip compression is supported to reduce the size of the responses.
* `PUT`, `PATCH` and `DELETE` are supported using the header `X-HTTP-Method-Override` and using the parameter `_method=PATCH` in the URL.
* Successful responses might include no data. Check for successful responses using the status code. Error responses will always have content with information about the error that happened.
* The API doesn't use sessions nor cookies.
* Parameters to filter or order results (e.g. “users=true”) can be sent in the URL or as part of the content of the request’s body. More complex parameters, such as the attributes of a resource being created (e.g. attributes of a meeting being created) should always be send in the body of the request.
* All times are in UTC, using always the ISO8601 format including the milliseconds. For example: "2012-01-01T12:00:00.000Z".


