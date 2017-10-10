# To Be Defined

* Accept the parameters in the URL too, makes it easier for an API Mate-like tool. Possible with Bearer or only HMAC?
* Amazon's AWS authentication is easier and maybe better.
* Options to return or not return nested resources
* Easily extendable, how?
* Guildelines on how to add parameters, use more metadata
* Use more internalMeetingId and internalUserId as unique identifiers
* jsonapi.org
* Mudar o camelcase?
* Specify possible metadata values/format
* Accommodate new requirements (e.g. upload presentation to a running session)
* Sort options, attribute filtering options, pagination (see https://bestbuyapis.github.io/api-documentation/#sort)

* In case a more specific version is required, include it in the headers. (See [1] and [11]: “I'm a big fan of the approach that Stripe has taken to API versioning - the URL has a major version number (v1), but the API has date based sub-versions which can be chosen using a custom HTTP request header. In this case, the major version provides structural stability of the API as a whole while the sub-versions accounts for smaller changes (field deprecations, endpoint changes, etc).”)
* Use the correct HTTP status code for success and error responses. (Define which)
* No URL parameters that require percent encoding or allow arbitrary user input are used. If necessary, percent encode to encode parameters and values passed in the URL. Calculate the checksum after percent encoding the values. (Use normalized encoding method defined in OAuth 1a)
