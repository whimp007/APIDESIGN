  | Change Version | API Version | Change nots | Change Date | Author |Architect Team Reviewer | 
  | - | - | - | - | - |- |
  | 1.0 | v1 |Comm100 Voice API | 2023-01-03 | Leon|  Allon|
 
  ## Summary

### Voice Twilio Service API

#### Twilio API
 - POST /voicetwilio/voice - [Twilio voice income callback URL](#twilio-voice-income-callback-url). 
 - POST /voicetwilio/statuscallback - [Twilio voice status callback URL](#twilio-voice-status-callback-url). 
 - POST /voicetwilio/gather - [Gather call user input callback URL](#gather-call-user-input-callback-url).  

#### Agent Console API
 - Post /voicetwilio/tokens -[Create a Client Token ](#create-a-client-token).
 - POST /voicetwilio/calls/{id}:transfer - [Transfer new call](#transfer-new-call). 
 - POST /voicetwilio/calls/{id}:onhold - [On hold call](#on-hold-call). 
 - POST /voicetwilio/calls/{id}:resume - [Resume Call](#resume-call).
 - POST /voicetwilio/agent/{agentid}/status - [Update agent status](#update-agent-status).

### Voice Config Service API

#### Phone Number API
 - GET /voiceconfig/phonenumbers- [Get the list of Twilio Phone Numbers](#get-the-list-of-twilio-phone-numbers).


#### Voice Config API
 - GET /voiceconfig/configs- [Get the list of Voice Configs](#get-the-list-of-voice-config).
 - POST /voiceconfig/configs- [Create a Voice Config](#create-a-voice-config).
 - PUT /voiceconfig/configs/{id} - [Update the Voice Config](#update-the-voice-config). 
 - DELETE /voiceconfig/configs/{id} - [Delete the Voice Config](#delete-the-voice-config). 

#### Phone Line API
 - GET /voiceconfig/phonelines- [Get the list of Twilio Phone Lines](#get-the-list-of-twilio-phone-numbers).
 - POST /voiceconfig/phonelines- [Create a Twilio Phone Line](#create-a-agent-2fa-config).
 - PUT /voiceconfig/phonelines/{id} - [Update the Twilio Phone Line](#update-the-twilio-phone-line). 
 - DELETE /voiceconfig/phonelines/{id} - [Delete the Twilio Phone Line](#delete-the-twilio-phone-line). 
  
#### Voice Agent API
 - GET /voiceconfig/voiceagents- [Get the list of Voice Agents](#get-the-list-of-voice-agents).
 - POST /voiceconfig/voiceagents- [Create a Voice Agent](#create-a-voice-agent).
 - PUT /voiceconfig/voiceagents/{id} - [Update the Voice Agent](#update-the-voice-agent). 
 - DELETE /voiceconfig/voiceagents/{id} - [Delete the Voice Agent](#delete-the-voice-agent). 

#### Voice Call API
 - GET /voiceconfig/calls- [Get the list of calls](#get-the-list-of-calls).
 - POST /voiceconfig/calls- [Create a call](#create-a-call).
 - PUT /voiceconfig/calls/{id} - [Update the call](#update-the-call). 
 - DELETE /voiceconfig/calls/{id} - [Delete the call](#delete-the-call). 

#### Voice CallLog API
 - GET /voiceconfig/calllogs- [Get the list of calllogs](#get-the-list-of-calllogs).
 - POST /voiceconfig/calllogs- [Create a calllog](#create-a-calllogs).
 - PUT /voiceconfig/calllogs/{id} - [Update the calllog](#update-the-calllog). 
 - DELETE /voiceconfig/calllogs/{id} - [Delete the calllog](#delete-the-calllog). 

## Endpoints
### Create a client token sample
`GET /voicetwilio/token`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `preLoginToken` | string | yes | 2fa jwt token containing agent id,login time |  
  |`secretKey` |string |no| 2FA secret key,when setup secretkey |
  | `code` | string | yes |  2FA code or backup code|  
  | `isSetCookie` | bool | yes | set token to cookie |  
  | `isSKip2fa` | bool | yes |if `true` Don't ask again on this computer for 14 days | 
  #### Response
The Response body contains data with the following 
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`error` |string |no| `Code verify failed`,`Time out`,`Number of retries exceeded`，`Account locked` |
|`message` |string |no| |
| `Verifiedtoken` | string | no |Verified token containing `2fa verified time`|
|`jwtToken` | string | no |  jwt token for logining | 
|`retryRemainingNumber` | string | no | 2FA code retry Remaining Number | 
```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
   "Verifiedtoken":"sdfasdf3452tsdfsd4werrtewr"
   "jwtToken":"sdfasdf3452t4werrtewr"
}

HTTP/1.1 400 OK
{
   "error": "Timeout",
   "message":"",
}
```



### Twilio voice income callback url
`POST /voicetwilio/voice`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `callSid` | string | yes | call resource sid |  
  | `from` |string |yes| call in phonenumber or clientid|  
  #### Response
The Response body contains data with the following 
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`data` | string | no | TwiMLResult that twilio deal | 
```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
   "data":"<?xml version="1.0" encoding="UTF-8"?>
          <Response>
            <Say voice="alice">hello world!</Say>
          </Response>"   
}

HTTP/1.1 400 OK
{
   "error": "Timeout",
   "message":"",
}
```
## Endpoints
### Twilio voice status callback url
`POST /voicetwilio/statuscallback`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `callSid` | string | yes | call resource sid |  
  | `from` |string |yes| call in phonenumber or clientid|
  | `to` |string |yes| call to phonenumber or clientid|
  | `callStatus` |enum ([callStatus](#callStatus-Response))|yes| call status|
  #### Response
The Response body contains data with the following 
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`data` | string | no | TwiMLResult that twilio deal |  
```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
   "data":"<?xml version="1.0" encoding="UTF-8"?>
          <Response>
            <Say voice="alice">hello world!</Say>
          </Response>"   
}

HTTP/1.1 400 OK
{
   "error": "Timeout",
   "message":"",
}
```

### Gather call user input callback url
`POST /voicetwilio/gather`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `callSid` | string | yes | call resource sid |  
  | `from` |string |yes| call in phonenumber or clientid|
  | `to` |string |yes| call to phonenumber or clientid|
  | `callStatus` |enum ([callStatus](#callStatus-Response))|yes| call status|
  | `speechResult` |string)|yes| speech result|
  | `digits` |string|yes| dtmf result|
  | `finishedOnKey` |string|yes| dtmf finished key|
  #### Response
The Response body contains data with the following 
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`data` | string | no | TwiMLResult that twilio deal |  
```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
   "data":"<?xml version="1.0" encoding="UTF-8"?>
          <Response>
            <Say voice="alice">hello world!</Say>
          </Response>"   
}

HTTP/1.1 400 OK
{
   "error": "Timeout",
   "message":"",
}
```
### Create a client token
`GET /voicetwilio/token`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  
  #### Response
The Response body contains data with the following 
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`token` | string | yes |  jwt token for voice client register | 
 
```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCIsImN0eSI6InR3aWxpby1mcGE7dj0xIn0.eyJpc3MiOiJTSzliZjJjMzVmNGYzNDg1OTZmZjVmYWI4NjNjNWY5YTBlIiwiZXhwIjoxNjcyODA2MjQ2LCJqdGkiOiJTSzliZjJjMzVmNGYzNDg1OTZmZjVmYWI4NjNjNWY5YTBlLTE2NzI4MDI2NDYiLCJzdWIiOiJBQ2ZjZjRhYWIwZDA5ZmExM2Q0ZjM4Y2JhN2YxNDBkZjg1IiwiZ3JhbnRzIjp7ImlkZW50aXR5Ijoic3VwcG9ydF9hZ2VudCIsInZvaWNlIjp7ImluY29taW5nIjp7ImFsbG93Ijp0cnVlfSwib3V0Z29pbmciOnsiYXBwbGljYXRpb25fc2lkIjoiQVA1MGViNTJiNmViMzYxMWVhNzA5Mzk1ZWFlMWQwZDcyYiJ9fX19.seHbLAY6PA8wkiXlfXFwpXsMMvsiL1tcl7T8E6V6yeM"
}

HTTP/1.1 400 OK
{
   "error": "Timeout",
   "message":"",
}
```
### Transfer new call
` POST /voicetwilio/calls/{id}:transfer`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `callSid` | string | yes | call resource sid |  
  | `type` |enum ([TransferToType](#Transfer-To-Type)) |yes| phonenumber,client|
  | `to` |string |yes| transfer to distination|  
  #### Response
The Response body contains data with the following 
  | Name  | Type | Required  | Description |     
  | - | - | - | - |   
```Json 
  HTTP/1.1 200 OK
  

HTTP/1.1 400 OK
{
   "error": "Timeout",
   "message":"",
}
```
### On hold call
`POST /voicetwilio/calls/{id}:onhold`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `callSid` | string | yes | call resource sid |  
  
  #### Response
The Response body contains data with the following 
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  
```Json 
  HTTP/1.1 200 OK 

HTTP/1.1 400 OK
{
   "error": "Timeout",
   "message":"",
}
```
### Resume call
`POST /voicetwilio/calls/{id}:resume`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `callSid` | string | yes | call resource sid | 
  #### Response
The Response body contains data with the following 
  | Name  | Type | Required  | Description |     
  | - | - | - | - |  
```Json 
  HTTP/1.1 200 OK  

HTTP/1.1 400 OK
{
   "error": "Timeout",
   "message":"",
}
```
### Update agent status
`POST /voicetwilio/agent/{agentid}/status`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `agentid` | string | yes | agent id |    
  | `available` |bool |yes | is agent available to call |
  #### Response
The Response body contains data with the following 
  | Name  | Type | Required  | Description |     
  | - | - | - | - |   
```Json 
  HTTP/1.1 200 OK
 

HTTP/1.1 400 OK
{
   "error": "Timeout",
   "message":"",
}
```

### Get the list of twilio phone numbers
`GET /voiceconfig/phonenumbers`

#### Parameters
Path parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `CountryCode` | String | yes  |   |

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Description |     
  | - | - | - | 
  | `phoneNumberList`  | [PhoneNumber](#PhoneNumber-object)[] | - | 
  
  ### PhoneNumber Object
|Name| Type|  Default |  Description     |
| - | - | :-: |  - | 
  | `phoneNumber` | String ||sample: 604) xx0-8183  |  
  | `countryCode` | String | |two-letter country codes which are also used to create the ISO 3166-2 country subdivision codes and the Internet country code top-level domains.  |  

#### Example
Response
```Json
  HTTP/1.1 200 OK
  Content-Type:  application/json
{
  "phoneNumberList":[
    {
      "phoneNumber": "604) xx0-8183",    
      "CountryCode": "US",    
    }
  ]
}
```

# Model
### Transfer To Type
Transfer to type.
|Enums| | 
| - | - | 
|`phonenumber` | 	phonenumber. | 
|`client` | software phone client. | 

### callStatus Response
  Call status.
  |Enums| | 
  | - | - | 
  |`queued` | 	Twilio has received your request to create the call. All new calls are created with a status of queued. | 
  |`initiated` | Twilio has dialed the call. | 
  |`ringing` | The destination number has started ringing. | 
  |`in-progress` | The call has been connected, and the connection is currently active. | 
  |`completed` | The connected call has now been disconnected. Completed calls will remain in this state in going forward. | 
  |`busy` | Twilio dialed the number, but received a busy response. | 
  |`no-answer` | Twilio dialed the number but no one answered before the timeout parameter value elapsed. This can be configured for each call, but by default is set to 60 seconds on outbound API calls, and 30 seconds on outbound <Dial> calls. |
  |`canceled` | Prior to being answered, an outbound call was canceled via an HTTP POST request to the REST API, or an incoming call was disconnected by the calling party |
  |`failed` | Twilio's carriers could not connect the call. Possible causes include the destination is unreachable, or the number may have been input incorrectly. |

  


