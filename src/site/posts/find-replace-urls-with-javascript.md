---
layout: layouts/post.njk
title: Find/Replace URL's with Javascript
subtitle: How to change URL's once a page loads
date: 2016-08-08
tags:
    - tutorial
    - javascript
    - post
---

I recently had a need to ensure all URL's on site A were pointing to site B. What I needed was some javascript that would rewrite the URL's once the page had finished loading, and change any site A links to site B links.

Here's a handy piece of javascript that [@n00b2pr0](https://twitter.com/n00b2pr0) shared with me that get's the job done.

``` js
var linkRewriter = function(a, b) {
  $('a[href*="' + a + '"]').each(function() {
    $(this).attr('href', $(this).attr('href').replace(a, b));
  });
};

linkRewriter('current-domain.com', 'desired-domain.com');
```