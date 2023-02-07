
## Summary
### Voice Config Service API

#### Voice Flow API 
  - POST /voiceconfig/phoneLines/{phoneLineId}/voiceflows  - [Create A Call Voice Flow](#create-a-call-voice-flow)
  - POST /voiceconfig/calls/{callId}/messages - [Call receive A Message](#Call-receive-a-message). 

#### Phone Number API
 - GET /voiceconfig/phonenumbers- [Get the list of Phone Numbers](#get-the-list-of-phone-numbers).

The following APIs are  autocoding APIs.

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
  
#### Voice Action API
 - GET /voiceconfig/voiceactions- [Get the list of Voice Actions](#get-the-list-of-voice-actions).
 - POST /voiceconfig/voiceactions- [Create a Voice Action](#create-a-voice-action).
 - PUT /voiceconfig/voiceactions/{id} - [Update the Voice Action](#update-the-voice-action). 
 - DELETE /voiceconfig/voiceactions/{id} - [Delete the Voice Action](#delete-the-voice-action). 

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

### Create A Call Voice Flow  
`POST /voiceconfig/phoneLines/{phoneLineId}/voiceflows`

#### Parameters
Path parameters 
  | Name | Type | Required | Description |    
  | - | - | :-: | - | 
  | `phoneLineId` | Guid | yes |  |

Request body 
The request body contains data with the follow structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `visitor` | [Visitor](#visitor-object) Object | No | |  Visitor information.   |
  | `variables`  |  [Variable](#variable-object)[]   | No  |   | Variables    |


#### example:
```Json 
  { 
    "visitor": { 
        "phone":"123-4355-212", 
      }, 
    "variables": [{ 
        "name":"string", 
        "value":"string"
    }] 
  } 
```

#### Response
The Response body contains data with the following structure:

  | Name | Type |  Description |    
  | - | - | :-: | 
  |`callId` | Guid | the unique id of the session   |
  |`content`  |  [VoiceAction](#voiceaction-object)[]  |   |

Response
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
```



### Voice receive a message  
`POST /voiceconfig/calls/{callid}/messages`

#### Parameters
Path parameters 
  | Name | Type | Required | Description |    
  | - | - | :-: | - | 
  | `callId` | Guid | yes | Call id of the voice conversation  |

Request body 
The request body contains data with the follow structure:  
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  | `textInput` | String  | Yes | |  Text input to phone line    |
  | `isTransferFailed`  |  Bool   | No  |   | If the call Transfer Chat to agent failed    |

#### example:
```Json 
  { 
    "textInput":"I want to buy NBN", 
    "isTransferFailed": false, 
  }  
```

#### Response
the response is: VoiceOutput Object 

Response
```Json
  HTTP/1.1 200 OK 
  Content-Type:application/json 
  {     
    "id": "f9928d68-92e6-4487-a2e8-8234fc9d1f48", 
    "content": [ 
          { 
            "type":"playText", 
            "content": { 
                "message":"Hi, what can I do for you?", 
            } 
          } 
        ] 
  } 
```
### Get the list of phone numbers
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

## Visitor Object   
  |Name| Type | Default | Description | 
  | - | - | :-: | - | 
  | `name` | String  | | Name of the visitor  |
  | `callerID` | String  | | Phone Number of the visitor  |
  | `state` | String  | | State/province of the visitor  |
  | `country` | String  | | Country/region of the visitor  |
  | `city` | String  | | City of the visitor  |

