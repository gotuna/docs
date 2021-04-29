---
title: "App"
description: "App"
lead: ""
date: 2020-11-12T15:22:20+01:00
lastmod: 2020-11-12T15:22:20+01:00
draft: false
images: []
menu: 
  docs:
    parent: "basics"
weight: 20
toc: true
---


The `App` is the central place for every GoTuna application. 
This is where all the app's dependencies are configured.

```
type App struct {
	Logger         *log.Logger
	Router         *mux.Router
	Static         fs.FS
	StaticPrefix   string
	ViewFiles      fs.FS
	ViewHelpers    []ViewHelperFunc
	Session        *Session
	UserRepository UserRepository
	Locale         Locale
}
```

## Hello world
To illustrate a simple app, we can create a hello world `main.go`:

```
package main

import (
	"fmt"
	"net/http"

	"github.com/gotuna/gotuna"
)

func main() {
	app := gotuna.App{
		Router: gotuna.NewMuxRouter(),
	}

	app.Router.Handle("/", http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, "Hello World!")
	}))

	fmt.Println("Running on http://localhost:8888")
	http.ListenAndServe(":8888", app.Router)
}
```

Run this app and visit http://localhost:8888 in your browser:
```
go run main.go
```
