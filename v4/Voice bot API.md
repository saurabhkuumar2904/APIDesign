  | Change Version | API Version | Change nots | Change Date | Author |
  | - | - | - | - | - |
  | 2.0 | v1 | Voice Bot API | 2022-3-18 | Leon，Carl，Page，Zack |  


# Summary

## Channel Adapter API 
The ChannelURI can be any valid URI that implements this API, and it is configured in the system when a new channel needs to access. 
  - POST /{channelURI}/input - Channel Adapter receives input. 
## Voice Service API  
  - POST /voiceservice/sessions - Create Session. Voice service creates a session 
  - POST /voiceservice/sessions/{sessionId}/questions - Receive a question.  Voice service receives call question.  
  - DELETE /voiceservice/sessions/{sessionId} - Delete a session. Voice service deletes a session. 
  - POST /voiceservice/sessions/{sessionId}/variables - Receive the variables of the Voice Bot Session  
## Voice Service Input API   
  - POST /voiceservice/sessions/{sessionId}/answers - Voice Service receives answers. 
## Voice Bot Service API 
  - POST /voicebot/voicebots/{VoicebotId}/sessions  - Create Session Voice Bot creates session 
  - POST /voicebot/sessions/{sessionId}/messages - Receive an input. Voice Bot receives input.    
  - DELETE /voicebot/sessions/{sessionId} - Delete Session Voice Bot deletes the session. 
  - POST /voicebot/sessions/{sessionId}/variables - Receive the variables of the Voice Bot Session  
## STT & TTS API 
Provide STT (Speech to Text) and TTS (Text to Speech) capabilities. 
  - POST /stttts/stt:speechToText - Speech To Text 
  - POST /stttts/tts:textToSpeech - Text To Speech 


# Endpoints

### Create A New VoiceBot Session
`POST /voicebots/{VoicebotId}/sessions`

#### Parameters
Request body
The request body contains data with the following structure:
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `voiceBotId` | Guid | yes | |  the unique id of the voice bot |
  |`channel`  |  string  |yes |   | type of the response,including `Twilio` , `SIP` |
  |`visitor`  |  [Visitor](#visitor-object) Object  |no |   | visitor information |

example:
```Json 
  {
    "VoiceBotId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
    "channel": "Twillio",
    "visitor": {
        "phone":"123-4355-212",
      }
    }
  }
```

#### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`sessionId` | Guid | the unique id of the session |
  |`content`  |  [VoiceBotAction](#voicebotaction-object)[]    |  |


#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "VoiceBotId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48", 
    "visitor": {
      "phone":"123-4355-212",
    }
  }' -X POST https://domain.comm100.com/api/voicebots/{VoicebotId}/sessions
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

  {    
          "sessionId":"d3f5b968-ad51-42af-b759-64c0afc40b84",
          "content":[{
              "type":"playText",
              "content":{
                    "message": "Hi there! I'm a VoiceBot, here to help answer your questions.",
              }
    }]

  }
```
# Model
### VoicebotOutput Object
  
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `id` | Guid  | | |
  |`content`  |  [VoiceBotAction](#voicebotaction-object)[]    |  |

### VoiceBotAction Object
  Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`type` | string | | type of the response,including `PlayAudio`,`PlayText`,`CollectDTMFDigits`,`CollectSpeechResponse`,`IVRMenu`,`TransferChat`, `EndCall`|
  | `content` | object | |  response's content. when type is `PlayAudio`, it represents [PlayAudio](#PlayAudio-object); when type is `PlayText`,it represents [PlayText](#PlayText-object);when type is `CollectDTMFDigits`,it represents [CollectDTMFDigits](#CollectDTMFDigits-object); when type is `CollectSpeechResponse`, it represents [CollectSpeechResponse](#CollectSpeechResponse-object);when type is `EndCall`, it represents [EndCall](#EndCall-object);when type is `IVRMenu`, it represents [IVRMenu](#IVRMenu-object);when type is `TransferChat`, it represents [TransferChat](#TransferChat-object);|
