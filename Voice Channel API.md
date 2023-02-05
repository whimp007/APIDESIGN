  | Change Version | API Version | Change nots | Change Date | Author |Architect Team Reviewer | 
  | - | - | - | - | - |- |
  | 1.0 | v1 |Comm100 Voice API | 2023-02-03 | Leon|  Allon|
 
  ## Summary

### Voice Channel Service API

### Voice Channel Adapter API 

The channelURL can be any valid URL that implements this API, and it is configured in the system when a new channel needs to access. 
  - POST /{channelURL} - [Voice Channel Adapter receives input](#voice-channel-adapter-receives-input). 

#### Voice Channel API
 - POST /voicechannel/calls - [create a call session](#create-a-call-session). 
 - POST /voicechannel/calls/{id}/inputs - [Receives a call input](#receives-a-call-input). 
 - POST /voicechannel/calls/{id}/status - [Update the call status](#update-the-call-status).  

#### Agent Console Voice App API
 - POST /voicechannel/agents/{agentid}/tokens -[Create a Client Token ](#create-a-client-token).
 - POST /voicechannel/agents/{agentid}/status - [Update agent status](#update-agent-status).
 - POST /voicechannel/agents/{agentid}/heartbeats - [Notify server agent heartbeat](#Notify-server-agent-heartbeat).
 - POST /voicechannel/calls/{id}:transfer - [Transfer new call](#transfer-new-call). 
 - POST /voicechannel/calls/{id}:onhold - [On hold call](#on-hold-call). 
 - POST /voicechannel/calls/{id}:resume - [Resume Call](#resume-call).
 

## Endpoints

## Voice Channel Adapter API 
The ChannelURL can be any valid URL that implements this API, and it is configured in the system when a new channel needs to access.  

### Voice Channel Adapter Receives Input
`POST /{channelURL}`


#### Parameters
Request body
Request body the request body contains data with the following structure: 
Request body is Voice Message Object 
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `callId` | string | yes | |  channelIdentifier .  |
  | `content`  |  [VoiceAction](#voiceaction-object)[]  |yes |   |  |
  
#### example:
```Json 
  {     
          "callId":"d3f5b968-ad51-42af-b759-64c0afc40b84", 
          "content": [{ 
	        "type":"playAudioAction",
		"content":{ 
			"type":"voice",
                        "voice":"string", 
    	        	"voiceConfig":{ 
                 		 "encoding": "LINEAR16" , 
                  		"sampleRateHertz": 8000, 
              		 },
			 
                }
         }] 
} 
```

### Create A Call Session
`POST /voicechannel/calls`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `callSid` | string | yes | call resource sid |  
  | `from` |string |yes| call in phonenumber or clientid|
  | `to` |string |yes| call in phonenumber or clientid|
  #### Response
The Response body contains data with the following 
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `callId` | string | yes | call Id |  
  |`content`  |  [VoiceAction](#voiceaction-object)[]  |   |

```Json 
  HTTP/1.1 200 OK
  Content-Type:  application/json 
  {     
          "callId":"d3f5b968-ad51-42af-b759-64c0afc40b84", 
          "content":[{ 
              "type":"playText", 
              "content":{ 
                    "message": "Hi there! I'm a Voice, here to help answer your questions.", 
              } 
            }] 
  } 

HTTP/1.1 400 OK
{
   "error": "Timeout",
   "message":"",
}
```
### Receives a call input
`POST /voicechannel/calls/{id}/inputs`

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
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`content`  |  [VoiceAction](#voiceaction-object)[]  |   |

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

### Update the call status
`POST /voicechannel/calls/{id}/status`

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

### Create a client token
`POST /voicechannel/agents/{agentid}/tokens`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `agentid` | string | yes | agent id |    
  
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
` POST /voicechannel/calls/{id}:transfer`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | string | yes | twilio call resource sid |  
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
`POST /voicechannel/calls/{id}:onhold`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | string | yes | twilio call resource sid |  
  
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
`POST /voicechannel/calls/{id}:resume`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | string | yes | twilio call resource sid | 
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
`POST /voicechannel/agents/{agentid}/status`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `agentid` | string | yes | agent id |    
  | `status` |enum ([AgentStatus](#Agent-Status))  |yes | agent status |
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
### Notify server agent heartbeat
`POST /voicechannel/agents/{agentid}/heartbeats`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - |   
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


# Model
## VoiceAction Object 
  |Name| Type| Default | Description     | 
  | - | - | :-: | - | 
  | `type` | string | | type of the response,including `PlayAudio`,`PlayText`,`IVRMenu`,`RouteCall`,`EndCall`,`StartRecording`,`StopRecording`|
  | `content` | object | |  response's content. when type is `PlayAudio`, it represents [PlayAudio](#playaudio-object); when type is `PlayText`,it represents [PlayText](#playtext-object);when type is `EndCall`, it represents [EndCall](#endcall-object);when type is `IVRMenu`, it represents [IVRMenu](#ivrmenu-object);when type is `TransferCall`, it represents [RouteCall](#routecall-object);when type is `StartRecording`, it represents [StartRecording](#startrecording-object);when type is `StopRecording`, it represents [StopRecording](#stoprecording-object);|
  
## PlayAudio Object  
  Text Response is represented as simple flat json objects with the following keys: 
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `audioPath` | String  | | String |
  
## PlayText Object  
  Text Response is represented as simple flat json objects with the following keys: 
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `message` | String  | | String |
  | `delayTime` | int  | | second |

## RouteCall Object   
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `type` | String  | | Type of the route,including `AnyOnlineAgent`,`Department`,`PhoneNumber`.  |
  | `routeTo` | Guid  | | Support Department , Agent or Voice Bot.  |
  | `routeToPhoneNumber` | String  | | Support Phone Number and Sip URI.  |


## EndCall Object   
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  
## StartRecording Object   
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  
## StopRecording Object   
  |Name| Type | Default | Description | 
  | - | - | :-: | - |   
  
## IVRMenu Object   
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `message` | String  | | String  |
  | `greetingType ` | String  | | Type of the greeting,including `PlayMessage`,`PlayAudio`.  |
  | `audioPath ` | String  | | Thr url of audio.  |


### Transfer To Type
Transfer to type.
|Enums| | 
| - | - | 
|`phonenumber` | 	phonenumber. | 
|`client` | software phone client. | 

### Agent Status
Agent Status
|Enums| | 
| - | - | 
|`offline` | 	Any status can be switched to this status. When the agent closes his browser or loses Twilio connection, the agent status switches to this status. | 
|`available` | When the agent's status is in call and completed a call, or when the agent's status is unavailable and manually set to available.  | 
|`unavailable` | When the agent first connected to Twilio or manually set to unavailable status.  | 
|`inCall` | when the agent is available, the agent dials or picks up.   | 

### callStatus Response
  Call status.
  |Enums| | 
  | - | - | 
  |`queued` | Comm100's carriers has received your request to create the call. All new calls are created with a status of queued. | 
  |`initiated` | Comm100's carriers has dialed the call. | 
  |`ringing` | The destination number has started ringing. | 
  |`in-progress` | The call has been connected, and the connection is currently active. | 
  |`completed` | The connected call has now been disconnected. Completed calls will remain in this state in going forward. | 
  |`busy` | Comm100's carriers dialed the number, but received a busy response. | 
  |`no-answer` | Comm100's carriers dialed the number but no one answered before the timeout parameter value elapsed. This can be configured for each call, but by default is set to 60 seconds on outbound API calls, and 30 seconds on outbound <Dial> calls. |
  |`canceled` | Prior to being answered, an outbound call was canceled via an HTTP POST request to the REST API, or an incoming call was disconnected by the calling party |
  |`failed` | Comm100's carriers could not connect the call. Possible causes include the destination is unreachable, or the number may have been input incorrectly. |

  


