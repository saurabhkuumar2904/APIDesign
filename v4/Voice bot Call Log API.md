
  | Change Version | API Version | Change nots | Change Date | Author |Architect Team Reviewer | 
  | - | - | - | - | - |- |
  | 1.0 | v1 | Voice bot Call Log API | 2022-7-25 | Leon |  Allon|

## Summary
### Voice Service Calllog API 
  - GET voiceservice/voiceCallLogs - [Get the list of Voice Call Log](#get-the-list-of-voice-call-log)
  - GET voiceservice/voicebotCallLogs/{id} - [Get a single Voice Call Log](#get-a-single-voice-call-log)
### Voice bot Calllog API 
  - GET voicebot/voicebotCallLogs - [Get the list of VoiceBot Call Log](#get-the-list-of-voicebot-call-log)
  - GET voicebot/voicebotCallLogs/{id} - [Get a single VoiceBot Call Log](#get-a-single-voicebot-call-log)

## Endpoints

### Get the list of Voice Call Log
  `GET voiceservice/voiceCallLogs`

- #### Parameters:
Request body 
The request body contains data with the follow structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `channel` | enum | yes | |   type of the channel, including `Twilio`, `SIP` |
  |`channelIdentifier`  |  string  |yes |   | The Unique ID corresponding to voicebotId,such as phone number，SIP URI  |

- #### Response:
An array of [Voice Call Log](#voicebot-call-log-json-format)

- #### Example
Sample Request:
```shell
curl https://api11.comm100.io/v4/voiceservice/voiceCallLogs \ 
    -X 'GET' \ 
    -H 'Authorization: Bearer {access_token}' \ 
```
Response:

   HTTP/1.1 200 OK
  ```json
  {
    "voiceCallLogs": [
       {
            "id": "eefe4538-bec3-47ef-89ea-879b59a16941",
            "from": "+184640930943",
            "to": "+184640930967",
            "voicebotId": "q3f5b438-xw31-44af-b729-64swaf3d0b56",
            "transferredTime": "",
            "status": "completed",
            "startTime": "2021-05-06T08:29:00.973Z",
            "callDuration": 10,
            "transferredTo":"",
            "recordingFileURL":"https://aws.amazone.com/eefe4538-bec3-47ef-89ea-879b59a16941.wav",
            "transcript": [{"role":"voiceBot","name":"Voice Bot","time":"00:00:02.3423523","text":"Hello, this is echo bot. I\"ll repeat what you say. You can start talking now.","type":"text"},
                              {"role":"visitor","name":"visitor","time":"00:00:05.3423523","text":"hello","type":"text"},
                              {"role":"voiceBot","name":"Voice Bot","time":"00:00:05.3423523","text":"hello","type":"text"},
                              {"role":"visitor","name":"+18448586997","time":"00:00:11.3423523","text":"nice to meet you","type":"text"},
                              {"role":"voiceBot","name":"Voice Bot","time":"00:00:11.3423523","text":"nice to meet you","type":"text"},
                              {"role":"visitor","name":"+18448586997","time":"00:00:15.3423523","text":"goodbye","type":"text"},
                              {"role":"voiceBot","name":"Voice Bot","time":"00:00:15.3423523","text":"goodbye","type":"text"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingStarted"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingPaused"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingResumed"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingStopped"},
                              {"role":"visitor","name":"+18448586997","time":"00:00:15.3423523","text":"1234","type":"dtmf"}]
        }
    ],
    "nextPage": null,
    "previousPage": null,
    "total": 1
  } 
  ```
### Get a single Voice Call Log
  `GET voiceservice/voiceCallLogs/{id}`

- #### Parameters:
Path parameters 
  | Name | Type | Required | Description |    
  | - | - | :-: | - | 
  | `id` | string |  | Original Call ID |


- #### Response:
[VoiceBot Call Log](#voicebot-call-log-json-format)

- #### Example
Sample Request:
```shell
curl https://api11.comm100.io/v4/voiceservice/voiceCallLogs/eefe4538-bec3-47ef-89ea-879b59a16941 \ 
    -X 'GET' \ 
    -H 'Authorization: Bearer {access_token}' \ 
```
Response:

   HTTP/1.1 200 OK
  ```json
       {
            "id": "eefe4538-bec3-47ef-89ea-879b59a16941",
            "from": "+184640930943",
            "to": "+184640930967",
            "voicebotId": "q3f5b438-xw31-44af-b729-64swaf3d0b56",
            "transferredTime": "",
            "status": "completed",
            "startTime": "2021-05-06T08:29:00.973Z",
            "callDuration": 10,
            "transferredTo":"",
            "recordingFileURL":"https://aws.amazone.com/eefe4538-bec3-47ef-89ea-879b59a16941.wav",
            "transcript": [{"role":"voiceBot","name":"Voice Bot","time":"00:00:02.3423523","text":"Hello, this is echo bot. I\"ll repeat what you say. You can start talking now.","type":"text"},
                              {"role":"visitor","name":"visitor","time":"00:00:05.3423523","text":"hello","type":"text"},
                              {"role":"voiceBot","name":"Voice Bot","time":"00:00:05.3423523","text":"hello","type":"text"},
                              {"role":"visitor","name":"+18448586997","time":"00:00:11.3423523","text":"nice to meet you","type":"text"},
                              {"role":"voiceBot","name":"Voice Bot","time":"00:00:11.3423523","text":"nice to meet you","type":"text"},
                              {"role":"visitor","name":"+18448586997","time":"00:00:15.3423523","text":"goodbye","type":"text"},
                              {"role":"voiceBot","name":"Voice Bot","time":"00:00:15.3423523","text":"goodbye","type":"text"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingStarted"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingPaused"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingResumed"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingStopped"},
                              {"role":"visitor","name":"+18448586997","time":"00:00:15.3423523","text":"1234","type":"dtmf"}]
        }
  ```

### Get the list of VoiceBot Call Log
  `GET voicebot/voicebotCallLogs`

- #### Parameters:
Request body 
The request body contains data with the follow structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `voicebotId` | Guid | yes | |  Voicebot ID |
  | `from` | string | no | |  Visit phone number |
  | `dateRange` | datetime | no | | the search starttime of calllogs |  
  | `dateRange` | datetime | no | |  the search endtime of calllogs  |  
  | `keywords` | string | no | | the search keyword of transript，Calling From/To and Transferred To |
- #### Response:
An array of [VoiceBot Call Log](#voicebot-call-log-json-format)

- #### Example
Sample Request:
```shell
curl https://api11.comm100.io/v4/voicebot/voicebotCallLogs \ 
    -X 'GET' \ 
    -H 'Authorization: Bearer {access_token}' \ 
```
Response:

   HTTP/1.1 200 OK
  ```json
  {
    "voicebotCallLogs": [
       {
            "id": "eefe4538-bec3-47ef-89ea-879b59a16941",
            "from": "+184640930943",
            "to": "+184640930967",
            "voicebotId": "q3f5b438-xw31-44af-b729-64swaf3d0b56",
            "transferredTime": "",
            "status": "completed",
            "startTime": "2021-05-06T08:29:00.973Z",
            "callDuration": 10,
            "transferredTo":"",
            "recordingFileURL":"https://aws.amazone.com/eefe4538-bec3-47ef-89ea-879b59a16941.wav",
            "transcript": [{"role":"voiceBot","name":"Voice Bot","time":"00:00:02.3423523","text":"Hello, this is echo bot. I\"ll repeat what you say. You can start talking now.","type":"text"},
                              {"role":"visitor","name":"visitor","time":"00:00:05.3423523","text":"hello","type":"text"},
                              {"role":"voiceBot","name":"Voice Bot","time":"00:00:05.3423523","text":"hello","type":"text"},
                              {"role":"visitor","name":"+18448586997","time":"00:00:11.3423523","text":"nice to meet you","type":"text"},
                              {"role":"voiceBot","name":"Voice Bot","time":"00:00:11.3423523","text":"nice to meet you","type":"text"},
                              {"role":"visitor","name":"+18448586997","time":"00:00:15.3423523","text":"goodbye","type":"text"},
                              {"role":"voiceBot","name":"Voice Bot","time":"00:00:15.3423523","text":"goodbye","type":"text"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingStarted"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingPaused"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingResumed"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingStopped"},
                              {"role":"visitor","name":"+18448586997","time":"00:00:15.3423523","text":"1234","type":"dtmf"}]
        }
    ],
    "nextPage": null,
    "previousPage": null,
    "total": 1
  } 
  ```
### Get a single VoiceBot Call Log
  `GET voicebot/voicebotCallLogs/{id}`

- #### Parameters:
Path parameters 
  | Name | Type | Required | Description |    
  | - | - | :-: | - | 
  | `id` | string |  | Voicebot Session ID |
  
Request body 
The request body contains data with the follow structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `voicebotId` | Guid | yes | |  Voicebot ID |

- #### Response:
[VoiceBot Call Log](#voicebot-call-log-json-format)

- #### Example
Sample Request:
```shell
curl https://api11.comm100.io/v4/voicebot/voicebotCallLogs/eefe4538-bec3-47ef-89ea-879b59a16941 \ 
    -X 'GET' \ 
    -H 'Authorization: Bearer {access_token}' \ 
```
Response:

   HTTP/1.1 200 OK
  ```json
       {
            "id": "eefe4538-bec3-47ef-89ea-879b59a16941",
            "from": "+184640930943",
            "to": "+184640930967",
            "voicebotId": "q3f5b438-xw31-44af-b729-64swaf3d0b56",
            "transferredTime": "",
            "status": "completed",
            "startTime": "2021-05-06T08:29:00.973Z",
            "callDuration": 10,
            "transferredTo":"",
            "recordingFileURL":"https://aws.amazone.com/eefe4538-bec3-47ef-89ea-879b59a16941.wav",
            "transcript": [{"role":"voiceBot","name":"Voice Bot","time":"00:00:02.3423523","text":"Hello, this is echo bot. I\"ll repeat what you say. You can start talking now.","type":"text"},
                              {"role":"visitor","name":"visitor","time":"00:00:05.3423523","text":"hello","type":"text"},
                              {"role":"voiceBot","name":"Voice Bot","time":"00:00:05.3423523","text":"hello","type":"text"},
                              {"role":"visitor","name":"+18448586997","time":"00:00:11.3423523","text":"nice to meet you","type":"text"},
                              {"role":"voiceBot","name":"Voice Bot","time":"00:00:11.3423523","text":"nice to meet you","type":"text"},
                              {"role":"visitor","name":"+18448586997","time":"00:00:15.3423523","text":"goodbye","type":"text"},
                              {"role":"voiceBot","name":"Voice Bot","time":"00:00:15.3423523","text":"goodbye","type":"text"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingStarted"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingPaused"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingResumed"},
                              {"role":"system","name":"","time":"00:00:15.3423523","text":"","type":"recordingStopped"},
                              {"role":"visitor","name":"+18448586997","time":"00:00:15.3423523","text":"1234","type":"dtmf"}]
        }
  ```


# Model
### VoiceBot Call Log Json Format
VoiceBot Call Log is represented as simple flat JSON objects with the following keys:
|Name|Type|Description|
|:---|:---|:----------|
|`id`|guid|Id of the Voicebot Call Log.|
|`from`|string|Phone number or SIP URI.|
|`to`|string|Phone number or SIP URI.|
|`voicebotId`|guid|Id of the Voicebot.|
|`transferredTime`|datetime|only happens when a call is transferred.|
|`status`|enum|Allowed values are `completed`, `transferred`.|
|`startTime`|datetime|Start Time.|
|`callDuration`|integer|Call duration, expressed in seconds|
|`transferredTo`|string|Id of the Voicebot Call Log.|
|`recordingFileURL`|string|The recording file will be compressed into MP3 format to save space, and uploaded to the S3 storage server of AWS, and the URL will be saved in the field.|
|`transcript`|[TextRecord](#voicebot-text-record-object)[]|JSON format. The text record list of this call, including the content, the start time, and the speaker of each sentence.|

### VoiceBot Text Record Json Format
VoiceBot Call Log is represented as simple flat JSON objects with the following keys:
|Name|Type|Description|
|:---|:---|:----------|
|`role`|enum|The role that generated this record.Allowed values are `voiceBot`, `visitor`, `system`|
|`name`|string|The name of role `Voice Bot` or user `phonenumber` or user `sipuri`|
|`text`|string|Text record content.|
|`time`|time|start time.|
|`type`|enum|Allowed values are `recordingStarted`, `recordingPaused`, `recordingResumed`, `recordingStopped`，`text`，`dtmf`.|
