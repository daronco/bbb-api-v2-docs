# Recordings

## Recording Attributes


```json
{
    "recordingId": "183f0bf3a0982a127bdb8161-1383782781",
    "meetingId": "random-9826-kksu",
    "name": "On-line session for CS 101",
    "published": true
    "startTime": "2015-09-05T12:25:03.520Z",
    "endTime": "2015-09-05T14:25:03.520Z",
    "metadata": {
        "title": "Test Recording",
        "subject": "English 232 session",
        "description": "First Class",
        "creator": "Fred Dixon",
        "contributor": "Richard Alam",
        "language": "en_US",
    },
    "playbackFormats": []
}
```

Parameter       | Type     | Description
--------------- | -------- | -----------
recordingId     | String   | The recording identifier.
meetingId       | String   | The meeting identifier of the meeting that generated the recording.
name            | String   | The name of the recording, that is the same name given to the meeting that generated the recording.
startTime       | DateTime | The “startTime” of the meeting that generated the recording.
endTime         | DateTime | The “endTime” of the meeting that generated the recording.
metadata        | Object   | A JSON object containing all the metadata information set in the meeting that generated this recording. Will be an empty object ({}) if there is no metadata set for the meeting.
playbackFormats | Array    | An array of playback formats, see Playback Format Attributes.
