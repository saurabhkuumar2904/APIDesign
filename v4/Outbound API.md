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
