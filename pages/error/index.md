---
title: Error
---

# {% $markdoc.frontmatter.title %}  [\#](#api-error) {% #api-error %}


### Error codes [\#](#error-codes) {% #error-codes %}
### 401
Sent in case of missing or wrong API key.

All requests except POST /v1/register.

### 400
Sent when the validation fails for the related response.

All POST and PATCH request.

Validation message are sent back under `message` key as an array of strings.

```json
{
    "statusCode": 400,
    "message": [
        "appName should not be empty",
        "appName must be a string"
    ],
    "error": "Bad Request",
    "timestamp": "2022-09-05T07:00:36.244Z"
}
```
