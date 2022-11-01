---
title: Event
---

# {% $markdoc.frontmatter.title %}  [\#](#event-title) {% #event-title %}

Event objects represents the details about the event that has been scheduled. You can use this data to store details about the event or use it to later make changes to the event itself.


## Endpoints {% #event-endpoints %}

- POST [/v1/events](#post-event)
- GET [/v1/events](#get-events)
- GET [/v1/event/:event-id](#get-event)
- PATCH [/v1/events/:event-id](#patch-event)
- DELETE [/v1/events/:event-id](#delete-event)
- POST [/v1/events/:event-id/pause](#pause-event)
- POST [/v1/events/:event-id/resume](#resume-event)


## The Event Object [\#](#event-object) {% #event-object %}

#### **Attributes**
---
**eventId** *string*  
Unique identifier for the object
---
**status** *enum*
The status of the event. It denotes in what stage the event is currently in.

Possible enum values

- `INACTIVE`  
The callback has not been executed even once yet. This may be because the stipulated time for running the callback has not been reached or it is in the job queue. 

- `ACTIVE`  
The callback has been called at least once and may have been queued again for future execution.

- `PAUSED`  
The event is paused causing no further callbacks to be executed unless resumed.

- `FINISHED`  
The end date has passed. No further callbacks will be executed.
---
**createdDate** *date*  
The createdDate of the event. 
---
**eventOwner** *email*  
String identifying the event creator. usually the user requesting the event creation. Can be used for forwarding logs in case of callback errors.
---
**endDate** *date*  
Date in UTC time. It is the ending date for the event. The callback never executes past the end date. End date should be in the future.
---
**interval** *string*  
Unix cron format. e.g. (* 5 * * *). Interval specifies when and on what interval the callback should execute. Must be valid cron expression parsable by [cron-parser](https://github.com/harrisiirak/cron-parser)
---
**callbackUrl** *url*  
The url which is called when the job is run. The url is run with a POST request. The data from `contextObject` is sent in the request body. 
---
**context** *object*  
The context with which the `callbackUrl` is called. This can be any relevant data that is needed with the callback. e.g. 
```js
{
	"user_id": 330
}
```
---
**tags** *string[]*  
List of strings for tagging the event. Useful for filterting the event through the API.
---



## Create an Event [\#](#post-event) {% #post-event %}
Creates an Event to be run at specific intervals.


#### **Parameters**
---
**eventOwner**  *email* `REQUIRED`  
String identifying the event creator. usually the user requesting the event creation. Can be used for forwarding logs in case of callback errors.
---
**endDate** *date* `REQUIRED`  
Date in UTC time. It is the ending date for the event. The callback never executes past the end date. End date should be in the future.
---
**interval** *string* `REQUIRED`  
Unix cron format. e.g. (* 5 * * *). Interval specifies when and on what interval the callback should execute. Must be valid cron expression parsable by [cron-parser](https://github.com/harrisiirak/cron-parser)
---
**callbackUrl** *url* `REQUIRED`  
The url which is called when the job is run. The url is run with a POST request. The data from `contextObject` is sent in the request body. 
---
**context** *object*   
The context with which the `callbackUrl` is called. This can be any relevant data that is needed with the callback. e.g. 
```js
{
	"user_id": 330
}
```
---
**tags** *string[]* 
List of strings for tagging the event. Useful for filterting the event through the API.
---

```js
// POST v1/events
const req = await axios.post('{SCHEDULER_API}/v1/events', {
  "eventOwner": "user@example.com",
  "endDate": "2023-09-02T05:11:59.708Z",
  "interval": "* * * 1 * *",
  "callbackUrl": "https://api.yourapi.com",
  "context": {
    "user": 12345
  },
  "tags": [
    "prod"
  ]
})
```


**Returns**  
---
Returns an Event object.
---
## Get list of Events [\#](#get-events) {% #get-events %}
Returns a list of Events associated with the current application.  
Takes in url parameters to filter the records.


#### **Parameters**
---
**limit** *number*
The maximum number of filtered responses for the request.
---
**offset** *number*  
The number of items to skip.
---
**query** *QueryEvent*  
Filters the events according to `QueryEvent` type.  
Contains the following:
---
- **eventOwner**  *Array<string|email>*   
String identifying the event creator. usually the user requesting the event creation. Can be used for forwarding logs in case of callback errors.
---
- **tags** *string[]*   
List of strings for tagging the event. Useful for filterting the event through the API.
---
- **context** *ContextRequest[]*   
Filter event by context. The Array needs to be `ContextRequest` type e.g.   
--
```js
interface ContextRequest {
	path: Array<string>;
	value: string;
	isArray?: boolean;
	type: 'Number' | 'String'
}
```
e.g.
```js
[{
	"path": [
		"user"
	],
	"value": "12345",
	"type": "Number",
	"isArray": false
}]
```
---

```js
// GET /v1/events
const req = await axios.get('{SCHEDULER_API}/v1/events', {
	params: {
		query: {
			"eventOwner": ["user@example.com"],
			"tags": ["prod"],
			"context": [{
				"path": [
					"user"
				],
				"value": "12345",
				"type": "Number",
				"isArray": false
			}]
		},
		limit: 10,
		offset: 0
	}
})
```


**Returns**  
---
Returns a list of Event object and meta object with the total events for the request.
---
## Get Event details [\#](#get-event) {% #get-event %}
Gets the detail for a specific event based on it's eventId.

```js
// GET /v1/event/:event-id
const req = await axios.get('{SCHEDULER_API}/v1/event/:event-id')
```

**Returns**  
---
Returns an Event object.
---
## Update an Event [\#](#patch-event) {% #patch-event %}
Updates an Event object.

#### **Parameters**
---
**endDate** *date*   
Date in UTC time. It is the ending date for the event. The callback never executes past the end date. End date should be in the future.
---
**interval** *string*   
Unix cron format. e.g. (* 5 * * *). Interval specifies when and on what interval the callback should execute. Must be valid cron expression parsable by [cron-parser](https://github.com/harrisiirak/cron-parser)
---
**callbackUrl** *url* `REQUIRED`  
The url which is called when the job is run. The url is run with a POST request. The data from `contextObject` is sent in the request body. 
---
**context** *object*   
The context with which the `callbackUrl` is called. This can be any relevant data that is needed with the callback. e.g. 
```js
{
	"user_id": 330
}
```
---
**tags** *string[]* 
List of strings for tagging the event. Useful for filterting the event through the API.
---

```js
// POST v1/events
const req = await axios.patch('{SCHEDULER_API}/v1/events', {
  "endDate": "2023-09-02T05:11:59.708Z",
  "interval": "* * * 1 * *",
  "callbackUrl": "https://api.yourapi.com",
  "context": {
    "user": 12345
  },
  "tags": [
    "prod"
  ]
})
```

**Returns**  
---
Returns the updated Event object.
---

## Delete an Event [\#](#delete-event) {% #delete-event %}
Deletes the evnt based on it's eventId. The callback will not be executed after deletion.

```js
// GET /v1/event/:event-id
const req = await axios.delete('{SCHEDULER_API}/v1/event/:event-id')
```

**Returns**  
---
Returns the deleted Event object.
---

## Pause an Event [\#](#pause-event) {% #pause-event %}
Pauses event based on it's eventId. The callback will not be paused after pausing the event.

```js
// GET /v1/event/:event-id
const req = await axios.post('{SCHEDULER_API}/v1/event/:event-id/pause')
```

**Returns**  
---
Returns the paused Event object.
---
## Resume an Event [\#](#resume-event) {% #resume-event %}

Resumes an already paused event based on it's eventId. The callback will be resumed after resuming the event.

```js
// GET /v1/event/:event-id
const req = await axios.post('{SCHEDULER_API}/v1/event/:event-id/resume')
```

**Returns**  
---
Returns the resumed Event object.
---
