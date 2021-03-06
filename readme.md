<h1 align="center">Koa Access</h1>

<p align="center">
  <i>
    A <b><a href="http://koajs.com">Koa</a></b> middleware for
    creating <b>consistent</b>, <b>per-request</b> access logs for your requests.
  </i>
</p>

<p align="center">
  <b><a href="#overview">Overview</a></b>
  |
  <b><a href="#usage">Usage</a></b>
  |
  <b><a href="#api">Api</a></b>
  |
  <b><a href="#contributors">Contributors</a></b>
</p>


[![Contributors](https://img.shields.io/badge/contributors-2-orange.svg?style=for-the-badge)](#contributors)
[![License](https://img.shields.io/github/license/mashape/apistatus.svg?style=for-the-badge)]()
![type](https://img.shields.io/badge/⚡-library-c45366.svg?style=for-the-badge)
![language](https://img.shields.io/badge/❤-Node-da776c.svg?style=for-the-badge)
[![test](https://img.shields.io/badge/🔬-Jest-e9a279.svg?style=for-the-badge)](https://facebook.github.io/jest/)
[![style](https://img.shields.io/badge/🎨-Standard-e4ca93.svg?style=for-the-badge)](https://standardjs.com)

## Overview

This package is a [**Koa**](http://koajs.com/) middleware inspired by
[**Morgan**](https://github.com/expressjs/morgan), an **access log**
middleware for **Express**.

It tries to address the following;

* 🖌 **Custom formatting** - Allow you to inject your own formatting
* 💰 **Custom tokens** - Allow you to add your own extra logging tokens
* 📓 **JSON tokens** - All of this is handled using JSON rather than strings

All of this allows you to create **consistent** access logs decoupled
from the actual implementation of your code.

---

### Usage

```js
import Koa from 'koa'
import access from 'koa-access'

const app = new Koa()

app.use(access())           /* Default configuration */
app.use(access([ 'id' ]))   /* Add `id` from `ctx.state` to access log */

app.use(access(readContext)) /* Read extra poperties by calling `readContext` on `ctx` */

app.on('koa-access:access', Logger.log)
```

This package uses [**Event
Emitters**](https://nodejs.org/api/events.html) to decouple the
handling of logging from the implementation of your code.

### API

By default, the `koa-access` will bundle the following properties into
an object and fire them on the the `koa-access:access` event.

```js
{
  "res": {
    "responseTime": 23    // Response time in `ms`
    "length": 23232       // Content length of the response
    "status": 200         // Response status
    "statusBucket": "2XX" // Status as a bucket string
    "time": "2017-..."    // Timestamp of the response
  },
  "req": {
    "method": "GET"       // Method of request
    "path": "/foo/bar"    // Path being accessed
    "time": "2017-..."    // Timestamp of request start
    "host": "127.0.0.1"   // Host of the request
  }
}
```

The `koa-access` can be **configured** with _extra_ parameters in one
of two ways,

#### Array - `access([ 'id', 'errors' ])`

This will add the `id` and `errors` properties from the **Koa**
`ctx.state` object onto the access log object.

#### Function - `access((ctx) => ({ id: ctx.state.id, errors: ctx.state.errors }))`

This will return an object by calling the function on **Koa's** `ctx`
object, in this example, it'll just grab the `id` and `errors`
properties from the state.

#### Events

Once a request access log has been built, the following event is fired
with the access object

`koa-access:access => (ctx, { req, res, ...extras })`

The event can be imported from the `koa-access` module, as

```js
import { eventAccess } from 'koa-access
```

## Contributors

Thanks goes to these wonderful people ([emoji key](https://github.com/kentcdodds/all-contributors#emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
| [<img src="https://avatars1.githubusercontent.com/u/5881414?v=4" width="100px;"/><br /><sub>Dom Charlesworth</sub>](http://domcharlesworth.co.uk)<br />[📖](https://github.com/uswitch/koa-access/commits?author=domtronn "Documentation") [💻](https://github.com/uswitch/koa-access/commits?author=domtronn "Code") [🤔](#ideas-domtronn "Ideas, Planning, & Feedback") [🔌](#plugin-domtronn "Plugin/utility libraries") | [<img src="https://avatars3.githubusercontent.com/u/1567681?v=4" width="100px;"/><br /><sub>David Annez</sub>](http://davidannez.com)<br />[💻](https://github.com/uswitch/koa-access/commits?author=annez "Code") [🤔](#ideas-annez "Ideas, Planning, & Feedback") [🔌](#plugin-annez "Plugin/utility libraries") |
| :---: | :---: |
<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/kentcdodds/all-contributors) specification. Contributions of any kind welcome!
