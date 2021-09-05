---
layout: layouts/post.njk
title: A Quick Tutorial on Telnet
subtitle: How to use telnet with terminal commands
date: 2013-10-06
tags:
    - tutorial
    - post
---

>Telnet is a network protocol used on the Internet or local area networks to provide a bidirectional interactive text-oriented communication facility using a virtual terminal connection. – *[Wikipedia](http://en.wikipedia.org/wiki/Telnet)*

## Download

If you don't have telnet installed, install it with this following terminal command:

```bash
sudo apt install telnet
```

## How to use

Enter the following lines into terminal

```bash
telnet robkjohnson.com 80
GET / HTTP/1.1
host: robkjohnson.com
```

What is returned is essentially plain text of the source code. Which is exactly what your browser receives through the same GET/HTTP1.0 or GET/HTTP1.1 request.

## Security

Telnet is an old protocol that does not have any security, so anyone can eavesdrop on your connection. So make sure you're not accessing anything private with it.

Thanks goes to [@xunker](http://twitter.com/xunker) for teaching me this.

```bash
http://rubular.com \s-\s(\d{7})
```