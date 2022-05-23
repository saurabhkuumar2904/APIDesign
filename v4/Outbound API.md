  | Change Version | API Version | Change nots | Change Date | Author |Architect Team Reviewer | 
  | - | - | - | - | - |- |
  | 1.0 | v1 |Outbound API | 2022-5-18 | Leon，Frank,Yogesh|  Allon|

## Summary

### Outbound Message Callback API 

The CallbackURL can be any valid URL that implements this API, and it is configured in the system when a new Outbound Message Campaign is created. 
  - POST /{callbackURL} - [Outbound Message Callback API](#voice-channel-adapter-receives-input). 

### Outbound Message API  

#### Outbound Campaign
  - GET /outbound/campaigns/ - [Get the list of Campaign](#get-the-list-of-campaign). 
  - GET /outbound/campaigns/{id} - [Get a single Campaign](#get-a-single-campaign). 
  - POST /outbound/campaigns/ - [Create A New Campaign](#create-a-new-campaign).  
  - PUT /outbound/campaigns/{id} - [Update the Campaign](#update-the-campaign).  
  - DELETE /outbound/campaigns/{id} - [Delete the campaign](#delete-the-campaign). 
#### Outbound Message
  - GET /outbound/messages/ - [Get the list of Message](#get-the-list-of-message). 
  - GET /outbound/messages/{id} - [Get a single Message Record](#get-a-single-message). 
  - POST /outbound/campaigns/{id}/messages - [Send A Message](#send-a-message).   
  - PUT /outbound/messages/{id} - [Update the Message](#update-the-Message).  

### Contact API 
####  Contact
  - GET /contact/contacts/ - [Get the list of Contact](#get-the-list-of-contact).  
  - GET /contact/contacts/{id} - [Get a single Contact](#get-a-single-contact).  
  - POST /contact/contacts/ - [Create A New Contact](#create-a-new-contact).  
  - PUT /contact/contacts/{id} - [Update the Contact](#update-the-contact).  
  - DELETE /contact/contacts/{id} - [Delete the Contact](#delete-the-contact). 

### Ticketing Outbound Message API 
  - POST /ticketing/outboundmessages/ - [Create A New outbound message](#create-a-new-outbound-message). 
  - GET /ticketing/outboundmessages/{id} - [Get a single outbound message](#get-a-single-outbound-message). 

## Endpoints

### Get the list of Outbound Campaign
`GET /outbound/campaigns/`
#### Parameters
No parameters

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outboundCampaignList` |[OutboundCampaign](#variablevalue-object)[]  |Yes|  An array of [OutboundCamgaign](#variablevalue-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"outboundCampaigns": [
    {
		"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"name": "test-campaign",
		"description": "test campaign",
		"channel": "SMS",
		"triggerMode": "Manual",
		"status": "Draft",
		"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
		"message": "Hello, please fill in your application form by the end of this week!",
		"isNewTicketAutoCreatedForMessage": "Yes",
		"timeToAutoCreateNewTicket": "When the message is successfully sent",
		"contactFilterConditionMetType": "Any",
		"contactFilterLogicalExpresssion": "",
		"whenToSend": "Right Away",
		"scheduledStartTime": ""
	}
    ]
}
```

### Get a single Outbound Campaign
`GET /outbound/campaigns/{id}`
#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outbound Campaign |  

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outboundCampaign` |[OutboundCampaign](#variablevalue-object)  |Yes|   [OutboundCampaign](#variablevalue-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"name": "test-campaign",
	"description": "test campaign",
	"channel": "SMS",
	"triggerMode": "Manual",
	"status": "Draft",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
	"isNewTicketAutoCreatedForMessage": "Yes",
	"timeToAutoCreateNewTicket": "When the message is successfully sent",
	"contactFilterConditionMetType": "Any",
	"contactFilterLogicalExpresssion": "",
	"whenToSend": "Right Away",
	"scheduledStartTime": ""
}
```

### Create a new Outbound Campaign
`POST /outbound/campaigns/`
#### Parameters
No parameters

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `name` | string | yes |  Name of this Outbound Campaign. |  
  | `description` | string | no |  Description of this Outbound Campaign. |  
  | `channel` | string | no |  App channel of this Outbound Campaign. |  
  | `triggerMode` | string | yes |  Trigger mode of this Outbound Campaign, allowed values are "Manual", "Scheduled", "API". |  
  | `status` | string | no |  Status of this Outbound Campaign, allowed values are "Draft", "Published", "Running", "Finished". |  
  | `channelAccountId` | Guid | no |  Channel account id of this Outbound Campaign. |  
  | `message` | string | no |  Message of this Outbound Campaign. |  
  | `isNewTicketAutoCreatedForMessage` | bool | no |  Whether a new ticket will be auto created or not for the message.|  
  | `timeToAutoCreateNewTicket` | string | no |  Allowed values are "When the message is successfully sent", "When contact replies the first message after the message is successfully sent". |  
  | `contactFilterConditionMetType` | string | no |  Allowed values are "All", "Any", "Logical Expression". |  
  | `contactFilterLogicalExpresssion` | string | no |  Contact Filter Logical Expresssion of this Condition. |  
  | `whenToSend` | string | no | Allowed values are "Right Away", "Scheduled". |  
  | `scheduledStartTime` | timestamp | no |  Message of this Outbound Campaign. |  
  
example:
```Json 
{
	"name": "test-campaign",
	"description": "test campaign",
	"channel": "SMS",
	"triggerMode": "Manual",
	"status": "Draft",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
	"isNewTicketAutoCreatedForMessage": "Yes",
	"timeToAutoCreateNewTicket": "When the message is successfully sent",
	"contactFilterConditionMetType": "Any",
	"contactFilterLogicalExpresssion": "",
	"whenToSend": "Right Away",
	"scheduledStartTime": ""
}
```

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outboundCampaign` |[OutboundCampaign](#variablevalue-object)  |Yes|   [OutboundCamgaign](#variablevalue-object)  |

```Json 
  HTTP/1.1 201 Created
  Content-Type: application/json
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"name": "test-campaign",
	"description": "test campaign",
	"channel": "SMS",
	"triggerMode": "Manual",
	"status": "Draft",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
	"isNewTicketAutoCreatedForMessage": "Yes",
	"timeToAutoCreateNewTicket": "When the message is successfully sent",
	"contactFilterConditionMetType": "Any",
	"contactFilterLogicalExpresssion": "",
	"whenToSend": "Right Away",
	"scheduledStartTime": ""
}
```

### Update a Outbound Campaign
`PUT /outbound/campaigns/{id}`
#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outbound Campaign |  

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outbound Campaign |  
  | `name` | string | yes |  Name of this Outbound Campaign. |  
  | `description` | string | no |  Description of this Outbound Campaign. |  
  | `channel` | string | no |  App channel of this Outbound Campaign. |  
  | `triggerMode` | string | yes |  Trigger mode of this Outbound Campaign, allowed values are "Manual", "Scheduled", "API". |  
  | `status` | string | no |  Status of this Outbound Campaign, allowed values are "Draft", "Published", "Running", "Finished". |  
  | `channelAccountId` | Guid | no |  Channel account id of this Outbound Campaign. |  
  | `message` | string | no |  Message of this Outbound Campaign. |  
  | `isNewTicketAutoCreatedForMessage` | bool | no |  Whether a new ticket will be auto created or not for the message.|  
  | `timeToAutoCreateNewTicket` | string | no |  Allowed values are "When the message is successfully sent", "When contact replies the first message after the message is successfully sent". |  
  | `contactFilterConditionMetType` | string | no |  Allowed values are "All", "Any", "Logical Expression". |  
  | `contactFilterLogicalExpresssion` | string | no |  Contact Filter Logical Expresssion of this Condition. |  
  | `whenToSend` | string | no | Allowed values are "Right Away", "Scheduled". |  
  | `scheduledStartTime` | timestamp | no |  Message of this Outbound Campaign. |  

example:
```Json 
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"name": "test-campaign 2",
	"description": "test campaign",
	"channel": "SMS",
	"triggerMode": "Manual",
	"status": "Published",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
	"isNewTicketAutoCreatedForMessage": "Yes",
	"timeToAutoCreateNewTicket": "When the message is successfully sent",
	"contactFilterConditionMetType": "Any",
	"contactFilterLogicalExpresssion": "",
	"whenToSend": "Right Away",
	"scheduledStartTime": ""
}
```

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`campaign` |[Campaign](#variablevalue-object)  |Yes|   [Camgaign](#variablevalue-object)  |

```Json 
  HTTP/1.1 200 Ok
  Content-Type: application/json
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"name": "test-campaign 2",
	"description": "test campaign",
	"channel": "SMS",
	"triggerMode": "Manual",
	"status": "Published",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
	"isNewTicketAutoCreatedForMessage": "Yes",
	"timeToAutoCreateNewTicket": "When the message is successfully sent",
	"contactFilterConditionMetType": "Any",
	"contactFilterLogicalExpresssion": "",
	"whenToSend": "Right Away",
	"scheduledStartTime": ""
}
```

### Delete a Campaign
`DELETE /outbound/campaigns/{id}`
#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outbound Campaign |  

#### Response
No response

```Json 
  HTTP/1.1 204 Ok
  Content-Type: application/json
{
}
```

### Get the list of Outbound Message
`GET /outbound/messages/`
#### Parameters
No parameters

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outboundMessageList` |[OutboundMessage](#variablevalue-object)[]  |Yes|  An array of [OutboundMessage](#variablevalue-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"outboundMessages": [
    {
		"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"sentTime": "2022-05-23 03:00:36.277",
		"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
		"message": "Hello, please fill in your application form by the end of this week!",
		"from": "Tom Cruise",
		"to": "XXX University",
		"status": "Successful",
		"failedReason": "",
		"outreachCampaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
		"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48"
	}
    ]
}
```

### Get a single Outbound Message
`GET /outbound/messages/{id}`
#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outbound Message |  

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outboundMessage` |[OutboundMessage](#variablevalue-object)  |Yes|   [OutboundMessage](#variablevalue-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"sentTime": "2022-05-23 03:00:36.277",
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
	"from": "Tom Cruise",
	"to": "XXX University",
	"status": "Successful",
	"failedReason": "",
	"outreachCampaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
	"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48"
}
```

### Create a new Outbound Message
`POST /outbound/campaigns/{id}/messages`
#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outbound Campaign |  

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `sentTime` | timestamp | yes |  The outreach campaign id of the Outbound Message. |  
  | `contactId` | Guid | yes |  Contact id of the Outbound Message. |  
  | `message` | string | yes |  Message. |  
  | `from` | string | yes |  Where the Outbound Message from. |  
  | `to` | string | yes |  Where the Outbound Message to. |  
  | `status` | string | yes |  Status of the Outbound Message. Allowed values are "Inqueue", "Successful", "Failed" |  
  | `failedReason` | string | no |  The failed reason of the Outbound Message. |  
  | `outreachCampaignId` | Guid | yes |  The outreach campaign id the Outbound Message. |  
  | `attachedToTicketId` | Guid | no |  The attached ticked Id of the Outbound Message. |  
  
example:
```Json 
{
	"sentTime": "2022-05-23 03:00:36.277",
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
	"from": "Tom Cruise",
	"to": "XXX University",
	"status": "Successful",
	"failedReason": "",
	"outreachCampaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
	"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48"
}
```

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outboundMessage` |[OutboundMessage](#variablevalue-object)  |Yes|   [OutboundMessage](#variablevalue-object)  |

```Json 
  HTTP/1.1 201 Created
  Content-Type: application/json
{
	"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"sentTime": "2022-05-23 03:00:36.277",
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
	"from": "Tom Cruise",
	"to": "XXX University",
	"status": "Successful",
	"failedReason": "",
	"outreachCampaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
	"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48"
}
```

### Update a Outbound Message
`PUT /outbound/messages/{id}`
#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outbound Message |  

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outbound Message. |  
  | `sentTime` | timestamp | yes |  The outreach campaign id of the Outbound Message. |  
  | `contactId` | Guid | yes |  Contact id of the Outbound Message. |  
  | `message` | string | yes |  Message. |  
  | `from` | string | yes |  Where the Outbound Message from. |  
  | `to` | string | yes |  Where the Outbound Message to. |  
  | `status` | string | yes |  Status of the Outbound Message. Allowed values are "Inqueue", "Successful", "Failed" |  
  | `failedReason` | string | no |  The failed reason of the Outbound Message. |  
  | `outreachCampaignId` | Guid | yes |  The outreach campaign id the Outbound Message. |  
  | `attachedToTicketId` | Guid | no |  The attached ticked Id of the Outbound Message. |  
  
example:
```Json 
{
	"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"sentTime": "2022-05-23 03:00:36.277",
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
	"from": "Tom Cruise",
	"to": "XXX University",
	"status": "Successful",
	"failedReason": "",
	"outreachCampaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
	"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48"
}
```

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outboundMessage` |[OutboundMessage](#variablevalue-object)  |Yes|   [OutboundMessage](#variablevalue-object)  |

```Json 
  HTTP/1.1 200 Ok
  Content-Type: application/json
{
	"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"sentTime": "2022-05-23 03:00:36.277",
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
	"from": "Tom Cruise",
	"to": "XXX University",
	"status": "Successful",
	"failedReason": "",
	"outreachCampaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
	"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48"
}
```

### Create A New outbound message
`POST /ticketing/outboundmessages/`

#### Parameters
Request body 
Request body the request body contains data with the following structure: 
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  |`id`  |  string  |yes |   | outbound message id| 
  |`from`  |  string  |yes |   |  phone number |
  |`to`  | string | yes |   |   |
  |`message`  | string | yes |   |   |
  |`contactId`  | Guid | yes |   |   |
  |`outboundCampaign`  |  [Outbound Campaign](#outbound-campaign-object) Object |yes |   |  |
  #### example:
```Json 
 {
    "id":"d3f5b968-ad51-42af-b759-64c0afc40b84",
    "from": "+19857473631", 
    "to": "+19857473632", 
    "contactId":"q3f5b438-xw31-44af-b729-64swaf3d0b56"
    "outboundCampaign": { 
        "id"":"q3f5b438-xw31-44af-b729-64swaf3d0b56",
        "channel":"sms", 
        "triggerMode":"API"
        "channelAccountId":"q3f5b438-xw31-44af-b729-64swaf3d0b56",
        "isNewTicketAutoCreatedForMessage":true
      }
  } 
```

#### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`ticketId` | Guid |  |
  |`ticketMessageId`  |   | |

Response
```Json
HTTP/1.1 200 OK 
  Content-Type:  application/json 
  {     
          "ticketId":"d3f5b968-ad51-42af-b759-64c0afc40b84",         
          "ticketMessageId":"d3f5b968-ad51-42af-b759-64c0afc40b84", 
          
  } 
```

## Model
### Outbound Campaign JSON Format
Outbound Campaign is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outbound Campaign. |  
  | `name` | string | yes |  Name of this Outbound Campaign. |  
  | `description` | string | no |  Description of this Outbound Campaign. |  
  | `channel` | string | no |  App channel of this Outbound Campaign. |  
  | `triggerMode` | string | yes |  Trigger mode of this Outbound Campaign, allowed values are "Manual", "Scheduled", "API". |  
  | `status` | string | no |  Status of this Outbound Campaign, allowed values are "Draft", "Published", "Running", "Finished". |  
  | `channelAccountId` | Guid | no |  Channel account id of this Outbound Campaign. |  
  | `message` | string | no |  Message of this Outbound Campaign. |  
  | `isNewTicketAutoCreatedForMessage` | bool | no |  Whether a new ticket will be auto created or not for the message.|  
  | `timeToAutoCreateNewTicket` | string | no |  Allowed values are "When the message is successfully sent", "When contact replies the first message after the message is successfully sent". |  
  | `contactFilterConditionMetType` | string | no |  Allowed values are "All", "Any", "Logical Expression". |  
  | `contactFilterLogicalExpresssion` | string | no |  Contact Filter Logical Expresssion of this Condition. |  
  | `whenToSend` | string | no | Allowed values are "Right Away", "Scheduled". |  
  | `scheduledStartTime` | timestamp | no |  Message of this Outbound Campaign. |  
  
### Outbound Campaign Contact Filter Condition JSON Format
Outbound Campaign Contact Filter Condition is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the contact filter condition. |  
  | `outreachCampaignId` | Guid | yes |  The Outbound Campaign id of the contact filter condition. |  
  | `fieldName` | string | yes |  Field name of the contact filter condition. |  
  | `operator` | string | yes |  Allowed values are "Contains", "Does Not Contain", "Is", "Is Not", "Is More Than", "Is Less Than", "Before", "After". |  
  | `value` | string | yes |  Trigger mode of the contact filter condition. |  
  | `order` | integer | yes |  Order of the contact filter condition. | 
  
### Outbound Message JSON Format
Outbound Message is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outbound Message. |  
  | `sentTime` | timestamp | yes |  The sent time the Outbound Message. |  
  | `contactId` | Guid | yes |  Contact id of the Outbound Message. |  
  | `message` | string | yes |  Message. |  
  | `from` | string | yes |  Where the Outbound Message from. |  
  | `to` | string | yes |  Where the Outbound Message to. |  
  | `status` | string | yes |  Status of the Outbound Message. Allowed values are "Inqueue", "Successful", "Failed" |  
  | `failedReason` | string | no |  The failed reason of the Outbound Message. |  
  | `outreachCampaignId` | Guid | yes |  The Outbound Campaign id the Outbound Message. |  
  | `attachedToTicketId` | Guid | no |  The attached ticked Id of the Outbound Message. |  
        
