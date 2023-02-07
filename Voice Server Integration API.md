
  | Change Version | API Version | Change nots | Change Date | Author |Architect Team Reviewer | 
  | - | - | - | - | - |- |
  | 1.0 | v1 |Comm100 Voice API | 2023-02-05 | Leon|  Allon|
 
  ## Summary
![image.png](/.attachments/image-31dbb66c-6473-49fe-a53e-68dc2d9254f4.png)

### Voice Channel API
 - POST /voicechannel/calls - [create a call session](#create-a-call-session). 
 - POST /voicechannel/calls/{id}/inputs - [Receives a call input](#receives-a-call-input). 
 - POST /voicechannel/calls/{id}/status - [Update the call status](#update-the-call-status).  
 
### Voice Channel Adapter API 

The adapterURL can be any valid URL that implements this API, and it is configured in the system when a new voice Channel needs to access. 
  - POST /{adapterURL} - [Voice Channel Adapter receives input](#voice-channel-adapter-receives-input). 

## Endpoints

### Create A Call Session
`POST /voicechannel/calls`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `callSid` | string | yes | call origin sid |  
  | `contact` |string |yes| contact phonenumber |
  | `phoneLine` |string |yes| call in phonenumber |
  | `agent` |string |no| AgentName|
  | `type` |string |Yes| `Inbound`,`Outbound`|
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
  | `digits` |string|yes| dtmf result|
  | `finishedOnKey` |string|no| dtmf finished key|
  #### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`content`  |  [VoiceAction](#voiceaction-object)[]  |   |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
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

### Update the call status
`POST /voicechannel/calls/{id}/status`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `callStatus` |enum ([CallStatus](#callstatus))|yes| call status|
  | `recordingUrl` |string |yes| |

#### Response
The Response body contains data with the following 
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`content`  |  [VoiceAction](#voiceaction-object)[]  |   |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
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
## Voice Channel Adapter API 
The AdapterURL can be any valid URL that implements this API, and it is configured in the system when a new channel needs to access.  

### Voice Channel Adapter Receives Input
`POST /{adapterURL}`


#### Parameters
Request body
Request body the request body contains data with the following structure: 
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `callId` | string | yes | |  call Id.  |
  | `content`  |  [VoiceAction](#voiceaction-object)[]  |yes |   |  |
  
#### example:
```Json 
  HTTP/1.1 200 OK
  Content-Type:  application/json 
  {     
    "callId":"d3f5b968-ad51-42af-b759-64c0afc40b84", 
    "content": [{ 
    "type":"playText",
    "content":{ 
    "message": "Hi there! I'm a Voice, here to help answer your questions."			 
        }
    }] 
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
  | `routeTo` | String| | Support Department , Agent or Voice Bot.  |
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
  | `audioPath ` | String  | | The url of audio.  |

### Agent Status
Agent Status
|Enums| | 
| - | - | 
|`offline` | 	Any status can be switched to this status. When the agent closes his browser or loses connection, the agent status switches to this status. | 
|`online` | When the agent connects to the voice provider server ,when the agent's status is in call and completed a call, or when the agent's status is away and manually set to online.  | 
|`away` | When the agent manually set to away status.  | 
|`inCall` | when the agent is online, the agent dials or picks up.   | 

### CallStatus
  Call status.
  |Enums| | 
  | - | - | 
  |`initiated` | Comm100's carriers has dialed the call. | 
  |`ringing` | The destination number has started ringing. | 
  |`in-progress` | The call has been connected, and the connection is currently active. | 
  |`completed` | The connected call has now been disconnected. Completed calls will remain in this state in going forward. | 
  |`busy` | Comm100's carriers dialed the number, but received a busy response. | 
  |`no-answer` | Comm100's carriers dialed the number but no one answered before the timeout parameter value elapsed. This can be configured for each call, but by default is set to 60 seconds on outbound API calls, and 30 seconds on outbound <Dial> calls. |
  |`canceled` | Prior to being answered, an outbound call was canceled via an HTTP POST request to the REST API, or an incoming call was disconnected by the calling party |
  |`failed` | Comm100's carriers could not connect the call. Possible causes include the destination is unreachable, or the number may have been input incorrectly. |

  


