  | Change Version | API Version | Change nots | Change Date | Author |Architect Team Reviewer | 
  | - | - | - | - | - |- |
  | 1.0 | v1 |Outreach API | 2022-5-18 | Leon，Frank,Yogesh|  Allon|

## Summary

### Outreach Message Callback API 

The CallbackURL can be any valid URL that implements this API, and it is configured in the system when a new Outreach Message Campaign is created. 
  - POST /{callbackURL} - [Outreach Message Callback API](#voice-channel-adapter-receives-input). 

### Outreach Message API  

#### Outreach Campaign
  - GET /outreach/campaigns/ - [Get the list of Outreach Campaign](#get-the-list-of-outreach-campaign). 
  - GET /outreach/campaigns/{id} - [Get a single Outreach Campaign](#get-a-single-outreach-campaign). 
  - POST /outreach/campaigns/ - [Create a new Outreach Campaign](#create-a-new-outreach-campaign).  
  - PUT /outreach/campaigns/{id} - [Update the Outreach Campaign](#update-the-outreach-campaign).  
  - DELETE /outreach/campaigns/{id} - [Delete the campaign](#delete-the-campaign). 
#### Outreach Message
  - GET /outreach/messages/ - [Get the list of Outreach Message](#get-the-list-of-outreach-message). 
  - GET /outreach/messages/{id} - [Get a single Outreach Message](#get-a-single-outreach-message). 
  - POST /outreach/campaigns/{id}/messages - [Create a new Outreach Message](#create-a-new-outreach-message).   
  - PUT /outreach/messages/{id} - [Update the Outreach Message](#update-the-outreach-message).  

### Contact API 
####  Contact
  - GET /contact/contacts/ - [Get the list of Contact](#get-the-list-of-contact).  
  - GET /contact/contacts/{id} - [Get a single Contact](#get-a-single-contact).  
  - POST /contact/contacts/ - [Create a new Contact](#create-a-new-contact).  
  - PUT /contact/contacts/{id} - [Update the Contact](#update-the-contact).  
  - DELETE /contact/contacts/{id} - [Delete the Contact](#delete-the-contact). 
  - POST /contact/contacts:merge - [Merge Contact](#merge-contacts). 

####  Contact Field
  - GET /contact/fields - [Get the list of Contact Field](#get-the-list-of-contact-field).
  - GET /contact/fields/{id} - [Get a single Contact Field](#get-a-single-contact-field).  
  - POST /contact/fields - [Create a new Contact Field](#create-a-new-contact-field).  
  - PUT /contact/fields/{id} - [Update a Contact Field](#update-a-contact-field).  
  - DELETE /contact/fields/{id} - [Delete a Contact Field](#delete-a-contact-field).  

### Ticketing Outreach Message API 
  - POST /ticketing/outreachmessages/ - [Create a new ticket outreach message](#create-a-new-ticket-outreach-message). 
  - GET /ticketing/outreachmessages/{id} - [Get a single ticket outreach message](#get-a-single-ticket-outreach-message). 

## Endpoints

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
	"outreachCampaigns": [
    {
		"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"name": "test-campaign",
		"description": "test campaign",
		"channel": "SMS",
		"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
		"message": "Hello, please fill in your application form by the end of this week!",
		"isMessageAutoAttachedToTicket": "Yes",
		"preferredTicketToAutoAttach":"New ticket",
		"timeToAutoAttachToTicket": "When the message is sent",
		"contactFilterConditionMetType": "Any",
		"contactFilterLogicalExpresssion": "",
		"scheduledStartTime": ""
	}
    ]
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
	"preferredTicketToAutoAttach":"New ticket",
	"timeToAutoCreateNewTicket": "When the message is sent",
	"contactFilterConditionMetType": "Any",
	"contactFilterLogicalExpresssion": "",
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
	"preferredTicketToAutoAttach":"New ticket",
	"timeToAutoAttachToTicket": "When the message is sent",
	"contactFilterConditionMetType": "Any",
	"contactFilterLogicalExpresssion": "",
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
	"preferredTicketToAutoAttach":"New ticket",
	"timeToAutoAttachToTicket": "When the message is sent",
	"contactFilterConditionMetType": "Any",
	"contactFilterLogicalExpresssion": "",
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
	"preferredTicketToAutoAttach":"New ticket",
	"timeToAutoAttachToTicket": "When the message is sent",
	"contactFilterConditionMetType": "Any",
	"contactFilterLogicalExpresssion": "",
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
	"preferredTicketToAutoAttach":"New ticket",
	"timeToAutoAttachToTicket": "When the message is sent",
	"contactFilterConditionMetType": "Any",
	"contactFilterLogicalExpresssion": "",
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

### Get the list of Outreach Message
`GET /outreach/messages/`
#### Parameters
No parameters

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`outreachMessageList` |[OutreachMessage](#outreachmessage-object)[]  |Yes|  An array of [OutreachMessage](#outreachmessage-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"outreachMessages": [
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
	"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48"
}
```

### Create a new Outreach Message
`POST /outreach/campaigns/{id}/messages`
#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outreach Campaign |  

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
  | `attachedToTicketId` | Guid | no |  The attached ticked Id of the Outreach Message. |  
  
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
	"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48"
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
  | `attachedToTicketId` | Guid | no |  The attached ticked Id of the Outreach Message. |  
  
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
	"attachedToTicketId": "a1128d68-92e6-4487-a2e8-8234fc9d1f48"
}
```

### Get the list of Contact
`GET /contact/contacts/`
#### Parameters
No parameters

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`contactList` |[Contact](#contact-object)[]  |Yes|  An array of [Contact](#contact-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"contacts": [{
		"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"name": "Tom cruise",
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
		"timeZone": "",
        "createTime": "2022-03-18 01:12:32.0000000",
        "lastUpdatedTime": "2022-03-18 01:12:32.0000000",
        "mergeToContactId": "00000000-0000-0000-0000-000000000000"
	}]
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
	"timeZone": "",
	"createTime": "2022-03-18 01:12:32.0000000",
	"lastUpdatedTime": "2022-03-18 01:12:32.0000000",
	"mergeToContactId": "00000000-0000-0000-0000-000000000000"
}
```

### Create a new Contact
`POST /contact/contacts/`
#### Parameters
No parameters

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `name` | string | yes |  The name of the Contact. |  
  | `firstName` | string | no |  The first name of the Contact. |  
  | `lastName` | string | no |  The last name of the Contact. |  
  | `description` | string | no |  The description of the Contact. |  
  | `alias` | string | no |  The alis of the Contact. |  
  | `title` | string | no |  The title of the Contact. |  
  | `company` | string | no |  The company of the Contact. |  
  | `fax` | string | no |  The fax of the Contact. |  
  | `phone` | string | no |  The phone of the Contact. |  
  | `mailingAddress` | string | no |  The mailing address of the Contact. |  
  | `city` | string | no |  The city of the Contact. |  
  | `stateOrProvince` | string | no |  The state or province of the Contact. |  
  | `countryOrRegion` | string | no |  The country or region of the Contact. |  
  | `postalOrZipCode` | string | no |  The postal or zip code of the Contact. |  
  | `timeZone` | string | no |  The time zone of the Contact. |  
  | `mergeToContactId` | Guid | no |  The Contact id of merge to. |  
  
example:
```Json 
{
	"name": "Tom cruise",
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
	"timeZone": "",
	"mergeToContactId": "00000000-0000-0000-0000-000000000000"
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
	"timeZone": "",
	"createTime": "2022-03-18 01:12:32.0000000",
	"lastUpdatedTime": "2022-03-18 01:12:32.0000000",
	"mergeToContactId": "00000000-0000-0000-0000-000000000000"
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
  | `firstName` | string | no |  The first name of the Contact. |  
  | `lastName` | string | no |  The last name of the Contact. |  
  | `description` | string | no |  The description of the Contact. |  
  | `alias` | string | no |  The alis of the Contact. |  
  | `title` | string | no |  The title of the Contact. |  
  | `company` | string | no |  The company of the Contact. |  
  | `fax` | string | no |  The fax of the Contact. |  
  | `phone` | string | no |  The phone of the Contact. |  
  | `mailingAddress` | string | no |  The mailing address of the Contact. |  
  | `city` | string | no |  The city of the Contact. |  
  | `stateOrProvince` | string | no |  The state or province of the Contact. |  
  | `countryOrRegion` | string | no |  The country or region of the Contact. |  
  | `postalOrZipCode` | string | no |  The postal or zip code of the Contact. |  
  | `timeZone` | string | no |  The time zone of the Contact. |  
  | `mergeToContactId` | Guid | no |  The Contact id of merge to. |  

example:
```Json 
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"name": "Tom cruise 2",
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
	"timeZone": "",
	"createTime": "2022-03-18 01:12:32.0000000",
	"mergeToContactId": "00000000-0000-0000-0000-000000000000"
}
```

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`contact` |[Contact](#contact-object)  |Yes|   [Contact](#contact-object)  |

```Json 
  HTTP/1.1 200 Ok
  Content-Type: application/json
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"name": "Tom cruise 2",
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
	"timeZone": "",
	"createTime": "2022-03-18 01:12:32.0000000",
	"lastUpdatedTime": "2022-05-18 11:12:32.0000000",
	"mergeToContactId": "00000000-0000-0000-0000-000000000000"
}
```

### Delete the Contact
`DELETE /contact/contacts/{id}`
#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Contact |  

#### Response
No response

```Json 
  HTTP/1.1 204 Ok
  Content-Type: application/json
{
}
```




### Get the list of contact field
`GET /contact/fields/`
#### Parameters
No parameters

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`contact fields` |[Contact Field](#contact-field-object)  |Yes| [Contact](#contact-field-object)  |

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
  | `isMessageAutoAttachedToTicket` | bool | no |  Whether a new ticket will be auto created or not for the message.| 
  | `preferredTicketToAutoAttach` | string | no |  Allowed values are "New ticket", "Existing unresolved ticket whose last message is from SMS channel".|  
  | `timeToAutoAttachToTicket` | string | no |  Allowed values are "When the message is sent", "When contact replies the message". |  
  | `contactFilterConditionMetType` | string | no |  Allowed values are "All", "Any", "Logical Expression". |  
  | `contactFilterLogicalExpresssion` | string | no |  Contact Filter Logical Expresssion of this Condition. |  
  | `scheduledStartTime` | timestamp | no |  Message of this Outreach Campaign. |  
  
### OutreachCampaignContactFilterCondition Object
Outreach Campaign Contact Filter Condition is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the contact filter condition. |  
  | `outreachCampaignId` | Guid | yes |  The Outreach Campaign id of the contact filter condition. |  
  | `fieldName` | string | yes |  Field name of the contact filter condition. |  
  | `operator` | string | yes |  Allowed values are "Contains", "Does Not Contain", "Is", "Is Not", "Is More Than", "Is Less Than", "Before", "After". |  
  | `value` | string | yes |  Trigger mode of the contact filter condition. |  
  | `order` | integer | yes |  Order of the contact filter condition. | 
  
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
  | `outreachCampaignId` | Guid | yes |  The Outreach Campaign id the Outreach Message. |  
  | `attachedToTicketId` | Guid | no |  The attached ticked Id of the Outreach Message. |  
        
### Contact Object
Contact is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Contact. |  
  | `name` | string | yes |  The name of the Contact. |  
  | `firstName` | string | no |  The first name of the Contact. |  
  | `lastName` | string | no |  The last name of the Contact. |  
  | `createTime` | timestamp | no |  The create time of the Contact. |  
  | `lastUpdatedTime` | timestamp | no |  The last updated time of the Contact. |  
  | `mergeToContactId` | Guid | no |  The Contact id of merge to. |  
  | `customFields` | [custom field value](#custom-field-value-object)[] | custom field value array | 

### custom field value object
| Name | Type | Description | 
| - | - | - | 
| `id` | string | the id of custom field |
| `name` | string | the name of custom field |
| `value` | string | the value of custom field |

### contact field object
| Name | Type | Description | 
| - | - | - | 
| `id` | guid | the id of custom field |
| `name` | string | unique name of the field |
| `isSystem` | bool | Whether the field is a system field or not. |
| `isIdentity` | bool | Whether the field is an identity field or not. |
| `identityType` | string |  |
| `isRequired` | bool | Whether the field is required or not. |
| `isReadOnly` | string | Whether the field is readyonly or not. |
| `type` | string | Type of the field. Allowed values are "text", "textArea", "email", "url", "date", "integer", "float" |
| `length` | integer | Field length |
| `helpText` | string | Help text of the field. |
| `defaultValue` | string | Default value of the field. |
| `order` | integer | |
| `fieldOptions` | fieldOptions[#field-option-object] | Reference to Field Option. |

### Field option object
| Name | Type | Description | 
| - | - | - | 
| `id` | guid | Id of the field option. |
| `fieldId` | guid | Id of the field which the option belongs to. |
| `value` | string | Value of the option. |
| `order` | integer | Order of the option. |
| `displayText` | string | Display text of the option. |