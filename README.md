# Trading-Platform Document

## **Authentication & Authorization**

HTTP Header:

- exp-authorization: {JWT_token}
	
JWT payload 内容如下
	
```javascript
{
	"uid": 2,			// user id
	"iss": "ex",		// issuer (system)
	"exp": 1517148660,	// JWT token expiry time
	"group": "group1"	// user group
}
```
	
- JWT token 在用户 login/signup 的时候获取


## Websocket API
[Websocket API document](websocket-api.doc)

## REST API

[REST API document](rest-api.doc)