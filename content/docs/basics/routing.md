---
title: "Routing"
description: "Routing"
lead: ""
date: 2020-11-12T15:22:20+01:00
lastmod: 2020-11-12T15:22:20+01:00
draft: false
images: []
menu: 
  docs:
    parent: "basics"
weight: 620
toc: true
---

Routing is performed using well known [gorilla/mux](https://github.com/gorilla/mux) package.
For more info on how to use the Gorilla router please [visit the official docs](https://github.com/gorilla/mux).

## Action handlers
For every route you should create a corresponding action handler.

```
package main

import (
	"fmt"
	"net/http"

	"github.com/gorilla/mux"
	"github.com/gotuna/gotuna"
)

func main() {
	app := gotuna.App{}
	app.Router = mux.NewRouter()
	app.Router.Handle("/", handlerHome(app))

	fmt.Println("Running on http://localhost:8888")
	http.ListenAndServe(":8888", app.Router)
}

func handlerHome(app gotuna.App) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, "Hello World!")
	})
}
```

## Grouping routes
You can group several routes that share the same functionality. This is called "subrouting".


```
app.Router = mux.NewRouter()

// middlewares for all routes
app.Router.Use(app.Logging())

// for logged in users
user := app.Router.NewRoute().Subrouter()
user.Use(app.Authenticate("/login"))
user.Use(app.StoreUserToContext())
user.Handle("/", handlerHome(app)).Methods(http.MethodGet)
user.Handle("/profile", handlerProfile(app)).Methods(http.MethodGet, http.MethodPost)
user.Handle("/logout", handlerLogout(app)).Methods(http.MethodPost)

// for guests
auth := app.Router.NewRoute().Subrouter()
auth.Use(app.RedirectIfAuthenticated("/"))
auth.Handle("/login", handlerLogin(app)).Methods(http.MethodGet, http.MethodPost)
```
