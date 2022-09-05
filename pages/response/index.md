---
title: Response
---

# {% $markdoc.frontmatter.title %}  [\#](#api-response) {% #api-response %}

The api response is formatted to be a specific standard accross all the REST API.

## Response format [\#](#response-format) {% #response-format %}

### Parameters
---
**data**  *Object|Array*   
The data key contains the actual response for the API request. For get events requesting more than one data, the value of `data` is an Array with the corresponding data, for everything else, `data` represents an instance of the corresponding data.
---

**meta** *Object*  
Meta contains additional information regarding the response. Generally used for pagination data.
---

```json
	{
		"data": [
			{ .... },
			{ .... }
		],
		"meta": {
			"total": 2
		}
	}
```

## Response status codes [\#](#reponse-status-codes) {% #reponse-status-codes %}

### 200  
Denotes that the api response is successfully executed.

- GET /v1/events
- GET /v1/event/:event-id
- POST /v1/event/:event-id/pause
- PATCH /v1/event/:event-id/pause
- POST /v1/event/:event-id/resume
- DELETE /v1/event/:event-id/resume

### 201
Sent in the case of new resource creation

- POST /v1/register
- POST /v1/events

