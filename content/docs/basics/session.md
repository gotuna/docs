---
title: "Session"
description: "User session"
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

A session is a non-persistent, per-user data storage, which is created when the user is 
logged in and destroyed when the user is logged out.

The session is used to store flash messages or any temporary user-specific data.


## Configuration
Before using the session you must configure the main app with a session driver,
and provide a unique secret session key.

Under the hood, the framework uses [gorilla/sessions](https://github.com/gorilla/sessions)
so you can choose one of many store implementations.

```
key := os.Getenv("SESSION_KEY")

app := gotuna.App{
	Session: gotuna.NewSession(sessions.NewCookieStore([]byte(key)), "app_session"),
}
```

## Using the session
First, you need to setup a session for the authenticated user.  After successful 
authentication, your UserRepository will return a User object which you can 
use to get the user ID and initialize the session:
```
func handlerLogin(app App) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		user, _ := app.UserRepository.Authenticate(w, r)
		app.Session.SetUserID(w, r, user.GetID())
	})
}
```

You can now store, retrive and delete string values from the session:
```
func handlerHome(app App) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		app.Session.Put(w, r, "color", "red")
		app.Session.Get(r, "color")
		app.Session.Delete(w, r, "color")
	})
}
```
If you wish to log out the current user, you can simply destroy this session:
```
func handlerLogout(app App) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		app.Session.Destroy(w, r)
	})
}
```

## Flash messages
Sometimes you may wish to store a message or status update in the session for the next request. 
Data stored in the session using the flash method will be available in the first subsequent request.

```
msg := "Welcome!"
app.Session.Flash(w, r, gotuna.NewFlash(msg))

app.Session.Flash(w, r, gotuna.FlashMessage{
  Kind: "success",
  Message: "Another info message",
})
```

You can range over `.Flashes` to render this in your template:
```
{{range $e := .Flashes}}
<div class="notification {{if $e.Kind}}is-{{$e.Kind}}{{end}}">
  {{$e.Message}}
</div>
{{end}}
```

## Storing non-string values
If you need to store your custom struct into the session, you can use 
`TypeToString` and `TypeFromString` helper functions:
```
type myType struct {
	Str string
	Bl  bool
}

val := myType{
	Str: "test string",
	Bl:  true,
}

s, err := gotuna.TypeToString(val)

r := myType{}
err = gotuna.TypeFromString(s, &r)
```
