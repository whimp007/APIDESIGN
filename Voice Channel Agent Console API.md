
  | Change Version | API Version | Change nots | Change Date | Author |Architect Team Reviewer | 
  | - | - | - | - | - |- |
  | 1.0 | v1 |Comm100 Voice API | 2023-02-05 | Leon|  Allon|
 
  ## Summary

### Voice Channel Agent Console API
 <!-- - POST /voicechannel/agents/{agentid}/tokens -[Create a Client Token ](#create-a-client-token). -->
 - POST /voicechannel/agents/{agentid}/status - [Update agent status](#update-agent-status).
 - POST /voicechannel/agents/{agentid}/heartbeats - [Notify server agent heartbeat](#Notify-server-agent-heartbeat).
 - POST /voicechannel/calls/{id}:transfer - [Transfer a call](#transfer-a-call). 
 - POST /voicechannel/calls/{id}:hold - [hold the call](#hold-the-call). 
 - POST /voicechannel/calls/{id}:resume - [Resume the Call](#resume-the-call).

## Endpoints
<!-- ### Create a client token
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
``` -->
### Transfer a call
` POST /voicechannel/calls/{id}:transfer`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `callid` | string | yes | call id |  
  | `type` |string |yes|  type of the response,including `phonenumber`,`client`,`department`|
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

### Hold the call
`POST /voicechannel/calls/{id}:hold`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `callId` | string | yes | call Id |  
  
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
### Resume the call
`POST /voicechannel/calls/{id}:resume`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `callId` | string | yes |call Id | 
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

## Model

### Agent Status

|Enums| | 
| - | - | 
|`offline` | 	Any status can be switched to this status. When the agent closes his browser or loses connection, the agent status switches to this status. | 
|`online` | When the agent connects to the voice provider server ,when the agent's status is in call and completed a call, or when the agent's status is away and manually set to online.  | 
|`away` | When the agent manually set to away status.  | 
|`inCall` | when the agent is online, the agent dials or picks up.   | 
