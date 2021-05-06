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
All middlewares have the common `MiddlewareFunc` signature.

## Attaching middlewares

Middlewares are attached to routes with ```app.Router.Use()```

```
app.Router.Handle("/", handlerHome(app))
app.Router.Use(app.Logging())
app.Router.Use(myCustomMiddleware())
```

## Built-in middlewares
The framework comes with several built-in middlewares:

- Authenticate
- RedirectIfAuthenticated
- StoreParamsToContext
- StoreUserToContext (required for some features)
- Cors
- Logging
- Recoverer (recommended)

### Authenticate
This middleware can be used to guard routes from non-authenticated users.
You must provide a destination for guests to be redirected.

```
// attach middleware, redirect guests to /login route
app.Router.Use(app.Authenticate("/login"))
```

### RedirectIfAuthenticated
The exact opposite of Authenticate middleware. Authenticated users will
be redirected to the provided destination.

```
app.Router.Use(app.RedirectIfAuthenticated("/"))
```

### StoreParamsToContext
This middleware will add all parameters from the current request to the context, 
this includes query, form, and route params.

```
// attach middleware
app.Router.Use(app.StoreParamsToContext())

...

// use it in handlers, e.g. http://example.com/buycar?color=red
color := gotuna.GetParam(r.Context(), "color")
```

### StoreUserToContext
This middleware will add the current logged in  user object (if any) to the 
request context for further use.

```
// attach middleware
app.Router.Use(app.StoreUserToContext())

...

// use it in handlers
raw, _ := gotuna.GetUserFromContext(r.Context())
user := raw.(gotuna.InMemoryUser)
```

### Cors
Cross-Origin Resource Sharing middleware will respond to OPTIONS requests 
with appropriate headers and 204 status.

### Logging
Log every requests to the app's Logger.

### Recoverer
This middleware is used to recover the app from panics, to log the incident,
and to redirect user to the error page.

