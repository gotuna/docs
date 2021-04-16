---
title: "Introduction"
description: "GoTuna - progressive web framework written in go"
lead: ""
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

{{< alert icon="💡" text="This project is still under heavy development and is not ready for production use." >}}

## Main Features
- Router (gorilla)
- Standard `http.Handler` interface
- Middleware support
- User session managment (gorilla)
- Session flash messages
- Native view rendering (html/template) with helpers
- Static file server included with configurable prefix
- Standard logger interface
- Request logging and panic recovery
- Full support for embedded templates and static files
- User authentication (via user provider interface)
- Sample InMemory user provider included
- Multi-language support
- Database agnostic

## Requirements
- Make sure you have Go >= 1.16 installed

## Testing
```
go test -race -v ./...
```

## External dependencies
External modules are mostly used when the feature is too complex to build or maintain - Router, Secure cookies

## Contributing

Find out how to contribute to this project. [Contributing →]({{< relref "how-to-contribute" >}})
