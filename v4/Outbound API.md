  | Change Version | API Version | Change nots | Change Date | Author |Architect Team Reviewer | 
  | - | - | - | - | - |- |
  | 1.0 | v1 |Outbound API | 2022-5-18 | Leon，Frank,Yogesh|  Allon|

## Summary

### Outbound Message Callback API 

The CallbackURL can be any valid URL that implements this API, and it is configured in the system when a new Outbound Message Campaign created . 
  - POST /{callbackURL} - [Outbound Message Callback API](#voice-channel-adapter-receives-input). 

### Outbound Message API  

#### Outbound Campaign
  - GET /outbound/campaigns/ - [Get the list of Campaign](#get-the-list-of-campaign).  Outbound service gets the list of Campaign
  - GET /outbound/campaigns/{campaignId} - [Get a single Campaign](#get-a-single-campaign).  Outbound service gets a single Campaign
  - POST /outbound/campaigns/ - [Create A New Campaign](#create-a-new-campaign).  Outbound service creates a Campaign
  - PUT /outbound/campaigns/{campaignId} - [Update the Campaign](#update-the-campaign).  Outbound service updates the Campaign
  - DELETE /outbound/campaigns/{campaignId} - [Delete the campaign](#delete-the-campaign). Outbound service deletes the campaign.
#### Outbound Message
  - GET /outbound/messages/ - [Get the list of Message](#get-the-list-of-message).  Outbound service gets the list of Message
  - GET /outbound/messages/{messageId} - [Get a single Message](#get-a-single-message).  Outbound service gets a single Message
  - POST /outbound/campaigns/{campaignId}/messages - [Create A Message](#create-a-message).  Outbound service creates a message 

### Contact API 
####  Contact
  - GET /contact/contacts/ - [Get the list of Contact](#get-the-list-of-contact).  Contact service gets the list of contact
  - GET /contact/contacts/{id} - [Get a single Contact](#get-a-single-contact).  Contact service gets a single Contact
  - POST /contact/contacts/ - [Create A New Contact](#create-a-new-contact).  Contact service creates a Contact
  - PUT /contact/contacts/{id} - [Update the Contact](#update-the-contact).  Contact service updates the Contact
  - DELETE /contact/contacts/{id} - [Delete the Contact](#delete-the-contact). Contact service deletes the Contact.

### Ticketing Outbound Message API 
  - GET /ticketing/outboundmessages/ - [Get the list of outboundmessages](#get-the-list-of-outbound-messages).  Ticketing service gets the list of outbound messages
  - GET /ticketing/campaigns/{campaignId} - [Get a single outbound messages](#get-a-single-campaign).  Ticketing service gets a single outbound message
  - POST /ticketing/outboundmessages/ - [Create A New outbound message](#create-a-new-outbound-message).  Ticketing service creates a outbound message
  - PUT /ticketing/outboundmessages/{Id} - [Update the outboundmessages](#update-the-outbound-message).  Ticketing service updates the outbound messages


## Endpoints

## Model
