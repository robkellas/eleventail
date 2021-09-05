---
layout: layouts/post.njk
title: How to Set & Read Javascript Cookies
subtitle: A tutorial on how to set and read javascript cookies
date: 2015-09-05
tags:
    - tutorial
    - javascript
    - post
---

Here are a few tricks with javascript that can be adapted for many uses. That said, the users browser will need to have javascript enabled. The following code doesn't require jQuery.

## Step 1: Create a cookie

```js
document.cookie="cookiename=cookievalue";
```

## Step 2: Initial script to allow you to read cookies

```js
var cookies;

function readCookie(name,c,C,i){
  if(cookies){ return cookies[name]; }

  c = document.cookie.split('; ');
  cookies = {};

  for(i=c.length-1; i>=0; i--){
    C = c[i].split('=');
    cookies[C[0]] = C[1];
  }

  return cookies[name];
}

window.readCookie = readCookie; // or expose it however you want
```

## Step 3: Request the value of a specific cookie

```js
readCookie("cookiename");
```