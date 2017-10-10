# Meetings

## Meeting Attributes

```json
{
    "meetingId": "random-9826-kksu",
    "uniqueMeetingId": "e7f6899eb3f495d95b697a8b76a9279c2626a37e-1507664924353",
    "name": "Test",
    "createTime": "2015-09-05T12:25:03.520Z",
    "startTime": "2015-09-05T14:25:03.520Z",
    "endTime": "0",
    "voiceBridge": 76999,
    "dialNumber": "613-555-1234",
    "running": true,
    "duration": 120,
    "hasUserJoined": true,
    "recording": true,
    "hasBeenForciblyEnded": false,
    "userCount": 5,
    "moderatorCount": 2,
    "listenerCount": 4,
    "voiceUserCount": 3,
    "videoCount": 3,
    "maxUsers": 0,
    "welcome": "Welcome to %%CONFNAME%%! Dial %%DIALNUM%% to join the voice conference.",
    "moderatorOnlyMessage": "You’re a moderator, please use your powers wisely.",
    "logoutUrl": "https://my-server/my-logout-url",
    "autoStartRecording": false,
    "allowStartStopRecording": true,
    "locale": "en_US",
    "metadata": {
        "courseName": "English 101",
        "integrationName": "Moodle"
    }
}
```

Parameter              | Type     | Description
---------------------- | -------- | -----------
meetingId              | String   | The external meeting ID, as defined by an external application when the meeting was created via API.
meetingId              | String   | The unique meeting ID, as defined internally after the meeting was created. This ID is unique in this server, it will never be used for more than one meeting.
createTime             | DateTime | The time when the meeting was created on the server.
startTime              | DateTime | The time corresponding to when the meeting started (i.e. the first user joined the session). Will be null if the meeting hasn’t started yet.
endTime                | DateTime | The unix timestamp corresponding to the time the meeting ended (i.e. the meeting was removed from the server’s memory). Will be zero if the meeting hasn’t ended yet.
voiceBridge            | Integer  | The meeting’s voice bridge.
dialNumber             | String   | The meeting’s dial number.
running                | Boolean  | Whether the meeting is running or not.
duration               | Integer  | The duration set when the meeting was created. Defaults to "0".
hasUserJoined          | Boolean | Whether a user already joined the meeting or not.
recording              | Boolean | Whether the meeting has the recording capabilities on or not.
hasBeenForciblyEnded   | Boolean | Whether the meeting has been ended by a call to the "end" API call or not.
userCount              | Integer | The total number of users (i.e. total number of users minus the moderators) in the conference.
moderatorCount         | Integer | The total number of moderators (i.e. total number of users minus the users) in the conference.
listenerCount          | Integer | The number of users with the "listen only" mode active.
voiceUserCount         | Integer | The number of users sharing their microphone.
videoCount             | Integer | The number of users sharing their webcam.
maxUsers               | Integer | The maximum number of users allowed into the conference, as set when the meeting was created. If zero, it means it was not set.
welcome                | String  | The welcome message that will be shown in the chat to all users. When setting in a meeting, you can include keywords (`%%CONFNAME%%`, `%%DIALNUM%%`, `%%CONFNUM%%`) which will be substituted automatically.
moderatorOnlyMessage   | String  | A message that is displayed to all the moderators in the chat.
logoutURL              | String  | The URL that the BigBlueButton client will go to after users click the OK button on the 'You have been logged out message'.
autoStartRecording     | Boolean | This meeting will automatically starts recording when first user joins. NOTE: Don't set to autoStartRecording to false and allowStartStopRecording to false as the user won't be able to record.
allowStartStopRecording | Boolean | This meeting allows the user to start/stop recording. This means the meeting can start recording automatically (autoStartRecording=true) with the user able to stop/start recording from the client.
locale                  | String  | The default locale for the meeting (overrides the server's default).
metadata                | Object  | A JSON object containing all the metadata information set when the meeting was created (or the metadata to be used when the meeting is being created). Will be an empty object ({}) if there is no metadata set for the meeting.

## Get All Meetings

```shell
curl "https://example.com/api/v2/meetings?include=users"
  -H "Authorization: Bearer 6b3701cbbedb4ba88b79920d8c2955f2"
```

> Response:

```json
{
    "data": [{
        "type": "meeting",
        "id": "e7f6899eb3f495d95b697a8b76a9279c2626a37e-1507664924353",
        "attributes": {
            "meetingId": "random-9826-kksu",
            "uniqueMeetingId": "e7f6899eb3f495d95b697a8b76a9279c2626a37e-1507664924353",
            "name": "Test",
            "createTime": "2015-09-05T12:25:03.520Z",
            "startTime": "2015-09-05T14:25:03.520Z",
            "endTime": "0",
            "voiceBridge": 76999,
            "dialNumber": "613-555-1234",
            "running": true,
            "duration": 120,
            "hasUserJoined": true,
            "recording": true,
            "hasBeenForciblyEnded": false,
            "userCount": 5,
            "moderatorCount": 2,
            "listenerCount": 4,
            "voiceUserCount": 3,
            "videoCount": 3,
            "maxUsers": 0,
            "welcome": "Welcome to %%CONFNAME%%! Dial %%DIALNUM%% to join the voice conference.",
            "moderatorOnlyMessage": "You’re a moderator, please use your powers wisely.",
            "logoutUrl": "https://my-server/my-logout-url",
            "autoStartRecording": false,
            "allowStartStopRecording": true,
            "locale": "en_US",
            "metadata": {
                "courseName": "English 101",
                "integrationName": "Moodle"
            }
        },
        "relationships": {
            "users": {
                "data": { "type": "user", id: "user-1893-oakid_1" }
            }
        }
    }],
    "included": [{
        "type": "user",
        "id": "user-1893-oakid_1",
        "attributes": {
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
            }
        }
    }]
}
```

This endpoint retrieves all meetings currently created in the server. A meeting can be created but not running, so this can return also meetings that are not running.

### HTTP Request

`GET https://example.com/api/v2/meetings`

### Query Parameters

Parameter         | Required? | Default | Description
----------------- | --------- | ------- | -----------
filter[running]=  | No        |         | If set to true, returns only the meetings currently running. If set to false, only the meetings not running. If not set (default), returns both.
include=users     | No        | false   | If set, the result will also include the users currently in the meeting.


## Get Meeting


```shell
curl "https://example.com/api/v2/meetings/e7f6899eb3f495d95b697a8b76a9279c2626a37e-1507664924353"
  -H "Authorization: Bearer 6b3701cbbedb4ba88b79920d8c2955f2"
```

> Response:

```json
{
    "data": {
        "type": "meeting",
        "id": "e7f6899eb3f495d95b697a8b76a9279c2626a37e-1507664924353",
        "attributes": {
            "meetingId": "random-9826-kksu",
            "uniqueMeetingId": "e7f6899eb3f495d95b697a8b76a9279c2626a37e-1507664924353",
            "name": "Test",
            "createTime": "2015-09-05T12:25:03.520Z",
            "startTime": "2015-09-05T14:25:03.520Z",
            "endTime": "0",
            "voiceBridge": 76999,
            "dialNumber": "613-555-1234",
            "running": true,
            "duration": 120,
            "hasUserJoined": true,
            "recording": true,
            "hasBeenForciblyEnded": false,
            "userCount": 5,
            "moderatorCount": 2,
            "listenerCount": 4,
            "voiceUserCount": 3,
            "videoCount": 3,
            "maxUsers": 0,
            "welcome": "Welcome to %%CONFNAME%%! Dial %%DIALNUM%% to join the voice conference.",
            "moderatorOnlyMessage": "You’re a moderator, please use your powers wisely.",
            "logoutUrl": "https://my-server/my-logout-url",
            "autoStartRecording": false,
            "allowStartStopRecording": true,
            "locale": "en_US",
            "metadata": {
                "courseName": "English 101",
                "integrationName": "Moodle"
            }
        }
    }
}
```

Returns information about an specific meeting.

### HTTP Request

`GET https://example.com/api/v2/meetings/:id`

### URL Parameters

Parameter         | Required? | Default | Description
----------------- | --------- | ------- | -----------
:id               | Yes       |         | The unique ID of the meeting to retrieve
include=users     | No        | false   | If set, the result will also include the users currently in the meeting.


## Create Meeting


```shell
curl -X POST "https://example.com/api/v2/meetings"
  -H "Authorization: Bearer 6b3701cbbedb4ba88b79920d8c2955f2"
  -d '{ 
    "data": {
        "type": "meeting",
        "attributes": {
            "meetingId": "random-9826-kksu",
            "name": "Test",
            "voiceBridge": 76999,
            "dialNumber": "613-555-1234",
            "duration": 120,
            "recording": true,
            "welcome": "Welcome to %%CONFNAME%%! Dial %%DIALNUM%% to join the voice conference.",
            "moderatorOnlyMessage": "You’re a moderator, please use your powers wisely.",
            "logoutUrl": "https://my-server/my-logout-url",
            "autoStartRecording": false,
            "allowStartStopRecording": true,
            "locale": "en_US",
            "metadata": {
                "courseName": "English 101",
                "integrationName": "Moodle"
            }
        }
    }
  }'
```

> Response:

```json
{
    "data": {
        "type": "meeting",
        "id": "e7f6899eb3f495d95b697a8b76a9279c2626a37e-1507664924353",
        "attributes": {
            "meetingId": "random-9826-kksu",
            "uniqueMeetingId": "e7f6899eb3f495d95b697a8b76a9279c2626a37e-1507664924353",
            "name": "Test",
            "createTime": "2015-09-05T12:25:03.520Z",
            "startTime": "2015-09-05T14:25:03.520Z",
            "endTime": "0",
            "voiceBridge": 76999,
            "dialNumber": "613-555-1234",
            "running": true,
            "duration": 120,
            "hasUserJoined": true,
            "recording": true,
            "hasBeenForciblyEnded": false,
            "userCount": 5,
            "moderatorCount": 2,
            "listenerCount": 4,
            "voiceUserCount": 3,
            "videoCount": 3,
            "maxUsers": 0,
            "welcome": "Welcome to %%CONFNAME%%! Dial %%DIALNUM%% to join the voice conference.",
            "moderatorOnlyMessage": "You’re a moderator, please use your powers wisely.",
            "logoutUrl": "https://my-server/my-logout-url",
            "autoStartRecording": false,
            "allowStartStopRecording": true,
            "locale": "en_US",
            "metadata": {
                "courseName": "English 101",
                "integrationName": "Moodle"
            }
        }
    }
}
```

Creates a meeting. Expects a meeting in the body of the request with the attributes that will be used to create it.

### HTTP Request

`POST https://example.com/api/v2/meetings/`

### Meeting attributes accepted

Parameter               | Required? | Default
----------------------- | --------- | -------
meetingId               | Yes       |
name                    | Yes       |
recording               | No        | false
voiceBridge             | No        |
duration                | No        | 0
welcome                 | No        |
logoutUrl               | No        |
moderatorOnlyMessage    | No        |
autoStartRecording      | No        |
allowStartStopRecording | No        |
metadata                | No        | {}
