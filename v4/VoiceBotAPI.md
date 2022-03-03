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
## Dialog Flow Adapter API
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
`GET /phonenumbers`

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
  phoneNumberList:[
    {
      "phoneNumber": "604) xx0-8183",    
      "CountryCode": "US",    
    }
  ]
}
```

### Create A New Phonenumber
`POST /phonenumbers`

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
  | `sid` | String | Twilio phone number identification id   |  
#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
  "phoneNumber": "604) xx0-8183"
}' -X POST https://domain.comm100.com/twillioadapter/phonenumbers
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

{  
  "sid": "CA6cf29f28b03b9dbfad8ce758fa66xxx",  
}
```


### Remove Phonenumber
`Delete /phonenumber/{sid}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `sid` | String | yes  | Twilio phone number identification id  |

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

#### PlayAudio Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`audioPath` | String|  | String  |

#### PlayText Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`message` | String|  | String  |

 #### CollectDTMFDigits Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`message` | String|  | String  |
  |`numberOfDigits` | String|  | Enumeration. 1, 2, … 29, 30, Variable. The number of digits entered by the caller in dialer. Default: Not sure.   |
  |`stopGatherAfterPresskey` | String|    | Enumeration. *, #. Available when Number of Digits is Not sure.|

#### CollectSpeechResponse Object
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

#### TransferChat Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`transferTo` | String |  | Support Phone Number and URI.  |

#### EndCall Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  
#### IVRMenu Object
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
