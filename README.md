# HTTP API client

Primary designed for [dc-api-core](https://github.com/DimaCrafter/dc-api-core).

[![NPM](https://nodei.co/npm/dc-api-client.png)](https://npmjs.com/package/dc-api-client)

## Import dc-api-client to your project

```js
// Use Node.JS require
const API = require('dc-api-client');
// OR use import for ES6+/TS
import API from 'dc-api-client'
```

## Make request to backend

```js
const res = await API.Controller.action({
    // Any supported data types.
    // It can be any JSON-compitable type, such as strings, objects etc.
    // Also you can use any Blob/File value.
});
```

* `Controller` - your controller name
* `action` - your action name in controller
* `res` - Response object

## Response object

| Name    | Type      | Note                                                                          |
|---------|-----------|-------------------------------------------------------------------------------|
| success | `Boolean` | `true` if response code is `200`                                              |
| code    | `Number`  | HTTP response code                                                            |
| msg     | `any`     | If backend returned JSON, it will be parsed, otherwise contains raw response. |

## Example

Sending user data to `register` method in `Auth` controller.

```js
const API = require('dc-api-client');
const res = await API.Auth.register({
    email: 'test@best.mail',
    password: '123123'
});
console.log(res);
```

Example console output:

```js
{
    success: true,
    code: 200,
    msg: {
        status: 'user_registred',
        id: 42
    }
}
```

## Settings

Settings strored in `API.settings` object.

| Field  | Type       | Note                                                               |
|--------|------------|--------------------------------------------------------------------|
| secure | `Boolean`  | `true` - use HTTPS / `false` - use HTTP                            |
| base   | `String`   | Hostname with prefix path                                          |
| dev    | `Function` | Enables or disables dev mode as setter, returns `Bolean` as getter |

### Settings example #1

```js
const API = require('dc-api-client');

API.settings.secure = true;
API.settings.base = 'yourdomain.com:8080/api';
```

Where `yourdomain.com` is your domain or IP address.

### Settings example #2

```js
import API from 'dc-api-client';

API.settings.secure = false;
API.settings.base = `${location.hostname}:8080/api`;
```

## Socket controller

WebSockets connection will be initialized when controller used for first time.

WS URI: `ws[s]://<settings.base>/socket`

```js
API.Socket: {
    // Calling <event> method in Socket controller on back-end
    emit (event: String, ...arguments: any[]);

    // Sets trigger for <event> reply
    on (event: String, listener: (...arguments: any[]) => {});

    // Sets one time trigger for <event> reply
    once (event: String, listener: (...arguments: any[]) => {});

    // Removing trigger for <event>
    off (event: String, listener: (...arguments: any[]) => {});
};
```

## Tokens

Token will be delivered to backend with `token` header.
You can override token from backend by sending `token` header,
you don't should send this header with all requests.

Current token stores in localStorage. Read with `localStorage.getItem('token')`
or write `localStorage.getItem('token', 'new-token')`.

## POST utility

Sends POST request in browser with form.

```js
API.post(url: String, data: Object, newtab: Boolean = false);
```

## Alternative sending

```js
API.send(controller: String, action: String, data: any, callback: Response => void);
```

## Vanilla fallback

Compatibility:

| Internet Explorer | Edge | Firefox | Chrome | Safari | Opera |
|:-----------------:|:----:|:-------:|:------:|:------:|:-----:|
| **Not supported** | 12+  |   22+   |  49+   |  10+   |  36+  |
