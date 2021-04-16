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
weight: 710
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


## In-memory user provider
To make your life easier, default inMemory user implementation is already provided.

```
	// User1 is a sample user #1.
	var User1 = gotuna.InMemoryUser{
		ID: "123",
		Email:    "john@example.com",
		Name:     "John",
		Password: "pass123",
	}

	// User2 is a sample user #2.
	var User2 = gotuna.InMemoryUser{
		ID: "456",
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

## MySQL user provider
To see how you can build your own, custom user providers, please take a look 
at this sample [mysql user provider](https://github.com/gotuna/mysqlusers).

This is most common way to store users, but you can also keep your users in MongoDB
or any other type of storage. You can also add as many fields to your users as 
you need in your application, add methods to update, or create new users.

