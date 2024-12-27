---
title: "Introduction"
description: "GoTuna - Web framework for Go"
lead: "GoTuna is a lightweight web framework for Go with mux router, middlewares, user sessions, templates, embedded views, and static file server."
date: 2020-10-06T08:48:57+00:00
lastmod: 2020-10-06T08:48:57+00:00
draft: false
images: []
menu:
  docs:
    parent: "getstarted"
weight: 100
toc: true
---

## Main Features
- Router (gorilla)
- Standard `http.Handler` interface
- Middleware support
- User session management (gorilla)
- Session flash messages
- Native view rendering (html/template) with helpers
- Static file server included with the configurable prefix
- Standard logger interface
- Request logging and panic recovery
- Full support for embedded templates and static files
- User authentication (via user provider interface)
- Sample InMemory user provider included
- Multi-language support
- Database agnostic

## Requirements
- Make sure you have Go >= 1.23 installed

## Testing
```
go test -race -v ./...
```

## External dependencies
External modules are mostly used when the feature is too complex to build or maintain - Router, Secure cookies

## Contributing

Find out how to contribute to this project. [Contributing →]({{< relref "how-to-contribute" >}})
