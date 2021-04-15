---
title: "Users"
description: "Users"
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

In order to use the session and other user-related features, 
you must provide a way for the app to find and authenticate users.

This is done by implementing two interfaces `gotuna.UserRepository` and `gotuna.User`.

## User provider interface
The two main interfaces are simple to implement, the only requirement is 
that user ids are unique.
```
type User interface {
	GetID() string
}

type UserRepository interface {
	GetUserByID(id string) (User, error)
	Authenticate(w http.ResponseWriter, r *http.Request) (User, error)
}
```


## In memory user provider
To make your life easier, default inMemory user implementation is already provided.

```
	// User1 is a sample user #1.
	var User1 = gotuna.InMemoryUser{
		UniqueID: "123",
		Email:    "john@example.com",
		Name:     "John",
		Password: "pass123",
	}

	// User2 is a sample user #2.
	var User2 = gotuna.InMemoryUser{
		UniqueID: "456",
		Email:    "bob@example.com",
		Name:     "Bob",
		Password: "bobby5",
	}

	userRepository := gotuna.NewInMemoryUserRepository([]gotuna.InMemoryUser{
		User1,
		User2,
	})

	app := gotuna.App{
		UserRepository: userRepository,
	}
```

