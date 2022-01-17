---
layout: single
title: "Building the SimiDex API"
date: 2022-01-16 23:00:00 -0500
categories: Programming SimiDex Flask
comments: true
typora-root-url: ..
---

# Life update

Hello world. I figured I would start this one off with an update about myself as these blogs are also a stand in for a personal journal for the time being. I'm trying to find a cross platform journal app where I can track things like how I feel, my health, and habits, as well as a journal entry - all on a plain text file that can be read even decades in the future once the app is no longer supported. This is proving pretty difficult to find, although it might make for a fun project in the future.

So I'm coming on 6 days with a fever, although it is finally starting to tame. Seeing how its covid season (and has been for the past two years) I took a self assessment online and it essentially told me to self isolate and NOT get a covid test so that front line and essential workers have quicker access to it. Researching my symptoms further, I don't think it is covid anyways, and it is far more likely I have a bacterial infection like strep throat or something. Oh well. The timing couldn't be worse though since I am coming down to two weeks left to find a new house or apartment. This said, my stress and existential dread has somewhat alleviated since I became motivated with my new project SimiDex. I haven't felt drive like this for a while! It's like a light at the end of a tunnel.

# SimiDex update

This said, I am also facing lots of challenges on this project already. I have this perfectionist attitude when it comes to new projects where I want to implement everything a good open source project should have - despite not really knowing how. For example, I want to implement CI testing, but in the past I have completely failed at writing tests for functions utilizing the database. Another example is Sphinx documentation. There is just a wealth of knowledge I still don't have, and I'm worried that if I ignore it now, it may become a mountain of work when this project is mature, so I am dedicating a bit more time to these things at the projects inception, which has the consequence of slowing down development. Anyways, lets talk about the API.

# Flask RESTful API

SimiDex's API is going to be built using Flask, Flask-RESTful, Flask-SQLAlchemy, and a number of other Flask extensions. I am using a bunch of libraries and utilities to build the API, so as opposed to breaking down every script, I wanted to list the libraries and utilities used, and describe them below.

## What is Flask

Flask is a micro web framework written in Python, designed to create web applications and APIs. It depends on the Jinja template engine and the Werkzeug WSGI toolkit. There are tons of extensions available for Flask, some of which will be discussed later in this post.

### Jinja

Jinja is a template engine for Python. I wrote about Jinja in a [previous blog post](https://georgeciesinski.me/blog/bootstrap/flask/front-end/programming/text-script/Jinja-Templates/).

### Werkzeug

The name Werkzeug is German for "tool". This is a Python utility library for building WSGI applications.

### WSGI

The **W**eb **S**ervice **G**ateway **I**nterface (pronounced "whiskey") is a calling convention for web servers to forward requests to web applications or frameworks written in Python.

## What is Flask-RESTful

Flask-RESTful is an extension for Flask that adds support to quickly build **REST APIs**. It adds basic building blocks called resources, which give you easy access to multiple HTTP methods, as well as routing to this resource.

### API

An API is an **A**pplication **P**oint **I**nterface. It is a set of definitions and protocols for building and integrating application software. It establishes the content required from the consumer or client (request) and the content required by the producer (response).

### REST

**RE**presentational **S**tate **T**ransfer is a set of architectural constraints. An API following these constraints is known as a REST API. When a request is made via a REST API, it transfers a representation of the state of the resource to the endpoint. This information is transferred through a variety of ways, but SimiDex will be using **JSON**. The server remains stateless, meaning no client information is stored between get requests.

### JSON

**J**ava**S**cript **O**bject **N**otation is a file format, or syntax, which despite it's name is language agnostic. This means that it is used in applications written in many different languages, and not necessarily in JavaScript.

## What is Flask-SQLAlchemy

Flask-SQLAlchemy is a Flask extension that adds SQLAlchemy support to Flask applications. It simplifies SQLAlchemy with Flask by providing useful defaults and helpers.

### SQLAlchemy

SQLAlchemy is a Python SQL toolkit and Object Relational Mapper (ORM) that gives applications the full power and flexibility of SQL. It allows you to access tables as if they were classes, and lets you get and store items as if they were objects, which is great for an object oriented language like Python.

### SQL

**S**tructured **Q**uery **L**anguage is a programming language designed for managing data in a relational database. It has been around since 1970, and now, 50 years later, is the most common method of accessing data in databases. There are many different SQL systems, such as PostgreSQL, SQLite3, and MySQL.

### Relational database

A relational database is a type of database that stores and provides data points related to one another. For example, items in an items table might be stored by a specific user in the users table. In a relational database, each row in the table is a record with a unique ID called a key.

## What is Flask-JWT

Adds basic JWT features to flask applications. This can be used to protect some resources and methods so that a token is required to access those methods.

It automatically creates an `/auth` resources, and POST requests which accept JSON content like:

```
{
  "username": "name",
  "password": "pass"
}
```

and returns a token that looks like:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2NDIzMDQyODYsImlhdCI6MTY0MjMwMzk4NiwibmJmIjoxNjQyMzAzOTg2LCJpZGVudGl0eSI6MX0.OK9oDWBf8cs0uO8S15ms5usXUIiLPG0DoMe1BG8SzWs"
}
```

### JWT

**J**son **W**eb **T**oken is a proposed internet standard for creating data with an optional signature and optional encryption.

The server generates a token which can contain an identity and provides it to the client.

### authenticate & identity

The Flask-JWT library requires authenticate and identity functions to be created in the application. The authenticate function takes a username and password and verifies it against the databse. Upon verification, it provides a token to the client. The identity function returns something like the user id or the user object. Any requests the client would make can contain this token, which also holds the client's identity information.

### hmac

Keyed-**H**ash **M**essage **A**uthentication **C**ode is a type of message authentication code (MAC) involving a cryptographic hash function and secret cryptographic key. it can be used to simultaneously verify the data integrity and authenticity of a message.

SimiDex uses the `hmac.compare_digest()` function as this function takes two arguments, and safely compares them. Normally if you check if a string is equal to another string, such as comparing two passwords, this would take the interpreter a certain amount of time depending on the number and kinds of characters. A skilled hacker can attempt different passwords and time the response to eventually determine the correct password. `compare_digest()` does the comparison in a way that takes an arbitrary amount of time and makes this crack method much more difficult if not impossible.

```
user = UserModel.find_by_username(username)

	# Compares two strings safely
	if user and hmac.compare_digest(user.password, password):
		return user
```

# Conclusion

So I think that about wraps it up for now. As this API grows, there will probably be more libraries and utilities I'll need to bring into the project, but this should be enough to build a basic functioning API.

I'm hoping to make the next blog about the front-end development as I want to build the most basic parts of each component of SimiDex before building out each component fully. This way, I can ensure these components work together as expected before it becomes a complicated project and needs a large rewrite.

I hope this was an enjoyable read. See you next time!
