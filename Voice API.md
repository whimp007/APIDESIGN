  | Change Version | API Version | Change nots | Change Date | Author |Architect Team Reviewer | 
  | - | - | - | - | - |- |
  | 1.0 | v1 |Comm100 Voice API | 2023-01-03 | Leon|  Allon|
 
  ## Summary

### Voice Twilio Service API
 -GET /voicetwilio/token -[Create a Client Token ](#create-a-client-token).
 
 -POST /voicetwilio/voice - [Twilio Voice Callback URL](#twilio-voice-callback-url). 
 
 -POST /voicetwilio/statuscallback - [Twilio Voice Status Callback URL](#twilio-voice-status-callback-url). 
 
 -POST /voicetwilio/fallback - [Twilio Voice Fallback URL](#twilio-voice-fallback-url). 
 

### Voice Config Service API

### Phone Number API
 - GET /voiceconfig/phonenumbers- [Get the list of Twilio Phone Numbers](#get-the-list-of-twilio-phone-numbers).

### Phone Line API
 - GET /voiceconfig/phonelines- [Get the list of Twilio Phone Lines](#get-the-list-of-twilio-phone-numbers).
 - POST /voiceconfig/phonelines- [Create a Twilio Phone Line](#create-a-agent-2fa-config).
 - UPDATE /voiceconfig/phonelines/{Id} - [Update the Twilio Phone Line](#update-the-agent-2fa-config). 
 - DELETE /voiceconfig/phonelines/{Id} - [Delete the Twilio Phone Line](#delete-the-2fa-config-of-agent). 


## Endpoints

### Verify the 2FA Code of Agent
`POST /global/endlogin`

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
