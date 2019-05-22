# Anytime Conversation
  Anytime=Anytime Conversation
# Authentication 
- Comm100 AnyTime Conversation API provides 2 authentication methods: 
    - API_key Authentication 
    - OAuth Authentication 
- [document](https://www.comm100.com/doc/api/introduction.htm#/) 

# Parameter introduction 
- Incoming parameters:
    - Get API passes parameters through the query string 
    - Put/Post API passes parameters through json data. 
    - DateTime Parameters: 
        - All time parameters need to be entered in the standard format of <a href="https://en.wikipedia.org/wiki/ISO_8601" target="_blank">ISO-8601</a>
    - The maximum size for a conversation’s attachments is 20MB.
- All time values are UTC time. The caller can convert to different time zones where needed. 

# Includes

- Following APIs can use `Includes` as parameters to get related objects.

    | Endpoints | Support including parameters |
    | - | - |
    | `get api/v3/anytime/conversations` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy |
    | `get api/v3/anytime/conversations/{id}` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy, messages |
    | `get api/v3/anytime/conversations/{id}/messages` | sender |
    | `get api/v3/anytime/deletedconversations` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy |
    | `get api/v3/anytime/deletedconversations/{id}` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy, messages |
    | `get api/v3/anytime/deletedconversations/{id}/messages` | sender |
    | `get api/v3/anytime/portalconversations/{id}` | contact, messages |
    | `get api/v3/anytime/portalconversations` | contact |
    | `get api/v3/anytime/portalconversations/{id}/messages` | sender | 

- Sample:
    - request: `get api/v3/anytime/conversations/{id}?include=agentAssignee,contact,createdBy,messages`
    - response:

        ``` javascript
        {
            "conversation": {
                "id": 1,
                "agentAssigneeId": 1,
                "agentAssignee": {  //included the agent object
                    "id": 1,
                    //...
                },
                "contactId":2
                "contact": {  //included the contact object
                    "id": 2,
                    //...
                },
                "createdById": 3,
                "createdByType": "agent",
                "createdBy": {  //included the agent or contact object according to the createdByType.
                    "id": 3,
                    //...
                },
                "messages":[    //included the messages.
                    {
                        "id": 56, 
                        //...
                    },
                    {
                          "id": 57, 
                        //...
                    }
                ]
            //...
            }
        }, 
        ``` 


# Resource List - AnytimeConversation
|Name|EndPoint|Note| 
|---|---|---| 
|[Conversations](#conversations)|/api/v3/anytime/conversations| | 
|[Messages](#message)|/api/v3/anytime/conversations/{id}/messages| | 
|[Filters](#filters)|/api/v3/anytime/filters| Agent console filters| 
|[Field & Mappings](#field-&-mappings)|/api/v3/anytime/fields| System fields and custom fields | 
|[Routing Rules](#routing-rules)|/api/v3/anytime/routingRules||
|[Auto Allocation](#auto-allocation)|/api/v3/anytime/autoAllocation||
|[Triggers](#triggers)|/api/v3/anytime/triggers||
|[Working Time & Holidays](#working-Time-Holidays)|/api/v3/anytime/workingTimeAndHolidays||
|[SLA Policies](#sla-policies)|/api/v3/anytime/sLAPolicies||
|[Blocked Senders](#blocked-sender)|/api/v3/anytime/blockedSenders|Blocked email or domain| 
|[Email Accounts](#email-accounts)|/api/v3/anytime/emailAccounts| Email accounts|
|[Integration Accounts](#integration-accounts)|/api/v3/anytime/IntegrationAccounts| (New)? |  
|[Junk Emails](#junke-mails)|/api/v3/anytime/junkEmails| Emails from blocked senders| 
|[Right Now Reports](#reports)|/api/v3/anytime/reports/rightNow| | 
|[Volume Reports](#reports)|/api/v3/anytime/reports/volume|  | 
|[Channel Reports](#reports)|/api/v3/anytime/reports/channel|  | 
|[Efficiency Reports](#reports)|/api/v3/anytime/reports/efficiency|  |
|[Portalconversations](#portalconversation)|/api/v3/anytime/portalconversations|  |
<!-- 
|[Webhooks](#webhooks)|/api/v3/realtime/webhooks | | 
|[Notification](#notification)|/api/v3/anytime/notification| (new)? angent notification to server | 
|[Department](#department)|/api/v3/anytime/departments|(move to public)  |
|[Config](#config)|/api/v3/anytime/configs| Get site settings| 
|[CannedResponse](#cannedresponse)|/api/v3/anytime/cannedResponses|(move to public) |
|[Attachment](#attachment)|/api/v3/anytime/attachments|(move to public)?| -->

# Conversations 
## objects 
### conversation 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id of conversation | 
| `subject` | string | conversation subject | 
| `agentAssigneeId` | integer | agent assignee id | 
| `departmentAssigneeId` | integer | department assignee id | 
| `contactId` | integer | contact id | 
| `receivedFrom` | string | received email address for email channel | 
| `channel` | string | `portal`, `email`| 
| `priority` | string | `urgent`, `high`, `normal`, `low` | 
| `status` | string | `new`, `pendingInternal`, <br/>`pendingExternal`, `onHold`, `closed` | 
| `isRead` | boolean | if the conversation is read | 
| `isReaByContact` | boolean | if the portal conversation is read by contact |
| `customFields` | [custom field value](#custom-field-value)[] | custom field value array | 
| `createdById` | integer | contact id or agent id | 
| `createdByType` |  string | agent or contact or system | 
| `createdTime` | datetime | create time of conversation | 
| `lastActivityTime` | datetime | last activity time of conversation | 
| `lastStatusChangeTime` | datetime | last status change time of conversation | 
| `lastRepliedTime` | datetime | last replied time | 
| `lastRepliedById` | integer | contact id or agent id | 
| `lastRepliedByType` | string | `agent` or `contact` or `system`| 
| `hasDraft` | boolean | if has draft | 
| `tagIds` | integer[] | tag id array | 
| `isDeleted` | boolean | if deleted | 
| `slaPolicyId` | integer | SLA id of this conversation matched | 
| `firstRespondBreachAt` | datetime | Timestamp that denotes when the first <br/> response is due | 
| `nextRespondBreachAt` | datetime | Timestamp that denotes when the next <br/> response is due | 
| `resolveBreachAt` | datetime | Timestamp that denotes when the conversation is <br/> due to be resolved | 
| `mentionedAgents`|[mentioned agent](#mentioned-agent)[]| mentioned agents list | 
| `isEditable`| boolean | if the current agent can update\reply the conversation | 
| `lastMessage`| string | plain text of last message | 

### custom field value
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | the id of custom field |
| `name` | string | the name of custom field |
| `value` | string | the value of custom field |

### mentioned agent 
| Name | Type | Description | 
| - | - | - | 
| `agentId` | integer | the agent id of mentioned | 
| `isRead`| boolean | if the mentioned conversation is read | 
| `messageId`| integer| message id| 

### message 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id of message | 
| `conversationId` | integer | id of conversation | 
| `type` | string | `note`, `email`, `reply` | 
| `source` | string | `agentConsole`, `helpDesk`, `webForm`, `API`, `chat`, `offlineMessage` | 
| `htmlBody` | string | html body of message | 
| `plainBody` | string | plain text body of message | 
| `quote` | string | quoted content of the message, only for email message | 
| `senderId`| integer | id of agent or contact | 
| `senderType`| string | `agent` or `contact` or `system` | 
| `time` | datetime | the sent time of the message | 
| `subject` | string | subject | 
| `from` | string | from email address| 
| `to` | string | the to email addresses | 
| `cc` | string | cc email addresses |  
| `attachments` | [attachment](#attachment)[] | attachment array| 
| `mentionedAgentIds` | integer[] | only for Note, @mentioned agents id array |

### conversation draft 
| Name | Type | Description | 
| - | - | - | 
| `draftId` | integer | id of conversation draft | 
| `conversationId` | integer | id of conversation | 
| `subject` | string | draft subject | 
| `htmlBody` | string | html body of conversation draft | 
| `plainBody` | string | plain text body of conversation draft | 
| `quote` | string | quoted body | 
| `from` | string | from email address | 
| `to` | string | to email address | 
| `cc` | string | cc email addresses | 
| `savedTime` | datetime | | 
| `savedById` | integer | the id of the agent who saved the conversation draft | 
| `attachments` | [attachment](#attachment)[] | draft attachments | 

## endpoints 
### List conversations 
`get api/v3/anytime/conversations` 
+ Each request returns a maximum of 50 conversations. 
+ Parameters 
    - filterId: integer, filter id  
    - tagId: integer, tag id
    - keywords: string
    - timeFrom: DateTime, last reply time, default search the last 30 days
    - timeTo: DateTime, last reply time, default value is the current time
    - timeZoneOffset, float, time zone of your time parameters
    - pageIndex: integer
    - sortBy: string, `nextSLABreach`, `lastReplyTime`, `lastActivityTime`, `priority`, `status` , default value: `lastReplyTime`
    - sortOrder: string, `ascending` or `descending`, default value: `descending`
    - conditions: parameter format: `conditions[0][field]=subject&conditions[0][matchType]=is&conditions[0][value]=hi&conditions[1][field]=status&conditions[1][matchType]=is&conditions[1][value]=new`, fields can be conversation system fields and custom fields.
        - field: string, field name
        - matchType: string 
        - value: string
+ Response 
    - conversations: [conversation](#conversations) list, 
    - total: integer, total number of conversations 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 
+ Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v3/anytime/conversations?include=agentAssignee` |
    | departmentAssignee | `get api/v3/anytime/conversations?include=departmentAssignee` |
    | contact | `get api/v3/anytime/conversations?include=contact` |
    | createdBy | `get api/v3/anytime/conversations?include=createdBy` |
    | lastRepliedBy | `get api/v3/anytime/conversations?include=lastRepliedBy` | 

### Get a conversation 
`get api/v3/anytime/conversations/{id} ` 
+ Parameters 
    - id: integer, conversation  
+ Response 
    - conversation: [conversation](#conversation) 
+ Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v3/anytime/conversations/{id}?include=agentAssignee` |
    | departmentAssignee | `get api/v3/anytime/conversations/{id}?include=departmentAssignee` |
    | contact | `get api/v3/anytime/conversations/{id}?include=contact` |
    | createdBy | `get api/v3/anytime/conversations/{id}?include=createdBy` |
    | lastRepliedBy | `get api/v3/anytime/conversations/{id}?include=lastRepliedBy` |
    | messages | `get api/v3/anytime/conversations/{id}?include=messages` |
 
### Submit a new conversation 
`post api/v3/anytime/conversations` 
- Parameters 
    - subject: string, conversation subject, required
    - channel: string, `portal`, `email`, required 
    - contactId: integer, contact id
    - agentAssigneeId: integer, agent id
    - departmentAssigneeId: integer, department id
    - priority: string, `urgent`, `high`, `normal`, `low`, default value: `normal` 
    - status: string, `new`, `pendingInternal`, `pendingExternal`, `onHold`, `closed`, default value: `new`  
    - customFields: [custom field value](#custom-field-value)[], custom field value array
    - tagIds: integer[], tag id array
    - message: the first message of the conversation, required
        - type: string, `note`, `email`, `reply`, required
        - source: string, `agentConsole`, `API`, default value: `API`
        - subject: string, for email message, email subject
        - htmlBody: string, html body of message
        - plainBody: string, plain text body of message
        - quote: string
        - from: string, for email type message, one of email account address 
        - cc: string, message cc emails 
        - attachments: [attachment](#attachment)[], attachment array
+ Response 
    - conversation: [conversation](#conversations)

### List messages of a conversation 
`get api/v3/anytime/conversations/{id}/messages` 
+ Parameters 
    - id: integer, conversation id 
+ Response 
    - messages: [message](#message) list 
+ Includes

    | Includes | Description |
    | - | - |
    | sender | `get api/v3/anytime/conversations/{id}/messages?include=sender` |

### Update a conversation 
`put api/v3/anytime/conversations/{id}` 
- Parameters 
    - id: integer, conversation id
    - subject: string, conversation subject
    - contactId: integer, the contact id
    - agentAssigneeId: integer, agent id
    - departmentAssigneeId: integer, department id
    - priority: string, priority: `urgent`, `high`, `normal`, `low`
    - status: string, `new`, `pendingInternal`, `pendingExternal,`, `onHold`, `closed`
    - isRead: boolean
    - customFields: [custom field value](#custom-field-value)[], custom field value array
    - tagIds: integer[], tag id array
- Response 
    - conversation: [conversation](#conversation) 

### Batch update conversations
`put api/v3/anytime/conversations/` 
+ Parameters 
    - ids: integer[], conversation id array
    - status, string
    - priority, string
    - agentAssigneeId, integer
    - departmentAssigneeId, integer
    - isRead, boolean
+ Response 
    - conversations: [conversation](#conversation) list 

### Reply a conversation 
`post api/v3/anytime/conversations/{id}/messages` 
- Parameters  
    - type: string, `note`, `email`, `reply`, required
    - source: string, `agentConsole`, `API`, default value: `API`
    - subject: string, for email message, email subject
    - htmlBody: string, html body of message, if you want to @mention an agent in a note, you can use the format: `<span data-id=agentId class="athighlight">note body</span>`
    - plainBody: string, plain text body of message
    - quote: string, quote content, only for email message
    - from: string, for email type message, one of email account address 
    - cc: string, message cc emails 
    - attachments: [attachment](#attachment)[], attachment array
- Response 
    - message: [message](#message) 

### Mark a conversation as read 
`put api/v3/anytime/conversations/{id}/read` 
+ Parameters 
    - id: integer, conversation id 
+ Response 
    - http status code

### Mark a conversation as unread 
`put api/v3/anytime/conversations/{id}/unread` 
+ Parameters 
    - id: integer, conversation id 
+ Response 
    - http status code

### Delete a conversation 
`delete api/v3/anytime/conversations/{id}` 
- Parameters 
    - id: integer, conversation id
- Response 
    - http status code 

### Batch delete conversations 
`delete api/v3/anytime/conversations` 
+ Parameters 
    - ids: integer[], id array
+ Response 
    - http status code 

### List deleted conversations 
`get api/v3/anytime/deletedconversations/` 
- Parameters 
    - keywords: string
    - pageIndex: integer
    - timeFrom: DateTime, last reply time, default search the last 30 days
    - timeTo: DateTime, last reply time, defautl value is the current time
- Response 
    - deletedconversations: [conversation](#conversation) list 
    - total: integer, total number of conversations 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 
- Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v3/anytime/deletedconversations?include=agentAssignee` |
    | departmentAssignee | `get api/v3/anytime/deletedconversations?include=departmentAssignee` |
    | contact | `get api/v3/anytime/deletedconversations?include=contact` |
    | createdBy | `get api/v3/anytime/deletedconversations?include=createdBy` |
    | lastRepliedBy | `get api/v3/anytime/deletedconversations?include=lastRepliedBy` | 

### Get a deleted conversation 
`get api/v3/anytime/deletedconversations/{id}` 
- Parameters 
    - id: integer, conversation id 
- Response 
    - deletedconversation: [conversation](#conversation) 
- Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v3/anytime/deletedconversations/{id}?include=agentAssignee` |
    | departmentAssignee | `get api/v3/anytime/deletedconversations/{id}?include=departmentAssignee` |
    | contact | `get api/v3/anytime/deletedconversations/{id}?include=contact` |
    | createdBy | `get api/v3/anytime/deletedconversations/{id}?include=createdBy` |
    | lastRepliedBy | `get api/v3/anytime/deletedconversations/{id}?include=lastRepliedBy` |
    | messages | `get api/v3/anytime/deletedconversations/{id}?include=messages` |

### List messages of a deleted conversation
`get api/v3/anytime/deletedconversations/{id}/messages` 
- Parameters 
    - id: integer, conversation id
- Response 
    - messages: [message](#message) 
- Includes

    | Includes | Description |
    | - | - |
    | sender | `get api/v3/anytime/deletedconversations/{id}/messages?include=sender` |


### Restore a deleted conversation 
`post api/v3/anytime/deletedconversations/{id}/restore ` 
- Parameters 
    - id: integer, conversation id 
- Response 
    - deletedconversation: [conversation](#conversation)  

### Delete a conversation permanently 
`delete api/v3/anytime/deletedconversations/{id}` 
- Parameters 
    - id: integer, conversation id 
- Response 
    - http status code 

### Get a conversation draft 
`get api/v3/anytime/conversations/{id}/draft` 
- Parameters 
    - id: integer, conversation id 
- Response 
    - conversationDraft: [conversation draft](#conversation-draft) 

### Create a conversation draft 
`post api/v3/anytime/conversations/{id}/draft` 
- Parameters 
    - [conversation draft](#conversation-draft) 
- Response 
    - conversationDraft: [conversation draft](#conversation-draft) 

### Update a conversation draft 
`put api/v3/anytime/conversations/{id}/draft` 
- Parameters 
    - [conversation draft](#conversation-draft) 
- Response 
    - conversationDraft: [conversation draft](#conversation-draft) 

### Delete a conversation draft 
`delete api/v3/anytime/conversations/{id}/draft` 
- Parameters 
    - id: integer, conversation id 
- Response 
    - http status code 

### Merge a conversation 
`post api/v3/anytime/conversations/{id}/merge`
- Parameters 
    - id: integer, target conversation id, 
    - sourceId: integer, source conversation id 
- Response 
    - conversation: [conversation](#conversation) 

### List unread conversations number for filters 
`get api/v3/anytime/conversations/unreadCount?filterIds={filterid1}&filterIds={filterid2}&filterIds={filterid3}`
- Parameters 
    - filterIds: filter id array
- Response 
    - allCount: integer, all unread conversation number.
    - array including: 
        - filterId: integer, filter id 
        - unreadCount: integer, count unread conversations of a filter 
        - unreadMentionedCount: integer, the number of conversations which is unread and mentioned to me 


# Filters
## objects
### filter
| Name | Type | Description |
| - | - | - |
| `id` | integer | filter id |
| `name` | string | filter name |
| `isPrivate` | boolean | if private filter|
| `createdById` | integer | agent id |
| `conditions` | [condition](#condition)[] | array of filter condition |

### condition 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | condition id | 
| `fieldId` | integer | field id | 
| `matchType` | string | `contains`, `notContains`, `is`, `isNot`, `isMoreThan`, `isLessThan`, `before`, `after` | 
| `value` | string | condition value | 

## endpoints 
### List all public and private filters 
`get /api/v3/anytime/filters`
- Parameters 
    - no parameters 
- Response 
    - filters: [filter](#filter) list, without conditions

### Create a new filter 
`post api/v3/anytime/filters`
- Parameters 
    - name: string, filter name, required 
    - isPrivate: boolean, if private filter, default value: `false` 
    - conditions: [condition](#condition)[], array of filter condition
- Response 
    - filters: [filter](#filter) list 

### Get a filter and its conditions 
`get api/v3/anytime/filters/{id}` 
- Parameters 
    - id: integer, filter id 
- Response 
    - filter: [filter](#filter) 

### Update a filter 
`put api/v3/anytime/filters/{id}` 
- Parameters 
    - id: integer, filter id 
    - name: string, filter name, required 
    - isPrivate: boolean, if private filter 
    - conditions: [condition](#condition)[], array of filter condition
- Response 
    - filter: [filter](#filter) 

### Delete a filter 
`delete api/v3/anytime/filters/{id}` 
- Parameters 
    - id: integer, filter id 
- Response 
    - http status code 


# Field & Mappings
## objects 
### field 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | field id | 
| `type` | string | `text`, `textarea`, `email`, `url`, `date`, `integer`, `float`, `operator`, <br/>`radio`, `checkbox`, `dropdownList`, `checkboxList`, `link`, `department` | 
| `name` | integer | field name | 
| `isSystemField` | boolean | if is system field | 
| `isRequired` | boolean | value if is required | 
| `defaultValue` | string | field default value | 
| `helpText` | string | field help text | 
| `length` | integer | field value max length | 
| `options` | [field option](#fieldoption)[] | value option | 

### fieldOption 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | option id | 
| `name` | string | option name | 
| `value` | string | field value | 
| `order` | integer | option order | 

## endpoints 
### List all fields and their options 
`get api/v3/anytime/fields` 
- Parameters
    - no parameters
- Response 
    - fields: [field](#field) list 

<!-- # Attachments 
## objects 
### attachment 
| Name | Type | Description | 
| - | - | - | 
| `guid` | string | attachment unique id | 
| `fileName` | string | attachment file name| 
| `url` | string | attachment download link | 
| `isAvailable` | boolean | if the attachment is available | 
## endpoints 
### Upload attachment 
`post /api/v3/anytime/attachments` 
- Content-type
    - multipart/form-data
- Parameters 
    - file: file
- Response 
    - attachment: [attachment](#attachment) 
    
### Update status of attachment
`Put /api/v3/livechat/attachments/{guid}`
#### Parameters:
- isAvailable - boolean `true` or  `false`
#### Response
 - attachment: [attachment](#attachment) 

### Delete attachment 
`delete /api/v3/anytime/attachments/{guid}` 
- Parameters 
    - guid: string, the guid of the attachment
- Response 
    - httpStatusCode -->
  
# Routing Rules
## objects
### routing rule
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | routing rule id | 
| `isEnable` | boolean | is enable | 
| `type` | string | `simple` / `cusomterrules` | 
| `SimpleRouteToObject` | string |  | 
| `SimpleRouteToId` | integer |  | 
| `SimpleRouteToPriority` | string |  | 
| `matchFailedRouteToObject` | string |  | 
| `matchFailedRouteToId` | int |  | 
| `matchFailedRouteToPriority` | string |  | 
| [customerRouteRules](###customerRule) | object |  | 

### customerRule
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | customer rule id | 
| `isEnable` | boolean | is enable | 
| `name` | string | rouleName | 
| `when` | int | `Any=0` `All=1` `logicalExpresion=2` (tbd)| 
| `logicExpression` | string |  | 
| `routeToObject` | string |  | 
| `routeToId` | int |  | 
| `routeToPriority` | string |  | 
| `index` | int | | 
| [conditions](###conditions) | object |  | 

<!--### conditions 
 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer |  condition id | 
| `FieldId` | integer |  Field Id | 
| `ConditionMatchType` | integer |  `Is=1`,`IsNot=2`,`IsMoreThan=3`,`IsLessThan=4`,`Contain=5`,`NotContain=6`,`Before=7`,`After=8`,`Between=9`| 
| `value` | string |  value  | 
| `index` | int |  order index  |  -->

### condition
-需要统一定义一个 matchType的枚举？
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | condition id | 
| `fieldId` | integer | field id | 
| `matchType` | string | `contains`, `notContains`, `is`, `isNot`, `isMoreThan`, `isLessThan`, `before`, `after` | 
| `value` | string | condition value | 
| `index` | int |  order index  | 


## endpoints 
### List all routingrules 
`Get api/v3/anytime/Routingrules` 
- Response 
    - routingrules: [routingrule](#routingrule) list 

### Enable a routingrules 
`Put api/v3/anytime/Routingrules/{id}/enable` 
- Parameters
  - isEnable:boolean , enable or not,first time to enable will add a default routing rule
- Response 
    - routingrule: [routingrule](#routingrule)  

### Update One routingrule 
`Put api/v3/anytime/Routingrules/{id}` 
- Parameters 
    - `isEnable` , boolean, is Enable 
    - `type`,string , `simple` / `cusomterrules` 
    - `SimpleRouteToObject` , string
    - `SimpleRouteToId` , integer 
    - `SimpleRouteToPriority` , string 
    - `matchFailedRouteToObject` , string 
    - `matchFailedRouteToId` , int ,  
    - `matchFailedRouteToPriority` , string 
- Response 
    - routingrule: [routingrule](#routingrule) 

### List all customerRules of routingrule
`Get api/v3/anytime/Routingrules/{id}/customerrules` 

- Response 
     - customerRules: [routingrules](###customerRul) list

### add a customerRule  
`post api/v3/anytime/Routingrules/{id}/customerrules` 
- Parameters 
        -customerRule:[customerRule](###customerRule)
- Response 
    - customerRule:[customerRule](###customerRule)
### copy a customerRule  
`put api/v3/anytime/Routingrules/{id}/customerrules/{customerruleId}/copy` 
- Parameters 
- Response 
    - customerRules: [routingrules](###customerRul) list
  
### sort a customerRule  
`put api/v3/anytime/Routingrules/{id}/customerrules/{customerruleId}/sort` 
- Parameters
    - customerRules:[customerRules](###customerRule)
- Response 
     - customerRules: [routingrules](###customerRul) list

### delete a customerRule 
`delete api/v3/anytime/Routingrules/customerrules/{id}` 

- Response 
    - http status code

# Auto Allocation 
## objects 
### auto allocation
(to add)

# Triggers 
## objects 
### trigger
<!-- |[Triggers](#triggers)|/api/v3/anytime/triggers|| -->
| Name | Type | Description | 
| - | - | - | 
| `id` | int | trigger Id | 
| `name` | string | trigger name | 
| `description` | string |description | 
| `triggerEvent` | smallint |[Trigger Event Enum](###trigger-enum) | 
| `ifEnable` | boolen |if enable | 
| `orderId` | int | order id |  
| `ifSendEmailToContacts` | boolean | if sendEmail to contacts |  
| `ifShownInTicketCorresdence` | boolean | With this option selected, the trigger email will be displayed in your Conversation Correspondence thread. | 
| `stayStatus` | boolean | trigger event in(`staysAtStatusPeriod`,`statusIsChanged`) | 
| `stayTimeSpan` | boolean | trigger event=`staysAtStatusPeriod` | 
| [conditions](###trigger-conditions) | object |  | 
| [actions](###trigger-action) | object |  | 


### trigger event
修改为string？
| Name | value | Description |  
| - | - | - |  
| `conversationCreated` | 1 |When a conversation is created|
| `conversationReplyIsReceived` | 2 | When a conversation reply is received(7&2) | 
| `agentReplied` | 3 | When agent replied | 
| `assigneeIsChanged` | 4 | When the assignee of a conversation is changed|
| `statusIsChanged` | 5 | When the status of a conversation is changed  | 
| `staysAtStatusPeriod` | 6 | When a conversation stays at a status for a specified period of time | 


### trigger conditions
<!-- 为什么要公用一个表？ -->
| Name | value | Description |  
| - | - | - |  
|[condition](###condition)| object | |
<!-- | `triggerId` | int | trigger id |   -->
### trigger actions
| Name | Type | Description | 
| - | - | - | 
| `action type` | string | trigger name | 
| `subject` | string |action type=`SendEmail` |
| `htmlBody` | string |action type=`SendEmail` |
| `textBody` | string |action type=`SendEmail` |

### trigger action type
| Name | value | Description |  
| - | - | - |  
| `Set Value` | `SetValue` | |
| `Send Email ` | `SendEmail`  |  | 
| `To Conversation Contact` | `ToConversationContact` |  | 
| `To Agent` | `ToAgent` | |
| `Set Email Content` | `SetEmailContent` || 


## endpoints 
### List triggers 
`get /api/v3/anytime/triggers` 
- Parameters 
    - email: string, domain or email address 
- Response 
    - blockedSenders: [block sender](#blocked-sender) list 

### Add/update a trigger  
`put api/v3/anytime/triggers` 
- Parameters 
    - `email`, string, domain or email address 
    - `blockType`, string, `blockEmailasJunk`, `rejectEmail`, `blockDomainasJunk`, `rejectDomain`
- Response 
    - blockedSender: [block sender](#blocked-sender) 

### Remove a trigger  
`delete api/v3/anytime/triggers` 
- Parameters 
   - email: string, domain or email address 
- Response 
    - http status code


# Working Time & Holidays 
## objects 
### working time & holidays 
(to add)

# SLA Policies 
## objects 
### sLA policies
(to add)


# Blocked Senders
## objects 
### blocked sender 
| Name | Type | Description | 
| - | - | - | 
| `email` | string | email or domain | 
| `blockType` | string | `blockEmailasJunk`, `rejectEmail`, `blockDomainasJunk`, `rejectDomain` | 

## endpoints 
### List blocked senders 
`get /api/v3/anytime/blockedSenders` 
- Parameters 
    - email: string, domain or email address 
- Response 
    - blockedSenders: [block sender](#blocked-sender) list 

### Add/update a block sender 
`put api/v3/anytime/blockedSenders` 
- Parameters 
    - `email`, string, domain or email address 
    - `blockType`, string, `blockEmailasJunk`, `rejectEmail`, `blockDomainasJunk`, `rejectDomain`
- Response 
    - blockedSender: [block sender](#blocked-sender) 

### Remove a block sender 
`delete api/v3/anytime/blockedSenders` 
- Parameters 
   - email: string, domain or email address 
- Response 
    - http status code

<!-- # CannedResponses 
## objects 
### canned response 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `name` | string | canned response name | 
| `htmlContent` | string | html format content | 
| `textContent` | string | text format content | 


## endpoints 
### List all canned responses 
`get api/v3/anytime/cannedResponses` 
- Parameters 
    - no parameters
- Response 
    - cannedResponses: [Canned responses](#canned-response) list  -->


<!-- # Configs 
### Get site configs about conversation 
`get api/v3/anytime/configs` 
- Parameters 
    - no parameters
- Response 
    - configs
        - isEnabledDepartment: boolean 
        - recipientLimitPerEmail: integer  -->

<!-- # Department 
## objects 
### department 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `name` | string | department name | 
| `description` | string | department description | 
| `members` | [department member](#department-member)[] | department member array | 

### department member 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `name` | string | member name | 
| `type` | string | agent or group | 

## endpoints 
### Get a department 
`get api/v3/anytime/departments/{id} ` 
- Response 
    - department: [department](#department) 

### List all departments 
`get api/v3/anytime/departments` 
- Response 
    - departments: [department](#department) List without department member.  -->

# Email Accounts 
## objects 
### email account 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `email` | string | email address |  
| `type` | string | pop3 or exchange | 
| `agentAssigneeId` | integer | agent id | 
| `departmentAssigneeId` | integer | department id | 
| `isDefault` | boolean | if default email account | 


## endpoints 
### List all enabled email accounts 
`get api/v3/anytime/emailAccounts` 
- Response 
    - emailAccounts: [email account](#email-account) list 

# IntegrationAccount 
## objects 
### integration account
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `appId` | string | App Id |  
| `accountName` | string | AccountName | 
| `accountOriginalId` | string | Account Original Id | 
| `isEnable` | boolean | if enable | 


## endpoints 
### List all enabled email accounts 
`get api/v3/anytime/integrationAccounts` 
- Response 
    - integrationAccounts: [integration account](#integration-account) list 
    - 

# Junk Emails 
## objects 
### junk email 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `subject` | string | email subject | 
| `time` | datetime | received time | 
| `name` | string | sender name | 
| `from` | string | email from email address | 
| `to` | string | to email addresses | 
| `cc` | string | cc email addresses | 
| `htmlBody` | string | html body | 
| `plainBody` | string | plain text body | 
| `emailAccountId` | integer | receive email account id | 
| `isRead` | boolean | if is read | 
| `attachments` | [attachment](#attachment)[] | attachments | 

## endpoints 
### List junk emails
`get api/v3/anytime/junkEmails` 

- Parameters 
    - keywords: string
    - pageIndex: integer
    - timeFrom: DateTime
    - timeTo: DateTime
- Response 
    - junkEmails: [junk email](#junk-email) list 
    - total: integer
    - previousPage: string, next page uri, the first page return null
    - nextPage: string, the last page return null
    - currentPage: string

### Get a junk email 
`get api/v3/anytime/junkEmails/{id}` 
- Parameters 
    - id: integer, email id 
- Response 
    - junkEmail: [junk email](#junk-email) 

### Update a junk email 
`put api/v3/anytime/junkEmails/{id}` 
- Parameters 
    - isRead: boolean, 
- Response 
    - junkEmail: [junk email](#junk-email) 

### Restore a junk email to a normal conversation 
`post api/v3/anytime/junkEmails/{id}/notJunk` 
- Parameters 
    - id: integer, email id 
- Response 
    - conversation: [conversation](#conversation) 

### Delete a junk email 
`delete api/v3/anytime/junkEmails/{id}` 
- Parameters 
    - id: integer, junk email id 
- Response 
    - http status code 
<!-- 
# Tags 
## objects 
### tag 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `name` | string | tag name | 
| `conversationCount` | integer | the number of conversations with the tag | 
## endpoints 
### List all tags 
`Get api/v3/anytime/tags` 
- Response 
    - tags: [tag](#tag) list 

### Add a tag 
`Post api/v3/anytime/tags` 
- Parameters 
    - name: string, tag name 
- Response 
    - tag: [tag](#tag) 

### Update One Tag 
`Put api/v3/anytime/tags/{id}` 
- Parameters 
    - id: integer, tag id 
    - name: string, tag name 
- Response 
    - tag: [tag](#tag) 

### Delete a tag 
`Delete api/v3/anytime/tags/{id}` 
- Parameters 
    - id: integer, tag id 
- Response 
    - http status code -->


# Right Now Reports
## objects
### right Now reports
(add)

# Volume Reports
## objects
### volume report
(add)

# Channel Reports
## objects
### channel report
(add)

# Efficiency Reports
## objects
### efficiency report
(add)

# Portalconversation
## objects
### portal conversation
| Name | Type | Description |
| - | - | - |
| `id` | integer | id of conversation |
| `subject` | string | subject |
| `contactId` | integer | id of the contact who submitted the portal conversation |
| `isClosed` | boolean | if the portal conversation is closed |
| `isReaByContact` | boolean | if the portal conversation is read by contact |
| `customFields` | [custom field value](#custom-field-value)[] | custom field value array |
| `createdTime` | datetime | create time |
| `closedTime` | datetime | close time |

### portal conversation message 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id of message | 
| `htmlBody` | string | html body | 
| `plainBody` | string | plain text body | 
| `senderId`| integer | id of agent or contact | 
| `senderType`| string | `agent` or `contact` or `system` | 
| `time` | datetime | |   
| `attachments` | [attachment](#attachment)[] | attachment array| 

## endpoints
### Get a portal conversation by id
`get api/v3/anytime/portalconversations/{id}`
- Parameters
    - id, integer, portal conversation id
    - contactId, integer
- Response
    - portalconversation: [portal conversation](#portal-conversation) 
- Includes

    |Includes| Description |
    | - | - |
    | contact | `get api/v3/anytime/portalconversations/{id}?include=contact` | 
    | messages | `get api/v3/anytime/portalconversations/{id}?include=messages` |
 
### List portal conversations
`get api/v3/anytime/portalconversations`
- Parameters:
    - contactId, integer, required
- Response: 
    - portalconversations: [portal conversation](#portal-conversation) list
- Includes

    |Includes| Description |
    | - | - |
    | contact | `get api/v3/anytime/portalconversations?include=contact` |  

### Submit a portal conversation
`post api/v3/anytime/portalconversations`
- Parameters: 
    - subject: string, subject, required
    - contactId: integer, id of the contact who submitted the portal conversation
    - customFields: [custom field value](#custom-field-value)[], custom field value array
    - message:  the first portal message
        - htmlBody: string, html body
        - plainBody: string, plain text
        - attachments: [attachment](#attachment)[], attachment array of message
- Response: 
  - portalconversation: [portal conversation](#portal-conversation) 

### Close a portalconversation
`put api/v3/anytime/portalconversations/{id}/close` 
- Parameters: 
    - id, integer, portal conversation id,
    - contactId, integer, required
- Response: 
    - portalconversation: [portal conversation](#portal-conversation) 

### Reopen a portalconversation
`put api/v3/anytime/portalconversations/{id}/reopen` 
- Parameters: 
    - id, integer, portal conversation id,
    - contactId, integer, required
- Response: 
    - portalconversation: [portal conversation](#portal-conversation) 

### List messages of a portal conversation 
`get api/v3/anytime/portalconversations/{id}/messages`
- Parameters: 
    - id, integer
    - contactId, integer
- Response: 
    - messages: [portal conversation message](#portal-conversation-message) list
- Includes

    |Includes| Description |
    | - | - |
    | sender| `get api/v3/anytime/portalconversations/{id}/messages?include=sender` | 

### Reply a portal conversation
 `post api/v3/anytime/portalconversations/{id}/messages`
- Parameters:
    - id: integer
    - contactId: integer required
    - htmlBody: string, html body
    - plainBody: string, plain text
    - attachments: [attachment](#attachment)[], attachment array
- Response: 
    - message: [portal conversation message](#portal-conversation-message)

### Contact mark a portal conversation as read
`put api/v3/anytime/portalconversations/{id}/read`
- Parameters 
    - id: integer, conversation id 
- Response 
    - http status code

### Contact mark a portal conversation as unread
`put api/v3/anytime/portalconversations/{id}/unread`
- Parameters 
    - id: integer, conversation id 
- Response 
    - http status code