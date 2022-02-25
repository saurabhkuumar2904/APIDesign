  | Change Version | API Version | Change nots | Change Date | Author |
  | - | - | - | - | - |
  | 2.0 | v1 | Voice Bot API | 2022-2-22 | Leon |  
  | 2.0 | v1 | Voice Bot API | 2022-2-23 | Carl |  
 



# Summary
## Voice Bot API
  - `POST /voicebots/{VoicebotId}/sessions` - [Create session](#create-a-new-voicebot-session)
  - `POST /sessions/{sessionId}:recieveMessage` - [Recieved Message](#recieved-message)
## Twillio Adapter  
  - `GET /phonenumber/available` - [Get Phonenumber](#Get-Phonenumber)
  - `POST /phonenumber` - [Create A New Phonenumber](#Create-A-New-Phonenumber)
  - `PUT /phonenumber/{pathSid}` - [Update Phonenumber](#Update-Phonenumber)
  - `DELETE /phonenumber/{pathSid}/VoiceUrl` - [Remove  Phonenumber](#Remove-Phonenumber)
## Call Adapter
  - `POST /Call` - [Call](#Call)
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
  }' -X POST https://domain.comm100.com/api/v4/voicebots/{VoicebotId}/sessions
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
                    "nextActionId": "00000000-0000-0000-0000-000000000000",
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

example:
```Json 
  {
    "textInput":"i want to buy NBN"
  }
```

#### Response
the response is: [VoicebotOutput](#voicebotoutput-object) Object

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "textInput":"i want to buy NBN"
  }' -X POST https://domain.comm100.com/sessions/f9928d68-92e6-4487-a2e8-8234fc9d1f48:detectIntent
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



```

### Call
`POST /call/say`

#### Parameters
No parameters

Request body

The request body contains data with the follow structure:

  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `from` | string  | yes | | |
  | `content` | string  | yes | | |
  | `to` | string  | yes | | |

example:
```Json 
{
  "from": "string",
  "content": "string",
  "to": "string"
}
```

#### Response
The Response body contains data with the follow structure:

  | Name  | Type | Description |     
  | - | - | - |
  | `accountSid` | String |   |
  | `apiVersion` | String |   |
  | `dateCreated` | datetime |   |
  | `dateUpdated` | datetime |   |
  | `sid` | String |   |
  | `parentCallSid` | String |   |
  | `to` | String |   |
  | `toFormatted` | String |   |
  | `from` | String |   |
  | `fromFormatted` | String |   |
  | `phoneNumberSid` | String |   |
  | `status` | String |   |
  | `startTime` | datetime |   |
  | `endTime` | datetime |   |
  | `duration` | String |   |
  | `price` | String |   |
  | `priceUnit` | String |   |
  | `direction` | String |   |
  | `answeredBy` | String |   |
  | `annotation` | String |   |
  | `forwardedFrom` | String |   |
  | `groupSid` | String |   |
  | `callerName` | String |   |
  | `queueTime` | String |   |
  | `trunkSid` | String |   |
  | `uri` | String |   |
  | `subresourceUris` | [subresourceUris](#subresourceUris-Object) |   |

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
  "from": "string",
  "content": "string",
  "to": "string"
}' -X POST https://domain.comm100.com/call/say
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

{
  "sid": "string",
  "dateCreated": "2022-02-24T11:16:26.626Z",
  "dateUpdated": "2022-02-24T11:16:26.626Z",
  "parentCallSid": "string",
  "accountSid": "string",
  "to": "string",
  "toFormatted": "string",
  "from": "string",
  "fromFormatted": "string",
  "phoneNumberSid": "string",
  "status": {},
  "startTime": "2022-02-24T11:16:26.626Z",
  "endTime": "2022-02-24T11:16:26.626Z",
  "duration": "string",
  "price": "string",
  "priceUnit": "string",
  "direction": "string",
  "answeredBy": "string",
  "annotation": "string",
  "apiVersion": "string",
  "forwardedFrom": "string",
  "groupSid": "string",
  "callerName": "string",
  "queueTime": "string",
  "trunkSid": "string",
  "uri": "string",
  "subresourceUris": {
    "additionalProp1": "string",
    "additionalProp2": "string",
    "additionalProp3": "string"
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
  
#### TwillioField Object
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
  
### Visitor Object

|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
|`name` | string |  |  |
|`phone` | string |  |  |
|`state/province` | string |  |  |
|`country/region` | string |  |  |
|`city` | string |  |  |


#### capabilities Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`mms` | bool |  |  bool |
  |`sms` | bool |  |  bool |
  |`voice` | bool |  |  bool |
  |`fax` | bool |  |   bool |
  
#### subresourceUris Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`additionalProp1` | string |  |  string |
  |`additionalProp2` | string |  |  string |
  |`additionalProp3` | string |  |  string |
