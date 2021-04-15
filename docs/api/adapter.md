---
title: "@remix-run/{adapter}"
---

Idiomatic Remix apps can be deployed anywhere because Remix adapt's the server's request/response to the [Web Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API). It does this through adapters. We maintain a few adapters:

- `@remix-run/express`
- `@remix-run/architect`
- `@remix-run/vercel`

We will be adding a few more eventually:

- `@remix-run/cf-workers`
- `@remix-run/netlify`

These adapters are imported into your server's entry and is not used inside of your Remix app itself.

Each adapter has the same API.

## `createRequestHandler`

Creates a request handler for your server to serve the app. This is the ultimate entry point of your Remix application.

```ts
const { createRequestHandler } = require("@remix-run/{adapter}");
createRequestHandler({ build, getLoadContext });
```

Here's a full example with express:

```ts [2, 9-20]
const express = require("express");
const { createRequestHandler } = require("@remix-run/express");

let app = express();

// needs to handle all verbs (GET, POST, etc.)
app.all(
  "*",
  createRequestHandler({
    // `remix build` and `remix run` output files to a build directory, you need
    // to pass that build to the request handler
    build: require("./build"),

    // return anything you want here to be available as `context` in your
    // loaders and actions. This is where you can bridge the gap between Remix
    // and your server
    getLoadContext(req, res) {
      return {};
    }
  })
);
```

Here's an example with Architect (AWS).

```ts
const { createRequestHandler } = require("@remix-run/architect");
exports.handler = createRequestHandler({ build: require("./build") });
```

You don't really interact much with this API thanks to our starter repos that set all this boilerplate up for you.

## Starter Repos

We maintain a few starter repos to get you deploying production apps quickly. The README in each starter has deployment instructions. We maintain the following:

- [Express](https://github.com/remix-run/starter-express)
- [Architect (AWS Lambda)](https://github.com/remix-run/starter-vercel)
- [Vercel](https://github.com/remix-run/starter-vercel)

We will be adding the following soon:

- Fly.io
- Cloudflare Workers
- Netlify