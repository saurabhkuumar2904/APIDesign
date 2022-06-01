  | Change Version | API Version | Change nots | Change Date | Author |Architect Team Reviewer | 
  | - | - | - | - | - |- |
  | 1.0 | v1 |Outreach API | 2022-5-18 | Leon,Frank,Yogesh|  Allon|

## Summary

### Outreach API  

#### Outreach Campaign
  - GET /outreach/campaigns/ - [Get the list of Outreach Campaign](#get-the-list-of-outreach-campaign). 
  - GET /outreach/campaigns/{id} - [Get a single Outreach Campaign](#get-a-single-outreach-campaign). 
  - POST /outreach/campaigns/ - [Create a new Outreach Campaign](#create-a-new-outreach-campaign).  
  - PUT /outreach/campaigns/{id} - [Update the Outreach Campaign](#update-the-outreach-campaign).  
  - DELETE /outreach/campaigns/{id} - [Delete the campaign](#delete-the-campaign). 
  - POST /outreach/campaigns/{id}:send - [Send the campaign](#Send-the-campaign). 
  
#### Outreach Message
  - GET /outreach/messages/ - [Get the list of Outreach Message](#get-the-list-of-outreach-message). 
  - GET /outreach/messages/{id} - [Get a single Outreach Message](#get-a-single-outreach-message). 
  - POST /outreach/messages - [Create a new Outreach Message](#create-a-new-outreach-message). 
  - PUT /outreach/messages/{id} - [Update the Outreach Message](#update-the-outreach-message).  
<!--   - POST /outreach/campaigns/{id}/messages - [Create a new Outreach Message](#create-a-new-outreach-message).    -->

### Outreach Message Callback API 

The CallbackURL can be any valid URL that implements this API, and it is configured in the system when a new Outreach Message is created. 
  - POST /{callbackURL} - [Outreach Message Callback](#outreach-message-callback). 
  
### Contact API 

####  Contact
  - GET /contact/contacts/ - [Get the list of Contact](#get-the-list-of-contact).  
  - POST /contact/contacts:query - [Query contact with conditions](#query-contact-with-conditions).  
  - GET /contact/contacts/{id} - [Get a single Contact](#get-a-single-contact).  
  - POST /contact/contacts/ - [Create a new Contact](#create-a-new-contact).  
  - PUT /contact/contacts/{id} - [Update the Contact](#update-the-contact).  
  - DELETE /contact/contacts/{id} - [Delete the Contact](#delete-the-contact). 
  - POST /contact/contacts:merge - [Merge Contact](#merge-contacts). 
  - POST /contact/contacts:import - [Import Contacts](#import-contacts). 
  - POST /contact/contacts:export - [Export Contacts](#export-contacts). 
  
####  Contact Field
  - GET /contact/fields - [Get the list of Contact Field](#get-the-list-of-contact-field).
  - GET /contact/fields/{id} - [Get a single Contact Field](#get-a-single-contact-field).  
  - POST /contact/fields - [Create a new Contact Field](#create-a-new-contact-field).  
  - PUT /contact/fields/{id} - [Update a Contact Field](#update-a-contact-field).  
  - DELETE /contact/fields/{id} - [Delete a Contact Field](#delete-a-contact-field).  

### Ticketing API 
  - POST /ticketing/ticket/{id}:attach - [attach a outreach message to the ticket](#attach-a-outreach-message-to-the-ticket). 

###  Outbound unichannel API 
  - POST /Outboundunichannel/messages/ - [Create a new Outbound message](#create-a-new-outreach-message). 
  - GET /Outboundunichannel/messages/{id} - [Get a single Outbound message](#get-a-single-outreach-message). 

## Endpoints

### Outreach Message Callback
`POST /{callbackURL}`

#### Parameters
No Parameters

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `sentTime` | timestamp | yes |  The outreach campaign id of the Outreach Message. |  
  | `contactId` | Guid | yes |  Contact id of the Outreach Message. |  
  | `message` | string | yes |  Message. |  
  | `from` | string | yes |  Where the Outreach Message from. |  
  | `to` | string | yes |  Where the Outreach Message to. |  
  | `status` | string | yes |  Status of the Outreach Message. Allowed values are "Queued", "Sending", "Sent", "Failed" |  
  | `failedReason` | string | no |  The failed reason of the Outreach Message. |  
  | `outreachCampaignId` | Guid | yes |  The outreach campaign id the Outreach Message. |  
  | `outreachCampaignSentTime` | timestamp | no |  The sent time of the Outreach Campaign. |  
  | `isMessageAutoAttachedToTicket` | string | no |  The Outreach Campaign id the Outreach Message. |  
  | `preferredTicketToAutoAttach` | string | no |  The Outreach Campaign id the Outreach Message. |  
  | `timeToAutoAttachToTicket` | string | no |  The Outreach Campaign id the Outreach Message. |  
  | `attachedToTicketId` | Guid | no |  The attached ticked Id of the Outreach Message. |  
  | `callbackURL` | string | no |  The callbackURL of the Outreach Message. |  
  
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
	"outreachCampaignSentTime": "2022-05-22 03:00:36.277",
	"isMessageAutoAttachedToTicket": "Yes",
	"preferredTicketToAutoAttach": "New ticket",
	"timeToAutoAttachToTicket": "When the message is sent",
	"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48",
	"callbackURL": "https://domainname.com/sms/callback"
}
```
#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outreachMessage` |[OutreachMessage](#outreachmessage-object)  |Yes|   [OutreachMessage](#outreachmessage-object)  |
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
	"outreachCampaignSentTime": "2022-05-22 03:00:36.277",
	"isMessageAutoAttachedToTicket": "Yes",
	"preferredTicketToAutoAttach": "New ticket",
	"timeToAutoAttachToTicket": "When the message is sent",
	"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48",
	"callbackURL": "https://domainname.com/sms/callback"
}
```

### Get the list of Outreach Campaign
`GET /outreach/campaigns/`

#### Parameters
No parameters

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outreachCampaignList` |[OutreachCampaign](#outreachcampaign-Object)[]  |Yes|  An array of [OutreachCamgaign](#outreachcampaign-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"outreachCampaigns": [{
		"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"name": "test-campaign",
		"description": "test campaign",
		"channel": "SMS",
		"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
		"message": "Hello, please fill in your application form by the end of this week!",
		"isMessageAutoAttachedToTicket": "Yes",
		"preferredTicketToAutoAttach": "New ticket",
		"timeToAutoAttachToTicket": "When the message is sent",
		"contactFilterConditionMetType": "Any",
		"contactFilterLogicalExpresssion": "",
		"contactFilterConditions": [{
			"FieldName": "AAA",
			"Operator": "is",
			"Value": "BBBB",
			"Order": "0"
		}],
		"scheduledStartTime": ""
	}]
}
```

### Get a single Outreach Campaign
`GET /outreach/campaigns/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outreach Campaign |  

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outreachCampaign` |[OutreachCampaign](#outreachcampaign-object)  |Yes|   [OutreachCampaign](#outreachcampaign-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"name": "test-campaign",
	"description": "test campaign",
	"channel": "SMS",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
	"isMessageAutoAttachedToTicket": "Yes",
	"preferredTicketToAutoAttach": "New ticket",
	"timeToAutoCreateNewTicket": "When the message is sent",
	"contactFilterConditionMetType": "Any",
	"contactFilterLogicalExpresssion": "",
	"contactFilterConditions": [{
		"FieldName": "AAA",
		"Operator": "is",
		"Value": "BBBB",
		"Order": "0"
	}],
	"scheduledStartTime": ""
}
```

### Create a new Outreach Campaign
`POST /outreach/campaigns/`

#### Parameters
No parameters

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `name` | string | yes |  Name of this Outreach Campaign. |  
  | `description` | string | no |  Description of this Outreach Campaign. |  
  | `channel` | string | no |  App channel of this Outreach Campaign. |  
  | `channelAccountId` | Guid | no |  Channel account id of this Outreach Campaign. |  
  | `message` | string | no |  Message of this Outreach Campaign. |  
  | `isMessageAutoAttachedToTicket` | bool | no |  Whether a new ticket will be auto created or not for the message.|  
  | `preferredTicketToAutoAttach` | string | no |  Allowed values are "New ticket", "Existing unresolved ticket whose last message is from SMS channel".|  
  | `timeToAutoAttachToTicket` | string | no |  Allowed values are "When the message is sent", "When contact replies the message". |  
  | `contactFilterConditionMetType` | string | no |  Allowed values are "All", "Any", "Logical Expression". |  
  | `contactFilterLogicalExpresssion` | string | no |  Contact Filter Logical Expresssion of this Condition. |  
  | `contactFilterConditions` | [ContactFilterCondition](#ContactFilterCondition-object)[] | no | An array of [ContactFilterCondition](#ContactFilterCondition-object)| 
  | `scheduledStartTime` | timestamp | no |  Message of this Outreach Campaign. |  
  
example:
```Json 
{
	"name": "test-campaign",
	"description": "test campaign",
	"channel": "SMS",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
	"isMessageAutoAttachedToTicket": "Yes",
	"preferredTicketToAutoAttach": "New ticket",
	"timeToAutoAttachToTicket": "When the message is sent",
	"contactFilterConditionMetType": "Any",
	"contactFilterLogicalExpresssion": "",
	"contactFilterConditions": [{
		"FieldName": "AAA",
		"Operator": "is",
		"Value": "BBBB",
		"Order": "0"
	}],
	"scheduledStartTime": ""
}
```

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outreachCampaign` |[OutreachCampaign](#outreachcampaign-object)  |Yes|   [OutreachCamgaign](#outreachcampaign-object)  |

```Json 
  HTTP/1.1 201 Created
  Content-Type: application/json
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"name": "test-campaign",
	"description": "test campaign",
	"channel": "SMS",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
	"isMessageAutoAttachedToTicket": "Yes",
	"preferredTicketToAutoAttach": "New ticket",
	"timeToAutoAttachToTicket": "When the message is sent",
	"contactFilterConditionMetType": "Any",
	"contactFilterLogicalExpresssion": "",
	"contactFilterConditions": [{
		"FieldName": "AAA",
		"Operator": "is",
		"Value": "BBBB",
		"Order": "0"
	}],
	"scheduledStartTime": ""
}
```

### Update the Outreach Campaign
`PUT /outreach/campaigns/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outreach Campaign |  

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outreach Campaign |  
  | `name` | string | yes |  Name of this Outreach Campaign. |  
  | `description` | string | no |  Description of this Outreach Campaign. |  
  | `channel` | string | no |  App channel of this Outreach Campaign. |  
  | `channelAccountId` | Guid | no |  Channel account id of this Outreach Campaign. |  
  | `message` | string | no |  Message of this Outreach Campaign. |  
  | `isMessageAutoAttachedToTicket` | bool | no |  Whether a new ticket will be auto created or not for the message.|  
  | `preferredTicketToAutoAttach` | string | no |  Allowed values are "New ticket", "Existing unresolved ticket whose last message is from SMS channel".|  
  | `timeToAutoAttachToTicket` | string | no |  Allowed values are "When the message is sent", "When contact replies the message". |  
  | `contactFilterConditionMetType` | string | no |  Allowed values are "All", "Any", "Logical Expression". |  
  | `contactFilterLogicalExpresssion` | string | no |  Contact Filter Logical Expresssion of this Condition. |  
  | `contactFilterConditions` | [ContactFilterCondition](#ContactFilterCondition-object)[] | no | An array of [ContactFilterCondition](#ContactFilterCondition-object)| 
  | `scheduledStartTime` | timestamp | no |  Message of this Outreach Campaign. |  

example:
```Json 
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"name": "test-campaign 2",
	"description": "test campaign",
	"channel": "SMS",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
	"isMessageAutoAttachedToTicket": "Yes",
	"preferredTicketToAutoAttach": "New ticket",
	"timeToAutoAttachToTicket": "When the message is sent",
	"contactFilterConditionMetType": "Any",
	"contactFilterLogicalExpresssion": "",
	"contactFilterConditions": [{
		"FieldName": "AAA",
		"Operator": "is",
		"Value": "BBBB",
		"Order": "0"
	}],
	"scheduledStartTime": ""
}
```

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outreachCampaign` |[OutreachCampaign](#outreachcampaign-object)  |Yes|   [OutreachCampaign](#outreachcampaign-object)  |

```Json 
  HTTP/1.1 200 Ok
  Content-Type: application/json
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"name": "test-campaign 2",
	"description": "test campaign",
	"channel": "SMS",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
	"isMessageAutoAttachedToTicket": "Yes",
	"preferredTicketToAutoAttach": "New ticket",
	"timeToAutoAttachToTicket": "When the message is sent",
	"contactFilterConditionMetType": "Any",
	"contactFilterLogicalExpresssion": "",
	"contactFilterConditions": [{
		"FieldName": "AAA",
		"Operator": "is",
		"Value": "BBBB",
		"Order": "0"
	}],
	"scheduledStartTime": ""
}
```

### Delete the Campaign
`DELETE /outreach/campaigns/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outreach Campaign |  

#### Response
No response

```Json 
  HTTP/1.1 204 Ok
  Content-Type: application/json
{
}
```

### Send the Campaign
`POST /outreach/campaigns/{id}:send`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outreach Campaign |  

#### Response
The Response body contains data is [OutreachCampaignRecord](#outreachcampaignrecord-object):

```Json 
  HTTP/1.1 200 Ok
  Content-Type: application/json
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"sentTime": "2022-05-23 03:00:36.277",
	"outreachCampaign":{
		"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"name": "test-campaign 2",
		"description": "test campaign",
		"channel": "SMS",
		"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
		"message": "Hello, please fill in your application form by the end of this week!",
		"isMessageAutoAttachedToTicket": "Yes",
		"preferredTicketToAutoAttach": "New ticket",
		"timeToAutoAttachToTicket": "When the message is sent",
		"contactFilterConditionMetType": "Any",
		"contactFilterLogicalExpresssion": "",
		"contactFilterConditions": [{
			"FieldName": "AAA",
			"Operator": "is",
			"Value": "BBBB",
			"Order": "0"
		}],
		"scheduledStartTime": ""
	}
}
```
### Get the list of Outreach Message
`GET /outreach/messages/`

#### Parameters
parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`contactId` |Guid  |No| The unique id of the Contact  |
|`outreachCampaignId` |Guid |No| The unique id of the Outreach Campaign |

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outreachMessageList` |[OutreachMessage](#outreachmessage-object)[]  |Yes|  An array of [OutreachMessage](#outreachmessage-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"outreachMessages": [{
		"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"sentTime": "2022-05-23 03:00:36.277",
		"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
		"message": "Hello, please fill in your application form by the end of this week!",
		"from": "Tom Cruise",
		"to": "XXX University",
		"status": "Successful",
		"failedReason": "",
		"outreachCampaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
		"outreachCampaignSentTime": "2022-05-22 03:00:36.277",
		"isMessageAutoAttachedToTicket": "Yes",
		"preferredTicketToAutoAttach": "New ticket",
		"timeToAutoAttachToTicket": "When the message is sent",
		"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48",
		"callbackURL": "https://domainname.com/sms/callback"
	}]
}
```

### Get a single Outreach Message
`GET /outreach/messages/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outreach Message |  

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outreachMessage` |[OutreachMessage](#outreachmessage-object)  |Yes|   [OutreachMessage](#outreachmessage-object)  |

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
	"outreachCampaignSentTime": "2022-05-22 03:00:36.277",
	"isMessageAutoAttachedToTicket": "Yes",
	"preferredTicketToAutoAttach": "New ticket",
	"timeToAutoAttachToTicket": "When the message is sent",
	"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48",
	"callbackURL": "https://domainname.com/sms/callback"
}
```

### Create a new Outreach Message
`POST /outreach/messages`

#### Parameters
No Parameters

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `sentTime` | timestamp | yes |  The outreach campaign id of the Outreach Message. |  
  | `contactId` | Guid | yes |  Contact id of the Outreach Message. |  
  | `message` | string | yes |  Message. |  
  | `from` | string | yes |  Where the Outreach Message from. |  
  | `to` | string | yes |  Where the Outreach Message to. |  
  | `status` | string | yes |  Status of the Outreach Message. Allowed values are "Queued", "Sending", "Sent", "Failed" |  
  | `failedReason` | string | no |  The failed reason of the Outreach Message. |  
  | `outreachCampaignId` | Guid | yes |  The outreach campaign id the Outreach Message. |  
  | `outreachCampaignSentTime` | timestamp | no |  The sent time of the Outreach Campaign. |  
  | `isMessageAutoAttachedToTicket` | string | no |  The Outreach Campaign id the Outreach Message. |  
  | `preferredTicketToAutoAttach` | string | no |  The Outreach Campaign id the Outreach Message. |  
  | `timeToAutoAttachToTicket` | string | no |  The Outreach Campaign id the Outreach Message. |  
  | `attachedToTicketId` | Guid | no |  The attached ticked Id of the Outreach Message. |  
  | `callbackURL` | string | no |  The callbackURL of the Outreach Message. |  
  
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
	"outreachCampaignSentTime": "2022-05-22 03:00:36.277",
	"isMessageAutoAttachedToTicket": "Yes",
	"preferredTicketToAutoAttach": "New ticket",
	"timeToAutoAttachToTicket": "When the message is sent",
	"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48",
	"callbackURL": "https://domainname.com/sms/callback"
}
```

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outreachMessage` |[OutreachMessage](#outreachmessage-object)  |Yes|   [OutreachMessage](#outreachmessage-object)  |

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
	"outreachCampaignSentTime": "2022-05-22 03:00:36.277",
	"isMessageAutoAttachedToTicket": "Yes",
	"preferredTicketToAutoAttach": "New ticket",
	"timeToAutoAttachToTicket": "When the message is sent",
	"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48",
	"callbackURL": "https://domainname.com/sms/callback"
}
```

### Update the Outreach Message
`PUT /outreach/messages/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outreach Message |  

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outreach Message. |  
  | `sentTime` | timestamp | yes |  The outreach campaign id of the Outreach Message. |  
  | `contactId` | Guid | yes |  Contact id of the Outreach Message. |  
  | `message` | string | yes |  Message. |  
  | `from` | string | yes |  Where the Outreach Message from. |  
  | `to` | string | yes |  Where the Outreach Message to. |  
  | `status` | string | yes |  Status of the Outreach Message. Allowed values are "Queued", "Sending", "Sent", "Failed" |  
  | `failedReason` | string | no |  The failed reason of the Outreach Message. |  
  | `outreachCampaignId` | Guid | yes |  The outreach campaign id the Outreach Message. |  
  | `outreachCampaignSentTime` | timestamp | no |  The sent time of the Outreach Campaign. |  
  | `isMessageAutoAttachedToTicket` | string | no |  The Outreach Campaign id the Outreach Message. |  
  | `preferredTicketToAutoAttach` | string | no |  The Outreach Campaign id the Outreach Message. |  
  | `timeToAutoAttachToTicket` | string | no |  The Outreach Campaign id the Outreach Message. |  
  | `attachedToTicketId` | Guid | no |  The attached ticked Id of the Outreach Message. |  
  | `callbackURL` | string | no |  The callbackURL of the Outreach Message. |  
  
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
	"outreachCampaignSentTime": "2022-05-22 03:00:36.277",
	"isMessageAutoAttachedToTicket": "Yes",
	"preferredTicketToAutoAttach": "New ticket",
	"timeToAutoAttachToTicket": "When the message is sent",
	"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48",
	"callbackURL": "https://domainname.com/sms/callback"
}
```

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outreachMessage` |[OutreachMessage](#outreachmessage-object)  |Yes|   [OutreachMessage](#outreachmessage-object)  |

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
	"outreachCampaignSentTime": "2022-05-22 03:00:36.277",
	"isMessageAutoAttachedToTicket": "Yes",
	"preferredTicketToAutoAttach": "New ticket",
	"timeToAutoAttachToTicket": "When the message is sent",
	"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48",
	"callbackURL": "https://domainname.com/sms/callback"
}
```

### Get the list of Contact
`GET /contact/contacts/`

#### Parameters

#### Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `keywords` | String | No  | search scope includes: contact name/identity id/identity value |  
  | `contactIdentityType` | String | No  | contact identity type |  
  | `contactIdentityValue` | String | No  | contact identity value | 
  | `contactIdentityName` | String | No  | contact identity name | 
  | `include` | String | No  | include | 
  | `pageIndex` | String | No  | page index | 
  | `pageSize` | String | No  | page size | 
  | `sortBy` | String | No  | sort by | 
  | `sortOrder` | String | No  | `asc`,`desc`, default `asc` |  
  | `ContactFilterConditionMetType` | string | no |  Contact Filter Logical Expresssion of this Condition. "All", "Any", "LogicalExpression" | 
  | `contactFilterLogicalExpresssion` | string | no |  Contact Filter Logical Expression | 
  | `contactFilterConditions` | [ContactFilterCondition](#ContactFilterCondition-object)[] | no | An array of [ContactFilterCondition](#ContactFilterCondition-object)| 

#### Request Body


#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`contactList` |[Contact](#contact-object)[]  |Yes|  An array of [Contact](#contact-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
  "contacts": [
	{
    "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
    "name": "Tom cruise",
    "createTime": "2022-03-18 01:12:32.0000000",
    "lastUpdatedTime": "2022-03-18 01:12:32.0000000",
    "mergeToContactId": "00000000-0000-0000-0000-000000000000",
    "customFields": {
        "firstName": "Tom",
        "lastName": "Cruise",
        "description": "Moive actor",
        "alias": "tomcruise",
        "title": "Senior Moive actor",
        "company": "Comm100",
        "fax": "",
        "phone": "",
        "mailingAddress": "",
        "city": "",
        "stateOrProvince": "",
        "countryOrRegion": "",
        "postalOrZipCode": "",
        "timeZone": ""
    },
    "contactIdentities":
      [{
          "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
          "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
          "contactIdentityType":"SMS",
          "name":"+033214561",
          "value":"+033214561",
          "avatarUrl":"",
          "infoUrl":"",
          "screenName":"",
          "originalContactPageUrl":""
      }]
	}],
  "previousPage":null,
  "nextPage":"http://demo.comm100.io/contacts?pageIndex=2&pageSize=50&siteId=10100000",
  "total":255
}
```

### Query contact with conditions
`POST /contact/contacts:query`

#### Parameters

#### Request Body
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `include` | String | No  | include | 
  | `pageIndex` | String | No  | page index | 
  | `pageSize` | String | No  | page size | 
  | `sortBy` | String | No  | sort by | 
  | `sortOrder` | String | No  | `asc`,`desc`, default `asc` |  
  | `ContactFilterConditionMetType` | string | no |  Contact Filter Logical Expresssion of this Condition. `All`, `Any`, `LogicalExpression` | 
  | `contactFilterLogicalExpresssion` | string | no |  Contact Filter Logical Expression | 
  | `contactFilterConditions` | [ContactFilterCondition object](#contactFilterCondition-object)[] | no | An array of [ContactFilterCondition object](#contactFilterCondition-object)| 

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`contactList` |[Contact](#contact-object)[]  |Yes|  An array of [Contact](#contact-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
  "contacts": [
	{
    "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
    "name": "Tom cruise",
    "createTime": "2022-03-18 01:12:32.0000000",
    "lastUpdatedTime": "2022-03-18 01:12:32.0000000",
    "mergeToContactId": "00000000-0000-0000-0000-000000000000",
    "customFields": {
        "firstName": "Tom",
        "lastName": "Cruise",
        "description": "Moive actor",
        "alias": "tomcruise",
        "title": "Senior Moive actor",
        "company": "Comm100",
        "fax": "",
        "phone": "",
        "mailingAddress": "",
        "city": "",
        "stateOrProvince": "",
        "countryOrRegion": "",
        "postalOrZipCode": "",
        "timeZone": ""
    },
    "contactIdentities":
      [{
          "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
          "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
          "contactIdentityType":"SMS",
          "name":"+033214561",
          "value":"+033214561",
          "avatarUrl":"",
          "infoUrl":"",
          "screenName":"",
          "originalContactPageUrl":""
      }]
	}],
  "previousPage":null,
  "nextPage":"http://demo.comm100.io/contacts?pageIndex=2&pageSize=50&siteId=10100000",
  "total":255
}
```

### Get a single Contact
`GET /contact/contacts/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Contact |  

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`contact` |[Contact](#contact-object)  |Yes| [Contact](#contact-object)   |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
  "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
  "name": "Tom cruise",
  "createTime": "2022-03-18 01:12:32.0000000",
  "lastUpdatedTime": "2022-03-18 01:12:32.0000000",
  "mergeToContactId": "00000000-0000-0000-0000-000000000000",
  "customFields": {
        "firstName": "Tom",
        "lastName": "Cruise",
        "description": "Moive actor",
        "alias": "tomcruise",
        "title": "Senior Moive actor",
        "company": "Comm100",
        "fax": "",
        "phone": "",
        "mailingAddress": "",
        "city": "",
        "stateOrProvince": "",
        "countryOrRegion": "",
        "postalOrZipCode": "",
        "timeZone": ""
    },
    "contactIdentities":
      [{
          "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
          "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
          "contactIdentityType":"SMS",
          "name":"+033214561",
          "value":"+033214561",
          "avatarUrl":"",
          "infoUrl":"",
          "screenName":"",
          "originalContactPageUrl":""
      }]
}
```

### Create a new Contact
`POST /contact/contacts/`

#### Parameters
No path parameters

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `name` | string | yes |  The name of the Contact. |  
  | `contactIdentity` | [contactIdentities](#contact-Identity-object) | yes | Contact identities array. |  
  | `customField` | [custom fields](#custom-field-value-object) | yes |  value of contact custom fields. |  
  
example:
```Json 
{
    "name":"Frank",
    "contactIdentities":[
        {
            "contactIdentityType":"Email",
            "value":"frank@1234.com",
            "name":"Frank"
        }
    ],
    "customFields":{
        "company": "Comm100",
    }
}

```

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`contact` |[Contact](#contact-object)  |Yes|   [Contact](#contact-object)  |

```Json 
  HTTP/1.1 201 Created
  Content-Type: application/json
{
  "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
  "name": "Tom cruise",
  "createTime": "2022-03-18 01:12:32.0000000",
  "lastUpdatedTime": "2022-03-18 01:12:32.0000000",
  "mergeToContactId": "00000000-0000-0000-0000-000000000000",
  "customFields": { 
      "company": "Comm100"
  },
  "contactIdentities":
    [{
        "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
        "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
        "contactIdentityType":"Email",
        "name":"frank",
        "value":"frank@1234.com",
        "avatarUrl":"",
        "infoUrl":"",
        "screenName":"",
        "originalContactPageUrl":""
    }]
}
```

### Update the Contact
`PUT /contact/contacts/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Contact |  

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `name` | string | yes |  The name of the Contact. |  
  | `contactIdentity` | [contactIdentities](#contact-Identity-object)[] | yes | Contact identities array. |  
  | `customField` | [custom fields](#custom-field-value-object) | yes |  value of contact custom fields. |  

example:
```Json 
{
  "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
  "name": "Tom cruise 2",
  "createTime": "2022-03-18 01:12:32.0000000",
  "mergeToContactId": "00000000-0000-0000-0000-000000000000",
  "customFields": { 
      "company": "Comm100"
    },
  "contactIdentities":
    [{
        "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
        "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
        "contactIdentityType":"Email",
        "name":"frank",
        "value":"frank@1234.com",
        "avatarUrl":"",
        "infoUrl":"",
        "screenName":"",
        "originalContactPageUrl":""
    }]
}
```

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact` |[Contact](#contact-object)  |Yes| [Contact](#contact-object)  |

### Delete the Contact
`DELETE /contact/contacts/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  | The unique id of the Contact |  

#### Response
No response


### Merge Contacts
`POST /contact/contacts:merge`

#### Parameters

##### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `originalContactId` | Guid | yes | The original ID of the Contact. |  
  | `targetContactId` | Guid | yes | The target ID of the Contact. |   

#### Response
No response

### Import Contacts
`POST /contact/contacts:import`

#### Parameters

##### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `file` | file | yes | The import file |     

#### Response
No response

### Export Contacts
`POST /contact/contacts:export`

#### Parameters
No Parameter     

#### Response
- file

### Get the list of contact field
`GET /contact/fields/`

#### Parameters
No parameters

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact fields` |[Contact Field](#contact-field-object)[]  |Yes| [Contact](#contact-field-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
  [{
    "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
    "isSystem": "true",
    "isIdentity":"false",
    "identityType":"",
    "isRequired": "false",
    "isReadOnly": "false",
    "type": "text",
    "name": "Company",
    "length": 256,
    "helpText": "company name",
    "defaultValue": "Comm100",
    "order": 5,
    "fieldOptions":[]
  }]
```

### Get a single contact field
`GET /contact/fields/{id}`

#### Parameters
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`id` | Guid  |Yes| contact field ID |

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact field` |[Contact Field](#contact-field-object)  |Yes| [Contact](#contact-field-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
  "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
  "isSystem": "true",
  "isIdentity":"false",
  "identityType":"",
  "isRequired": "false",
  "isReadOnly": "false",
  "type": "text",
  "name": "Company",
  "length": 256,
  "helpText": "company name",
  "defaultValue": "Comm100",
  "order": 5,
  "fieldOptions":[]
}
```

### Create a new contact field
`POST /contact/fields/`

#### Parameters
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `name` | string | unique name of the field |
  | `isRequired` | bool | Whether the field is required or not. |
  | `isReadOnly` | string | Whether the field is readyonly or not. |
  | `type` | string | Type of the field. Allowed values are "text", "textArea", "email", "url", "date", "integer", "float" |
  | `length` | integer | Field length |
  | `helpText` | string | Help text of the field. |
  | `defaultValue` | string | Default value of the field. |
  | `order` | integer | |
  | `fieldOptions` | [fieldOptions](#field-option-object)[] | Reference to Field Option. |

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact field` |[Contact Field](#contact-field-object)  |Yes| [Contact](#contact-field-object)  |

### Update a Contact Field
`PUT /contact/fields/{id}`

#### Parameters
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`id` | Guid  |Yes| contact field ID |
  | `name` | string | unique name of the field |
  | `isRequired` | bool | Whether the field is required or not. |
  | `isReadOnly` | string | Whether the field is readyonly or not. |
  | `type` | string | Type of the field. Allowed values are "text", "textArea", "email", "url", "date", "integer", "float" |
  | `length` | integer | Field length |
  | `helpText` | string | Help text of the field. |
  | `defaultValue` | string | Default value of the field. |
  | `order` | integer | |
  | `fieldOptions` | [fieldOptions](#field-option-object)[] | Reference to Field Option. |

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact field` |[Contact Field](#contact-field-object)  |Yes| [Contact](#contact-field-object)  |

### Delete a Contact Field
`Delete /contact/fields/{id}`

#### Parameters
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`id` | Guid  |Yes| contact field ID |

#### Response
No Response

### Create a new outreach message
`POST /ticketing/outreachmessages/`

#### Parameters
Request body 
Request body the request body contains data with the following structure: 
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  |`id`  |  string  |yes |   | outreach message id| 
  |`from`  |  string  |yes |   |  phone number |
  |`to`  | string | yes |   |   |
  |`message`  | string | yes |   |   |
  |`contactId`  | Guid | yes |   |   |
  |`outreachCampaign`  |  [Outreach Campaign](#outreach-campaign-object) Object |yes |   |  |
  #### example:
```Json 
 {
    "id":"d3f5b968-ad51-42af-b759-64c0afc40b84",
    "from": "+19857473631", 
    "to": "+19857473632", 
    "contactId":"q3f5b438-xw31-44af-b729-64swaf3d0b56",
    "outreachCampaign": { 
        "id":"q3f5b438-xw31-44af-b729-64swaf3d0b56",
        "channel":"sms", 
        "triggerMode":"API"
        "channelAccountId":"q3f5b438-xw31-44af-b729-64swaf3d0b56",
        "isMessageAutoAttachedToTicket":true
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

### OutreachCampaign Object
Outreach Campaign is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outreach Campaign. |  
  | `name` | string | yes |  Name of this Outreach Campaign. |  
  | `description` | string | no |  Description of this Outreach Campaign. |  
  | `channel` | string | no |  App channel of this Outreach Campaign. |  
  | `channelAccountId` | Guid | no |  Channel account id of this Outreach Campaign. |  
  | `message` | string | no |  Message of this Outreach Campaign. |  
  | `isMessageAutoAttachedToTicket` | bool | no |  Whether a new ticket will be auto created or not for the message.`yes`, `no`| 
  | `preferredTicketToAutoAttach` | string | no |  Allowed values are "New ticket", "Existing unresolved ticket whose last message is from SMS channel".|  
  | `timeToAutoAttachToTicket` | string | no |  Allowed values are "When the message is sent", "When contact replies the message". |  
  | `contactFilterConditionMetType` | string | no |  Allowed values are "All", "Any", "Logical Expression". |  
  | `contactFilterLogicalExpresssion` | string | no |  Contact Filter Logical Expresssion of this Condition. |  
  | `contactFilterConditions` | [ContactFilterCondition](#ContactFilterCondition-object)[] | no | An array of [ContactFilterCondition](#ContactFilterCondition-object)| 
  | `scheduledStartTime` | timestamp | no |  Message of this Outreach Campaign. |  
  
### ContactFilterCondition Object
Outreach Campaign Contact Filter Condition is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `fieldName` | string | yes |  Field name of the contact filter condition. |  
  | `operator` | string | yes |  Allowed values are "Contains", "Does Not Contain", "Is", "Is Not", "Is More Than", "Is Less Than", "Before", "After". |  
  | `value` | string | yes |  Trigger mode of the contact filter condition. |  
  | `order` | integer | yes |  Order of the contact filter condition. | 
  
### OutreachCampaignRecord Object
Outreach Campaign is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outreach Campaign Record. |  
  | `sentTime` | timestamp | yes | The unique id of the Outreach Campaign. | 
  | `outreachCampaign` |  [OutreachCampaign](#outreachcampaign-object) | yes | Outreach Campaign. |  
  
  
### OutreachMessage Object
Outreach Message is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outreach Message. |  
  | `sentTime` | timestamp | yes |  The sent time the Outreach Message. |  
  | `contactId` | Guid | yes |  Contact id of the Outreach Message. |  
  | `message` | string | yes |  Message. |  
  | `from` | string | yes |  Where the Outreach Message from. |  
  | `to` | string | yes |  Where the Outreach Message to. |  
  | `status` | string | yes |  Status of the Outreach Message. Allowed values are "Queued", "Sending", "Sent", "Failed" |  
  | `failedReason` | string | no |  The failed reason of the Outreach Message. |  
  | `outreachCampaignId` | Guid | no |  The Outreach Campaign id the Outreach Message. |  
  | `outreachCampaignSentTime` | timestamp | no |  The sent time of the Outreach Campaign. |  
  | `isMessageAutoAttachedToTicket` | string | no |  Whether a new ticket will be auto created or not for the message.`yes`, `no` |  
  | `preferredTicketToAutoAttach` | string | no |  Allowed values are "New ticket", "Existing unresolved ticket whose last message is from SMS channel". |  
  | `timeToAutoAttachToTicket` | string | no |   Allowed values are "When the message is sent", "When contact replies the message". |  
  | `attachedToTicketId` | Guid | no |  The attached ticked Id of the Outreach Message. |  
  | `callbackURL` | string | no |  The callbackURL of the Outreach Message. |  
  
### Contact Object
Contact is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the contact. |  
  | `name` | string | yes |  The name of the contact. |  
  | `first Name` | string | no | The first name of the contact |
  | `last name` | string | no | The last name of the contact |
  | `createTime` | timestamp | no |  The create time of the Contact. |  
  | `lastUpdatedTime` | timestamp | no |  The last updated time of the Contact. |  
  | `mergeToContactId` | Guid | no |  The Contact id of merge to. |  
  | `customFields` | [custom field value](#custom-field-value-object)[] | no | custom field value array | 
  | `contactIdentities` | [contact identities](#contact-identity-object)[] | yes | custom field value array | 

### Contact Identity Object
Contact Identity is represented as simple flat JSON objects with the following keys:

| Name | Type | Description | 
| - | - | - |
| `id` | string | read-only, the id of identity |
| `type` | string | required, contact identity type |
| `value` | string | required, the value of one identity, should be unique |
| `name` | string | required, the name of one identity |
| `avatar` | string | optional, the avatar of one identity |
| `identityInfoUrl` | string | optional, the original identity info url of one identity |
| `originalContactPageUrl` | string | optional, the original contact page url |
| `screenName` | string | optional, only for twitter |
| `isDeleted` | boolean | if is deleted |


### Contact custom field value object
| Name | Type | Description | 
| - | - | - | 
| `name` | string | the name of custom field |
| `value` | string | the value of custom field |

### Contact field object
| Name | Type | Description | 
| - | - | - | 
| `id` | guid | the id of custom field |
| `name` | string | unique name of the field |
| `isSystem` | bool | Whether the field is a system field or not. |
| `isIdentity` | bool | Whether the field is an identity field or not. |
| `identityType` | string | "visitor", "email", "SMS", "facebook", "twitter", "wechat", "SSOID", "externalID", "whatsApp", "instagram", "telegram", "line". Available when Is Identity is true. |
| `isRequired` | bool | Whether the field is required or not. |
| `isReadOnly` | string | Whether the field is readyonly or not. |
| `type` | string | Type of the field. Allowed values are "text", "textArea", "email", "url", "date", "date time", "integer", "float", "radio", "checkbox", "dropdownlist", "checkboxlist", "link", "timezone" |
| `length` | integer | Field length |
| `helpText` | string | Help text of the field. |
| `defaultValue` | string | Default value of the field. |
| `order` | integer | |
| `fieldOptions` | [fieldOptions](#field-option-object)[] | Reference to Field Option. |

### Field option object
| Name | Type | Description | 
| - | - | - | 
| `id` | guid | Id of the field option. |
| `fieldId` | guid | Id of the field which the option belongs to. |
| `value` | string | Value of the option. |
| `order` | integer | Order of the option. |
| `displayText` | string | Display text of the option. |


