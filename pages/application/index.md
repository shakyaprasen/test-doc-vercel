---
title: Application
---

# {% $markdoc.frontmatter.title %}  [\#](#application-title) {% #application-title %}

Application object represents the details about the application that has been registered. An application should be registered for the API key which is then used in the subsequent requests.


## Endpoints {% #application-endpoints %}

- POST [/v1/register](#register-application)

## The Application Object [\#](#application-object) {% #application-object %}

#### **Attributes**
---
**appName** *string*  
Unique identifier for the registered application. It is linked with the API key
---
**apiKey** *string*  
A unique key issues for the application. It is used as a way of authenticating other requests.
---  


## Register and application [\#](#register-application) {% #register-application %}

### Parameters
---
**appName**  *string* `REQUIRED`  
String identifying the registered application. 
---

```js
// POST v1/register
const req = await axios.post('api.scheduler.com/v1/register', {
	appName: "RLP"
})
```

**Returns**  
---
Returns an Application object.
---
