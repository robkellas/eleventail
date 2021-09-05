---
layout: layouts/post.njk
title: GitHub Terminal Cheatsheet
subtitle: Helpful terminal commands when using Git
date: 2017-07-07
tags:
    - tutorial
    - git
    - post
---

## Setting up git on a remote server

When setting up git on a server for the first time, first initialize git in the root directory of your project. E.g. ```/var/www/html```. I prefer to initialize git before adding any source code and the instructions below reflect this.

```bash
# then initialize the git repo
git init

# now clone the project from your git repo into the directory
git remote add origin git@github.com:username/repository
```

## Check to see if GitHub has your SSH key

Check to see if your machine has SSH keys stored on GitHub. It will attempt to ssh to GitHub Enterprise

```bash
ssh -T git@hostname
```

You should see something like this if you already have an SSH ket installed

```bash
The authenticity of host 'hostname (192.30.252.1)' can't be established.
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)?
```

Type `yes` and you should see

```bash
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```
