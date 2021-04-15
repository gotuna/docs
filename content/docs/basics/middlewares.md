---
title: "Middlewares"
description: "Middlewares"
lead: ""
date: 2020-11-12T15:22:20+01:00
lastmod: 2020-11-12T15:22:20+01:00
draft: false
images: []
menu: 
  docs:
    parent: "basics"
weight: 720
toc: true
---

Middlewares are small pieces of code which take one request, do something with it,
and pass it down to another middleware or the final handler.
Some common use cases for middleware are request logging or header manipulation.
All middlewares have the common gorilla signature `mux.MiddlewareFunc`

## Attaching middlewares

Middlewares can be added to a router using ```app.Router.Use()```

### Example
```
app.Router = mux.NewRouter()

app.Router.Handle("/", handlerHome(app))
app.Router.Use(app.Logging())
app.Router.Use(myCustomMiddleware())
```

## Built-in middlewares
The framework comes with the several built-in middlewares:
- Authenticate
- RedirectIfAuthenticated
- Cors
- Logging
- Recoverer
- StoreUserToContext
