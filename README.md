# Trading-Platform Document

## **Authentication & Authorization**

HTTP Header:

- exp-authorization: {JWT_token}
	
JWT payload
	
```javascript
{
	"uid": 2,		// user id
	"iss": "ex",		// issuer (system)
	"exp": 1517148660,	// JWT token expiry time
	"group": "group1"	// user group
}
```
	
- Get JWT token
You can find JWT token from login/signup response


## Websocket API
[Websocket API document](websocket-api.md)

## REST API

[REST API document](rest-api.md)
