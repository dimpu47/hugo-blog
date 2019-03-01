+++
date = "2018-08-25T17:09:14-05:00"
draft = false
title = "systems programming in Go."
slug = 'go-systems-programming'

+++

> [***why golang for systems programming... ?***](https://www.mphasis.com/content/dam/mphasis-com/global/en/downloads/WhitePapers/Mphasis-Diital-Use-Go-for-System-Programming.pdf) .

Go was written by google as famously sited many times over the internet. Some of the people involved in development of Golang are working professionals at Google, who were also involved with many Unix & C developments, and they developed Golang because of increasing complex demands of System Engineering at google, and C was becoming hard to maintain.

Go's concurrency model is built upon [CSP - Communicating Sequencial Processes ](http://www.usingcsp.com/cspbook.pdf) Where *values* are shared b/w independent activities or *goroutines* but *variables* are, for the most part, confined to single activity. There are certain hazards and pitfalls of concurrent programming as done traditionally, but i'll cover that some other post. 

Go's support for concurrency is one of it's greatest strength, `go` spawns lightweight (2mb) threads of `func()` execution that runs on available OS threads. All goroutines in a single program share same address space. Programs start with a small stack that grows by allocation and frees up heap storage as required.
  
_p.s: this is currently being refactored. consider it **wip**._