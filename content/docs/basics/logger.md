---
title: "Logger"
description: "Logger"
lead: ""
date: 2020-11-12T15:22:20+01:00
lastmod: 2020-11-12T15:22:20+01:00
draft: false
images: []
menu: 
  docs:
    parent: "basics"
weight: 920
toc: true
---

Logger is a standard `log.Logger` implementations that you must supply to the app.


{{< alert icon="💡" text="The logging middleware will use this interface to log every request." >}}

## Stdout logger
```
app := gotuna.App{
	Logger: log.New(os.Stdout, "", 0),
}
```
