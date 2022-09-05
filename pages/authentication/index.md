---
title: Authentication
---

# {% $markdoc.frontmatter.title %}  [\#](#api-authentication) {% #api-authentication %}

## Authentication strategy
Scheduler microservice uses API keys to authenticate requests. API keys are issued once when registering your application with the service

Your API keys carry many privileges, so be sure to keep them secure! Do not share your secret API keys in publicly accessible areas such as GitHub, client-side code, and so forth.

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

Use your API key by setting it in the initial configuration of axios or other http client. You can also set a per-request key. This is often useful for Connect applications that use multiple API keys during the lifetime of a process.

### X-API-Key

The API key needs to be added to the header `X-API-Key` for each request. If failed to do so, it will result in a `401` response. You can set the header at the configuration stage so that all the request will include it as a default.

```js
axios.defaults.headers.common['X-API-Key'] = 'Your API Key';

const req = await axios.get('api.scheduler.com/v1/event/:event-id')
```


