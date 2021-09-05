---
layout: layouts/post.njk
title: A Quick Way to Remove all Local git Branches Except for Master
subtitle: I create a new branch for each new task, so I can end up with a plethora of branches. This is a quick way to remove all branches except master
date: 2017-06-26
tags:
    - git
    - post
---

Below is a quick way to remove all git branches on your local machine, except for master. This is really helpful for those of us who commit new branches for each new task.

```bash
git branch | grep -v "master" | xargs git branch -D
```

This snippet was written by [Adan Alvarado](https://twitter.com/adantj).
