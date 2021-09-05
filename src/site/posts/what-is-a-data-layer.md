---
layout: layouts/post.njk
title: What is a Data Layer?
subtitle: A quick intro into dataLayers and how to create and access data from them
date: 2016-03-16
tags:
    - analytics
    - javascript
    - feature
    - post
---

A `dataLayer` is an efficient way of presenting multiple data attributes to several services. For example, if you needed to integrate multiple services into your website, such as Google Analytics, Optimizely and Olark, you could have each of them reference data from the same store of data, or `dataLayer`, instead of rendering multiple variables with the same values for each service. The idea being that a developer can present the data once and it can be easily passed along to multiple services making new service integrations easy. A tag manager can also grab these variables and pass them along to any third-party service also.

> A `dataLayer` is a javascript variable array that can hold custom information which can be easily accessed by other services. This data can originate from an event such as a page view or a click event. It's a great way to standardize your data for tracking purposes and, once setup, can be managed without development/IT resources.

Here's how the `dataLayer` is initiated:

``` html
<script>
  dataLayer = [];
</script>
```

Here's a `dataLayer` that has some data in it:

``` html
<script>
  dataLayer = {
    'userId': '12345',
    'visitorType': 'high-value',
    'siteSection': 'computers',
    'product': 'MacBook Pro 15 Inch'
  };
</script>
```

Here's an example of how to access the value of the variable called `visitorType` referenced above via javascript.

``` js
dataLayer.visitorType
```

The above code is referencing the first array via `[0]`. There can be many arrays compositing the `dataLayer` and traversing between them is as simple as defining this value.

One of the issues above is that we're mixing different types of data into the same array. For example, I'd prefer to keep customer/global data in the first array and then page/product specific data in the second array. Here's An example of the prior `dataLayer` but with multiple arrays.

```js
dataLayer = {
  'customerInfo': {
    'userId': '12345',
    'visitorType': 'high-value'
  }
  'pageInfo': {
    'siteSection': 'computers',
    'product': 'MacBook Pro 15 Inch'
  }
};
```

Here's how you can extract the value of userId from the above example:

```js
dataLayer.customerInfo.userId
```

The above command is accessing the first array, which is the dataLayer, then requesting the variable `customerInfo` which is also an array, so we request the first array via `[0]` and then the variable `userId` which returns the value `12345`.

## Summary

Essentially the `dataLayer` is nothing more than a javascript variable that contains an array. The advantage of doing this though is that new services are implementing standardized features that target this variable and its contents.

There are no predefined rules for what data should be included in a `dataLayer` and so it's an open book depending on what your needs are. This makes things extremely flexible, but also partially confusing if you don't know where to start.

Resources:

- [Google Tag Manager Guide](https://developers.google.com/tag-manager/devguide#datalayer)
- [SimoHava.com](http://www.simoahava.com/analytics/data-layer/)
