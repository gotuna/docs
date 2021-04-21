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
weight: 700
toc: true
---

Middlewares are small pieces of code that take one request, do something with it,
and pass it down to another middleware or the final handler.
Some common use-cases for middleware are request logging or header manipulation.
All middlewares have the common `MiddlewareFunc` signature.

## Attaching middlewares

Middlewares are attached to routes using ```app.Router.Use()```

```
app.Router = gotuna.NewMuxRouter()

app.Router.Handle("/", handlerHome(app))
app.Router.Use(app.Logging())
app.Router.Use(myCustomMiddleware())
```

## Built-in middlewares
The framework comes with several built-in middlewares:

- Authenticate
- RedirectIfAuthenticated
- Cors
- Logging
- Recoverer
- StoreToContext

### Authenticate
This middleware can be used to guard routes from non-authenticated users.
You must provide a destination for guests to be redirected.

### RedirectIfAuthenticated
The exact opposite of Authenticate middleware. Authenticated users will
be redirected to the provided destination.

### Cors
Cross-Origin Resource Sharing middleware will respond to OPTIONS requests 
with appropriate headers and 204 status.

### Logging
Log every requests to the app's Logger.

### Recoverer
This middleware is used to recover the app from panics, to log the incident,
and to redirect user to the error page.

### StoreToContext
This middleware will add common values to the request context for further use. 

The request context will be used to store:
- current authenticated user object.
- all of the parameters for the current request, including query parameters, 
 route parameters, and submitted form values.

Getting the user object:
```
raw, _ := gotuna.GetUserFromContext(r.Context())
user := raw.(gotuna.InMemoryUser)
```

Getting route parameters or form values:
```
color := gotuna.GetParam(r.Context(), "color")
```

