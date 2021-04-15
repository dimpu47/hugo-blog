+++
date = "2019-12-22T17:09:14-05:00"
draft = false
title = "Structural Patterns in Go."
slug = 'facade-and-flyweight'
+++

Structural design patterns describe the relationships between the entities. They are used to form large structures using classes and objects. These patterns are used to create a system with different system blocks in a flexible manner.  Adapter, bridge, composite, decorator, facade, flyweight, private class data, and proxy are the [Gang of Four (GoF)](http://wiki.c2.com/?GangOfFour) structural design patterns. The private class data design pattern is the other design pattern covered in this section.

### Proxy pattern
Proxy pattern usually wraps an object to hide some of it's characteristics. these characteristics could be the fact that it is a remote object ( remote proxy ), a vert heavy object like a big image, or dump of terrabyte database ( virtual proxy ) or restricted access object ( protection proxy ).

#### Objectives

In general, following are the objectives:
- Hide an object behind the proxy so the features can be hidden, restricted, and so on
- Provide a new abstraction layer that is easy to work with, and can be changed easily

#### Example
As an example we'll create remote proxy, that'll be a cache of objects, before accessing a database. Let's say in order to avoid querying the database everytime we require user info we can implement a FIFO Stack of users in a Proxy Pattern.

Let's say we wrap an imaginary database in slice with our proxy pattern. Then we have to stick to following points:
- all access to database user will done via Proxy Type.
- A stack of `n` no. of users will be kept in Proxy.
- If a user exist in stack, don't query db and return stored result
- If queried user doesn't exist, query the db, delete the oldest user if it exists and store the new one in stack