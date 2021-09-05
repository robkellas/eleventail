---
layout: layouts/post.njk
title: How to use Macros in Google Tag Manager
subtitle: A use case for using macros in Google Tag Manager
date: 2014-05-18
tags:
    - tutorial
    - analytics
    - javascript
    - feature
    - post
---

<div class="callout"><span class="is-info">Update: </span> Google Tag Manager has since changed the name "Macros", to "Variables". There is now a section, similar to tags and triggers, called variables where you can manage these variables and call them into your tags.</div>

I’ve been using [Google Tag Manager](http://google.com/tagmanager) for a number of months and have been thoroughly enjoying deploying tag changes on my sites instantly without having wait for a production deploy. Also, having the power to select what tags fire on what pages is a huge plus for me. It removes the need for development resources and gives me greater control in the delivery of my tags; all of which are delivered asynchronously. Pretty smart.

One tool available in Google Tag Manager, that I’ve found extremely useful recently, are macros. [Macros can mean and be used in a lot of different ways](http://en.wikipedia.org/wiki/Macro_computer_science) and so, initially, I was a little confused as to how macros could be used in Google Tag Manager.

Below is one of the many use cases that illustrates how you can use macros to minimize code on your site and deliver much greater functionality to your web applications.

## Use Case: Deliver a unique offer to users who click through to a page via an email campaign

If you’re using Google Analytics and sending emails to customers. Chances are you’re already including **utm_campaign** in your email URL’s to track your campaigns. An example URL may be

``` html
https://example.com/example-product?&utm_source=example.com&utm_medium=email&utm_campaign=abandon_cart
```

To grab the value of the query key or parameter create a new macro in GTM with the macro type as **URL** and the component type as **Query**. Then enter in the query key which, in this example, is

``` js
utm_campaign
```

![Macros in Google Tag Manager](/assets/img/content/macros-google-tag-manager-example.png)

This will then assign the value of **utm_campaign** from the URL, which in this case will be **abandon_cart**, to the macro

``` js
{% raw %}{{ campaign_name }}
{% endraw %}
```

This macro value will now be available as a variable for code on any page where the parameter exists in the URL. Nifty.

## Pass the macro into the code

We can then initiate a popup modal via javascript with a simple check

``` js
{% raw %}if ( {{ campaign_name }} == "abandon_cart") {

    // offer code for popup modal

}
{% endraw %}
```

There are lots of other use cases that include passing these variables into other tracking services etc. The great thing about GTM is that it enables you to be creative with minimal development knowledge.

Feel free to reach out via [Twitter](http://twitter.com/robkellas "@robkellas") if you have any questions.
