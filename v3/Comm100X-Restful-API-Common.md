# Comm100 API
# Resource List - Common
|Name|EndPoint|Note| 
|---|---|---| 
|[Contact](#contact)| /api/v3/contacts|| 
|[Contact identities](#contact)| /api/v3/contacts/{Id}/identities|| 
|[Visitor SSO Settings](#visitor-sso-settings)|/api/v3/visitorSSO | 从realtime转移到公共模块| 
|[Agent](#agent)| /api/v3/agents||
|[Site](#site)| /api/v3/sites|Account & Billing|
|[Tag](#tag)|/api/v3/tags|从anytime转移过来，realtime暂不实现| 
|[Canned Messages](#agent)|/api/v3/cannedMessages|从anytime和realtime合并并转移到公共模块|
|[Canned Message Categorys](#canned-message-categorys)|/api/v3/cannedMessageCategories|从anytime和realtime移交,需要合并数据|
|[Departments](#departments)|/api/v3/departments|从anytime和realtime合并并转移到公共模块|
|[Attachments](#attachments)|/api/v3/attachments|从anytime和realtime合并并转移到公共模块|
|[Credit Card Masking](#credit-card-masking)|/api/v3/creditcardMasking||
|[Password Policy](#password-policy)|/api/v3/passwordPolicys||
|[Audit Log](#audit-log)|/api/v3/auditLogs||
|[Agent Single Sign-On](#agent-single-sign-on)|/api/v3/agentSingleSignOn||
|[Agent reports ](#agent-reports)|/api/v3/reports/agentReports |Availability/Canned Message  |
|[webhooks](#webhooks)|/api/v3/webhooks/ |  |

# Contact
## Objects
### Contact
| Name | Type | Description |
| - | - | - |
| `id` | integer | id of contact |
| `name` | string |  the name of the contact |
| `alias` | string |  the alias name of the contact |
| `identities` | [identity](#identity)[] | identity array  |
| `description` | string | a small description of the contact |
| `company` | string | the primary company name which this contact belongs to |
| `title` | string | the title of the contact|
| `phoneNumber` | string | telephone number of the contact|
| `faxNumber` | string | fax number of the contact |
| `address` | string | the address of the contact  |
| `city` | string | the city of the contact  |
| `stateOrProvince` | string | the state or province of the contact |
| `country` | string |  the country of the contact |
| `postalOrZipCode` | string | the postal or zip code of the contact  |
| `createdTime` | datetime | the time the contact was created |
  
### Identity
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | the id of identity |
| `type` | string | `email`, `SSOUserId`, `externalId` |
| `value` | string | the value of one identity, should be unique |

- Note: We currently only allow one for each type.

### Endpoints
#### Get a contact by contact id
`get  /api/v2/account/contacts/{id}`
- Parameters
    - id: integer, id of the contact
- Response
    - [contact object](#contact)

#### Create a contact
`post  /api/v2/account/contacts`
- Parameters 

| Name | Type | Description |
| - | - | - |
| `name` | string |  the name of the contact, required |
| `alias` | string |  the alias name of the contact |
| `identities` | [identity](#identity)[] | the array of identities |
| `description` | string | a small description of the contact |
| `company` | string | the primary company name which this contact belongs to |
| `title` | string | the title of the contact |
| `phoneNumber` | string | telephone number of the contact|
| `faxNumber` | string | fax number of the contact |
| `address` | string | the address of the contact  |
| `city` | string | the city of the contact  |
| `stateOrProvince` | string | the state or province of the contact |
| `country` | string |  the country of the contact |
| `postalOrZipCode` | string | the postal or zip Code of the contact  |

- Response
    - contact: [contact object](#contact)

#### Search contacts
- Max 50 contacts are responded for each request.
- `get  /api/v2/account/contacts`
- Parameters
    - pageIndex, integer, default 1
    - keywords, string, search scope includes: name/identity value/alias 
- Response
    - contacts: [contact object](#contact) list
    - total: int, total number of contacts.
    - previousPage: string, next page uri, the first page return null.
    - nextPage: string, the last page return null.
    - currentPage: string, current page uri.
- Example
    - `get  /api/v2/account/contacts?keywords=comm100`
- Note
    - Deleted contact will not be included in the results.
    - The query must be URL encoded.
    - Fuzzy search is supported.

#### Update a contact
`put  /api/v2/account/contacts/{id}`
- Parameters

| Name | Type | Description |
| - | - | - |
| `name` | string |  the name of the contact |
| `alias` | string |  the alias name of the contact |
| `description` | string | a small description of the contact |
| `company` | string | the primary company name which this contact belongs to|
| `title` | string | the title of the contact|
| `phoneNumber` | string | telephone number of the contact|
| `faxNumber` | string | fax number of the contact |
| `address` | string | the address of the contact  |
| `city` | string | the city of the contact  |
| `stateOrProvince` | string | the state or province of the contact |
| `country` | string |  the country of the contact |
| `postalOrZipCode` | string | the postal or zip code of the contact |
| `identities` | [identity](#identity)[] | the array of identities |
- Response
    - contact: [contact object](#contact)

#### Delete a contact
 `delete  /api/v2/account/contacts/{id}`
- Parameters
    - id: integer, id of the contact
- Response
    - http status code and message

#### Add identity
`post  /api/v2/account/contacts/{contactId}/identities`
- Parameters
    - contactId: integer, contact id
    - type: string, identity type
    - value: string, identity value
- Response
    - identity: [identity object](#identity)

#### Update identity
`put  /api/v2/account/contacts/{contactId}/identities/{id}`
- Parameters
    - contactId, integer, contact id
    - id, integer, contact identity id
    - value, string, the value of the identity
- Response
    - identity: [identity object](#identity)

#### Delete identity
 `delete  /api/v2/account/contacts/{contactId}/identities/{id}`
- Parameters
    - contactId, integer, contact id
    - id, integer, contact identity id
- Response
    - Http status code and message

# Agent
[Reference document](https://www.comm100.com/doc/api/introduction.htm#/Account?id=agent-json-format)
