# Users

## User Attributes


```json
{
    "userId": "user-1893-oakid",
    "uniqueUserId": "user-1893-oakid_1",
    "meetingId": "random-9826-kksu",
    "uniqueMeetingId": "e7f6899eb3f495d95b697a8b76a9279c2626a37e-1507664924353",
    "fullName": "Richard Alam",
    "role": "moderator",
    "avatarUrl": "https://my-server/my-avatar.png",
    "isPresenter": true,
    "voiceMode": "TwoWay",
    "voiceMethod": "WebRTC",
    "hasVideo": false,
    "clientType": "HTML5",
    "userAgent": "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:39.0) Gecko/20100101 Firefox/39.0",
    "locale": "en_US",
    "metadata": {
        "userAge": 35,
        "userRoleOnMoodle": "Professor - English 101"
    },
    "clients": {
        "WebFlash": {
            "url": "https://bigbluebutton.org/client.html?token=9dulgkl72kmy",
            "default": true
        },
        "HTML5": {
            "url": "https://bigbluebutton.org/html5?token=9dulgkl72kmy",
            "default": false
        },
        "Mobile": {
            "url": "https://bigbluebutton.org/mobile.html?token=9dulgkl72kmy",
            "default": false
        }
    }
}
```

Parameter       | Type    | Description
--------------- | ------- | -----------
fullName        | String  | The user’s full name.
role            | String  | The user’s role in the conference. One of: `moderator`, `attendee`.
userId          | String  | The user’s external identifier.
uniqueUserId    | String  | The user’s unique identifier.
meetingId       | String  | The identifier of the meeting the user is in.
uniqueMeetingId | String  | The unique identifier of the meeting the user is in.
avatarUrl       | String  | The link for the user's avatar to be displayed when the "displayAvatar" feature if on.
isPresenter     | Boolean | Whether the user is a presenter or not.
voiceMode       | String  | How the user has joined the voice conference. If the user is not in any type of voice conference, will return null. Otherwise it will be one of the values: "ListenOnly", "TwoWay".
voiceMethod     | String  | The method (mechanism, technology) used by the user to join the voice conference. If the user is not in any type of voice conference, will return null. Otherwise it will be one of the values: "WebRTC", "SIP", "Flash".
hasVideo        | Boolean | Whether the user is sharing his webcam.
clientType      | String  | A string defining the generic type of client the user is on. If the user hasn’t joined yet, it will be null. Supported values are: "WebFlash", "HTML5", "Mobile".
userAgent       | String  | The string with the "User-Agent" HTTP header sent by the user when joining the session, if any.
metadata        | Object  | A JSON object containing all the metadata information set when the user was created (or the metadata to be used when the user is being created). Will be an empty object ({}) if there is no metadata set.
clients         | Object  | A JSON object with the URLs for all the clients available for the user to join. Clients are indexed by their type and each client consists of: 1) a URL the user should be redirected to to effectively join the meeting; 2) a flag indicating whether the client is the default client or not (only one of the clients should have this flag set). Example: `{ "HTML5": { url: "https://bigbluebutton.org/html5?token=123123123", default: false }, "WebFlash": { url: "https://bigbluebutton.org/client.html?token=123123123", default: true }`.

## Create User


```shell
curl -X POST "https://example.com/api/v2/meetings"
  -H "Authorization: Bearer 6b3701cbbedb4ba88b79920d8c2955f2"
  -d '{ 
    "userId": "user-1893-oakid",
    "fullName": "Richard Alam",
    "role": "moderator",
    "avatarUrl": "https://my-server/my-image.jpg",
    "metadata": {
        "worksAt": "BigBlueButton",
        "roleOnMoodle": "Professor - Filipino 101"
    }
  }'
```

> Response:

```json
{
    "userId": "user-1893-oakid",
    "uniqueUserId": "user-1893-oakid_1",
    "meetingId": "random-9826-kksu",
    "uniqueMeetingId": "e7f6899eb3f495d95b697a8b76a9279c2626a37e-1507664924353",
    "fullName": "Richard Alam",
    "role": "moderator",
    "avatarUrl": "https://my-server/my-avatar.png",
    "isPresenter": true,
    "voiceMode": "TwoWay",
    "voiceMethod": "WebRTC",
    "hasVideo": false,
    "clientType": "HTML5",
    "userAgent": "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:39.0) Gecko/20100101 Firefox/39.0",
    "locale": "en_US",
    "metadata": {
        "userAge": 35,
        "userRoleOnMoodle": "Professor - English 101"
    },
    "clients": {
        "WebFlash": {
            "url": "https://bigbluebutton.org/client.html?token=9dulgkl72kmy",
            "default": true
        },
        "HTML5": {
            "url": "https://bigbluebutton.org/html5?token=9dulgkl72kmy",
            "default": false
        },
        "Mobile": {
            "url": "https://bigbluebutton.org/mobile.html?token=9dulgkl72kmy",
            "default": false
        }
    }
}
```

Create a user into a meeting previously created.

This API call will simply register a new user in the meeting, but the user will not yet join the meeting. It responds with a list of URLs for the available clients that the user can access to effectively join.

The client URLs have an access token encoded in them. This token will be unique and can be only used once. When the user accesses the client URL, it will validate the token and then invalidate it for future uses. The page rendered by this URL will be responsible for maintaining the user session in a way that the user is able to reconnect to the conference when necessary without having to access the client URL (the one that contains the token) again and without having to perform an API call again.

Expects a user in the body of the request with the attributes that will be used to create it.

### HTTP Request

`POST https://example.com/api/v2/meetings/:id/users`

### User attributes accepted

Parameter   | Required? | Default
----------- | --------- | -------
userId      | No        | A random string
fullName    | Yes       |
role        | Yes       |
avatarUrl   | No        |
locale      | No        | The meeting's locale
metadata    | No        | {}
