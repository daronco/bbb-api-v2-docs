# Errors

Errors are returned as a JSON in the format seen beside.

```json
{
    “error”: {
        "code" : 1234,
        "message" : "Something bad happened :(",
        "description" : "More details about the error here"
    }
}
```

List of possible errors:

Error Code | Message
---------- | -------
123 | Something wrong.
321 | Something else also wrong.

List of possible HTTP status codes on error:

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request sucks.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- The kitten requested is hidden for administrators only.
404 | Not Found -- The specified kitten could not be found.
405 | Method Not Allowed -- You tried to access a kitten with an invalid method.
406 | Not Acceptable -- You requested a format that isn't json.
410 | Gone -- The kitten requested has been removed from our servers.
418 | I'm a teapot.
429 | Too Many Requests -- You're requesting too many kittens! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
