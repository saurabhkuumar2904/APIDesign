  | Change Version | API Version | Change nots | Change Date | Author |
  | - | - | - | - | - |
  |X2 4.0 | v4 | Bot API | 2021-9-8 | Leon |  
 
 # Summary
 ## Bot Integration Structure Diagram
![image](https://user-images.githubusercontent.com/8872646/146502833-661bd26b-2e35-4e71-88af-fea3f1a4a6f7.png)


## Integration Steps
To integrate your Chatbot with Comm100 Platform, you only need to do the following two steps:
### 1. Building Your Bot Adapter Service
  As shown in the diagram, You need to establish your own Chatbot Adapter, which connects to your BOT Engine service and implements the interfaces in this API documents.
  You can first implement the following two important interfaces and start chatting
  - `POST /chatbotSessions` - [Create a new Chatbot Session](#create-a-new-chatbot-session)
  - `POST /chatbotSessions/{id}/interactions` - [Send a  Chatbot Input and get a Chatbot Output](#create-a-chatbot-interaction)  
  See [API Description](#api-description) for all interface definitions  
  Click [Data Struct](#data-struct) to view key data structure definitions  
### 2. Creating a new Comm100 Chatbot and fill in your adapter service base URI
Create a new Chatbot in the Comm100 Control panel and configure it as follows   
1. Fill in the Bot name  
2. Select "Third Party Engine" as the bot Engine  
3. Fill in your adapter root URI in the "Webhook target URL" input box  
4. Select the Comm100 Live Chat channel   
![image](https://user-images.githubusercontent.com/8872646/146148035-2b0d215e-0064-45a2-bcd7-07cbd16356e5.png)


# API Description
## ChatbotSession
  - `POST /chatbotSessions` - [Create a new Chatbot Session](#create-a-new-chatbot-session)
  - `DELETE /chatbotSessions/{chatbotSessionId}` - [Delete the Chatbot Session](#delete-the-chatbot-session)
## ChatbotInteraction  
  - `POST /chatbotSessions/{id}/interactions` - [Send a  Chatbot Input and get a Chatbot Output](#create-a-chatbot-interaction)
<!-- ## ChatbotSessionVariable -->
<!--   - `GET /chatbotSessions/{id}/variables` - [get the variables of the Chatbot Session ](#get-the-variables) -->
<!--   - `PUT /chatbotSessions/{id}/variables` - [Update the variables of the Chatbot Session ](#update-the-variables) -->

# Endpoints

### Create A New Chatbot Session
`POST /chatbotSessions`

#### Parameters
Request Body
The request body contains data with the following structure:

  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `chatbotId` | Guid | yes | |  The unique id of the bot |
  | `channel` | string | yes | |  The channel of the bot |
  | `visitor`  |  [SessionVisitorInfo](#sessionvisitorinfo-object)  |no |   |  |

example:
```Json 
  {
    "chatbotId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
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
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`sessionId` | string | The unique id of the session |
  |`content`  |  [GeneralResponse](#GeneralResponse-object)[]     |  |


#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "chatbotId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48", 
    "visitor": {
      "name":"Kart",
      "email":"kart@yahoo.com",
      "phone":"123-4355-212",
    }
  }' -X POST https://domain.comm100.com/api/v4/chatbotSessions
```
.net code example
![image](https://user-images.githubusercontent.com/8872646/153159849-198ff873-de2b-405c-b65b-aea100374a16.png)


Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

{
    "sessionId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
    "content": [
        {
            "type": "text",
            "content": {
                "links": [
                    {
                        "buttonText": "ok",
                        "url": "http://baidu.com",
                        "type": "intent",
                        "openStyle": "tail",
                        "openIn": "sideWindow",
                        "order": 1
                    }
                ],
                "message": "Hi there! I'm a chatbot, here to help answer your questions."
            },
            "delaytime":1
        }
    ]
}
```
### Delete The Chatbot Session
`DELETE /chatbotSessions/{ChatbotSessionId}`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `chatbotSessionId` | string | yes  |  the unique id of the Chatbot Session |  


#### Response
This method does not specify any sample responses.

#### Example
Using curl
```
curl -X DELETE https://domain.comm100.com/api/v4/chatbotSessions/a9928d68-92e6-4487-a2e8-8234fc9d1f48
```
Response Example
```Json
  HTTP/1.1 200 OK

```

### Create a Chatbot Interaction
`POST /chatbotSession/{chatbotSessionId}/interactions`

#### Parameters

Request body

The request body  is: [ChatbotInput](#chatbotinput-object) Object

example:
```Json 
  {
    "input":{
      "type":"text",
      "content":{
        "text":"i want to buy NBN"
      }
    }
  }
```
.net code example:
![image](https://user-images.githubusercontent.com/8872646/146140038-e9e1be33-ce9c-4b0f-8b4a-75d4cd63ff75.png)

#### Response
the response is: [ChatbotOutput](#chatbotoutput-object) Object

#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '{
    "input":{
      "type":"text",
      "content":{
        "text":"i want to buy NBN"
      }
    }
  }' -X POST https://domain.comm100.com/api/v4/chatbotSessions/f9928d68-92e6-4487-a2e8-8234fc9d1f48/interactions
```
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json

  { 
    "output":{
          "id":"d3f5b968-ad51-42af-b759-64c0afc40b84",
          "content":[
              {
                "type":"text",
                "content": {
                    "links": [{
                      "buttonText":"ok",
                      "url":"http://baidu.com",
                      "type":"intent",
                      "openStyle":"tail",
                      "openIn":"sideWindow",
                      "order":1,
                    }],
                    "message": "Hi there! I'm a chatbot, here to help answer your questions.",                        
              },
                "delaytime":1
              }
          ]
    }   
  }
```
<!--
 
### Get The Variables
`GET /chatbotSession/{chatbotSessionId}/variables`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `chatbotSessionId` | Guid | yes  |  the unique id of the Chatbot Session |  

Request body

#### Response
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`variables` |[VariableValue](#variablevalue-object)[]  | |  an array of [VariableValue](#variablevalue-object)  |

```Json 
  {
    "variables":[
      {
        "name":"ename",
        "value":"leon"
      }
    ]
  }
```

#### Example
Using curl
```
curl -H "Content-Type: application/json"  -X GET https://domain.comm100.com/api/v4/chatbotSessions/f9928d68-92e6-4487-a2e8-8234fc9d1f48/variables
```
Response
```Json
  HTTP/1.1 200 OK
    {
    "variables":[
      {
        "name":"ename",
        "value":"leon"
      }
    ]
  }
```
### Update The Variables
`PUT /chatbotSession/{chatbotSessionId}/variables`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `chatbotSessionId` | Guid | yes  |  the unique id of the Chatbot Session |  

Request body
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`variables` |[VariableValue](#variablevalue-object)[]  | |  an array of [VariableValue](#variablevalue-object)  |
example:
```Json 
  {
    "variables":[
      {
        "name":"ename",
        "value":"leon"
      }
    ]
  }
```

#### Response


#### Example
Using curl
```
curl -H "Content-Type: application/json" -d '  {
    "variables":[
      {
        "name":"ename",
        "value":"leon"
      }
    ]
  }' -X PUT https://domain.comm100.com/api/v4/chatbotSessions/f9928d68-92e6-4487-a2e8-8234fc9d1f48/variables
```
Response
```Json
  HTTP/1.1 200 OK
```
 -->
# Data Struct

   - [ChatbotSession](#chatbotsession)    A ChatbotSession means a Session between the user and the chatbot
     - [ChatbotInteraction](#chatbotinteraction) One ChatbotInteraction means one user input and one Chatbot output
       - [ChatbotInput](#chatbotinput-object) One User Input
       - [ChatbotOutput](#chatbotoutput-object) One Chatbot Output
         - [ChatbotResponse](#chatbotresponse-object) One Chatbot output consists of multiple ChatbotResponse
### CreateChatbotSessionRequest Object

  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `chatbotId` | Guid  | | chatbotId |  
  | `channel` | string  | | including`Live Chat`,`Facebook Messager`,`Twitter Direct Message`,`WeChat`,`WhatsApp`,`IVR`,`SMS`,`Default` | 
  | `visitor` | [SessionVisitorInfo](#sessionvisitorinfo-object)  |   |  |

### CreateChatbotSessionResponse Object
   

  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `sessionId` | string  | | sessionId |  
  | `content` | [GeneralResponse](#generalResponse-object)[]  |   |  |
### InteractionRequest Object

  |Name| Type | Default | Description | 
  | - | - | :-: | - |   
  | `input` |  [ChatbotInput](#chatbotinput-object)|  |  |  
### InteractionResponse Object

  |Name| Type | Default | Description | 
  | - | - | :-: | - |     
  | `output` |  [ChatbotOutput](#chatbotoutput-object)|  |  |

### ChatbotInput Object

  |Name| Type | Default | Description | 
  | - | - | :-: | - |   
  | `type` | String  | | 	type of the response,including`text`,`location`,`option`,`form`|
  | `content` | object  | | [InputText](#inputText-object), [InputLocation](#inputLocation-object), [InputOption](#inputOption-object),[InputForm](#inputform-object)

  ### InputText Object
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `text` | String  | |  |
  ### InputLocation Object
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `location` | String  | | the longitude and latitude of the location, e.g. "-39.900000,116.300000" |
  | `action` | string  | | submit, cancel |
  ### InputOption Object
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `optionId` | String  | |optionId |
  ### InputForm Object
  |Name| Type | Default | Description | 
  | - | - | :-: | - |
  | `formValues` | [FieldValue](#FieldValue-object)[]  | |  an array of [FieldValue](#FieldValue-object) |
  | `action` | string  | | submit, cancel |
  
### FieldValue Object

|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
|`name` | string |  | the name of a field in a form. |
|`value` | string |  | the value of a field. |
### VariableValue Object
|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
|`name` | string |  | the name of a variable in a form. |
|`value` | string |  | the value of a variable. |
### ChatbotOutput Object
  ChatbotMessage Object is represented as simple flat JSON objects with the following keys:  

  |Name| Type| Default | Description     |
  | - | - | :-: | - |
  | `id` | Guid  |  | the unique id of the response |
  | `content` | [GeneralResponse](#generalResponse-object)[]|  |   |


### GeneralResponse Object
  Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`type` | string | | type of the response,including `Text`,`QuickReply`、 `Image`、`Video`、`Authentication`,`Location`,`VariableData`,`Form`,`TransferChat`|
  | `content` | object | |  response's content. when type is `Text`, it represents [OutputText](#outputtext-object); when type is `QuickReply`,it represents [OutputQuickReply](#outputquickreply-object);when type is `Image`,it represents [OutputImage](#outputimage-object);when type is `Video`,it represents [OutputVideo](#outputvideo-object); when type is `Location`, it represents [OutputLocation](#outputLocation-object);when type is `Form`, it represents [OutputForm](#form-object);when type is `TransferChat`, it represents [OutputTransferChat](#transferchat-object); when type is `OutputAudio`, it represents [OutputAudio](#OutputAudio-object);when type is `OutputEndCall`, it represents [OutputEndCall](#OutputEndCall-object);when type is `OutputIVRMenu`, it represents [OutputIVRMenu](#OutputIVRMenu-object);|
  |`delayTime` | decimal | 1 | how many seconds delay to show  |

### OutputText Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`message` | string |  | string  |
  |`Links` | [TextLink](#textLink-object)[]|  |   |
 #### TextLink Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`buttonText` | String |  |   |
  |`url` | String |  |   |
  |`type` | String |  | webPage,webview|
  |`openStyle` | String |  | full,tall,compact  |
  |`openIn` | String |  | newWindow, sideWindow,currentWindow|
  |`order` | int |  |   |
 ### OutputQuickReply Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - |   
  |`message` | string |  | string  |
  |`options` | [option](#option-object)[] |  |   |
  |`isForce` | bool |  | must select and can not input  text  |
   
### OutputImage Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`imageUrl` | string |  | string  |
  |`message` | string |  | string  |
### OutputVideo Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`videoUrl` | string |  | string  |
  |`message` | string |  | string  |

 ### OutputLocation Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`buttonText` | string |  | string  |
  |`isForce` | bool |  | must location and can not input  text  |
  |`message` | string |  | string  |

  ### OutputTransferChat Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`transferTo` | Guid |  |agentid  when type is `transferToAgent` ,departmentid when type is `transferToDepartment`|
  |`type` | string |  | type:`transferToAgent`,`transferToDepartment`,`transferRoutingRules`  |
  |`messageWhenAgentOffline` | string |  | Message when agent is offline  |

### OutputAudio Object
  Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`audioPath` | String|  | String  |
  
### OutputEndCall Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  
### OutputIVRMenu Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`message` | String |  |  String |
  
agentid can obtain from there

![ac0859808d9d39f528d6863e9d46a2d](https://user-images.githubusercontent.com/8872646/152307132-9dd5918d-3a18-4fb6-b5b4-84d0f689d561.png)

departmentid can obtain from there

![image](https://user-images.githubusercontent.com/8872646/153702520-2b9ebd22-1eb8-4143-8e84-4b0cd1e86ae4.png)

### OutputForm Object
FormReplyResponse is represented as simple flat json objects with the following keys:

|Name| Type| Default | Description     | 
| - | - | :-: | - | 
|`message` | string |   | A separate message which is sent before the form is sent.|
|`title` | string |  | the title of that form|
|`isConfirmationRequired` | bool |   | whether visitor needs to click confirm before the form is submitted.|
|`fields` | [ChatbotActionSendFormField](#ChatbotActionSendFormField-object)[] | | an array of [ChatbotActionSendFormField](#ChatbotActionSendFormField-object)  |
|`submitButtonText` | string |   | |
|`cancelButtonText` | string |   | |
|`confirmButtonText` | string |   | |
|`isforce` | string |   | |


#### ChatbotActionSendFormField Object
Field is represented as simple flat json objects with the following keys:

|Name| Type| Default | Description     | 
| - | - | :-: | - | 
|`type` | string | | enums: `text` ,`textArea`,`radio` ,`checkBox` ,`dropDownList` ,`checkBoxList`,`email` type refers to the different kinds of fields which can be used in a form. |
|`name` | string |  |Name of the form field. |
|`value` | string | | Value of the field |
|`isRequired` | bool |  | Whether the field is required or not |
|`options` | string[] |  | an array of of string when the fieldType is `radio` ,`dropDownList` ,`checkBoxList`|
|`order` | integer |  | Must be greater than or equal 0, ascending sort |
|`variableName` | string |  |  |

### Option Object
Text Response is represented as simple flat json objects with the following keys:

  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  |`optionId` | Guid |  |   |
  |`order` | int |  |   |
  |`text` | String |  |   |
  |`type` | String | text | type of the option,including `TriggerAnIntent`,`ContactAnAgent`、 `Text`  |
### SessionVisitorInfo Object

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
|`longitude` | number |  |  |
|`latitude` | number |  |  |
|`pageViews` | integer |  |  |
|`browser` | integer |  |  |
|`chats` | integer |  |  |
|`company` | string |  |  |
|`currentBrowsing` | string |  |  |
|`department` | string |  |  |
|`firstVisitTime` | time |  |  |
|`flashVersion` | string |  |  |
|`keywords` | string |  |  |
|`landingPage` | string |  |  |
|`language` | string |  |  |
|`operatingSystem` | string |  |  |
|`productService` | string |  |  |
|`ticketId` | string |  |  |
|`referrerUrl` | string |  |  |
|`screenResolution` | string |  |  |
|`searchEngine` | string |  |  |
|`state` | string |  |  |
|`status` | integer |  |  |
|`timeZone` | string |  |  |
|`visitTime` | time |  |  |
|`visits` | integer |  |  |
|`ssoId` | integer |  |  |
|`chatRequestingPageUrl` | integer |  |  |
|`campaignId` | string |  |  |
|`channelAccountId` | string |  |  |


