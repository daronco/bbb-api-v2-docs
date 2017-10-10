# Authentication

The authentication method is based on OAuth 2.0, with some extensions. There are two types of authentication supported:

1. Bearer (recommended): In this mode the Authorization HTTP header has the value `Bearer` followed by a space then the text value of the authentication token (shared secret). To use Bearer authentication, you **MUST** use an HTTPS connection.
2. MAC: This mode signs the request by using an HMAC algorithm keyed using the authentication token (shared secret). This is a modification of OAuth 2.0 to use a static shared secret rather than dynamic authentication tokens.

If you are using an insecure HTTP connection, you **MUST** use MAC authentication. You **MAY** use MAC authentication on an HTTPS connection, but Bearer is preferred since it's simpler.

## Bearer authentication

Simply include the server's shared secret in the `Authorization` header following the word "Bearer" and a space. Example: `Authorization: Bearer 6b3701cbbedb4ba88b79920d8c2955f2`.

## MAC authentication

This authentication method is based on [https://tools.ietf.org/html/draft-ietf-oauth-v2-http-mac-05](https://tools.ietf.org/html/draft-ietf-oauth-v2-http-mac-05). Note that this specification does not include support for protecting body content, so the following description extends the protection via the HTTP "Digest" header.

The core steps are as follows:

1. If the request contains a body, create an HTTP "Digest" header using the SHA-256 algorithm as specified in [http://tools.ietf.org/html/rfc5843](http://tools.ietf.org/html/rfc5843). For example: `Digest: SHA-256=MWVkMWQxYTRiMzk5MDQ[...]NTQwYzI2M2QwM2U2MQ==`.

2. Create the HTTP "Authentication" header according to [https://tools.ietf.org/html/draft-ietf-oauth-v2-http-mac-05#section-5](https://tools.ietf.org/html/draft-ietf-oauth-v2-http-mac-05#section-5), using only the following parameters:

Parameter | Value and description
---------- | -------
kid | "" (an empty string)
ts  | The timestamp
mac | You **MUST** use `hmac-sha-256`. The "key" value used in the hmac calculation is the authentication token (shared secret).
h   | `host:digest:content-type`

**Important**: **MUST NOT** include the parameter `access_token` in any request.

### MAC authentication Example

> 1: Initial header and body of a request to the API:

```http
POST /bigbluebutton/api/v1/meeting/Demo%20Meeting?running=false HTTP/1.1
Host: dev.bigbluebutton.org
Accept: application/json
Content-Type: application/json
Connection: close
User-Agent: Example User Agent
Content-Length: 76

{ "meetingId": "random-9826-kksu", "name": "My meeting" }
```

> 2: Calculate the SHA-256 digest of the body:

```sh
$ printf '{ "meetingId": "random-9826-kksu", "name": "My meeting" }\n' |
    openssl dgst -sha256 -binary | openssl enc -base64
XS+iykWgp5hI3MSy0/yIsvf7Z/iajin9w+A/HOd5VLo=
```

> 3: Request with the digest header:

```http
POST /bigbluebutton/api/v1/meeting/Demo%20Meeting?running=false HTTP/1.1
Host: dev.bigbluebutton.org
Accept: application/json
Content-Type: application/json
Connection: close
User-Agent: Example User Agent
Content-Length: 76
Digest: SHA-256=XS+iykWgp5hI3MSy0/yIsvf7Z/iajin9w+A/HOd5VLo=

{ "meetingId": "random-9826-kksu", "name": "My meeting" }
```

> 4: MAC input string:

```
POST /bigbluebutton/api/v1/meeting/Demo%20Meeting?running=false HTTP/1.1
dev.bigbluebutton.org
SHA-256=XS+iykWgp5hI3MSy0/yIsvf7Z/iajin9w+A/HOd5VLo=
application/json
1431102122
```

> 5: Calculate the MAC for the authorization header:

```sh
$ openssl dgst -sha256 -hmac '6b3701cbbedb4ba88b79920d8c2955f2' -binary \
    < mac_input.txt | openssl enc -base64
+p0UNFXe+1Z0E6yLxhAz+LfYUO0EG9z6o/hN1ZgAIe4=
```

> 6: Final request with the `Authorization` header:

```http
POST /bigbluebutton/api/v1/meeting/Demo%20Meeting?running=false HTTP/1.1
Host: dev.bigbluebutton.org
Accept: application/json
Content-Type: application/json
Connection: close
User-Agent: Example User Agent
Content-Length: 76
Digest: SHA-256=XS+iykWgp5hI3MSy0/yIsvf7Z/iajin9w+A/HOd5VLo=
Authorization: MAC kid="",
    ts=1431102122,
    h="host:digest:content-type",
    mac=+p0UNFXe+1Z0E6yLxhAz+LfYUO0EG9z6o/hN1ZgAIe4=

{ "meetingID": "random-9826-kksu", "name": "My meeting" }
```

This example is fabricated and may not represent a real request, but illustrates the full usage of this authentication method. For the purpose of calculating the body checksum, the body content in this example is terminated in a single unix newline. Relevant values used in this example are:

* Authorization token (shared secret): ASCII string `6b3701cbbedb4ba88b79920d8c2955f2`.
* Timestamp: `1431102122`, which represents `Fri May 8 16:22:02 UTC 2015`.

Steps to calculate the authentication headers:

1. Assume an initial request.
2. Calculate the SHA-256 digest of the body. This can be done on the unix command line with openssl.
3. Add the digest header to the request.
4. Create the MAC input string for the `Authorization` header. Since the value of the `h` parameter is `host:digest:content-type`, all of those headers (the host, the digest, and the content-type) must be included - once - if present in the message. A header being absent is not an error, and is skipped when generating the MAC input string. The order of fields is:
  * The first line of the HTTP request.
  * The value of each header field listed in the h parameter, in the order listed in that parameter. If a header is not present in the request, it is skipped.
  * The timestamp.
  * If present, the seq-nr field.
  * All lines (including the last line) are terminated with a unix newline `\n`.
5. The MAC for the Authorization header can then be generated from the input string using a unix command.
6. Add the HTTP `Authorization` header to the message using the parameters as specified above.


## Server-side validation

When doing server-side validation of authentication, ensure to validate the following parameters:

### For Bearer Authorization

Bearer Authorization **MUST NOT** be accepted on an insecure HTTP connection.

### For MAC Authorization

* The `h` parameter on the Authorization header **MUST** contain host, digest, content-type (in any order). It **MAY** contain additional header names.
* If the request type includes a body the Content-Type header **MUST** be `application/json` unless otherwise specified for that API endpoint.
* When using MAC authorization on a request type that includes a body, the Digest header **MUST** be included. If the Digest header is missing, return an authentication failure.
* The Digest header **MUST** include SHA-256. Other digest types **MAY** be included in addition to SHA-256.
* The server **MUST** reject requests where the `ts` parameter is too far in the past. A suggested maximum time delta is 30 seconds.
* The `seq-nr` parameter is not currently supported, and will be ignored. However, if present in a request, it **MUST** be included when calculating the authorization MAC.
