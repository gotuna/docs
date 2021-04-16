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
	Session: gotuna.NewSession(sessions.NewCookieStore([]byte(key))),
}
```

## Flash messages
Sometimes you may wish to store a message or status update in the session for the next request. 
Data stored in the session using the flash method will be available only in the subsequent request.

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

