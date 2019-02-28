+++
date = "2018-06-25T17:09:14-05:00"
draft = false
title = "websockets in Go."
slug = 'go-websockets'

+++

Websockets are way of providing full-duplex communication b/w client and server with lower overheads. URI schemes  `ws` or `wss` is a websocket (unencrypted) or websocket secure (encrypted) protocol respectively. It's designed to work over HTTP port `80` or `443` to support HTTP Proxies and Intermediaries. 

When establishing connection, client sends an HTTP Upgrade Header to switch from HTTP protocol to WebSocket protocol. Communications i.e. exchange of data or byte streams are dealt by TCP alone. This is beneficial in environments protected by Firewall. 

> Remember there only need to be **one** established connection b/w client and server for 'em to communicate with **each other**. Unlike HTTP client needing to make repetitive requests to server to respond back to it. 

##### I'll be building a chat application in Go, covering following topics:

- use [net/http](https://golang.org/pkg/net/http/) package to server HTTP Requests
- Deliver template-driven driven content to users
- Satisfy a [_Go Interface_](https://gobyexample.com/interfaces) to build our own `http.Handler` types.
- Use _gorutines_ for multitasking concurrently.
- use _channels_ to share information b/w goroutines.
- upgrade HTTP requests to use [websockets](https://en.wikipedia.org/wiki/WebSocket).
- Add *Tracing* to our app to better understand it's inner workings.
- Write a complete package using [TDD](http://agiledata.org/essays/tdd.html) Methodology.
- Return *Unexported types* via *Exported interfaces*.

> [**OWASP Testing Guide for Websockets**] (https://github.com/OWASP/OWASP-Testing-Guide-v5/blob/master/document/4%20Web%20Application%20Security%20Testing/4.12%20Client%20Side%20Testing/4.12.10%20Testing%20WebSockets%20(OTG-CLIENT-010).md) -- Not covered here but useful.
