---
title: Pagination
---

# {% $markdoc.frontmatter.title %}  [\#](#api-pagination) {% #api-pagination %}

Pagination parameters are optional and are supported by the bulk fetch API.

#### **Attributes**
---
**limit** *number*
The maximum number of filtered responses for the request.
---
**offset** *number*  
The number of items to skip.
---

```js
// GET /v1/events
const req = await axios.get('{SCHEDULER_API}/v1/events', {
	params: {
		limit: 10,
		offset: 0
	}
})
```

### Response  

Pagination details are sent back with the response in the `meta` key. Normally contains the total number of items from the request.

```json
{
	"data": [
		{},
		.....
	],
	"meta": {
		"total": 100
	}

}
```
