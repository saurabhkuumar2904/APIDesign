  | Change Version | API Version | Change nots | Change Date | Author |Architect Team Reviewer | 
  | - | - | - | - | - |- |
  | 2.0 | v1 | Voice Bot API | 2022-3-18 | Leon，Carl，Page，Zack |  Allon|


# Summary

## Voice Channel Adapter API 

The channelURL can be any valid URL that implements this API, and it is configured in the system when a new channel needs to access. 
  - POST /{channelURL} - [Voice Channel Adapter receives input](#Voice-Channel-Adapter-receives-input). 

## Voice Service Input API  
  - POST /voiceservice/sessions - [Create Session](#Create-Sessions). Voice service creates a session 
  - POST /voiceservice/sessions/{sessionId}/inputs - [Receive a input](#Receive-a-input).  Voice service receives call input.
  - DELETE /voiceservice/sessions/{sessionId} - [Delete a session](#Delete-a-session). Voice service deletes a session.
<!--   - POST /voiceservice/sessions/{sessionId}/variables - [Update Variables](#Update-Variables).Receive the variables of the Voice Bot Session -->

## Voice Service Notification API   
  - POST /voiceservice/sessions/{sessionId}/notifications - [Voice Service receives notifications](#Voice-Service-receives-notifications)

## Voice Bot Service API 
  - POST /voicebot/voicebots/{VoicebotId}/sessions  - [Create Session Voice Bot creates session](#Create-A-New-Voice-Bot-Session)
  - POST /voicebot/sessions/{sessionId}/messages - [Voice Bot receive a message](#Voice-Bot-receive-a-message). Voice Bot receives input   
  - DELETE /voicebot/sessions/{sessionId} - [Delete Session Voice Bot deletes the session](#Delete-Voice-Bot-session)
  - POST /voicebot/voicebotCallLog:total - [All Voice Bot Call Log](#Voice-Bot-Call-Log)
  - GET /voicebot/voicebotCallLog:count - [Get Voice Bot Call Log](#Get-Voice-Bot-Call-Log)
  - GET /voicebot/fileservice/token - [Get Token](#Get-Token)
<!--   - POST /voicebot/sessions/{sessionId}/variables - [Update Variables](#Update-Variable).Receive the variables of the Voice Bot Session -->

## STT & TTS API 
Provide STT (Speech to Text) and TTS (Text to Speech) capabilities. 
  - POST /stttts/stt:speechToText - [Speech To Text](#Speech-To-Text) 
  - POST /stttts/tts:textToSpeech - [Text To Speech](#Text-To-Speech)


# Endpoints

## Voice Channel Adapter API 
The ChannelURL can be any valid URL that implements this API, and it is configured in the system when a new channel needs to access.  

### Voice Channel Adapter receives Input. 
`POST /{channelURL}`


#### Parameters
Request body
Request body the request body contains data with the following structure: 
Request body is Voice Message Object 
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `channelIdentifier` | string | yes | |  channelIdentifier .  |
  |`content`  |  [VoiceAction[]](#VoiceAction-Object)  |yes |   |  |

#### example:
```Json 
  {     
          "channelIdentifier":"d3f5b968-ad51-42af-b759-64c0afc40b84", 
          "content": [{ 
	        "type":"playAudioAction",
		"content":{ 
			"type":"voice",
                        "voice":"string", 
    	        	"voiceConfig":{ 
                 		 "encoding": "LINEAR16" , 
                  		"sampleRateHertz": 8000, 
              		 },
			 
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

#### example:
```Json 
 {    
    "channel": "Twilio", 
    "channelIdentifier": "q3f5b438-xw31-44af-b729-64swaf3d0b56", 
    "visitor": { 
        "phone":"123-4355-212", 
      },
    "variables": [{ 
                  "name":"abc", 
                  "value": "123", 
      }]  
  } 
```

#### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`sessionId` | Guid | the unique id of the call  |
  |`content`  |  [VoiceAction[]](#VoiceAction-Object)  | Greeting output  |

Response
```Json
HTTP/1.1 200 OK 
  Content-Type:  application/json 
  {     
          "sessionId":"d3f5b968-ad51-42af-b759-64c0afc40b84", 
          "content": [{ 
	        "type":"playAudioAction",
		"content":{ 
			"type":"voice",
                        "voice":"string", 
    	        	"voiceConfig":{ 
                 		 "encoding": "LINEAR16" , 
                  		"sampleRateHertz": 8000, 
              		 } 
                }
    		}] 
  } 
```




### Receive a input 
`POST /voiceservice/sessions/{sessionId}/inputs`

#### Parameters
Path parameters 
  | Name | Type | Required | Description |    
  | - | - | :-: | - | 
  | `sessionId` | Guid | yes |  |

Request body 
The request body contains data with the follow structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `type` | string | yes | |  type of the response, including textInput, voiceInput   |
  | `textInput`  |  string  | When type is textInput  |   | Text input to voice robot   |
  | `voiceInput`  |  string  | When type is voiceInput  |   | The audio data bytes encoded as specified in VoiceConfig. Note: as with all bytes fields, proto buffers use a pure binary representation, whereas JSON representations use base64.A base64-encoded string.  |
  | `voiceConfig`  |  [VoiceConfig Object](#VoiceConfig-Object)  | When type is voiceInput  |   | Provides information to the recognizer that specifies how to process the request.  |
<!--   | `isTransferFailed`  |  bool  | When type is chatTransferStatus  |   | If the bot Transfer Chat to agent failed  | -->

#### example:
```Json 
{  
	"type":"voiceInput",  
	"voiceInput":"string", 
    	"voiceConfig":{ 
      		"encoding": "AMR" , 
      		"sampleRateHertz": 8000, 
	   } 
} 
```

#### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`content`  |  [VoiceAction[]](#VoiceAction-Object)  |   |
  <!--   |`sessionId` | Guid | the unique id of the call  | -->

Response
```Json
 HTTP/1.1 200 OK 
  Content-Type:  application/json 
 
  {     
          "content": [{ 
	        "type":"playAudioAction",
		"content":{ 
			"type":"voice",
                        "voice":"string", 
    	        	"voiceConfig":{ 
                 		 "encoding": "LINEAR16" , 
                  		"sampleRateHertz": 8000, 
              		 } 
                }
    		}] 
  }
```

### Delete a Session   
`DELETE /voiceservice/sessions/{sessionsId} `

#### Parameters
Path parameters 
  | Name | Type | Required | Description |    
  | - | - | :-: | - | 
  | `sessionId` | Guid | yes |  |

#### Response

 HTTP/1.1 200 OK 


<!-- ### Update variables
`POST /voiceservice/sessions/{sessionId}/variables`

#### Parameters
Path parameters 
  | Name | Type | Required | Description |    
  | - | - | :-: | - | 
  | `sessionId` | Guid | yes |  |

Request body 
The request body contains data with the follow structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `variables` | [Variable[]](#Variable-Object)  | yes | |  Variables  |

#### example:
```Json 
{ 
    "variables": [ 
    { 
        "name":"string", 
        "value":"string", 
    } 
    ] 
} 
```

#### Response
  HTTP/1.1 200 OK 
 -->

## Voice Service Notification API

### Voice Service receives notifications 
`POST /voiceservice/sessions/{sessionId}/notifications`

#### Parameters
Path parameters 
  | Name | Type | Required | Description |    
  | - | - | :-: | - | 
  | `sessionId` | Guid | yes |  |

Request body 
The Request body contains data with the following structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `content` | [VoiceBotAction[]](#VoiceBotAction-Object) | yes | |    |

#### example:
```Json 
  {     
          "content":[{ 
              "type":"playText", 
              "content":{ 
                    "message": "Hi there! I'm a VoiceBot, here to help answer your questions", 
              } 
           }] 
  } 
```

#### Response
HTTP/1.1 200 OK

## Voice Bot Service API 

### Create A New Voice Bot Session  
`POST /voicebot/voicebots/{VoicebotId}/sessions`

#### Parameters
Path parameters 
  | Name | Type | Required | Description |    
  | - | - | :-: | - | 
  | `voicebotId` | Guid | yes |  |

Request body 
The request body contains data with the follow structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `visitor` | [Visitor Object](#Visitor-Object)  | No | |  Visitor information.   |
  | `variables`  |  [Variable[]](#Variable-Object)   | No  |   | Variables    |


#### example:
```Json 
  { 
    "visitor": { 
        "phone":"123-4355-212", 
      }, 
    "variables": [{ 
        "name":"string", 
        "value":"string"
    }] 
  } 
```

#### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`sessionId` | Guid | the unique id of the session   |
  |`content`  |  [VoiceBotAction[]](#VoiceBotAction-Object)  |   |

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



### Voice Bot receive a message  
`POST /voicebot/sessions/{sessionid}/messages`

#### Parameters
Path parameters 
  | Name | Type | Required | Description |    
  | - | - | :-: | - | 
  | `sessionId` | Guid | yes | Session id of the voice conversation  |

Request body 
The request body contains data with the follow structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `textInput` | String  | No | |  Text input to voice robot    |
  | `isTransferFailed`  |  Bool   | No  |   | If the bot Transfer Chat to agent failed    |


#### example:
```Json 
  { 
    "textInput":"I want to buy NBN", 
    "isTransferFailed": false, 
  }  
```

#### Response
the response is: VoicebotOutput Object 

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




### Delete Voice Bot session 
`DELETE /voicebot/sessions/{sessionId}`

#### Parameters
Path parameters 
  | Name | Type | Required | Description |    
  | - | - | :-: | - | 
  | `sessionId` | Guid | yes |  |

#### example:
Using curl 

curl -H "Content-Type: application/json" -d 

 -X Delete https://domain.comm100.com/voicebot/sessions/{sessionId} 

#### Response

  HTTP/1.1 204 OK 



### Voice Bot Call Log  
`POST /voicebot/voicebotCallLog:total`

#### Parameters
Request body 
The request body contains data with the follow structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `SiteId` | Int  | Yes | |  Id for the Site  |
  | `CallingFrom`  |  String   | Yes  |   | The visitor's phone number    |
  | `Status`  |  String   | Yes  |   |     |
  | `TransferTo`  |  String   | No  |   | The number to be transferred    |
  | `CallingTo`  |  String   | Yes  |   | The Voice Bot phone number    |
  | `StarTime`  |  DateTime   | Yes  |   | Call start time    |
  | `EndTime`  |  DateTime   | Yes  |   | Call end time    |


#### example:
```Json 
  { 
    "SiteId":10000, 
    "CallingFrom": "+10215846214",
    "Status": "Completed",
    "TransferTo": "+15736544228",
    "CallingTo": "+12012126861",
    "StarTime": "2022-04-20 12:15:58.145631",
    "EndTime": "2022-04-20 12:23:21.923631"
  }  
```

#### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`CallDuration` | Int | Call duration   |
  |`BilledCallDuration`  |  Int  |  Billed call duration |

Response
```Json
  HTTP/1.1 200 OK 
  Content-Type:application/json 
  {     
    "CallDuration": 206, 
    "BilledCallDuration": 186
  } 
```


### Get Voice Bot Call Log 
`POST /voicebot/voicebotCallLog:count`

#### Parameters
Path parameters 
  | Name | Type | Required | Description |    
  | - | - | :-: | - | 
  | `StarTime` | DateTime | yes |   |
  | `DateTime` | DateTime | yes |   |
  | `SiteId` | DateTime | yes |   |

#### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`Total` | Int | Total call duration   |
  |`CompletedTotal`  |  Int  |  Completed call duration |
  |`TransferredTotal`  |  Int  |  Transfer call duration |

Response
```Json
  HTTP/1.1 200 OK 
  Content-Type:application/json 
  {     
    "Total": 843, 
    "CompletedTotal": 500,
    "TransferredTotal": 343
  } 
```

### Get Token
`GET /voicebot/fileservice/token`

#### Parameters
Path parameters 
  | Name | Type | Required | Description |    
  | - | - | :-: | - | 
  | `SiteId` | DateTime | yes |   |



#### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`Url` | String |    |
  |`secret`  |  String  |   |
  |`ip`  |  String  |   |

Response
```Json
  HTTP/1.1 200 OK 
  Content-Type:application/json 
  {     
    "Url": "https://dash11.comm100.io", 
    "secret": "eyJhbGciOiJodHRwOi8vd3d3LnczLm9yZy8yMDAxLzA0L3htbGRzaWctbW9yZSNyc2Etc2hhMjU2IiwidHlwIjoiSldUIn0.eyJqdGkiOiI0MDZhOWU5Zi1jMjY4LTQzOTktOWQxMi1iYWU0OTk0NjIxZjEiLCJhZ2VudElkIjoiNTU5NWFiZTMtODdhMC00MDJlLWFkYjYtNzNjYzc1Y2U0MjgyIiwic2l0ZUlkIjoiMTAwMDAiLCJ0aHVtYnByaW50IjoiOTYxNjZDQTNCMzRBQkYyRDA0REY5NkEwMTVCQkRERDA5QjBBN0M1OSIsInN1Y2Nlc3MiOiJUcnVlIiwibmJmIjoxNjUwNDIwMjQxLCJleHAiOjE2NTA0Mjc0NDEsImlzcyI6InZvaWNlZGFzaC50ZXN0aW5nLmNvbW0xMDBkZXYuaW8ifQ.FNlReRDZ1G329juwxh1i9PGj4_E0kewmLss1L3M-HwVaPCHNd7DmiQ9KJkakbs3d4XUI3WJmtI4IFCmoMJadguGR_NM3syt1lBcFa9eUPnuESdAamXgN4r3Tg2dHPwb4XCdLcxHjGlek6beG186g2vh-pIdqevAEFzDrZekTmNywT_T6OcEj4TF6Ip1EqA0Nmfk3p_1b7Tk2FHIrb7897bj1UeEA4xDF61xZPz3JcOkBT6aRCq869qATqYMbXsHF3BdHr8wMWlH0glIhtVwBX52IcCPy8A1nKXjs_cujWZtxq9d155w7alMgcWTpmBpUwhaHxGKs4x5_AOCyet1v2Q",
    "ip": "192.168.0.52"
  } 
```


### Update variable  
`POST /voicebot/sessions/{sessionId}/variables `

#### Parameters
Path parameters 
  | Name | Type | Required | Description |    
  | - | - | :-: | - | 
  | `sessionId` | Guid | yes | Session id of the voice conversation  |

Request body 
The request body contains data with the follow structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `variables` | [Variable[]](#Variable-Object)  | Yes | |  Variables    |


#### example:
```Json 
{ 
    "variables": [{ 
        "name":"string", 
        "value":"string" 
      }] 
} 
```

#### Response
HTTP/1.1 200 OK



## STT & TTS API  

### Speech To Text  
Performs synchronous speech recognition: receive results after all audio has been sent and processed.

`POST /stttts/stt:speechToText`

#### Parameters

Request body 
The request body contains data with the follow structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `config` | [STTVoiceConfig](#STTVoiceConfig-Object)  | yes | |  Provides information to the recognizer that specifies how to process the request.    |
  | `audio`  |  string   | yes  |   | The audio data to be recognized.    |
  | `engine`  |  string   | no  | google  | e.g. google    |


#### example:
```Json 
 { 
    "config": { 
	"encoding": "AMR" , 
	"sampleRateHertz": 8000, 
	"languageCode": "en-US", 
    }, 
    "audio": "string"    
  } 
```

#### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`results` | [SpeechRecognitionResult[]](#SpeechRecognitionResult-Object)  | Sequential list of transcription results corresponding to sequential portions of audio.    |

Response
```Json
 HTTP/1.1 200 OK 
  Content-Type:  application/json 
  {     
    "results": [ 
      {   
        "alternatives": [ 
          { 
            "transcript": "string", 
            "confidence": 0.8, 
          } 
        ]        
      } 
    ] 
  }  
```

### Text To Speech  
Synthesizes speech synchronously: receive results after all text input has been processed.

`POST /stttts/tts:textToSpeech`

#### Parameters

Request body 
The request body contains data with the follow structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `input` | [SynthesisInput](#SynthesisInput-Object)  | yes | |  The Synthesizer requires plain text as input.     |  
  | `config`  |  [TTSVoiceConfig](#TTSVoiceConfig-Object)   | yes  |   | The configuration of the synthesized audio.     |
  | `engine`  |  string   | no  | google | e.g. google   |



#### example:
```Json 
 { 
    "input": { 
      "text": "string", 
    }, 
    "config":{ 
        "encoding":"Mulaw",
	"sampleRateHertz":8000,
	"languageCode": "en-US", 
	"gender": "MALE", 	
    }    
} 
```

#### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :- | 
  |`voiceContent` | string  | The audio data bytes encoded as specified in the request, including the header for encodings that are wrapped in containers (e.g. MP3, OGG_OPUS). For LINEAR16 audio, we include the WAV header. Note: as with all bytes fields, protobuffers use a pure binary representation, whereas JSON representations use base64.A base64-encoded string. |  

Response
```Json
 HTTP/1.1 200 OK 
  Content-Type:  application/json 
  { 
      "voiceContent": "string"
  } 
```


# Model
## Voice Message Object 
  
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `id` | Guid  | | |
  |`content`  |  [VoiceAction](#voiceAction-object)[]    |  |

## VoicebotOutput Object 
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `id` | Guid  | | |
  | `content`  |  [VoiceBotAction](#voicebotaction-object)[]    |  |

## VoicebotAction Object 
  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  | `type` | string | | type of the response,including `PlayAudio`,`PlayText`,`CollectDTMFDigits`,`CollectSpeechResponse`,`IVRMenu`,`TransferCall`, `EndCall`|
  | `content` | object | |  response's content. when type is `PlayAudio`, it represents [PlayAudio](#PlayAudio-object); when type is `PlayText`,it represents [PlayText](#PlayText-object);when type is `CollectDTMFDigits`,it represents [CollectDTMFDigits](#CollectDTMFDigits-object); when type is `CollectSpeechResponse`, it represents [CollectSpeechResponse](#CollectSpeechResponse-object);when type is `EndCall`, it represents [EndCall](#EndCall-object);when type is `IVRMenu`, it represents [IVRMenu](#IVRMenu-object);when type is `TransferCall`, it represents [TransferCall](#TransferCall-object);|
  
  ## VoiceAction Object 
  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  | `type` | string | | type of the response,including `PlayAudioAction`,`CollectDTMFDigitsAction`,`TransferCallAction`, `EndCallAction`|
  | `content` | object | |  response's content. when type is `PlayAudioAction`, it represents [PlayAudioAction](#PlayAudioAction-object);when type is `CollectDTMFDigitsAction`,it represents [CollectDTMFDigitsAction](#CollectDTMFDigitsAction-object); when type is `EndCallAction`, it represents [EndCallAction](#EndCallAction-object);when type is `TransferCallAction`, it represents [TransferCallAction](#TransferCallAction-object);|

## PlayAudio Object  
  Text Response is represented as simple flat json objects with the following keys: 
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `audioPath` | String  | | String |
  
## PlayText Object  
  Text Response is represented as simple flat json objects with the following keys: 
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `message` | String  | | String |
  | `delayTime` | int  | | second |

## CollectDTMFDigits Object   
  Text Response is represented as simple flat json objects with the following keys: 
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `message` | String  | | String |
  | `numberOfDigits` | String  | | Enumeration. 1, 2, … 29, 30, Variable. The number of digits entered by the caller in dialer. Default: Not sure. |
  | `stopGatherAfterPresskey` | String  | | Enumeration. *, #. Available when Number of Digits is Not sure.  |

## CollectSpeechResponse Object    
  Text Response is represented as simple flat json objects with the following keys: 
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `message` | String  | | String |
  | `lowSTTConfidenceMessage` | String  | | We set a default STT Confidence Score for all Voice Bots in system level, customers cannot change in this version.  |
  | `lowSTTConfidenceRepeatTimes` | Int  | | Available value: 0 - 9. Default: 2.  |
  | `isConfirmationRequired` | Bool  | | If the bot will reply to the answer to the caller to confirm.   |
  | `confirmationMessage` | String  | | Only available when Is Confirmation Required is “true”.   |
  | `confirmationText` | String  | | Visitor can speak the text to confirm the input. This text will not be read to visitors.   |
  | `confirmationKey` | int  | | Enumeration. 1, 2, 3, 4, 5, 6, 7, 8, 9, 0, *, #. Visitor can press the key to confirm.   |

## TransferCall Object   
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `transferTo` | String  | | Support Phone Number and Sip URI.  |


## EndCall Object   
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 


## IVRMenu Object   
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `message` | String  | | String  |

## Visitor Object   
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `name` | String  | | Name of the visitor  |
  | `callerID` | String  | | Phone or sipURI of the visitor  |
  | `state` | String  | | State/province of the visitor  |
  | `country` | String  | | Country/region of the visitor  |
  | `city` | String  | | City of the visitor  |


## STTVoiceConfig Object
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `encoding` | enum([STTAudioEncoding](#STTAudioEncoding-Object))   | | Encoding of audio data. For details, see AudioEncoding.   |
  | `sampleRateHertz` | Int   | | Sample rate in Hertz of the audio data. Valid values are: 8000-48000. 16000 is optimal. For best results, set the sampling rate of the audio source to 16000 Hz. If that is not possible, use the native sample rate of the audio source (instead of re-sampling). This field is optional for FLAC and WAV audio files, but is required for all other audio formats. For details, see AudioEncoding.   |
  | `languageCode`|string|| The language of the voice expressed as a BCP-47 language tag, e.g. "en-US". |


## STTAudioEncoding Object   
The encoding of the audio data . 

For best results, the audio source should be captured and transmitted using a lossless encoding (FLAC or LINEAR16). The accuracy of the speech recognition can be reduced if lossy codecs are used to capture or transmit audio, particularly if background noise is present. Lossy codecs include MULAW, AMR, AMR_WB, OGG_OPUS, SPEEX_WITH_HEADER_BYTE, MP3, and WEBM_OPUS. 

The FLAC and WAV audio file formats include a header that describes the included audio content. You can request recognition for WAV files that contain either LINEAR16 or MULAW encoded audio. If you specify an AudioEncoding when you send FLAC or WAV audio, the encoding configuration must match the encoding described in the audio header; 
  |Enums|   |
  | - | - | 
  | `ENCODING_UNSPECIFIED` | Not specified.   |
  | `LINEAR16` | Uncompressed 16-bit signed little-endian samples (Linear PCM).   |
  | `FLAC` | FLAC (Free Lossless Audio Codec) is the recommended encoding because it is lossless--therefore recognition is not compromised--and requires only about half the bandwidth of LINEAR16. FLAC stream encoding supports 16-bit and 24-bit samples, however, not all fields in STREAMINFO are supported.   |
  | `MULAW` | 8-bit samples that compand 14-bit audio samples using G.711 PCMU/mu-law.   |
  | `AMR` | Adaptive Multi-Rate Narrowband codec. sampleRateHertz must be 8000.   |
  | `AMR_WB` | Adaptive Multi-Rate Wideband codec. sampleRateHertz must be 16000.   |
  | `OGG_OPUS` | Opus encoded audio frames in Ogg container (OggOpus). sampleRateHertz must be one of 8000, 12000, 16000, 24000, or 48000.   |
  | `SPEEX_WITH_HEADER_BYTE` | Although the use of lossy encodings is not recommended, if a very low bitrate encoding is required, OGG_OPUS is highly preferred over Speex encoding. The Speex encoding supported by Cloud Speech API has a header byte in each block, as in MIME type audio/x-speex-with-header-byte. It is a variant of the RTP Speex encoding defined in RFC 5574. The stream is a sequence of blocks, one block per RTP packet. Each block starts with a byte containing the length of the block, in bytes, followed by one or more frames of Speex data, padded to an integral number of bytes (octets) as specified in RFC 5574. In other words, each RTP header is replaced with a single byte containing the block length. Only Speex wideband is supported. sampleRateHertz must be 16000.   |
  | `WEBM_OPUS` | Opus encoded audio frames in WebM container (OggOpus). sampleRateHertz must be one of 8000, 12000, 16000, 24000, or 48000.   |
  
  
## TTSVoiceConfig Object
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `encoding` | enum([TTSAudioEncoding](#TTSAudioEncoding))   | | Encoding of audio data. For details, see AudioEncoding.   |
  | `sampleRateHertz` | Int   | | Sample rate in Hertz of the audio data. Valid values are: 8000-48000. 16000 is optimal. For best results, set the sampling rate of the audio source to 16000 Hz. If that is not possible, use the native sample rate of the audio source (instead of re-sampling). This field is optional for FLAC and WAV audio files, but is required for all other audio formats. For details, see AudioEncoding.   |
  | `languageCode` | String   | | The language of the voice expressed as a BCP-47 language tag, e.g. "en-US".   |
  | `Gender` | enum   | | MALE, FEMALE.  |
  
## TTSAudioEncoding Object     
Configuration to set up audio encoder. The encoding determines the output audio format that we'd like. 
  |Enums|   |
  | - | - | 
  | `AUDIO_ENCODING_UNSPECIFIED` | Not specified.   |
  | `LINEAR16` | Uncompressed 16-bit signed little-endian samples (Linear PCM).   |  
  | `MP3` | MP3 audio at 32kbps.   |  
  | `OGG_OPUS` | Opus encoded audio wrapped in an ogg container. The result will be a file which can be played natively on Android, and in browsers (at least Chrome and Firefox). The quality of the encoding is considerably higher than MP3 while using approximately the same bitrate.   |
  | `MULAW` | 8-bit samples that compand 14-bit audio samples using G.711 PCMU/mu-law. Audio content returned as MULAW also contains a WAV header.   |
  | `ALAW` | 8-bit samples that compand 14-bit audio samples using G.711 PCMU/A-law. Audio content returned as ALAW also contains a WAV header.   |
    
## SpeechRecognitionResult Object     
  A speech recognition results corresponding to a portion of the audio.  
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `alternatives` | [SpeechRecognitionAlternative](#SpeechRecognitionAlternative)   | | May contain one or more recognition hypotheses (up to the maximum specified in maxAlternatives). These alternatives are ordered in terms of accuracy, with the top (first) alternative being the most probable, as ranked by the recognizer.  |  

## SpeechRecognitionAlternative Object     
  Alternative hypotheses (a.k.a. n-best list).   
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `transcript` | String   | | Transcript text representing the words that the user spoke.   |
  | `confidence` | Number   | | The confidence estimates between 0.0 and 1.0. A higher number indicates an estimated greater likelihood that the recognized words are correct. This field is set only for the top alternative of a non-streaming result or, of a streaming result where isFinal=true. This field is not guaranteed to be accurate and users should not rely on it to be always provided. The default of 0.0 is a sentinel value indicating confidence was not set.   |

## SynthesisInput Object     
  Contains text input to be synthesized. The input size is limited to 5000 characters.    
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `text` | String   | | The raw text to be synthesized.  |

## Variable Object     
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `name` | String   | | the name of a variable in a form.  |
  | `value` | String  | | the value of a variable.  |
  
## PlayAudioAction Object  
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `type` | String  | Required| type of the response,including `voice`,`url`|
  | `voice` | string | Required when type is `voice`| The audio data bytes encoded as specified in VoiceConfig. Note: as with all bytes fields, proto buffers use a pure binary representation, whereas JSON representations use base64.A base64-encoded string. |
  | `voiceConfig` | [VoiceConfig Object](#VoiceConfig-Object)  |Required when type is `voice` | The encoding of the voice data sent in the request. |
  | `audioPath` | String  |Required when type is `url` | the audio file url  |
  
  ## CollectDTMFDigitsAction Object   
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `numberOfDigits` | String  | | Enumeration. 1, 2, … 29, 30, Variable. The number of digits entered by the caller in dialer. Default: Not sure. |
  | `stopGatherAfterPresskey` | String  | | Enumeration. *, #. Available when Number of Digits is Not sure.  |
  
  ## TransferCallAction Object   
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `transferTo` | String  | | Support Phone Number and Sip URI.  |
  
  ## EndCallAction Object   
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
