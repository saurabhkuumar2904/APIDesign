  | Change Version | API Version | Change nots | Change Date | Author |
  | - | - | - | - | - |
  | 2.0 | v1 | Voice Bot API | 2022-2-22 | Leon，Carl，Page，Zack |  


# Summary

## Voice Bot API
The API is implemented by the Voice Bot API Module.
  - `POST /voicebot/voicebots/{VoicebotId}/sessions` - [Create Session](#create-a-new-voicebot-session)Voice Bot receives requests and creates session
  - `POST /voicebot/sessions/{sessionId}:receiveMessage` - [Receive Message](#receive-message)Voice Bot receives the message and gives the output message
  - `DELETE /voicebot/sessions/{sessionId}` - [Delete Session](#delete-a-voicebot-session) Voice Bot receives the sessionId and delete the session.
## PhoneNumber Service API  (Twillio PhoneNumber API ) 
The PhoneNumber Service API provides phone number manager services API.
  - `GET /phonenumber/phonenumbers` - [Get Phonenumbers](#Get-Phonenumbers)
  - `POST /phonenumber/phonenumbers` - [Create A New Phonenumber](#Create-A-New-Phonenumber)
  - `DELETE /phonenumber/phonenumbers/{phonenumber}` - [Remove a Phonenumber](#Remove-Phonenumber)
## Voice Bot Intent Engine Adapter API
Synchronize Voice Bot information to thirdparty Intent engine
  - `PUT /dialogflow/voicebots/{voicebotId}` - [UPDATE Voicebot](#Update-Voicebot)
  - `POST /dialogflow/voicebots/{voicebotId}` - [Create A New Voicebot](#Create-A-New-Voicebot)
  - `DELETE /dialogflow/voicebots/{voicebotId}` - [Remove a Voicebot](#Remove-a-Voicebot)
   
Synchronize Voice Bot Intents information to thirdparty Intent engine
  - `PUT /dialogflow/intents/{intentId}` - [UPDATE Intent](#Update-Intent)
  - `POST /dialogflow/intents/{intentId}` - [Create A New Intent](#Create-A-New-Intent)
  - `DELETE /dialogflow/intents/{intentId}` - [Remove a Intent](#Remove-a-Intent)
   
Synchronize Voice Bot Entities information to thirdparty Intent engine  
  - `PUT /dialogflow/entities/{entityId}` - [UPDATE Entities](#Update-Entities)
  - `POST /dialogflow/entities/{entityId}` - [Create A New Entities](#Create-A-New-Entities)
  - `DELETE /dialogflow/entities/{entityId}` - [Remove a Entities](#Remove-a-Entities)
  
## STT & TTS Adatper API
Provide STT(Speech To Text) and TTS(Text To Speech) capabilities.
  - `POST /stt/speech:recognize` - [Speech To Text](#Speech-To-Text)
  - `POST /tts/text:synthesize`  - [Text To Speech](#Text-To-Speech)
# Endpoints

### Create A New VoiceBot Session
`POST /voicebots/{VoicebotId}/sessions`

#### Parameters
Request body
The request body contains data with the follow structure:
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
The Response body contains data with the follow structure:

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

### Receive Message
`POST /sessions/{sessionId}:receiveMessage`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `sessionId` | Guid | yes  | Session id of the voice conversation |

Request body

The request body contains data with the follow structure:

  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `textInput` | string  | no | | Text input to voice robot |
  | `isLowSTTConfidence` | bool  | false | | We set a default STT Confidence Score for all Voice Bots in system level,When the STT score is lower than the Confidence Score,set true. |
  | `isTransferFailed` | bool  | false | | If the bot Transfer Chat to agent failed |


example:
```Json 
  {
    "textInput":"I want to buy NBN",
    "isLowSTTConfidence": "false",
    "isTransferFailed": "false",
  }
```

#### Response
the response is: [VoicebotOutput](#voicebotoutput-object) Object

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "textInput":"I want to buy NBN"
  }' -X POST https://domain.comm100.com/sessions/f9928d68-92e6-4487-a2e8-8234fc9d1f48:receiveMessage
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:application/json

  {    
    "id": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
    "content": [
          {
            "type":"playText",
            "content": {
              "message":"Hi, what can I do for you?",
            }
          }
        ]
  }
```
### SendMessage
`POST /sessions/{sessionId}:sendMessage` 

#### Parameters
No parameters

Request body

The request body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`sessionId` | Guid | the unique id of the session |
  |`content`  |  [VoiceBotAction](#voicebotaction-object)[]    |The list of Voice Bot Actions  |

example:
```Json 
  {    
          "sessionId":"d3f5b968-ad51-42af-b759-64c0afc40b84",
          "content":[{
              "type":"playText",
              "content":{
                    "message": "Hi there! I'm a Voice Bot, here to help answer your questions.",
              }
    }]
  }
```

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Description |     
  | - | - | - |  

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{    
          "sessionId":"d3f5b968-ad51-42af-b759-64c0afc40b84",
          "content":[{
              "type":"playText",
              "content":{
                    "message": "Hi there! I'm a Voice Bot, here to help answer your questions.",
              }
    }]
  }' -X POST https://domain.comm100.com/sipadapterapi/sessions/f9928d68-92e6-4487-a2e8-8234fc9d1f48:sendmessage
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

```

### delete a voicebot session
`DELETE /voicebot/sessions/{sessionId}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `sessionId` | Guid | yes  | Session id  |

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d
 -X Delete https://domain.comm100.com/voicebot/sessions/{sessionId}
```
Response
```Json
  HTTP/1.1 204 OK
  Content-Type:  application/json
  {  
  }
```

### Get Phonenumber
`GET /phonenumber/phonenumbers`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `CountryCode` | String | yes  |   |

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Description |     
  | - | - | - | 
  | `phoneNumberList`  | [PhoneNumber](#PhoneNumber-object)[] | - | 
  
  ### PhoneNumber Object
|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
  | `phoneNumber` | String ||sample: 604) xx0-8183  |  
  | `countryCode` | String | |two-letter country codes which are also used to create the ISO 3166-2 country subdivision codes and the Internet country code top-level domains.  |  

#### Example
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json
{
  "phoneNumberList":[
    {
      "phoneNumber": "604) xx0-8183",    
      "CountryCode": "US",    
    }
  ]
}
```

### Create A New Phonenumber
`POST /phonenumber/phonenumbers`

#### Parameters
No parameters

Request body

The request body contains data with the following structure:

  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `phoneNumber` | string  | yes | | |

example:
```Json 
{
  "phoneNumber": "(604) xx0-8183"
}
```

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Description |     
  | - | - | - | 
  | `phoneNumber` | String |   |  
#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
  "phoneNumber": "(604) xx0-8183"
}' -X POST https://domain.comm100.com/phonenumber/phonenumbers
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

{  
  "phoneNumber": "(604)xx0-8183",  
}
```


### Remove Phonenumber
`Delete /phonenumber/phonenumbers/{phonenumber}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `phonenumber` | String | yes  |  phone number |

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d
 -X Delete https://domain.comm100.com/twillioadapter/phonenumber/{sid}
```
Response
```Json
  HTTP/1.1 204 OK
  Content-Type:  application/json
  {  
  }
```

### Update Voicebot
`PUT /dialogflow/voicebots/{voicebotId}?Siteid={SiteId}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `voicebotId` | Guid | yes  | Voice Bot id  |
  | `SiteId` | Int | yes  | Site id  |
Request body

The request body contains data with the following structure:

  | Name  | Type | Description |     
  | - | - | - | 
  | `VoicebotImportRestoreRequest` | [VoicebotImportRestoreRequest](#VoicebotImportRestoreRequest) |  |  

example:
```Json 
{
  "VoicebotImportRestoreRequest": 
  {
    "Entities"
    [
      "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db"
    ],
	
    "Intents"
    [
      "e11203f5-1d97-4c48-8fc2-fc6257d4e13b"
    ]
  }
}
```

#### Response
No Response 

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
  "VoicebotImportRestoreRequest": 
  {
    "Entities"
    [
      "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db"
    ],
	
    "Intents"
    [
      "e11203f5-1d97-4c48-8fc2-fc6257d4e13b"
    ]
  }
}' -X PUT https://domain.comm100.com/dialogflow/voicebots/{voicebotId}?Siteid=1000`
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

{   
}
```

### Create A New Voicebot
`POST /dialogflow/voicebots/{voicebotId}?Siteid={SiteId}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `voicebotId` | Guid | yes  | Voice Bot id  |
  | `SiteId` | Int | yes  | Site id  |

example:
```Json 
{
}
```

#### Response
No Response 

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
}' -X POST https://domain.comm100.com/dialogflow/voicebots/{voicebotId}?Siteid=1000`
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

{   
}
```

### Remove a Voicebot
`DELETE /dialogflow/voicebots/{voicebotId}?Siteid={SiteId}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `voicebotId` | Guid | yes  | Voice Bot id  |
  | `SiteId` | Int | yes  | Site id  |
Request body

The request body contains data with the following structure:

  | Name  | Type | Description |     
  | - | - | - | 
  | `VoicebotImportRestoreRequest` | [VoicebotImportRestoreRequest](#VoicebotImportRestoreRequest) |  |  

example:
```Json 
{
  "VoicebotImportRestoreRequest": 
  {
    "Entities"
    [
      "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db"
    ],
	
    "Intents"
    [
      "e11203f5-1d97-4c48-8fc2-fc6257d4e13b"
    ]
  }
}
```

#### Response
No Response 

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
  "VoicebotImportRestoreRequest": 
  {
    "Entities"
    [
      "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db"
    ],
	
    "Intents"
    [
      "e11203f5-1d97-4c48-8fc2-fc6257d4e13b"
    ]
  }
}' -X DELETE https://domain.comm100.com/dialogflow/voicebots/{voicebotId}?Siteid=1000`
```
Response
```Json
  HTTP/1.1 204 OK
  Content-Type:  application/json
{   
}
```

### Update Intent
`PUT /dialogflow/Intents/{IntentId}?Siteid={SiteId}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `IntentId` | Guid | yes  | Intent id  |
  | `SiteId` | Int | yes  | Site id  |

#### Response
No Response 

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
}' -X PUT https://domain.comm100.com/dialogflow/Intents/{IntentId}?Siteid=1000`
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json
{   
}
```

### Create A New Intent
`POST /dialogflow/Intents/{IntentId}?Siteid={SiteId}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `IntentId` | Guid | yes  | Intent id  |
  | `SiteId` | Int | yes  | Site id  |

#### Response
No Response 

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
}' -X POST https://domain.comm100.com/dialogflow/Intents/{IntentId}?Siteid=1000`
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json
{   
}
```

### Remove a Intent
`DELETE /dialogflow/Intents/{IntentId}?Siteid={SiteId}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `IntentId` | Guid | yes  | Intent id  |
  | `SiteId` | Int | yes  | Site id  |
  | `VoicebotId` | Guid | yes  | Voice bot id  |

#### Response
No Response 

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
}' -X DELETE https://domain.comm100.com/dialogflow/Intents/{IntentId}?Siteid=1000&VoicebotId='f8383a83-48e9-4d0d-a3bd-fb19ce5c12db'`
```
Response
```Json
  HTTP/1.1 204 OK
  Content-Type:  application/json
{   
}
```

### Update Entities
`PUT /dialogflow/entities/{entityId}?Siteid={SiteId}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `entityId` | Guid | yes  | Intent id  |
  | `SiteId` | Int | yes  | Site id  |

#### Response
No Response 

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
}' -X PUT https://domain.comm100.com/dialogflow/entities/{entityId}?Siteid=1000`
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json
{   
}
```

### Create A New Entities
`POST /dialogflow/entities/{entityId}?Siteid={SiteId}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `entityId` | Guid | yes  | Intent id  |
  | `SiteId` | Int | yes  | Site id  |

#### Response
No Response 

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
}' -X POST https://domain.comm100.com/dialogflow/entities/{entityId}?Siteid=1000`
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json
{   
}
```

### Remove a Entities
`DELETE /dialogflow/entities/{entityId}?Siteid={SiteId}&VoicebotId={VoicebotId}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `entityId` | Guid | yes  | Intent id  |
  | `SiteId` | Int | yes  | Site id  |
  | `VoicebotId` | Guid | yes  | Voice bot id  |

#### Response
No Response 

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
}' -X DELETE https://domain.comm100.com/dialogflow/entities/{entityId}?Siteid=1000&VoicebotId='f8383a83-48e9-4d0d-a3bd-fb19ce5c12db'`
```
Response
```Json
  HTTP/1.1 204 OK
  Content-Type:  application/json
{   
}
```

### Speech To Text
Performs synchronous speech recognition: receive results after all audio has been sent and processed.

`POST /stt/speech:recognize`

#### Parameters
Request body
The request body contains data with the follow structure:
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `config` | [RecognitionConfig](#RecognitionConfig) | yes | |  Provides information to the recognizer that specifies how to process the request. |
  |`audio`  |  [RecognitionAudio](#RecognitionAudio)  |yes |   | The audio data to be recognized. |

example:
```Json 
  {
    "config":{
      "encoding": enum [(AudioEncoding)](#AudioEncoding),
      "sampleRateHertz": integer,
      "languageCode": string,
    },
    "audio": {
      // Union field audio_source can be only one of the following:
      "content": string,
      "uri": string
      // End of list of possible types for union field audio_source.
    },
  }
```

#### Response
The Response body contains data with the follow structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`results[]` | [SpeechRecognitionResult](#SpeechRecognitionResult) | Sequential list of transcription results corresponding to sequential portions of audio. |


#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '  {
    "config":{
      "encoding": enum [(AudioEncoding)](#AudioEncoding),
      "sampleRateHertz": integer,
      "languageCode": string,
    },
    "audio": {
      // Union field audio_source can be only one of the following:
      "content": string,
      "uri": string
      // End of list of possible types for union field audio_source.
    },
  }' -X POST https://domain.comm100.com/stt/speech:recognize
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json
  {    
    "results": [
      {
        
        "alternatives": [
          {
            "transcript": string,
            "confidence": number,
          }
        ],
        "languageCode": string
        
      }
    ]

  }
```



### Text To Speech
Synthesizes speech synchronously: receive results after all text input has been processed.

`POST /tts/text:synthesize`

#### Parameters
Request body
The request body contains data with the follow structure:
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `input` | [SynthesisInput](#SynthesisInput) | yes | |  The Synthesizer requires either plain text or SSML as input. |
  |`voice`  |  [VoiceSelectionParams](#VoiceSelectionParams)  |yes |   | The desired voice of the synthesized audio. |
  |`audioConfig`  |  [AudioConfig](#AudioConfig)  |yes |   | The configuration of the synthesized audio. |

example:
```Json 
  {
    "input": {
      // Union field input_source can be only one of the following:
      "text": string,
      "ssml": string
       // End of list of possible types for union field input_source.
    },
   "voice": {
      "languageCode": string,
      "name": string,
      "ssmlGender": enum ,
      "customVoice": {
        "model": string,
        "reportedUsage": enum
      }
    },
    "audioConfig":{
      "audioEncoding": enum ,
      "speakingRate": number,
      "pitch": number,
      "volumeGainDb": number,
      "sampleRateHertz": integer,
      "effectsProfileId": [
        string
      ]
}
}
```

#### Response
The Response body contains data with the follow structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`audioContent` | string  | The audio data bytes encoded as specified in the request, including the header for encodings that are wrapped in containers (e.g. MP3, OGG_OPUS). For LINEAR16 audio, we include the WAV header. Note: as with all bytes fields, protobuffers use a pure binary representation, whereas JSON representations use base64.A base64-encoded string. |
  |`audioConfig`  |  [AudioConfig](#AudioConfig)    | A link between a position in the original request input and a corresponding time in the output audio. It's only supported via <mark> of SSML input. |


#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "input": {
      // Union field input_source can be only one of the following:
      "text": string,
      "ssml": string
       // End of list of possible types for union field input_source.
    },
   "voice": {
      "languageCode": string,
      "name": string,
      "ssmlGender": enum ,
      "customVoice": {
        "model": string,
        "reportedUsage": enum
      }
    },
    "audioConfig":{
      "audioEncoding": enum ,
      "speakingRate": number,
      "pitch": number,
      "volumeGainDb": number,
      "sampleRateHertz": integer,
      "effectsProfileId": [
        string
      ]
}
}' -X POST https://domain.comm100.com/tts/text:synthesize
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json
  {
  "audioContent": string,
  "audioConfig": {
    "audioEncoding": enum ,
    "speakingRate": number,
    "pitch": number,
    "volumeGainDb": number,
    "sampleRateHertz": integer,
    "effectsProfileId": [
      string
    ]
  }
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

### PlayAudio Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`audioPath` | String|  | String  |

### PlayText Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`message` | String|  | String  |
  |`delayTime` | int |  | second  |

 ### CollectDTMFDigits Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`message` | String|  | String  |
  |`numberOfDigits` | String|  | Enumeration. 1, 2, … 29, 30, Variable. The number of digits entered by the caller in dialer. Default: Not sure.   |
  |`stopGatherAfterPresskey` | String|    | Enumeration. *, #. Available when Number of Digits is Not sure.|

### CollectSpeechResponse Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`message` | String|  | String  |
  |`lowSTTConfidenceMessage` | String|  | We set a default STT Confidence Score for all Voice Bots in system level, customers cannot change in this version.   |
  |`lowSTTConfidenceRepeatTimes` | Int|  | Available value: 0 - 9. Default: 2.   |
  |`isConfirmationRequired` | Bool|  | If the bot will reply to the answer to the caller to confirm.    |
  |`confirmationMessage` | String|  | Only available when Is Confirmation Required is “true”.   |
  |`confirmationText` | String|  | Visitor can speak the text to confirm the input. This text will not be read to visitors.   |
  |`confirmationKey` | int|  | Enumeration. 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, *, #. Visitor can press the key to confirm.   |

### TransferChat Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`transferTo` | String |  | Support Phone Number and URI.  |

### EndCall Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  
### IVRMenu Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`message` | String |  |  String |
    
### Visitor Object

|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
|`name` | string |  |  |
|`phone` | string |  |  |
|`state/province` | string |  |  |
|`country/region` | string |  |  |
|`city` | string |  |  |

### VoicebotImportRestoreRequest

|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
|`Entities` | Guid[] |  |  |
|`Intents` | Guid[] |  |  |

### RecognitionConfig
|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
|`encoding` | enum([AudioEncoding](#AudioEncoding-request))  |  | Encoding of audio data sent in all RecognitionAudio messages. This field is optional for FLAC and WAV audio files and required for all other audio formats. For details, see [AudioEncoding](#AudioEncoding-request). |
|`sampleRateHertz` | Int |  | Sample rate in Hertz of the audio data sent in all RecognitionAudio messages. Valid values are: 8000-48000. 16000 is optimal. For best results, set the sampling rate of the audio source to 16000 Hz. If that's not possible, use the native sample rate of the audio source (instead of re-sampling). This field is optional for FLAC and WAV audio files, but is required for all other audio formats. For details, see [AudioEncoding](#AudioEncoding-request). |
|`languageCode` | string |  | Required. The language of the supplied audio as a BCP-47 language tag. Example: "en-US". See Language Support for a list of the currently supported language codes. |

### AudioEncoding request
The encoding of the audio data sent in the request.

All encodings support only 1 channel (mono) audio, unless the audioChannelCount and enableSeparateRecognitionPerChannel fields are set.

For best results, the audio source should be captured and transmitted using a lossless encoding (FLAC or LINEAR16). The accuracy of the speech recognition can be reduced if lossy codecs are used to capture or transmit audio, particularly if background noise is present. Lossy codecs include MULAW, AMR, AMR_WB, OGG_OPUS, SPEEX_WITH_HEADER_BYTE, MP3, and WEBM_OPUS.

The FLAC and WAV audio file formats include a header that describes the included audio content. You can request recognition for WAV files that contain either LINEAR16 or MULAW encoded audio. If you send FLAC or WAV audio file format in your request, you do not need to specify an AudioEncoding; the audio encoding format is determined from the file header. If you specify an AudioEncoding when you send send FLAC or WAV audio, the encoding configuration must match the encoding described in the audio header; otherwise the request returns an google.rpc.Code.INVALID_ARGUMENT error code.
|Enums| | 
| - | - | 
|`ENCODING_UNSPECIFIED` | Not specified. | 
|`LINEAR16` | 	Uncompressed 16-bit signed little-endian samples (Linear PCM). | 
|`FLAC` | FLAC (Free Lossless Audio Codec) is the recommended encoding because it is lossless--therefore recognition is not compromised--and requires only about half the bandwidth of LINEAR16. FLAC stream encoding supports 16-bit and 24-bit samples, however, not all fields in STREAMINFO are supported. | 
|`MULAW` | 8-bit samples that compand 14-bit audio samples using G.711 PCMU/mu-law. | 
|`AMR` | Adaptive Multi-Rate Narrowband codec. sampleRateHertz must be 8000. | 
|`AMR_WB` | Adaptive Multi-Rate Wideband codec. sampleRateHertz must be 16000.| 
|`OGG_OPUS` | Opus encoded audio frames in Ogg container (OggOpus). sampleRateHertz must be one of 8000, 12000, 16000, 24000, or 48000. | 
|`SPEEX_WITH_HEADER_BYTE` | Although the use of lossy encodings is not recommended, if a very low bitrate encoding is required, OGG_OPUS is highly preferred over Speex encoding. The Speex encoding supported by Cloud Speech API has a header byte in each block, as in MIME type audio/x-speex-with-header-byte. It is a variant of the RTP Speex encoding defined in RFC 5574. The stream is a sequence of blocks, one block per RTP packet. Each block starts with a byte containing the length of the block, in bytes, followed by one or more frames of Speex data, padded to an integral number of bytes (octets) as specified in RFC 5574. In other words, each RTP header is replaced with a single byte containing the block length. Only Speex wideband is supported. sampleRateHertz must be 16000. | 
|`WEBM_OPUS` | Opus encoded audio frames in WebM container (OggOpus). sampleRateHertz must be one of 8000, 12000, 16000, 24000, or 48000. | 

### RecognitionAudio
Contains audio data in the encoding specified in the RecognitionConfig. Either content or uri must be supplied. Supplying both or neither returns google.rpc.Code.INVALID_ARGUMENT. See content limits.

|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
|`content` | string  |  | The audio data bytes encoded as specified in RecognitionConfig. Note: as with all bytes fields, proto buffers use a pure binary representation, whereas JSON representations use base64.A base64-encoded string. |
|`uri` | string |  | URI that points to a file that contains audio data bytes as specified in RecognitionConfig. The file must not be compressed (for example, gzip). Currently, only Google Cloud Storage URIs are supported, which must be specified in the following format: gs://bucket_name/object_name (other URI formats return google.rpc.Code.INVALID_ARGUMENT). For more information, see Request URIs. |

### SpeechRecognitionResult
A speech recognition result corresponding to a portion of the audio.
|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
|`alternatives` | [SpeechRecognitionAlternative](#SpeechRecognitionAlternative) |  | May contain one or more recognition hypotheses (up to the maximum specified in maxAlternatives). These alternatives are ordered in terms of accuracy, with the top (first) alternative being the most probable, as ranked by the recognizer. |
|`languageCode` | String |  | Output only. The BCP-47 language tag of the language in this result. This language code was detected to have the most likelihood of being spoken in the audio. |

### SpeechRecognitionAlternative
Alternative hypotheses (a.k.a. n-best list).

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`transcript` | String |  |  Transcript text representing the words that the user spoke. |
  |`confidence` | Number |  |  The confidence estimate between 0.0 and 1.0. A higher number indicates an estimated greater likelihood that the recognized words are correct. This field is set only for the top alternative of a non-streaming result or, of a streaming result where isFinal=true. This field is not guaranteed to be accurate and users should not rely on it to be always provided. The default of 0.0 is a sentinel value indicating confidence was not set. |


### SynthesisInput
Contains text input to be synthesized. Either text or ssml must be supplied. Supplying both or neither returns google.rpc.Code.INVALID_ARGUMENT. The input size is limited to 5000 characters.

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`text` | String |  |  The raw text to be synthesized. |
  |`ssml` | String |  |  The SSML document to be synthesized. The SSML document must be valid and well-formed.  For more information, see SSML. |

### VoiceSelectionParams
Description of which voice to use for a synthesis request.

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`languageCode` | String |  |  The language (and potentially also the region) of the voice expressed as a BCP-47 language tag, e.g. "en-US". This should not include a script tag (e.g. use "cmn-cn" rather than "cmn-Hant-cn"), because the script will be inferred from the input provided in the SynthesisInput. The TTS service will use this parameter to help choose an appropriate voice. Note that the TTS service may choose a voice with a slightly different language code than the one selected; it may substitute a different region (e.g. using en-US rather than en-CA if there isn't a Canadian voice available), or even a different language, e.g. using "nb" (Norwegian Bokmal) instead of "no" (Norwegian)". |
  |`name` | String |  |  The name of the voice. If not set, the service will choose a voice based on the other parameters such as languageCode and gender. |
  |`ssmlGender` | enum([SsmlVoiceGender](#SsmlVoiceGender)) |  |  The preferred gender of the voice. If not set, the service will choose a voice based on the other parameters such as languageCode and name. Note that this is only a preference, not requirement; if a voice of the appropriate gender is not available, the synthesizer should substitute a voice with a different gender rather than failing the request. |
  |`customVoice` | [CustomVoiceParams](#CustomVoiceParams) |  |  The configuration for a custom voice. If [CustomVoiceParams.model] is set, the service will choose the custom voice matching the specified configuration. |

### SsmlVoiceGender
Gender of the voice as described in SSML voice element.
|Enums| | 
| - | - | 
|`SSML_VOICE_GENDER_UNSPECIFIED` | An unspecified gender. In VoiceSelectionParams, this means that the client doesn't care which gender the selected voice will have. In the Voice field of ListVoicesResponse, this may mean that the voice doesn't fit any of the other categories in this enum, or that the gender of the voice isn't known. | 
|`MALE` | 	A male voice. | 
|`FEMALE` | A female voice. | 
|`NEUTRAL` | A gender-neutral voice. This voice is not yet supported. | 

### CustomVoiceParams
Description of the custom voice to be synthesized.

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`model` | String |  |  Required. The name of the AutoML model that synthesizes the custom voice.|
  |`reportedUsage` | enum([ReportedUsage](#ReportedUsage)) |  |  Optional. The usage of the synthesized audio to be reported. |


### ReportedUsage
The usage of the synthesized audio. You must report your honest and correct usage of the service as it's regulated by contract and will cause significant difference in billing.
|Enums| | 
| - | - | 
|`REPORTED_USAGE_UNSPECIFIED` | Request with reported usage unspecified will be rejected. | 
|`REALTIME` | 	For scenarios where the synthesized audio is not downloadable and can only be used once. For example, real-time request in IVR system. | 
|`OFFLINE` | For scenarios where the synthesized audio is downloadable and can be reused. For example, the synthesized audio is downloaded, stored in customer service system and played repeatedly. | 

### AudioConfig
Description of audio data to be synthesized.

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`audioEncoding` | enum([AudioEncoding](#AudioEncoding-Response)) |  |  Required. The format of the audio byte stream. |
  |`speakingRate` | number |  |  Optional. Input only. Speaking rate/speed, in the range [0.25, 4.0]. 1.0 is the normal native speed supported by the specific voice. 2.0 is twice as fast, and 0.5 is half as fast. If unset(0.0), defaults to the native 1.0 speed. Any other values < 0.25 or > 4.0 will return an error. |
  |`pitch` | number |  |  Optional. Input only. Speaking pitch, in the range [-20.0, 20.0]. 20 means increase 20 semitones from the original pitch. -20 means decrease 20 semitones from the original pitch. |
  |`volumeGainDb` | number |  |  Optional. Input only. Volume gain (in dB) of the normal native volume supported by the specific voice, in the range [-96.0, 16.0]. If unset, or set to a value of 0.0 (dB), will play at normal native signal amplitude. A value of -6.0 (dB) will play at approximately half the amplitude of the normal native signal amplitude. A value of +6.0 (dB) will play at approximately twice the amplitude of the normal native signal amplitude. Strongly recommend not to exceed +10 (dB) as there's usually no effective increase in loudness for any value greater than that. |
  |`sampleRateHertz` | int |  |  Optional. The synthesis sample rate (in hertz) for this audio. When this is specified in SynthesizeSpeechRequest, if this is different from the voice's natural sample rate, then the synthesizer will honor this request by converting to the desired sample rate (which might result in worse audio quality), unless the specified sample rate is not supported for the encoding chosen, in which case it will fail the request and return google.rpc.Code.INVALID_ARGUMENT. |
  |`effectsProfileId[]` | string |  |  Optional. Input only. An identifier which selects 'audio effects' profiles that are applied on (post synthesized) text to speech. Effects are applied on top of each other in the order they are given. See audio profiles for current supported profile ids. |

### Timepoint
This contains a mapping between a certain point in the input text and a corresponding time in the output audio.

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`markName` | string |  |  Timepoint name as received from the client within <mark> tag. |
  |`timeSeconds` | number |  |  Time offset in seconds from the start of the synthesized audio. |

### AudioEncoding Response
Configuration to set up audio encoder. The encoding determines the output audio format that we'd like.
|Enums| | 
| - | - | 
|`AUDIO_ENCODING_UNSPECIFIED` |Not specified. Will return result google.rpc.Code.INVALID_ARGUMENT. | 
|`LINEAR16` | 	Uncompressed 16-bit signed little-endian samples (Linear PCM). Audio content returned as LINEAR16 also contains a WAV header. | 
|`MP3` | MP3 audio at 32kbps. | 
|`MP3_64_KBPS` | MP3 at 64kbps. | 
|`OGG_OPUS` | Opus encoded audio wrapped in an ogg container. The result will be a file which can be played natively on Android, and in browsers (at least Chrome and Firefox). The quality of the encoding is considerably higher than MP3 while using approximately the same bitrate. | 
|`MULAW` | 8-bit samples that compand 14-bit audio samples using G.711 PCMU/mu-law. Audio content returned as MULAW also contains a WAV header. | 
|`ALAW` | 8-bit samples that compand 14-bit audio samples using G.711 PCMU/A-law. Audio content returned as ALAW also contains a WAV header. | 
