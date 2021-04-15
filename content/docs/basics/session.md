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

Session are used to store flash messages, or any temporary user-specific data.


## Configuration
Before using the session you must configure the main app with a session driver,
and provide unique secret session key.

Under the hood the framework uses [gorilla/sessions](https://github.com/gorilla/sessions)
so you can choose one of many store implementations.

```
key := os.Getenv("SESSION_KEY")

app := gotuna.App{
	Session: gotuna.NewSession(sessions.NewCookieStore([]byte(key))),
}
```

