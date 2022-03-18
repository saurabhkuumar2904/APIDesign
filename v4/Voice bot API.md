  | Change Version | API Version | Change nots | Change Date | Author |
  | - | - | - | - | - |
  | 2.0 | v1 | Voice Bot API | 2022-3-18 | Leon，Carl，Page，Zack |  


# Summary

## Channel Adapter API 
The ChannelURI can be any valid URI that implements this API, and it is configured in the system when a new channel needs to access. 
  - POST /{channelURI}/input - [Channel Adapter receives input](#Channel-Adapter-receives-input). 
## Voice Service API  
  - POST /voiceservice/sessions - [Create Session](#Create-Sessions). Voice service creates a session 
  - POST /voiceservice/sessions/{sessionId}/questions - [Receive a question](#Receive-a-question).  Voice service receives call question.
  - DELETE /voiceservice/sessions/{sessionId} - [Delete a session](#Delete-a-session). Voice service deletes a session.
  - POST /voiceservice/sessions/{sessionId}/variables - [Update Variables](#Update-Variables).Receive the variables of the Voice Bot Session
## Voice Service Input API   
  - POST /voiceservice/sessions/{sessionId}/answers - [Voice Service receives answers](#Voice-Service-receives-answers)
## Voice Bot Service API 
  - POST /voicebot/voicebots/{VoicebotId}/sessions  - [Create Session Voice Bot creates session](#Create-A-New-Voice-Bot-Session)
  - POST /voicebot/sessions/{sessionId}/messages - [Voice Bot receive a message](#Voice-Bot-receive-a-message). Voice Bot receives input   
  - DELETE /voicebot/sessions/{sessionId} - [Delete Session Voice Bot deletes the session](#Delete-Voice-Bot-session)
  - POST /voicebot/sessions/{sessionId}/variables - [Update Variables](#Update-Variable).Receive the variables of the Voice Bot Session
## STT & TTS API 
Provide STT (Speech to Text) and TTS (Text to Speech) capabilities. 
  - POST /stttts/stt:speechToText - [Speech To Text](#Speech-To-Text) 
  - POST /stttts/tts:textToSpeech - [Text To Speech](#Text-To-Speech)


# Endpoints
## Channel Adapter API 
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

#### example:
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

#### example:
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
  |`content`  |  [VoiceBotAction[]](#VoiceBotAction-Object)  | Greeting output  |

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




### Receive a question 
`POST /voiceservice/sessions/{sessionId}/questions`

#### Parameters
Path parameters 
  | Name | Type | Required | Description |    
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

#### example:
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
  |`content`  |  [VoiceBotAction[]](#VoiceBotAction-Object)  | output  |

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

### Delete a Session   
`DELETE /voiceservice/sessions/{sessionsId} `

#### Parameters
Path parameters 
  | Name | Type | Required | Description |    
  | - | - | :-: | - | 
  | `sessionId` | Guid | yes |  |

#### Response

 HTTP/1.1 200 OK 


### Update variables
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


## Voice Service Input API
### Voice Service receives answers 
`POST /voiceservice/sessions/{sessionId}/answers`

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
        "value":"string", 
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
  | `config` | [VoiceConfig Object](#VoiceConfig-Object)  | yes | |  Provides information to the recognizer that specifies how to process the request.    |
  | `audio`  |  string   | yes  |   | The audio data to be recognized.    |


#### example:
```Json 
 { 
    "config": { 
      "encoding": enum [(AudioEncoding)](#AudioEncoding), 
      "sampleRateHertz": integer, 
      "languageCode": string, 
    }, 
    "audio": string
  } 
```

#### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`results[]` | [SpeechRecognitionResult](#SpeechRecognitionResult)  | Sequential list of transcription results corresponding to sequential portions of audio.    |

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

`POST /stttts/tts:textToSpeech`

#### Parameters

Request body 
The request body contains data with the follow structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `input` | [SynthesisInput](#SynthesisInput)  | yes | |  The Synthesizer requires plain text as input.     |
  | `voice`  |  [VoiceSelectionParams](#VoiceSelectionParams)   | yes  |   | The desired voice of the synthesized audio.     |
  | `voiceConfig`  |  [VoiceConfig Object](#voiceConfig-Object)   | yes  |   | The configuration of the synthesized audio.     |


#### example:
```Json 
 { 
    "input": { 
      // Union field input_source can be only one of the following: 
      "text": string, 
       // End of list of types for union field input_source. 
    }, 
   "voice": { 
      "languageCode": string, 
      "gender": enum , 
    }, 
    "voiceConfig":{ 
      "audioEncoding": enum , 
      "speakingRate": number, 
      "pitch": number, 
    }
} 
```

#### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :- | 
  |`voiceContent` | string  | The audio data bytes encoded as specified in the request, including the header for encodings that are wrapped in containers (e.g. MP3, OGG_OPUS). For LINEAR16 audio, we include the WAV header. Note: as with all bytes fields, protobuffers use a pure binary representation, whereas JSON representations use base64.A base64-encoded string. |
  |`voiceConfig` | [VoiceConfig Object](#VoiceConfig-Object)  | A link between a position in the original request input and a corresponding time in the output audio. It is only supported via SSML input.   |

Response
```Json
 HTTP/1.1 200 OK 
  Content-Type:  application/json 
  { 
  "voiceContent": string, 
  "voiceConfig": { 
    "audioEncoding": enum , 
    "speakingRate": number, 
    "pitch": number, 
  } 
} 
```


# Model
## Voice Message Object 
  
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `id` | Guid  | | |
  |`content`  |  [VoiceBotAction](#voicebotaction-object)[]    |  |

## VoicebotOutput Object 
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `id` | Guid  | | |
  | `content`  |  [VoiceBotAction](#voicebotaction-object)[]    |  |

## VoicebotAction Object 
  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  | `type` | string | | type of the response,including `PlayAudio`,`PlayText`,`CollectDTMFDigits`,`CollectSpeechResponse`,`IVRMenu`,`TransferChat`, `EndCall`|
  | `content` | object | |  response's content. when type is `PlayAudio`, it represents [PlayAudio](#PlayAudio-object); when type is `PlayText`,it represents [PlayText](#PlayText-object);when type is `CollectDTMFDigits`,it represents [CollectDTMFDigits](#CollectDTMFDigits-object); when type is `CollectSpeechResponse`, it represents [CollectSpeechResponse](#CollectSpeechResponse-object);when type is `EndCall`, it represents [EndCall](#EndCall-object);when type is `IVRMenu`, it represents [IVRMenu](#IVRMenu-object);when type is `TransferChat`, it represents [TransferChat](#TransferChat-object);|
  | `voiceConfig` | [VoiceConfig Object](#VoiceConfig-Object)  | | The encoding of the voice data sent in the request. |
  | `voice` | string | | The audio data bytes encoded as specified in VoiceConfig. Note: as with all bytes fields, proto buffers use a pure binary representation, whereas JSON representations use base64.A base64-encoded string. |
  

## PlayAudio Object  
  Text Response is represented as simple flat json objects with the following keys: 
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `audioPath` | String  | | String |
  
## PlayAudio Object  
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

## TransferChat Object   
  Text Response is represented as simple flat json objects with the following keys: 
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `transferTo` | String  | | Support Phone Number and URI.  |


## EndCall Object   
  Text Response is represented as simple flat json objects with the following keys: 
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 


## IVRMenu Object   
  Text Response is represented as simple flat json objects with the following keys: 
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `message` | String  | | String  |

## Visitor Object   
  Text Response is represented as simple flat json objects with the following keys: 
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `name` | String  | | Name of the visitor  |
  | `phone` | String  | | Phone of the visitor  |
  | `state/province` | String  | | State/province of the visitor  |
  | `country/region` | String  | | Country/region of the visitor  |
  | `city` | String  | | City of the visitor  |


## VoiceConfig Object
  Text Response is represented as simple flat json objects with the following keys: 
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `encoding` | enum([AudioEncoding](#AudioEncoding-request))   | | Encoding of audio data sent in all RecognitionAudio messages. This field is optional for FLAC and WAV audio files and required for all other audio formats. For details, see AudioEncoding.   |
  | `sampleRateHertz` | Int   | | Sample rate in Hertz of the audio data sent in all RecognitionAudio messages. Valid values are: 8000-48000. 16000 is optimal. For best results, set the sampling rate of the audio source to 16000 Hz. If that is not possible, use the native sample rate of the audio source (instead of re-sampling). This field is optional for FLAC and WAV audio files, but is required for all other audio formats. For details, see AudioEncoding.   |
  | `languageCode` | string   | | Required. The language of the supplied audio as a BCP-47 language tag. Example: "en-US". See Language Support for a list of the currently supported language codes.    |

## IVRMenu Object   
The encoding of the audio data sent in the request. 

For best results, the audio source should be captured and transmitted using a lossless encoding (FLAC or LINEAR16). The accuracy of the speech recognition can be reduced if lossy codecs are used to capture or transmit audio, particularly if background noise is present. Lossy codecs include MULAW, AMR, AMR_WB, OGG_OPUS, SPEEX_WITH_HEADER_BYTE, MP3, and WEBM_OPUS. 

The FLAC and WAV audio file formats include a header that describes the included audio content. You can request recognition for WAV files that contain either LINEAR16 or MULAW encoded audio. If you send FLAC or WAV audio file format in your request, you do not need to specify an AudioEncoding; the audio encoding format is determined from the file header. If you specify an AudioEncoding when you send FLAC or WAV audio, the encoding configuration must match the encoding described in the audio header; 
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
  
## SpeechRecognitionResult
  A speech recognition results corresponding to a portion of the audio.  
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `alternatives` | [SpeechRecognitionAlternative](#SpeechRecognitionAlternative)   | | May contain one or more recognition hypotheses (up to the maximum specified in maxAlternatives). These alternatives are ordered in terms of accuracy, with the top (first) alternative being the most probable, as ranked by the recognizer.  |
  | `languageCode` | String   | | Output only. The BCP-47 language tag of the language in this result. This language code was detected to have the most likelihood of being spoken in the audio.  |

## SpeechRecognitionAlternative
  Alternative hypotheses (a.k.a. n-best list).   
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `transcript` | String   | | Transcript text representing the words that the user spoke.   |
  | `confidence` | Number   | | The confidence estimates between 0.0 and 1.0. A higher number indicates an estimated greater likelihood that the recognized words are correct. This field is set only for the top alternative of a non-streaming result or, of a streaming result where isFinal=true. This field is not guaranteed to be accurate and users should not rely on it to be always provided. The default of 0.0 is a sentinel value indicating confidence was not set.   |

## SynthesisInput
  Contains text input to be synthesized. The input size is limited to 5000 characters.    
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `text` | String   | | The raw text to be synthesized.  |

## VoiceSelectionParams
  Description of which voice to use for a synthesis request.     
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `languageCode` | String   | | The language of the voice expressed as a BCP-47 language tag, e.g. "en-US".   |
  | `Gender` | enum   | | MALE, FEMALE.  |

## Variable Object
  Description of which voice to use for a synthesis request.      
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `name` | String   | | the name of a variable in a form.  |
  | `value` | String  | | the value of a variable.  |
