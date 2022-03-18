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
##Channel Adapter API 
The ChannelURI can be any valid URI that implements this API, and it is configured in the system when a new channel needs to access.  
### Channel Adapter receives Input. 
`POST /{channelURI}/input`


#### Parameters
Request body
Request body the request body contains data with the following structure: 
Request body is Voice Message Object 
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `sessionId` | string | yes | |  session Id .  |
  |`content`  |  [VoiceBotAction[]](#VoiceBotAction-Object)  |yes |   | type of the response,including `Twilio` , `SIP` |

example:
```Json 
  {     
          "sessionId":"d3f5b968-ad51-42af-b759-64c0afc40b84", 
          "content": [{ 
              "voice":"string", 
    	        "voiceConfig":{ 
                  "audioEncoding": enum , 
                  "speakingRate": number, 
                  "pitch": number 
               } 
         }] 
} 
```

#### Response
Response 
HTTP/1.1 200 OK 

## Voice Service API 
### Create Sessions 
`POST /voiceservice/sessions`

#### Parameters
Request body 
Request body the request body contains data with the following structure: 
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `channel` | string | yes | |  type of the response, including `Twilio`, `SIP`  |
  |`channelIdentifier`  |  string  |yes |   | The Unique ID corresponding to voicebotId,such as phone number，SIP URI  |
  |`visitor`  |  [Visitor Object](#Visitor-Object)  |no |   | visitor information |
  |`variables`  |  [Variable[]](#Variable-Object)  |no |   | Variables |

example:
```Json 
 {    
    "channel": "Twilio", 
    "channelIdentifier": "q3f5b438-xw31-44af-b729-64swaf3d0b56", 
    "visitor": { 
        "phone":"123-4355-212", 
      }， 
    "variables": [{ 
                  "name":"string", 
                  "value": "string", 
      }]  
  } 
```

#### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`sessionId` | Guid | the unique id of the call  |
  |`content`  |  [VoiceBotAction[]](#VoiceBotAction-Object)[]    | Greeting output  |

Response
```Json
HTTP/1.1 200 OK 
  Content-Type:  application/json 
 
  {     
          "sessionId":"d3f5b968-ad51-42af-b759-64c0afc40b84", 
          "content": [{ 
              "voice":"string", 
    	        "voiceConfig":{ 
      		          "audioEncoding": enum , 
      		          "speakingRate": number, 
      		          "pitch": number 
	            } 
    		}] 
  } 
```

### Receive Speech  
`POST /voiceservice/sessions/{sessionId}/questions`

#### Parameters
Path parameters 
  | Name | Type | Required |  | Description |    
  | - | - | :-: | - | 
  | `sessionId` | Guid | yes |  |

Request body 
The request body contains data with the follow structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `type` | string | yes | |  type of the response, including textInput, voiceInput, chatTransferStatus   |
  | `textInput`  |  string  | When type is textInput  |   | Text input to voice robot   |
  | `voiceInput`  |  string  | When type is voiceInput  |   | The audio data bytes encoded as specified in VoiceConfig. Note: as with all bytes fields, proto buffers use a pure binary representation, whereas JSON representations use base64.A base64-encoded string.  |
  | `voiceConfig`  |  [VoiceConfig Object](#VoiceConfig-Object)  | When type is voiceInput  |   | Provides information to the recognizer that specifies how to process the request.  |
  | `isTransferFailed`  |  bool  | When type is chatTransferStatus  |   | If the bot Transfer Chat to agent failed  |

example:
```Json 
{  
	"type":"voiceInput",  
	"voiceInput":"string", 
    	"voiceConfig":{ 
      		"audioEncoding": enum , 
      		"speakingRate": number, 
      		"pitch": number 
	   } 
} 
```

#### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`sessionId` | Guid | the unique id of the call  |
  |`content`  |  [VoiceBotAction[]](#VoiceBotAction-Object)[]    | output  |

Response
```Json
 HTTP/1.1 200 OK 
  Content-Type:  application/json 
 
  {     
          "content": [{ 
              "voice":"string", 
              "voiceConfig":{ 
      				"audioEncoding": enum , 
      				"speakingRate": number, 
      				"pitch": number 
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
