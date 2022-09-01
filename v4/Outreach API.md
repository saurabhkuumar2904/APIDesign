  | Change Version | API Version | Change nots | Change Date | Author |Architect Team Reviewer | 
  | - | - | - | - | - |- |
  | 1.0 | v1 |Outreach API | 2022-5-18 | Leon,Frank,Yogesh|  Allon|

## Summary

### Outreach Call API  

#### Outreach Campaign
  - GET /outreach/campaigns/ - [Get the list of Outreach Campaign](#get-the-list-of-outreach-campaign). 
  - GET /outreach/campaigns/{id} - [Get a single Outreach Campaign](#get-a-single-outreach-campaign). 
  - POST /outreach/campaigns/ - [Create a new Outreach Campaign](#create-a-new-outreach-campaign).  
  - PUT /outreach/campaigns/{id} - [Update the Outreach Campaign](#update-the-outreach-campaign).  
  - DELETE /outreach/campaigns/{id} - [Delete the Outreach campaign](#delete-the-outreach-campaign). 
  - POST /outreach/campaigns/{id}:send - [Send the Outreach campaign](#Send-the-outreach-campaign). 
  
#### Outreach Message
  - GET /outreach/messages/ - [Get the list of Outreach Message](#get-the-list-of-outreach-message). 
  - GET /outreach/messages/{id} - [Get a single Outreach Message](#get-a-single-outreach-message). 
  - POST /outreach/messages - [Create a new Outreach Message](#create-a-new-outreach-message). 
  - PUT /outreach/messages/{id} - [Update the Outreach Message](#update-the-outreach-message).
  - GET /outreach/unattachedMessages/ - [Get the list of Outreach Message that needs to be attached by unichannel](#get-the-list-of-unattached-outreach-message). 

### Outreach Callback API 

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
  - DELETE /contact/contacts - [Batch delete contacts](#batch-delete-contacts). 
  - POST /contact/contacts/{id}:merge - [Merge Contact](#merge-contacts). 
  - GET /contact/contacts/importTemplate - [Download Template](#download-template). 
  - POST /contact/contacts:import - [Import Contacts](#import-contacts). 
  - Get /contact/contactImportRecords/{id} - [Get contact import record](#Get-contact-import-record). 
  - POST /contact/contacts:export - [Export Contacts](#export-contacts). 

####  Contact Identity
  - Get /contact/contactIdentities/{id} - [Get contact identity](#get-contact-identity).  
  - GET /contact/contactIdentities:identify - [Identify contact identities with type and value](#Identify-contact-identities-with-type-and-value).
  - POST /contact/contactIdentities:query - [Query contact identities with conditions](#query-contactidentities-with-conditions).  
  - POST /contact/contactIdentities - [Create a new Contact identity](#create-a-new-contact-identity).      
  - PUT /contact/contactIdentities/{id} - [Update the Contact identity](#update-the-contact-identity). 
  - DELETE /contact/contactIdentities/{id} - [Remove the Contact identity](#remove-the-contact-identity). 

####  Contact Field
  - GET /contact/fields - [Get the list of Contact Field](#get-the-list-of-contact-field).
  - GET /contact/fields/{id} - [Get a single Contact Field](#get-a-single-contact-field).  
  - POST /contact/fields - [Create a new Contact Field](#create-a-new-contact-field).  
  - PUT /contact/fields/{id} - [Update a Contact Field](#update-a-contact-field).  
  - DELETE /contact/fields/{id} - [Delete a Contact Field](#delete-a-contact-field).  
  - POST /contact/fields:reorder - [Reorder Contact Fields](#reorder-contact-fields).  

###  Outbound Unichannel API 
  - POST /outboundunichannel/messages/ - [Create a new Outbound message](#create-a-outbound-message). 

###  Outbound Unichannel Callback API 
The CallbackURL can be any valid URL that implements this API, and it is configured in the system when a Outbound Message is created. 
  - POST /{callbackURL} - [Outbound Message Callback](#outbound-message-callback). 
  
### Ticketing API 
  - POST /ticketing/tickets/{id}/messages - [attach a message to the ticket](#attach-a-message-to-the-ticket). 

## Endpoints

### Get the list of Outreach Campaign
`GET /outreach/campaigns/`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `channel` | string | no |  App channel of this Outreach Campaign, allowed value is "SMS". |  
  | `channelAccountId` | Guid | no |  Channel account id of this Outreach Campaign. |  
  | `pageIndex` | int | No  | page index | 
  | `pageSize` | int | No  | page size, default is 10, if the value is 0, will return all matched contacts. | 
  | `sortBy` | string | No  | sort by | 
  | `sortOrder` | string | No  | `asc`,`desc`, default `asc` |  

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`campaigns` |[OutreachCampaign](#OutreachCampaign-Object)[]  |Yes|  An array of [OutreachCampaign](#OutreachCampaign-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"campaigns": [{
		"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"name": "test-campaign",
		"description": "test campaign",
		"channel": "SMS",
		"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
		"message": "Hello, please fill in your application form by the end of this week!",
		"isMessageAutoAttachedToTicket": "true",
		"timeToAutoAttachToTicket": "whenTheMessageIsSent",
		"contactFilterConditionMetType": "any",
		"contactFilterLogicalExpresssion": "",
		"contactFilterConditions": [{
			"id":"6538285a-b00f-4063-b37a-cf14170aaa36",
			"campaignId":"f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
			"FieldName": "AAA",
			"Operator": "is",
			"Value": "BBBB",
			"Order": "0"
		}],
		"scheduledStartTime": "",
		"lastUpdatedTime": "2022-07-26T09:31:36.693Z"
	}],
	"previousPage":null,
  	"nextPage":"http://demo.comm100.io/campaigns?pageIndex=2&pageSize=50&siteId=10100000",
  	"total":255
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
The Response body contains data with the [OutreachCampaign](#OutreachCampaign-object) structure:


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
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoCreateNewTicket": "whenTheMessageIsSent",
	"contactFilterConditionMetType": "any",
	"contactFilterLogicalExpresssion": "",
	"contactFilterConditions": [{
		"id":"6538285a-b00f-4063-b37a-cf14170aaa36",
		"campaignId":"f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"FieldName": "AAA",
		"Operator": "is",
		"Value": "BBBB",
		"Order": "0"
	}],
	"scheduledStartTime": "",
	"lastUpdatedTime": "2022-07-26T09:31:36.693Z"
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
  | `channel` | string | yes |  App channel of this Outreach Campaign, allowed value is "SMS". |  
  | `channelAccountId` | Guid | yes |  Channel account id of this Outreach Campaign. |  
  | `message` | string | yes |  Message of this Outreach Campaign. |  
  | `isMessageAutoAttachedToTicket` | bool | no |  Whether a message need to auto attached to ticket. |  
  | `preferredTicketToAutoAttach` | string | no |  Allowed values are "newTicket", "existing".|  
  | `timeToAutoAttachToTicket` | string | no |  Allowed values are "whenTheMessageIsSent", "whenContactRepliesTheMessage". |  
  | `contactFilterConditionMetType` | string | no |  Allowed values are "all", "any", "logicalExpression". |  
  | `contactFilterLogicalExpresssion` | string | no |  Contact Filter Logical Expresssion of this Condition. |  
  | `contactFilterConditions` | [ContactFilterCondition](#ContactFilterCondition-object)[] | no | An array of [ContactFilterCondition](#ContactFilterCondition-object)| 
  | `scheduledStartTime` | datetime | no |  Time when the schedule start. |  
  
example:
```Json 
{
	"name": "test-campaign",
	"description": "test campaign",
	"channel": "SMS",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"contactFilterConditionMetType": "any",
	"contactFilterLogicalExpresssion": "",
	"contactFilterConditions": [{
		"id":"6538285a-b00f-4063-b37a-cf14170aaa36",
		"campaignId":"f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"FieldName": "AAA",
		"Operator": "is",
		"Value": "BBBB",
		"Order": "0"
	}],
	"scheduledStartTime": "",
	"lastUpdatedTime": "2022-07-26T09:31:36.693Z"
}
```

#### Response
The Response body contains data with the [OutreachCampaign](#OutreachCampaign-object) structure:


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
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"contactFilterConditionMetType": "any",
	"contactFilterLogicalExpresssion": "",
	"contactFilterConditions": [{
		"id":"6538285a-b00f-4063-b37a-cf14170aaa36",
		"campaignId":"f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"FieldName": "AAA",
		"Operator": "is",
		"Value": "BBBB",
		"Order": "0"
	}],
	"scheduledStartTime": "",
	"lastUpdatedTime": "2022-07-26T09:31:36.693Z"
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
  | `name` | string | no |  Name of this Outreach Campaign. |  
  | `description` | string | no |  Description of this Outreach Campaign. |  
  | `channel` | string | no |  App channel of this Outreach Campaign, allowed value is "SMS". |  
  | `channelAccountId` | Guid | no |  Channel account id of this Outreach Campaign. |  
  | `message` | string | no |  Message of this Outreach Campaign. |  
  | `isMessageAutoAttachedToTicket` | bool | no |  Whether a message need to auto attached to ticket. |  
  | `preferredTicketToAutoAttach` | string | no |  Allowed values are "newTicket", "existing".|  
  | `timeToAutoAttachToTicket` | string | no |  Allowed values are "whenTheMessageIsSent", "whenContactRepliesTheMessage". |  
  | `contactFilterConditionMetType` | string | no |  Allowed values are "all", "any", "logicalExpression". |  
  | `contactFilterLogicalExpresssion` | string | no |  Contact Filter Logical Expresssion of this Condition. |  
  | `contactFilterConditions` | [ContactFilterCondition](#ContactFilterCondition-object)[] | no | An array of [ContactFilterCondition](#ContactFilterCondition-object)| 
  | `scheduledStartTime` | datetime | no |  Time when the schedule start. |  

example:
```Json 
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"name": "test-campaign 2",
	"description": "test campaign",
	"channel": "SMS",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"contactFilterConditionMetType": "any",
	"contactFilterLogicalExpresssion": "",
	"contactFilterConditions": [{
		"id":"6538285a-b00f-4063-b37a-cf14170aaa36",
		"campaignId":"f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"FieldName": "AAA",
		"Operator": "is",
		"Value": "BBBB",
		"Order": "0"
	}],
	"scheduledStartTime": "",
	"lastUpdatedTime": "2022-07-26T09:31:36.693Z"
}
```

#### Response
The Response body contains data with the [OutreachCampaign](#OutreachCampaign-object) object structure:

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
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"contactFilterConditionMetType": "any",
	"contactFilterLogicalExpresssion": "",
	"contactFilterConditions": [{
		"id":"6538285a-b00f-4063-b37a-cf14170aaa36",
		"campaignId":"f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"FieldName": "AAA",
		"Operator": "is",
		"Value": "BBBB",
		"Order": "0"
	}],
	"scheduledStartTime": "",
	"lastUpdatedTime": "2022-07-26T09:31:36.693Z"
}
```

### Delete the Outreach Campaign
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
```

### Send the Outreach Campaign
`POST /outreach/campaigns/{id}:send`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outreach Campaign |  
  | `isScheduled` | bool | Yes  | Whether is scheduled | 

#### Response
The Response body contains data with the [OutreachCampaignRecord](#OutreachCampaignRecord-object) object structure:

```Json 
  HTTP/1.1 200 Ok
  Content-Type: application/json
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"sentTime": "2022-05-23 03:00:36.277",
	"campaign":{
		"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"name": "test-campaign 2",
		"description": "test campaign",
		"channel": "SMS",
		"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
		"message": "Hello, please fill in your application form by the end of this week!",
		"isMessageAutoAttachedToTicket": "true",
		"timeToAutoAttachToTicket": "whenTheMessageIsSent",
		"contactFilterConditionMetType": "any",
		"contactFilterLogicalExpresssion": "",
		"contactFilterConditions": [{
			"id":"6538285a-b00f-4063-b37a-cf14170aaa36",
			"campaignId":"f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
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
| `channel` | string | no |  App channel of this Outreach Campaign, allowed value is "SMS". |  
| `channelAccountId` | Guid | no |  Channel account id of this Outreach Campaign. |  
| `status` | string | no |  Status of the Outreach Message. Allowed values are `queued`, `sending`, `sent`, `failed`，`delivered`,`undelivered` |  
|`campaignId` |Guid |No| The unique id of the Outreach Campaign | 
|`contactId` |Guid  |No| The unique id of the Contact  |
| `sentTime` | datetime | no |  The time when the Outreach Message is sent. |  
| `keywords` | string | No  | search scope includes: from/to/campaign.name/contact.name |
| `pageIndex` | int | No  | page index | 
| `pageSize` | int | No  | page size, default is 10, if the value is 0, will return all matched contacts. | 
| `sortBy` | string | No  | sort by | 
| `sortOrder` | string | No  | `asc`,`desc`, default `asc` | 

#### Response
The Response body contains data with the [OutreachMessage](#OutreachMessage-object) object structure:


```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"messages": [{
		"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"sentTime": "2022-05-23 03:00:36.277",
		"channel": "SMS",
		"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
		"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
		"message": "Hello, please fill in your application form by the end of this week!",
		"from": "13312145080",
		"to": "+19085520595",
		"status": "sent",
		"failedReason": "",
		"campaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
		"campaignSentTime": "2022-05-22 03:00:36.277",
		"isMessageAutoAttachedToTicket": "true",
		"timeToAutoAttachToTicket": "whenTheMessageIsSent",
		"attachedToTicketId": 1,
		"callbackURL": "https://domainname.com/sms/callback",
		"outboundMessageId": "d426e8b7-c83f-46f9-ad8b-0086813d8345"
	}],
	"previousPage":null,
  	"nextPage":"http://demo.comm100.io/messages?pageIndex=2&pageSize=50&siteId=10100000",
  	"total":255
}
```


### Get the list of Unattached Outreach Message
`GET /outreach/unattachedmessages/`

#### Parameters
parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contactId` |Guid  |yes| The unique id of the Contact  |

#### Response
The Response body contains data with the [OutreachMessage](#OutreachMessage-object) object structure:


```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"messages": [{
		"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"sentTime": "2022-05-23 03:00:36.277",
		"channel": "SMS",
		"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
		"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
		"message": "Hello, please fill in your application form by the end of this week!",
		"from": "13312145080",
		"to": "+19085520595",
		"status": "sent",
		"failedReason": "",
		"campaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
		"campaignSentTime": "2022-05-22 03:00:36.277",
		"isMessageAutoAttachedToTicket": "true",
		"timeToAutoAttachToTicket": "whenTheMessageIsSent",
		"attachedToTicketId": 0,
		"callbackURL": "https://domainname.com/sms/callback",
		"outboundMessageId": "d426e8b7-c83f-46f9-ad8b-0086813d8345"
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
The Response body contains data with the [OutreachMessage](#OutreachMessage-object) object structure:

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"sentTime": "2022-05-23 03:00:36.277",
	"channel": "SMS",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
	"from": "13312145080",
	"to": "+19085520595",
	"status": "sent",
	"failedReason": "",
	"campaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
	"campaignSentTime": "2022-05-22 03:00:36.277",
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"attachedToTicketId": 1,
	"callbackURL": "https://domainname.com/sms/callback",
	"outboundMessageId": "d426e8b7-c83f-46f9-ad8b-0086813d8345"
}
```

### Create a new Outreach Message
`POST /outreach/messages`

#### Parameters
No Parameters

#### Request body

  | Name | Type | Required | Description                                           |     
  | - | - | - | - |  
  | `contactId` | Guid | yes |  Contact id of the Outreach Message. |  
  | `channel` | string | yes |  App channel of this Outreach Campaign, allowed value is "SMS". |
  | `channelAccountId` | Guid | yes |  Channel account id of this Outreach Campaign. |  
  | `message` | string | yes |  Message. |  
  | `isMessageAutoAttachedToTicket` | bool | no |  Whether a message need to auto attached to ticket. |  
  | `timeToAutoAttachToTicket` | string | no |  Allowed values are "whenTheMessageIsSent", "whenContactRepliesTheMessage". |  
  | `callbackURL` | string | no |  The callbackURL of the Outreach Message. |  
  
example:
```Json 
{
	"channel": "SMS",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"callbackURL": "https://domainname.com/sms/callback",
}
```

#### Response
The Response body contains data with the [OutreachMessage](#OutreachMessage-object) object structure:

```Json 
  HTTP/1.1 201 Created
  Content-Type: application/json
{
	"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"sentTime": "2022-05-23 03:00:36.277",
	"channel": "SMS",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
	"from": "13312145080",
	"to": "+19085520595",
	"status": "queued",
	"failedReason": "",
	"campaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
	"campaignSentTime": "2022-05-23 03:00:36.277",
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"attachedToTicketId": 0,
	"callbackURL": "https://domainname.com/sms/callback",
	"outboundMessageId": "d426e8b7-c83f-46f9-ad8b-0086813d8345"
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
  | `attachedToTicketId` | int | no |  The attached ticked Id of the Outreach Message. |  
  
example:
```Json 
{
	"attachedToTicketId": 1
}
```

#### Response
The Response body contains data with the [OutreachMessage](#OutreachMessage-object) object structure:

```Json 
  HTTP/1.1 200 Ok
  Content-Type: application/json
{
	"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"sentTime": "2022-05-23 03:00:36.277",
	"channel": "SMS",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
	"from": "Tom Cruise",
	"to": "XXX University",
	"status": "sent",
	"failedReason": "",
	"campaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
	"campaignSentTime": "2022-05-22 03:00:36.277",
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"attachedToTicketId": 1,
	"callbackURL": "https://domainname.com/sms/callback",
	"outboundMessageId": "d426e8b7-c83f-46f9-ad8b-0086813d8345"
}
```

### Outreach Message Callback
`POST /{callbackURL}`

#### Parameters
No Parameters

#### Request body
  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outreach Message. |  
  | `sentTime` | datetime | yes |  The sent time the Outreach Message. |  
  | `contactId` | Guid | yes |  Contact id of the Outreach Message. |  
  | `from` | string | yes |  Where the Outreach Message from. |  
  | `to` | string | yes |  Where the Outreach Message to. |  
  | `status` | string | yes |  Status of the Outreach Message. Allowed values are `queued`, `sending`, `sent`, `failed`，`delivered`,`undelivered` |  
  | `failedReason` | string | no |  The failed reason of the Outreach Message. |  
  | `campaignId` | Guid | no |  The Outreach Campaign id the Outreach Message. |  
  | `campaignSentTime` | datetime | no |  The sent time of the Outreach Campaign. |  
  | `attachedToTicketId` | int | no |  The attached ticked Id of the Outreach Message. |  

example:
```Json 
{
	"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",		
	"status": "sent",
	"failedReason": "",
}
```
#### Response

```Json 
  HTTP/1.1 200 OK
```


### Get the list of Contact
`GET /contact/contacts/`

#### Parameters

#### Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `keywords` | string | No  | search scope includes: contact name/identity id/identity value |  
  | `contactIdentityType` | string | No  | contact identity type |  
  | `contactIdentityValue` | string | No  | contact identity value | 
  | `isIncludeDeleted` | boolean | No  | if including the deleted contact, default value: `false` | 
  | `pageIndex` | int | No  | page index | 
  | `pageSize` | int | No  | page size, default is 10, if the value is 0, will return all matched contacts. | 
  | `sortBy` | string | No  | sort by | 
  | `sortOrder` | string | No  | `asc`,`desc`, default `asc` |  

#### Request Body


#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`contacts` |[Contact](#contact-object)[]  |Yes|  An array of [Contact](#contact-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
  "contacts": [
	{
    "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
    "name": "Tom cruise",
    "firstName": "Tom",
    "lastName": "Cruise",
    "createTime": "2022-03-18 01:12:32.0000000",
    "lastUpdatedTime": "2022-03-18 01:12:32.0000000",
    "mergeToContactId": "00000000-0000-0000-0000-000000000000",
    "customFields": { 
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
          "value":"+033214561",
          "avatarUrl":"",
          "infoUrl":"",
          "displayName":"",
          "originalContactPageUrl":"",
          "isDeleted":false
      }],
      "isDeleted":false
	}],
  "previousPage":null,
  "nextPage":"http://demo.comm100.io/contacts?pageIndex=2&pageSize=50&siteId=10100000",
  "total":255
}
```

### Query contact with conditions
`POST /contact/contacts:query`

#### Parameters

##### Path Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `pageIndex` | int | No  | page index | 
  | `pageSize` | int | No  | page size, default is 10, the max value is 500. | 
  | `sortBy` | string | No  | sort by | 
  | `sortOrder` | string | No  | `asc`,`desc`, default `asc` |  

##### Request Body
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `keywords` | string | No  | keywords |  
  | `contactIdentityType` | string | No  | contact identity type |  
  | `filterConditionMetType` | string | no |  Contact Filter Logical Expresssion of this Condition. `All`, `Any`, `LogicalExpression` | 
  | `filterLogicalExpresssion` | string | no |  Contact Filter Logical Expression | 
  | `filterConditions` | [ContactFilterCondition object](#contactFilterCondition-object)[] | no | An array of [ContactFilterCondition object](#contactFilterCondition-object)| 
  | `Ids` | guid[] | No  | Contact ID array, if pass this parameter, the keywords and filter conditions don't work | 
  | `isIncludeDeleted` | boolean | No  | if including the deleted contact, default value: `false` | 

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`contacts` |[Contact](#contact-object)[]  |Yes|  An array of [Contact](#contact-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
  "contacts": [
	{
    "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
    "name": "Tom cruise",
    "firstName": "Tom",
    "lastName": "Cruise",
    "createTime": "2022-03-18 01:12:32.0000000",
    "lastUpdatedTime": "2022-03-18 01:12:32.0000000",
    "mergeToContactId": "00000000-0000-0000-0000-000000000000",
    "customFields": { 
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
          "value":"+033214561",
          "avatarUrl":"",
          "infoUrl":"",
          "displayName":"",
          "originalContactPageUrl":"",
          "isDeleted":false
      }],
    "isDeleted":false
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
  | `isIncludeDeleted` | boolean | No  | if including the deleted contact, default value: `false` | 

#### Response
The Response body contains data with the [Contact](#contact-object) object structure:

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
  "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
  "name": "Tom cruise",
  "firstName": "Tom",
  "lastName": "Cruise",
  "createTime": "2022-03-18 01:12:32.0000000",
  "lastUpdatedTime": "2022-03-18 01:12:32.0000000",
  "mergeToContactId": "00000000-0000-0000-0000-000000000000",
  "customFields": {
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
          "value":"+033214561",
          "avatarUrl":"",
          "infoUrl":"",
          "displayName":"",
          "originalContactPageUrl":"",
          "isDeleted":false
      }],
    "isDeleted":false
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
    "name":"Test",
    "contactIdentities":[
        {
            "contactIdentityType":"Email",
            "value":"test@1234.com",
        }
    ],
    "customFields":{
        "company": "Comm100",
    }
}

```

#### Response
The Response body contains data with the [Contact](#contact-object) object structure:

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
        "value":"Test@1234.com",
        "avatarUrl":"",
        "infoUrl":"",
        "displayName":"",
        "originalContactPageUrl":""
    }],
    "isDeleted":false
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

  | Name | Type | Required | Description |     
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
        "value":"Test@1234.com",
        "avatarUrl":"",
        "infoUrl":"",
        "displayName":"",
        "originalContactPageUrl":"",
        "isDeleted":false
    }],
    "isDeleted":false
}
```

#### Response
The Response body contains data with the [Contact](#contact-object) object structure:


### Delete the Contact
`DELETE /contact/contacts/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  | The unique id of the Contact |  

#### Response
No response

### Batch delete contacts
`DELETE /contact/contacts`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `ids` | Guid | Yes  | The ID array of the Contacts |  

#### Response
No response

### Merge Contacts
`POST /contact/contacts/{id}:merge`

#### Parameters

##### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description |     
  | - | - | - | - | 
  | `contactId` | Guid | yes | The original ID of the Contact. |   

#### Response
No response

### Download Template
`GET /contact/contacts/importTemplate`

#### Parameters
No parameter

#### Response
- template file

### Import Contacts
`POST /contact/contacts:import`

#### Parameters

##### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description |     
  | - | - | - | - | 
  | `uploadFile` | file | yes | The import file URL |     
  | `duplicateControlOption` | string | no | Avaiable options: `merge`,`replace`,`skip`, default is `merge` |   

#### Response
  HTTP STATUS 202 (Accepted), includes the import record URI and ID.

### Get contact import record
`GET /contact/contactsImportRecords/{id}`

#### Parameters
   | Name | Type | Required | Description |     
  | - | - | - | - | 
  | `id` | guid | no | The ID of contact import record, the response of contact import request contains this ID. |  

#### Response
  | Name | Type | Required | Description |     
  | - | - | - | - | 
  | `Contact import record` | [Contact import record](#contact-import-record-object) | no | Contact import record object |   

### Export Contacts
`POST /contact/contacts:export`

#### Parameters
##### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description |     
  | - | - | - | - | 
  | `ids` | guid[] | no | The selected Contact ID array. |   

#### Response
- Cvs file

### Get Contact Identity
`GET /contact/ContactIdentities/{id}`

#### Parameters
##### Request parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  |`id` | guid | Yes | The ID of Contact identity |
  | `isIncludeDeleted` | boolean | No  | if including the deleted contact, default value: `false` | 

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact identity` |[Contact Identity](#contact-identity-object)  | Yes | Contact identity |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
  {
          "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
          "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
          "contactIdentityType":"SMS",
          "value":"+033214561",
          "avatarUrl":"",
          "infoUrl":"",
          "displayName":"",
          "originalContactPageUrl":"",
          "isDeleted":false
    }
```

### Identify contact identities with type and value
`GET /contact/ContactIdentities:identify`
#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `value` | string | Yes  |  Contact identity value |
  | `identityType` | string | No |  Contact identity type |
  | `isIncludeDeleted` | boolean | No  | if including the deleted contact, default value: `false` | 

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact identity` |[Contact Identity](#contact-identity-object)[] | Yes | Contact identity array |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
    {
        "contactIdentities": [
            {
                "id": "213c2529-0112-ed11-a83d-00155de0e608",
                "contactId": "00a58408-ccb9-475c-ba6d-3c12e5317c9e",
                "contactIdentityType": "SMS",
                "value": "865550950",
                "avatarUrl": "",
                "infoUrl": "",
                "displayName": "",
                "originalContactPageUrl": "",
                "isDeleted": false
            },
            {
                "id": "223c2529-0112-ed11-a83d-00155de0e608",
                "contactId": "c4f19620-b724-4619-9aec-bc6847436ef8",
                "contactIdentityType": "SMS",
                "value": "865550951",
                "avatarUrl": "",
                "infoUrl": "",
                "displayName": "",
                "originalContactPageUrl": "",
                "isDeleted": false
            }
        ]
    }
```

### Query contactidentities with conditions  
`POST /contact/contactIdentities:query`
#### Parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `ids` | guid [] | No  |  Contact identity ID array, if this parameter has value, the value and identityType will be ignored.|
  | `value` | string | No  |  Contact identity value, support fuzzy search |
  | `identityType` | string | No |  Contact identity type |
  | `isIncludeDeleted` | boolean | No  | if including the deleted contact, default value: `false` | 

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact identity` |[Contact Identity](#contact-identity-object)[] | Yes | Contact identity array |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
    {
        "contactIdentities": [
            {
                "id": "213c2529-0112-ed11-a83d-00155de0e608",
                "contactId": "00a58408-ccb9-475c-ba6d-3c12e5317c9e",
                "contactIdentityType": "SMS",
                "value": "865550950",
                "avatarUrl": "",
                "infoUrl": "",
                "displayName": "",
                "originalContactPageUrl": "",
                "isDeleted": false
            },
            {
                "id": "223c2529-0112-ed11-a83d-00155de0e608",
                "contactId": "c4f19620-b724-4619-9aec-bc6847436ef8",
                "contactIdentityType": "SMS",
                "value": "865550951",
                "avatarUrl": "",
                "infoUrl": "",
                "displayName": "",
                "originalContactPageUrl": "",
                "isDeleted": false
            }
        ]
    }
```

### Create a new Contact identity
`POST /contact/contactIdentities`
#### Parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact identity` |[Contact Identity](#contact-identity-object) | Yes | Contact identity |

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact identity` |[Contact Identity](#contact-identity-object) | Yes | Contact identity |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
  {
    "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
    "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
    "contactIdentityType":"SMS",
    "value":"+033214561",
    "avatarUrl":"",
    "infoUrl":"",
    "displayName":"",
    "originalContactPageUrl":""
    }
```

### Update the Contact identity
`PUT /contact/contactIdentities/{id}` 
#### Parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact identity` |[Contact Identity](#contact-identity-object) | Yes | Contact identity |

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact identity` |[Contact Identity](#contact-identity-object) | Yes | Contact identity |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
  {
    "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
    "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
    "contactIdentityType":"SMS",
    "value":"+033214561",
    "avatarUrl":"",
    "infoUrl":"",
    "displayName":"",
    "originalContactPageUrl":""
    }
```
 
### Remove the Contact identity
`DELETE /contact/contactIdentities/{id}` 
#### Parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  |`id` | contact identity id | Yes | Contact identity ID |

#### Response
No response


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
The Response body contains data with the [Contact Field](#contact-field-object) object structure:

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
The Response body contains data with the [Contact Field](#contact-field-object) object structure:


### Update a Contact Field
`PUT /contact/fields/{id}`

#### Parameters
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`id` | Guid  |Yes| contact field ID |
  | `name` | string | unique name of the field |
  | `isRequired` | bool | Whether the field is required or not. |
  | `isReadOnly` | string | Whether the field is readyonly or not. |
  | `type` | string | Type of the field. Allowed values are `text`, `textArea`, `email`, `url`, `date`, `integer`, `float` |
  | `length` | integer | Field length |
  | `helpText` | string | Help text of the field. |
  | `defaultValue` | string | Default value of the field. |
  | `order` | integer | |
  | `fieldOptions` | [fieldOptions](#field-option-object)[] | Reference to Field Option. |

#### Response
The Response body contains data with the [Contact Field](#contact-field-object) object structure:


### Delete a Contact Field
`Delete /contact/fields/{id}`

#### Parameters
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`id` | Guid  |Yes| contact field ID |

#### Response
No Response

### Reorder Contact Fields
`POST /contact/fields:reorder`
#### Parameters
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`id` | Guid[]  | Yes | contact field ID array |

#### Response
No Response


### Create a outbound message
`POST /outboundunichannel/messages/`

#### Parameters
Request body 
Request body the request body contains data with the following structure: 
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  |`channelAccountId` | Guid | yes |  Channel account id. |  
  |`contactId`  | Guid | no |   |   | 
  |`to`  | string | yes |   |   |
  |`message`  | string | yes |   |   |
  |`attachments`  | [Attachment](#attachment-object)[] object | no |  the attachments of message|   
  | `callbackURL` | string | no |  The callbackURL of the Outbound Message. |  
   |`email`  |  [EmailMetaData](#EmailMetaData-object) | no |  email properties for outbound email| 
  #### example:
```Json 
 {
    "channelAccountId":"q3f5b438-xw31-44af-b729-64swaf3d0b56",
    "to": "+19857473632", 
    "contactId":"q3f5b438-xw31-44af-b729-64swaf3d0b56",
    "message":"hello!Leon"
    "callbackURL":"https://domainname.com/callback/"
  } 
```

#### Response
The Response body contains data with the [OutboundMessage](#OutboundMessage-object) object structure:


Response
```Json
HTTP/1.1 200 OK 
  Content-Type:  application/json 
{
	"id": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"originMessageid": "f9928d68-92e6-4487-a2e8-8234fc9d1f48", //This is a string type as different channel will return different type. 
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
	"from" : "13312155802"
	"to": "+13453746564",
	"status": "sent",
	"failedReason": "",
	"sentTime": "2022-04-26T10:52:24.336Z"
}
```
### Outbound Message Callback
`POST /{callbackURL}`

#### Parameters
No Parameters

#### Request body
Request body the request body contains data with the following structure: 
  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outbound Message. |    
  | `originMessageid` | string | yes | The origin channel unique id of the Outbound Message. |    
  | `contactId` | Guid | no |  Contact id of the Outbound Message. |  
  | `from` | string | yes |  Where the Outbound Message from. |   
  | `to` | string | yes |  Where the Outbound Message to. |  
  | `status` | string | yes |  Status of the Outbound Message. Allowed values are `queued`, `sending`, `sent`, `failed`，`delivered`,`undelivered` |   
  | `failedReason` | string | no |  The failed reason of the Outbound Message. |  
The request body contains data with the [OutboundMessage](#OutboundMessage-object) object structure:

example:
```Json 
{
	"id": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
	"attachments": []
	"to": "+13453746564",
	"from" : "13312155802"
	"status": "sent",
	"failedReason": "",
	"sentTime": "2022-04-26T10:52:24.336Z",
}
```
#### Response

```Json 
  HTTP/1.1 200 OK
```

### Attach a message to the ticket
`POST /ticketing/tickets/{id}/messages`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | int | Yes  |  The unique id of the ticket |  
#### Request body 
Request body the request body contains data with the [Ticket Message](#ticketmessage-object) object structure: 
  | Name | Type | Required |  Description |    
  | - | - | :-: |  - | 
  |`parentId` | guid | no | Id of current message's parent. |
  |`sentById` | guid | yes | Id of the message sender. |
  |`sentByType` | string | yes | Role of the sender. Allowed values are "contact", "visitor", "chatbot", "channelAccount", "system", "agent". For Outreach send value as "outreach." |
  |`time` | datetime | no | Time when the message was created. |
  |`type` | string | yes | Type of message content. Allowed values are "text", "html", "video", "audio", "image", "file", "location", "quickReply", "webView". |
  |`metadata` | object | yes | Message metadata, Json format. |
  |`originalId` | string | no | Id in original channel. |
  |`channelId` | string | yes | Name of the message channel. |
  |`body` | string | no | Content of the message. You can pass both plaintext and base64 encoded text. If the request containing plaintext is blocked by comm100 WAF, 	use base64 format. When using base64, add "data:text/plain;base64," before the content. |
  |`ifDisplayInTicketCorrespondences` | bool | no | Whether a trigger email should be included within a ticket's correspondence thread or not. |
  |`isRead` | string | no | Name of the message channel. |
  |`messageDelivery` | [messageDelivery](#MessageDelivery-object) | no | Delivery information of the message. |
  |`channelId` | [attachments[]](#Attachment-object) | no | The list of message attachment. |
  #### example:
```Json 
{
	"parentId": "ef50cc68-5b88-4405-88f0-84334581246d",
	"sentById": "31cb8d70-b5a6-4faa-b021-62335d6dcf6c",
	"sentByType": "visitor",
	"time": "2021-04-26T10:52:24.336Z",
	"type": "text",
	"metadata": {...}
	"originalId": "96e7964a-29cb-40d6-8251-4f334b5748bd",
	"channelId": "Email",
	"body": "data:text/plain;base64,PHA+MTExPC9wPg==",
	"ifDisplayInTicketCorrespondences": true,
	"isRead": true,
	"attachments": []
	"messageDelivery": {}
}
```

#### Response

Response
```Json
HTTP/1.1 200 OK 
```

## Model

### OutreachCampaign Object
Outreach Campaign is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outreach Campaign. |  
  | `name` | string | yes |  Name of this Outreach Campaign. |  
  | `description` | string | no |  Description of this Outreach Campaign. |  
  | `channel` | string | no |  App channel of this Outreach Campaign, allowed value is "SMS". |  
  | `channelAccountId` | Guid | yes |  Channel account id of this Outreach Campaign. |  
  | `message` | string | yes |  Message of this Outreach Campaign. |  
  | `isMessageAutoAttachedToTicket` | bool | no |  Whether a message need to auto attached to ticket. | 
  | `timeToAutoAttachToTicket` | string | no |  Allowed values are "whenTheMessageIsSent", "whenContactRepliesTheMessage". |  
  | `contactFilterConditionMetType` | string | no |  Allowed values are "all", "any", "logicalExpression". |  
  | `contactFilterLogicalExpresssion` | string | no |  Contact Filter Logical Expresssion of this Condition. |  
  | `contactFilterConditions` | [ContactFilterCondition](#ContactFilterCondition-object)[] | no | An array of [ContactFilterCondition](#ContactFilterCondition-object)| 
  | `scheduledStartTime` | datetime | no |  Time when the schedule start. |  
  | `lastUpdatedTime` | datetime | no |  Time when the campaign last updated. |  
  
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
  | `sentTime` | datetime | yes | The unique id of the Outreach Campaign. | 
  | `campaign` |  [OutreachCampaign](#outreachcampaign-object) | yes | Outreach Campaign. |  
  
  
### OutreachMessage Object
Outreach Message is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outreach Message. |  
  | `sentTime` | datetime | yes |  The sent time the Outreach Message. |  
  | `contactId` | Guid | yes |  Contact id of the Outreach Message. |  
  | `channel` | string | yes |  App channel of this Outreach Campaign, allowed value is "sms". |
  | `channelAccountId` | Guid | yes |  Channel account id of this Outreach Campaign. |  
  | `message` | string | yes |  Message. |  
  | `from` | string | yes |  Where the Outreach Message from. |  
  | `to` | string | yes |  Where the Outreach Message to. |  
  | `status` | string | yes |  Status of the Outreach Message. Allowed values are `queued`, `sending`, `sent`, `failed`，`delivered`,`undelivered` |  
  | `failedReason` | string | no |  The failed reason of the Outreach Message. |  
  | `campaignId` | Guid | no |  The Outreach Campaign id the Outreach Message. |  
  | `campaignSentTime` | datetime | no |  The sent time of the Outreach Campaign. |  
  | `isMessageAutoAttachedToTicket` | bool | no |  Whether a message need to auto attached to ticket. |    
  | `timeToAutoAttachToTicket` | string | no |  Allowed values are "whenTheMessageIsSent", "whenContactRepliesTheMessage". |  
  | `attachedToTicketId` | int | no |  The attached ticked Id of the Outreach Message. |  
  | `callbackURL` | string | no |  The callbackURL of the Outreach Message. |  
  | `outboundMessageId` | Guid | no |  The outbound message Id of the Outreach Message. |  
  
### Contact Object
Contact is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the contact. |  
  | `name` | string | yes |  The name of the contact. |  
  | `firstName` | string | no | The first name of the contact |
  | `lastName` | string | no | The last name of the contact |
  | `createTime` | datetime | no |  The create time of the Contact. |  
  | `lastUpdatedTime` | datetime | no |  The last updated time of the Contact. |  
  | `mergeToContactId` | Guid | no |  The Contact id of merge to. |  
  | `customFields` | [Contact Custom Field Value](#contact-custom-field-value-object) | no | custom field value | 
  | `contactIdentities` | [Contact Identity](#contact-identity-object)[] | yes | custom field value array | 
  | `isDeleted` | boolean | if is deleted |

### Contact Identity Object
Contact Identity is represented as simple flat JSON objects with the following keys:

| Name | Type | Description | 
| - | - | - |
| `id` | string | read-only, the id of identity |
| `type` | string | required, contact identity type |
| `value` | string | required, the value of one identity, should be unique |
| `avatar` | string | optional, the avatar of one identity |
| `identityInfoUrl` | string | optional, the original identity info url of one identity |
| `originalContactPageUrl` | string | optional, the original contact page url |
| `displayName` | string | the name of identity |
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
| `identityType` | string | `visitor`, `email`, `SMS`, `facebook`, `twitter`, `wechat`, `SSOID`, `externalID`, `whatsApp`, `instagram`, `telegram`, `line`. Available when Is Identity is true. |
| `isRequired` | bool | Whether the field is required or not. |
| `isVisible` | bool | Whether the field is visible in UI or not. |
| `isReadOnly` | string | Whether the field is readyonly or not. |
| `type` | string | Type of the field. Allowed values are `text`, `textArea`, `email`, `url`, `date`, `date time`, `integer`, `float`, `radio`, `checkbox`, `dropdownlist`, `checkboxlist`, `timezone` |
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

### Contact import record object
| Name | Type | Description | 
| - | - | - | 
| `id` | guid | Id of the field option. |
| `createdById` | guid | The Agent ID of createdBy. |
| `status` | string | enum: `in-process`, `completed`, `error` |
| `createdtime` | datetime | created time |
| `completedtime` | datetime | completed time |
| `result` | string | import result |
| `failedRecords` | string | json content of failed records |
| `importFileUrl` | string | the URL of imported file. |


### OutboundMessage Object
Outbound Message is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outbound Message. |    
  | `originMessageid` | string | yes | The origin channel unique id of the Outbound Message. |    
  | `channelAccountId` | Guid | yes |  Channel account id of this Outbound Campaign. |  
  | `contactId` | Guid | no |  Contact id of the Outbound Message. |  
  | `message` | string | yes |  Message. |  
  | `attachments` | string | no |  attachment. |  
  | `from` | string | yes |  Where the Outbound Message from. |   
  | `to` | string | yes |  Where the Outbound Message to. |  
  | `status` | string | yes |  Status of the Outbound Message. Allowed values are `queued`, `sending`, `sent`, `failed`，`delivered`,`undelivered` |   
  | `failedReason` | string | no |  The failed reason of the Outbound Message. |  
  | `callbackURL` | string | no |  The callbackURL of the Outreach Message. |  
  
 ### Attachment Object
Attachment Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `type` | string | yes | Type of the attachment. Allowed values are "video", "audio", "image", "file". |  
  | `url` | string | yes | Download URL of the attachment. |  
  | `size` | integer | no |  Size of the attachment. |  
  | `name` | string | no |  Name of the attachment. |  
  
 ### MessageDelivery Object
MessageDelivery Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `messageId` | guid | yes |  Id of the message which the delivery belongs to. |  
  | `status` | string | no |  Status of the delivery. Allowed values are "waitForSending", "sending", "failed", "success". |  
  | `failReason` | string | no | Reason why the delivery failed. |  
  | `time` | datetime | no | Time when message was delivered. |  
  | `groupId` | guid | no |  The Id of the message group, it is used for bot message, it should be delivered in a group and one by one in order. |  
  | `orderNum` | int | no |  Order of the delivery. |  
  
  ### CallbackRequest Object
MessageDelivery Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `Id` | guid | yes |  The unique id of the Outreach Message. |  
  | `status` | string | no |  Status of the delivery. Allowed values are "waitForSending", "sending", "failed", "success". |  
  | `failReason` | string | no | Reason why the delivery failed. |    

