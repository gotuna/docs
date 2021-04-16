---
title: "Views"
description: "Views and templating"
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

The framework comes with the basic templating engine which uses native `html/template` Go templates.

## Using templates
The templates are typically used inside the action handlers:

```
func handlerLogin(app gotuna.App) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		app.NewTemplatingEngine().Render(w, r, "app.html", "login.html")
	})
}
```

Here we have the `app.html` template file which is used as a main layout, and 
the `login.html` as an injected block component.
```
// app.html

{{- define "app" -}}
<!DOCTYPE html>
<html>
  <head></head>
  <body>
    <div>Main Menu</div>
    <div class="container">
      {{block "content" .}}{{end}}
    </div>
  </body>
</html>
{{- end -}}
```

```
// login.html
{{define "content"}}
  <div class="guest-form">
    <form method="POST" action="/login">
      <button class="button is-link">{{t "Log In"}}</button>
    </form>
  </div>
{{end}}

```
For more info on how to use the `html/template` package please [visit the official docs](https://golang.org/pkg/html/template/).

## Variables
You can pass variables to your templates:
```
app.NewTemplatingEngine().
	Set("message", "This is a 'message' variable.").
	Render(w, r, "app.html", "login.html")
```
and use them in your html template like this:
```
<div>{{.Data.message}}</div>
```
You can also set and get errors:
```
app.NewTemplatingEngine().
	SetError("email", "Your email is not working").
	Render(w, r, "app.html", "login.html")
```
and use them like this:
```
<span>{{index .Errors "email"}}</span>
```



## Helpers
View helpers are functions that you can use in your html templates, here we
are using the `t` helper to translate the `Email` string.
```
<label class="label">{{t "Email"}}</label>
```

You can add your own custom helpers to the app using the `gotuna.ViewHelperFunc` signature.

```
// custom view helpers added to the app constructor
app.ViewHelpers = []gotuna.ViewHelperFunc{
	func(w http.ResponseWriter, r *http.Request) (string, interface{}) {
		return "uppercase", func(s string) string {
			return strings.ToUpper(s)
		}
	},
}
```

## Built-in helpers
The framework comes with several built-in helpers:

- t (translate)
- tp (translate plural)
- request (current http.Request object)
- static (get static file with prefix)
- currentUser (current User object)
- currentLocale
- isGuest

## Template files
Template files are passed into the app's configuration using the `io/fs` package's `FS` interface.

This means that you can use both `os.DirFS` or `go:embed` to pack all your template files directly into the final binary.

Simplified example showing the structure of an app with templates:


```
..
├── views/
│   ├── app.html
│   ├── login.html
│   └── views.go
└── main.go
```

```
// main.go

...
// use embeded files
app := gotuna.App{
	ViewFiles: views.EmbededViews,
}

// ...or use operating system files
app := gotuna.App{
	ViewFiles: os.DirFS("./views"),
}
...
```

```
// views/views.go

package views

import "embed"

//go:embed *
var EmbededViews embed.FS
```

