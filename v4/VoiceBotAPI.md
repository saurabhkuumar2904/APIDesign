  | Change Version | API Version | Change nots | Change Date | Author |
  | - | - | - | - | - |
  | 2.0 | v1 | Voice Bot API | 2022-2-22 | Leon |  
  | 2.0 | v1 | Voice Bot API | 2022-2-23 | Carl |  
 



# Summary
  - Voicebot
    - [VoiceBotSession](#VoiceBotsession-object)
## Voice Bot API
  - `POST /voicebots/{VoicebotId}/sessions` - [Create session](#Create-A-New-VoiceBot-Session)
  - `POST /sessions/{sessionId}:recieveMessage` - [Recieved Message](#Recieved-Message)
## Twillio Adapter  
  - `GET /phonenumber/available` - [Get Phonenumber](#Get-Phonenumber)
  - `POST /phonenumber` - [Create A New Phonenumber](#Create-A-New-Phonenumber)
  - `PUT /phonenumber/{pathSid}` - [Update Phonenumber](#Update-Phonenumber)
  - `DELETE /phonenumber/{pathSid}/VoiceUrl` - [Remove  Phonenumber](#Remove-Phonenumber)
## Sip Adapter



# Endpoints

### Create A New VoiceBot Session
`POST /voicebots/{VoicebotId}/sessions`

#### Parameters
Request body

The request body contains data with the follow structure:

  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `VoiceBotId` | Guid | yes | |  the unique id of the bot |
  |`visitor`  |  [Visitor](#visitor-object) Object  |no |   |  |

example:
```Json 
  {
    "VoiceBotId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
    "channel": "LiveChat",
    "visitor": {
        "name":"Kart",
        "email":"kart@yahoo.com",
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
  |`greeting`  |  [VoiceBotOutput](#VoiceBotoutput-object) Object    |  |


#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "VoiceBotId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48", 
    "visitor": {
      "name":"Kart",
      "email":"kart@yahoo.com",
      "phone":"123-4355-212",
    }
  }' -X POST https://domain.comm100.com/api/v4/voicebots/{VoicebotId}/sessions
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

  {    
    "sessionId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
    "greeting":{
          "id":"d3f5b968-ad51-42af-b759-64c0afc40b84",
          "content":[{
              "type":"VoiceBotActionSendMessage",
              "content":{
                      "VoiceBotActionSendMessageLinks": [],
                    "message": "Hi there! I'm a VoiceBot, here to help answer your questions.",
                    "nextActionId": "00000000-0000-0000-0000-000000000000",
                    "typingDelay": 1
              }
    }]
    }

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
  | `channel` | string  | yes | | eg: `Live Chat`, `Facebook Messenger`, `Twitter Direct Message`, `WeChat`, `WhatsApp`, `SMS`, `Voice` |
  | `textInput` | string  | no | | text |

example:
```Json 
  {
    "channel":"Facebook Messenger",
    "textInput":"i want to buy NBN",
    "context": {
      "VoicebotId": "a9928d68-92e6-4487-a2e8-8234fc9d1f48",
      "customData": {
        "name":"Kart",
        "email":"kart@yahoo.com",
        "phone":"123-4355-212",
      }
    },
  }
```

#### Response
the response is: [VoicebotSession](#VoicebotSession-Object) Object

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "channel":"Facebook Messenger",
    "textInput":"i want to buy NBN",
    "context": {
      "VoicebotId": "a9928d68-92e6-4487-a2e8-8234fc9d1f48",
      "customData": {
        "name":"Kart",
        "email":"kart@yahoo.com",
        "phone":"123-4355-212",
      }
    },
  }' -X POST https://domain.comm100.com/sessions/f9928d68-92e6-4487-a2e8-8234fc9d1f48:detectIntent
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

  {    
    "channel":"Facebook Messenger",
    "id": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
    "context": {
      "VoicebotId": "a9928d68-92e6-4487-a2e8-8234fc9d1f48",
      "currentIntentId": "8d6892e6-92e6-4487-a2e8-92e68d6892e6",
      "customData": {
        "name":"Kart",
        "email":"kart@yahoo.com",
        "phone":"123-4355-212",
      }
    },
    "message":{
      "id":"d3f5b968-ad51-42af-b759-64c0afc40b84",
      "visitorQuestion":"i want to buy NBN",
      "type":"highConfidenceAnswer",
      "content":{
        "intentId": "7534fdsca-92e6-4487-a2e8-92e68d6892e6",
        "intentName": "buy nbn",
        "score": 89.25,
        "responses":[
          {
            "type":"htmlText",
            "content": {
              "text":"<div>Hi, what can i do for you?</div>",
            }
          },
          {
            "type":"image",
            "content": {
              "name":"greeting.png",
              "url": "https://bot.comm100.com/botapi/images/greeting.png"
            }
          }
        ]
      }
    }    
  }
```

### Get Phonenumber
`GET /phonenumber/available`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `pathCountryCode` | String | yes  |   |

#### Response
The Response body contains data with the follow structure:

  | Name  | Type | Description |     
  | - | - | - |
  | `friendlyName` | String |   |
  | `phoneNumber` | String |   |
  | `lata` | String |   |
  | `locality` | bool |   |
  | `rateCenter` | String |   |
  | `latitude` | String |   |
  | `region` | String |   |
  | `postalCode` | String |   |
  | `isoCountry` | String |   |
  | `addressRequirements` | String |   |
  | `beta` | bool |   |
  | `longitude` | bool |   |
  | `capabilities` | [capabilities](#capabilities-Object) |   |
#### Example
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

[
  {
    "friendlyName": {},
    "phoneNumber": {},
    "lata": "string",
    "locality": "string",
    "rateCenter": "string",
    "latitude": 0,
    "longitude": 0,
    "region": "string",
    "postalCode": "string",
    "isoCountry": "string",
    "addressRequirements": "string",
    "beta": true,
    "capabilities": {
      "mms": true,
      "sms": true,
      "voice": true,
      "fax": true
    }
  }
]
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
  "phoneNumber": "string"
}
```

#### Response
The Response body contains data with the follow structure:

  | Name  | Type | Description |     
  | - | - | - |
  | `accountSid` | String |   |
  | `addressSid` | String |   |
  | `addressRequirements` | String |   |
  | `beta` | bool |   |
  | `apiVersion` | String |   |
  | `dateCreated` | datetime |   |
  | `dateUpdated` | datetime |   |
  | `friendlyName` | String |   |
  | `identitySid` | String |   |
  | `phoneNumber` | String |   |
  | `sid` | String |   |
  | `smsFallbackMethod` | String |   |
  | `smsFallbackUrl` | String |   |
  | `smsMethod` | String |   |
  | `smsUrl` | String |   |
  | `statusCallback` | String |   |
  | `statusCallbackMethod` | String |   |
  | `trunkSid` | String |   |
  | `uri` | String |   |
  | `smsApplicationSid` | String |   |
  | `origin` | String |   |
  | `capabilities` | [capabilities](#capabilities-Object) |   |
  | `voiceReceiveMode` | String |   |
  | `voiceApplicationSid` | String |   |
  | `voiceCallerIdLookup` | bool |   |
  | `voiceFallbackMethod` | String |   |
  | `voiceFallbackUrl` | String |   |
  | `voiceMethod` | String |   |
  | `voiceUrl` | String |   |
  | `emergencyStatus` | String |   |
  | `emergencyAddressSid` | String |   |
  | `emergencyAddressStatus` | String |   |
  | `bundleSid` | String |   |
  | `status` | String |   |
#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
  "phoneNumber": "string"
}' -X POST https://domain.comm100.com/phonenumber
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

{
  "accountSid": "string",
  "addressSid": "string",
  "addressRequirements": {},
  "apiVersion": "string",
  "beta": true,
  "capabilities": {
    "mms": true,
    "sms": true,
    "voice": true,
    "fax": true
  },
  "dateCreated": "2022-02-24T02:48:50.323Z",
  "dateUpdated": "2022-02-24T02:48:50.323Z",
  "friendlyName": "string",
  "identitySid": "string",
  "phoneNumber": {},
  "origin": "string",
  "sid": "string",
  "smsApplicationSid": "string",
  "smsFallbackMethod": {},
  "smsFallbackUrl": "string",
  "smsMethod": {},
  "smsUrl": "string",
  "statusCallback": "string",
  "statusCallbackMethod": {},
  "trunkSid": "string",
  "uri": "string",
  "voiceReceiveMode": {},
  "voiceApplicationSid": "string",
  "voiceCallerIdLookup": true,
  "voiceFallbackMethod": {},
  "voiceFallbackUrl": "string",
  "voiceMethod": {},
  "voiceUrl": "string",
  "emergencyStatus": {},
  "emergencyAddressSid": "string",
  "emergencyAddressStatus": {},
  "bundleSid": "string",
  "status": "string"
}
```

### Update Phonenumber
`PUT /phonenumber/{pathSid}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `pathSid` | String | yes  |   |

Request body

The request body contains data with the follow structure:

  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `voiceUrl` | string  | yes | | |

example:
```Json 
{
  "voiceUrl": "string"
}
```

#### Response
The Response body contains data with the follow structure:

  | Name  | Type | Description |     
  | - | - | - |
  | `accountSid` | String |   |
  | `addressSid` | String |   |
  | `addressRequirements` | String |   |
  | `beta` | bool |   |
  | `apiVersion` | String |   |
  | `dateCreated` | datetime |   |
  | `dateUpdated` | datetime |   |
  | `friendlyName` | String |   |
  | `identitySid` | String |   |
  | `phoneNumber` | String |   |
  | `sid` | String |   |
  | `smsFallbackMethod` | String |   |
  | `smsFallbackUrl` | String |   |
  | `smsMethod` | String |   |
  | `smsUrl` | String |   |
  | `statusCallback` | String |   |
  | `statusCallbackMethod` | String |   |
  | `trunkSid` | String |   |
  | `uri` | String |   |
  | `smsApplicationSid` | String |   |
  | `origin` | String |   |
  | `capabilities` | [capabilities](#capabilities-Object) |   |
  | `voiceReceiveMode` | String |   |
  | `voiceApplicationSid` | String |   |
  | `voiceCallerIdLookup` | bool |   |
  | `voiceFallbackMethod` | String |   |
  | `voiceFallbackUrl` | String |   |
  | `voiceMethod` | String |   |
  | `voiceUrl` | String |   |
  | `emergencyStatus` | String |   |
  | `emergencyAddressSid` | String |   |
  | `emergencyAddressStatus` | String |   |
  | `bundleSid` | String |   |
  | `status` | String |   |
#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
  "voiceUrl": "string"
}' -X PUT https://domain.comm100.com/phonenumber/{pathSid}
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

{
  "accountSid": "string",
  "addressSid": "string",
  "addressRequirements": {},
  "apiVersion": "string",
  "beta": true,
  "capabilities": {
    "mms": true,
    "sms": true,
    "voice": true,
    "fax": true
  },
  "dateCreated": "2022-02-24T02:48:50.323Z",
  "dateUpdated": "2022-02-24T02:48:50.323Z",
  "friendlyName": "string",
  "identitySid": "string",
  "phoneNumber": {},
  "origin": "string",
  "sid": "string",
  "smsApplicationSid": "string",
  "smsFallbackMethod": {},
  "smsFallbackUrl": "string",
  "smsMethod": {},
  "smsUrl": "string",
  "statusCallback": "string",
  "statusCallbackMethod": {},
  "trunkSid": "string",
  "uri": "string",
  "voiceReceiveMode": {},
  "voiceApplicationSid": "string",
  "voiceCallerIdLookup": true,
  "voiceFallbackMethod": {},
  "voiceFallbackUrl": "string",
  "voiceMethod": {},
  "voiceUrl": "string",
  "emergencyStatus": {},
  "emergencyAddressSid": "string",
  "emergencyAddressStatus": {},
  "bundleSid": "string",
  "status": "string"
}
```

### Remove Phonenumber
`Delete /phonenumber/{pathSid}/VoiceUrl`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `pathSid` | String | yes  |   |

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
    true
  }

```

# Model

### VoicebotSessionContext Object
  VoicebotSessionContext Object is represented as simple flat JSON objects with the following keys:  

  |Name| Type | Default | Description     | 
  | - | - | :-: | - |   
  | `SessionId` | Guid  | | SessionId |
  | `VoicebotId` | Guid  | | VoicebotId |
  | `VoicebotName` | String  | | String |
  | `Channel` | String  | |  |
  | `Authentication` | string  | | authentication data |
  | `Location` | string  | | string |
  | `MessageGuid` | String |  | String |
  | `MessageId` | Int  |  | Int |
  | `WebhookEventType` | Type  |  | Type |
  | `Extra` | Dictionary  |  | Dictionary |
  | `TextInput` | String  | | String |
  | `CurrentIntentId` | Guid  |   | Guid |
  | `VoicebotResponseId` | Guid  |   | Guid |
  | `FormValues` | [FieldValue](#FieldValue-Object)  |   | Guid |
  | `IsFormSubmitted` | bool  |   | bool |
  | `InvalidInputTimes` | Int  |   | Int |
  | `PromptQuestion` | String  |   | String |
  | `VoicebotMessageRecordId` | Guid  |   | Guid |
  | `CustomData` | object  |   | object |
  | `LatestMessage` | [VoicebotMessage](#VoicebotMessage-object)[]  |   |  |
  | `IsFlowComplete` | bool  |   | bool |
  | `IsTest` | bool  |   | bool |
  | `Variables` | [Dictionary](#Dictionary-object)[]  |   | Guid |
  | `MessagesForSendMessageAction` | [VoicebotMessageData](#VoicebotMessageData-object)[]  |   | Guid |
  | `LatestNextActionIds` | List[Guid]  |   | List |
  | `LastMessageType` | Type  |   | Type |

### VoiceBotSession Object
  
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `id` | Guid  | | sessionId |
  | `Channel` | String  | | String |
  | `Message` |  [VoicebotMessage](#VoicebotMessage-object) Object |  |  |
  | `context` | [VoicebotSessionContext](#VoicebotSessionContext-object) Object  |   |  |

### VoicebotMessage Object
  VoicebotMessage Object is represented as simple flat JSON objects with the following keys:  

  |Name| Type| Default | Description     |
  | - | - | :-: | - |
  | `id` | Guid  |  | the unique id of the response |
  | `EnumVoicebotMessageType` | Type |  |  Type |
  | `VisitorQuestion` | String |  |  String |
  | `Content` | Object |  |  Object |
  | `DisableChatInputArea` | bool |  |  bool |
  | `IntentId` | Guid |  |  Id of the Intent. |
  | `IntentName` | String |  |  Name of the Intent. |
  | `Score` | Decimal |  |  Decimal |
  | `Message` | [VoicebotMessageData](#VoicebotMessageData-object)[] |  |   |
  | `VoicebotResponseId` | Guid |  |  the unique id of the response |
  | `MessageGuid` | String |  |  String |
  | `IsFlowComplete` | bool |  |  bool |
  | `BotId` | Guid |  |  Id of the Voicebot. |
  | `VoiceBotMessageId` | Guid |  |  Id of the Voicebot Message. |
 

### VoicebotMessageData Object
  VoicebotMessageData Object is represented as simple flat JSON objects with the following keys:  

  |Name| Type| Default | Description     |
  | - | - | :-: | - |
  | `id` | Guid  |  | the unique id of the response |
  | `EnumVoicebotActionType` | Type |  |  Type |
  | `Content` | Object |  |  Object |

  
### FieldValue Object

|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
|`name` | string |  | the name of a field in a form. |
|`MappingFieldName` | string |  |  |
|`value` | string |  | the value of a field. |
### VariableValue Object
|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
|`name` | string |  | the name of a variable in a form. |
|`value` | string |  | the value of a variable. |



### VoiceBotAction Object
  Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`type` | string | | type of the response,including `PlayAudio`,`PlayText`,`ClearValue`,`CollectDTMFDigits`,`CollectSpeechResponse`,`Condition`,`EndCall`,`GoToIntent`,`IVRMenu`,`SetVariableValue`,`TransferChat`,`Webhook`|
  | `content` | object | |  response's content. when type is `PlayAudio`, it represents [PlayAudio](#PlayAudio-object); when type is `PlayText`,it represents [PlayText](#PlayText-object);when type is `ClearValue`,it represents [ClearValue](#ClearValue-object);when type is `CollectDTMFDigits`,it represents [CollectDTMFDigits](#CollectDTMFDigits-object); when type is `CollectSpeechResponse`, it represents [CollectSpeechResponse](#CollectSpeechResponse-object);when type is `Condition`, it represents [Condition](#Condition-object);when type is `EndCall`, it represents [EndCall](#EndCall-object);when type is `GoToIntent`, it represents [GoToIntent](#GoToIntent-object);when type is `IVRMenu`, it represents [IVRMenu](#IVRMenu-object);when type is `SetVariableValue`, it represents [SetVariableValue](#SetVariableValue-object);when type is `TransferChat`, it represents [TransferChat](#TransferChat-object);when type is `Webhook`, it represents [Webhook](#Webhook-object);|
  |`delayTime` | decimal | 1 | how many seconds delay to show  |

#### PlayAudio Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  | Id of the Action.  |
  |`AudioPath` | String|  | String  |
  |`NextActionId` | Guid|  | Id of the  Next Action. |
#### PlayText Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  | Id of the Action.  |
  |`Message` | String|  | String  |
  |`NextActionId` | Guid|  | Id of the  Next Action. |
#### ClearValue Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  | Id of the Action.  |
  |`VariableId` | Guid|  | Id of the VariableId  |
  |`NextActionId` | Guid|  | Id of the  Next Action. |
 #### CollectDTMFDigits Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  | Id of the Action.  |
  |`Message` | String|  | String  |
  |`VariableName` | String|  | String  |
  |`NumberOfDigits` | Int|  | DTMF Digits  |
  |`StopGatherAfterPresskey` | int|  | int  |
  |`NextActionId` | Guid|  | Id of the  Next Action. |
#### CollectSpeechResponse Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  | Id of the Action.  |
  |`Message` | String|  | String  |
  |`VariableName` | String|  | String  |
  |`LowSTTConfidenceMessage` | String|  | String  |
  |`LowSTTConfidenceRepeatTimes` | Int|  | Int  |
  |`IsConfirmationRequired` | Bool|  | int  |
  |`ConfirmationMessage` | String|  | String  |
  |`ConfirmationText` | String|  | String  |
  |`ConfirmationKey` | int|  | int  |
  |`ActionIdWhenLowSTTConfidence` | Guid|  | int  |
  |`NextActionId` | Guid|  | Id of the  Next Action. |
#### Condition Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  | Id of the Action.  |
  |`OtherCaseActionId` | Guid |  | Id of the other case Action.  |
  |`VoicebotActionConditionCase` | [VoicebotActionConditionCase[]](#VoicebotActionConditionCase-object) |  |   |
#### VoicebotActionConditionCase Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  | Id of the Action.  |
  |`Order` | Int |  | Int  |
  |`ConditionExpressionType` | Type |  | Allowed values are "all", "any", "logicalExpression".  |
  |`LogicalExpression` | string |  | string  |
  |`GoToActionId` | Guid |  | Id of the Action.  |
  |`Id` | Guid |  | Id of the Condition Case.  |
  |`VoicebotActionConditionCaseConditions` | [VoicebotActionConditionCaseConditions[]](#VoicebotActionConditionCaseConditions-object) |  |   |

#### VoicebotActionConditionCaseConditions Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionConditionCaseId` | Guid |  | Id of the Condition Case.  |
  |`FieldName` | [FieldName](#FieldName-object) |  |   |
  |`operator` | Type |  | Allowed values are "is", "contains", "notContains", "isMoreThan", "isLessThan", "isNot", "isNotLessThan", "isNotMoreThan", "regularExpression", "isOneOf", "isNotIn", "dateNotEqualTo", "before", "after", "dateEqualTo".  |
  |`Value` | string |  | string  |
  |`Order` | Int |  | Int  |
  |`Id` | Guid |  | Guid  |
  |`Items` | List |  | List  |


#### FieldName Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Operator|
  | - | - |
  |`{!Visitor.Country/Region}` | is/isNot/contains/notContains/regularExpression |
  |`{!Visitor.State/Province}` | is/isNot/contains/notContains/regularExpression |
  |`{!Visitor.City}` | is/isNot/contains/notContains/regularExpression |
  |`{!Visitor.Time Zone}` | isOneOf/isNotIn |
  |`{!Visitor.Language}` | is/isNot/contains/notContains/regularExpression |
  |`{!Visitor.CardId}` | is/isNot/contains/notContains/regularExpression |
#### TransferChat Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  |  Id of the Action. |
  |`TransferTo` | String |  | String |
  |`ActionIdWhenTransferFailed` | Guid |  | Guid  |

#### EndCall Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  |  Id of the Action. |
  
#### GoToIntent Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  |  Id of the Action. |
  |`IntentId` | Guid |  |  Id of the Intent. |

#### IVRMenu Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  |  Id of the Action. |
  |`Message` | String |  |  String |
  |`InvalidInputMessage` | String |  |  String |
  |`InvalidInputRepeatTime` | Int |  |  Int |
  |`ActionIdWhenInvalidInput` | Guid |  |  Guid |
  |`VoicebotActionIVRMenuOptions` | [VoicebotActionIVRMenuOptions[]](#VoicebotActionIVRMenuOptions-object) |  |  List |
  
#### VoicebotActionIVRMenuOptions Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`Text` | String |  |  String |
  |`Key` | Int |  |  Int |
  |`NextActionId` | Guid |  |  Id of the Next Action. |
  |`VoicebotActionId` | Int |  |   Id of the Action. |
  |`Order` | Int |  |  Int |
  
#### SetVariableValue Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  |  Id of the Action. |
  |`Value` | String |  |  String |
  |`SaveToType` | Type |  | Allowed values are "LiveChat", "CustomVariable", "Tick", "Variable". |
  |`FieldId` | Guid |  |  Id of the Field |
  |`Field` | [LiveChatField[]](#LiveChatField-object) |  |   |
  |`CustomVariableId` | Guid |  | Id of the Custom Variable  |
  |`CustomVariable` | [CustomVariable[]](#CustomVariable-object) |  |   |
  |`TicketingFieldId` | Guid |  | Id of the Custom Ticket Field.  |
  |`TicketingField` | [TicketingField[]](#TicketingField-object) |  |   |
  |`Variable` | String |  |  String |

#### LiveChatField Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`Id` | Guid |  |  Id of the Field. |
  |`Name` | String |  |  Name of the Field. |
 
#### CustomVariable Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`Id` | Guid |  |  Id of the CustomVariable. |
  |`Name` | String |  |  Name of the CustomVariable. |
  
#### TicketingField Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`Id` | Guid |  |  Id of the TicketingField. |
  |`Name` | String |  |  Name of the TicketingField. |
  
#### Webhook Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`VoicebotActionId` | Guid |  |  Id of the Action. |
  |`Url` | Srting |  |  Url of the Webhook. |
  |`IfSendChatTranscript` | bool |  |   |
  |`AdditionalPostBody` | String |  |   |
  |`OtherResponseToActionId` | Guid |  |   |
  |`VoicebotActionWebhookHeaders` | [VoicebotActionWebhookHeaders[]](#VoicebotActionWebhookHeaders-object) |  |   |
  |`VoicebotActionWebhookResponseCodeToActions` | [VoicebotActionWebhookResponseCodeToActions[]](#VoicebotActionWebhookResponseCodeToActions-object) |  |   |
  |`VoicebotActionWebhookResponseToVariables` | [VoicebotActionWebhookResponseToVariables[]](#VoicebotActionWebhookResponseToVariables-object) |  |   |
  
#### VoicebotActionWebhookHeaders Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`Id` | Guid |  |  Id of the WebhookHeaders. |
  |`VoicebotActionId` | String |  |  Id of the Action. |
  |`Key` | String |  |  String |
  |`Value` | String |  |  String |
  |`Order` | Int |  |  Int |

#### VoicebotActionWebhookResponseCodeToActions Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`Id` | Guid |  |  Id of the WebhookHeaders. |
  |`VoicebotActionId` | String |  |  Id of the Action. |
  |`ResponseCode` | String |  |  String |
  |`NextActionId` | Guid |  |   Id of the Next Action. |
  |`Order` | Int |  |  Int |

#### VoicebotActionWebhookResponseToVariables Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`Id` | Guid |  |  Id of the WebhookHeaders. |
  |`VoicebotActionId` | String |  |  Id of the Action. |
  |`ResponseKey` | String |  |  String |
  |`VariableName` | String |  |  String |
  |`Order` | Int |  |  Int |

### Form Object
FormReplyResponse is represented as simple flat json objects with the following keys:

|Name| Type| Default | Description     | 
| - | - | :-: | - | 
|`message` | string |   | A separate message which is sent before the button is sent.|
|`title` | string |  | when a button is sent to visitor, clicking this button will open a form that contains information bot wants to collect from the visitor. the title refers to the title of that form, and it is also placed on the button as a name.|
|`isConfirmationRequired` | bool |   | whether visitor needs to click confirm after filling out the information in a form.|
|`fields` | [Field](#field-object)[] | | an array of [Field](#field-object) Object |
|`submitButtonText` | string |   | |
|`cancelButtonText` | string |   | |
|`confirmButtonText` | string |   | |


### Field Object
Field is represented as simple flat json objects with the following keys:

|Name| Type| Default | Description     | 
| - | - | :-: | - | 
|`type` | string | | enums: `text` ,`textArea`,`radio` ,`checkBox` ,`dropDownList` ,`checkBoxList`,`email` type refers to the different kinds of fields which can be used in a form. |
|`name` | string |  | a field’s name in a form. |
|`defaultValue` | string | | a field’s value |
|`isRequired` | bool |  | to mark whether a field in a form is required or not. |
|`isMasked` | bool |  | if this is true, visitor information will be masked with symbols in chat logs. |
|`options` | string[] |  | an array of of string when the fieldType is `radio` ,`dropDownList` ,`checkBoxList`|
|`order` | integer |  | must greater than or equal 0, ascending sort |

### Visitor Object

|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
|`name` | string |  |  |
|`email` | string |  |  |
|`phone` | string |  |  |
|`state/province` | string |  |  |
|`country/region` | string |  |  |
|`city` | string |  |  |
|`ip` | string |  |  |
|`email` | string |  |  |
|`currentPageURL` | string |  |  |
|`searchEngine` | string |  |  |
|`searchKeywords` | string |  |  |


#### capabilities Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`mms` | bool |  |  bool |
  |`sms` | bool |  |  bool |
  |`voice` | bool |  |  bool |
  |`fax` | bool |  |   bool |
