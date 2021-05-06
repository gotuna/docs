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

GoTuna handles routing with the famous [gorilla/mux](https://github.com/gorilla/mux) package.
For more info on how to use the Gorilla router please [visit the official docs](https://github.com/gorilla/mux).

## Action handlers
For every route, you should create a corresponding action handler.

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

	app.Router.Handle("/", handlerHome(app))
	app.Router.Handle("/login", handlerLogin(app)).Methods(http.MethodGet, http.MethodPost)
	app.Router.Handle("/api/shapes", handlerShapes(app))

	fmt.Println("Running on http://localhost:8888")
	http.ListenAndServe(":8888", app.Router)
}

func handlerHome(app gotuna.App) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, "Dashboard")
	})
}

func handlerLogin(app gotuna.App) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, "Login form...")
	})
}

func handlerShapes(app gotuna.App) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		cars := []string{
			"Circle",
			"Square",
			"Triangle",
		}

		json.NewEncoder(w).Encode(cars)
	})
}
```

## Grouping routes
You can group several routes that share the same functionality. This is called "subrouting".


```
// middlewares for all routes
app.Router.Use(app.Logging())
app.Router.Use(app.StoreParamsToContext())
app.Router.Use(app.StoreUserToContext())

// for logged in users
user := app.Router.NewRoute().Subrouter()
user.Use(app.Authenticate("/login"))
user.Handle("/", handlerHome(app)).Methods(http.MethodGet)
user.Handle("/profile", handlerProfile(app)).Methods(http.MethodGet, http.MethodPost)
user.Handle("/logout", handlerLogout(app)).Methods(http.MethodPost)

// for guests
guests := app.Router.NewRoute().Subrouter()
guests.Use(app.RedirectIfAuthenticated("/"))
guests.Handle("/login", handlerLogin(app)).Methods(http.MethodGet, http.MethodPost)
```

## Serving static files
A special handler is provided by the framework to serve your static files 
configured under `app.Static` with optional prefix `app.StaticPrefix`.

You can provide a custom handler to be used when the file is not found (404 page).

```
app.Static = os.DirFS("./static")

// this route is usually placed at the bottom (catch-all)
app.Router.PathPrefix(app.StaticPrefix).
	Handler(http.StripPrefix(app.StaticPrefix, app.ServeFiles(http.NotFoundHandler()))).
	Methods(http.MethodGet)
```

Static files are passed into the app's configuration using 
the `io/fs` package's `FS` interface. 
This means that you can use both `os.DirFS` or `go:embed` to 
pack all your static files directly into the final binary.
