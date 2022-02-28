  | Change Version | API Version | Change nots | Change Date | Author |
  | - | - | - | - | - |
  | 2.0 | v1 | Voice Bot API | 2022-2-22 | Leon，Carl，Page，Zack |  



# Summary

## Voice Bot API
The API is implemented by the Voice Bot API Module.
  - `POST /voicebots/{VoicebotId}/sessions` - [Create session](#create-a-new-voicebot-session)Voice Bot receives requests and creates sessions
  - `POST /sessions/{sessionId}:recieveMessage` - [Recieved Message](#recieved-message)Voice Bot receives the message and gives the output message
## Twillio&Sip Adapter Call API
  Voice BOT actively sends messages to the Adapter，the API is implemented by the Adapter Module.
  - `POST /sessions/{sessionId}:sendMessage` - [SendMessage](#SendMessage) 
## Twillio PhoneNumber API  
The Twillio Adapter provides phone number registration services API.
  - `GET /phonenumber/available` - [Get Phonenumber](#Get-Phonenumber)
  - `POST /phonenumber` - [Create A New Phonenumber](#Create-A-New-Phonenumber)
  - `DELETE /phonenumber/{pathSid}/VoiceUrl` - [Remove  Phonenumber](#Remove-Phonenumber)

# Endpoints

### Create A New VoiceBot Session
`POST /voicebots/{VoicebotId}/sessions`

#### Parameters
Request body
The request body contains data with the follow structure:
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `VoiceBotId` | Guid | yes | |  the unique id of the bot |
  |`channel`  |  string  |yes |   | type of the response,including`Twilio` , `SIP` |
  |`visitor`  |  [Visitor](#visitor-object) Object  |no |   |  |

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

### Recieved Message
`POST /sessions/{sessionId}:recieveMessage`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `sessionId` | Guid | yes  |  id of the chat or conversation |

Request body

The request body contains data with the follow structure:

  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `textInput` | string  | no | | text |
  | `isLowSTTConfidence` | bool  | false | | We set a default STT Confidence Score for all Voice Bots in system level,When the STT score is lower than the Confidence Score,set true. |
  | `isTransferFailed` | bool  | false | | If the bot Transfer Chat to agent failed |


example:
```Json 
  {
    "textInput":"i want to buy NBN",
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
    "textInput":"i want to buy NBN"
  }' -X POST https://domain.comm100.com/sessions/f9928d68-92e6-4487-a2e8-8234fc9d1f48:recieveMessage
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

  {    
    "id": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
    "content": [
          {
            "type":"playText",
            "content": {
              "Message":"Hi, what can i do for you?",
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

The request body contains data with the follow structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`sessionId` | Guid | the unique id of the session |
  |`content`  |  [VoiceBotAction](#voicebotaction-object)[]    |  |

example:
```Json 
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

#### Response
The Response body contains data with the follow structure:

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
                    "message": "Hi there! I'm a VoiceBot, here to help answer your questions.",
              }
    }]
  }' -X POST https://domain.comm100.com/sipadapterapi/sessions/f9928d68-92e6-4487-a2e8-8234fc9d1f48:sendmessage
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

```

### Get Phonenumber
`GET /phonenumber/available`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `CountryCode` | String | yes  |   |

#### Response
The Response body contains data with the follow structure:

  | Name  | Type | Description |     
  | - | - | - | 
  | `phoneNumberList`  | PhoneNumber(#PhoneNumber-object)[] | - | 
  
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
`POST /phonenumber`

#### Parameters
No parameters

Request body

The request body contains data with the follow structure:

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
The Response body contains data with the follow structure:

  | Name  | Type | Description |     
  | - | - | - | 
  | `sid` | String | Twilio phone number identification id   |  
#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
  "phoneNumber": "604) xx0-8183"
}' -X POST https://domain.comm100.com/phonenumber
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
`Delete /phonenumber/{sid}/VoiceUrl`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `sid` | String | yes  | Twilio phone number identification id  |

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d
 -X Delete https://domain.comm100.com/phonenumber/{pathSid}/VoiceUrl
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
  |`numberOfDigits` | Int|  | Enumeration. 1, 2, … 29, 30, Variable. The number of digits entered by the caller in dialer. Default: Not sure.   |
  |`stopGatherAfterPresskey` | int|  | int  | Enumeration. *, #. Available when Number of Digits is Not sure.|

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
