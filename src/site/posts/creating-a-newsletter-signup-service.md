---
layout: layouts/post.njk
title: How to create an email newsletter sign up service Pt. 1
subtitle: Create a basic HTML form that will post it's data to an endpoint via javascript
date: 2020-08-28
tags:
    - tutorial
    - javascript
    - feature
    - post
---

<div class="callout"><span class="jam jam-alert is-info"></span> I wrote this code for a project in 2015. There is likely a much better way today. Still, the principles here remain relevant</div>

Here is a brief overview of how you can create a newsletter form which will post the data entered to an endpoint using javascript. This will enable you to create your own email newsletter subscription service.

## Create a simple HTML form with a message box

The first thing we want to do here is create an entry form for an email. Unlike usual HTML forms, we won't be using the `action` method to send data, instead we will create a javascript listener that will pass along what is entered and send it to an endpoint for processing. Here I'm replacing the action with `javascript:;` which doesn't actually do anything and instead I'm using the `onClick="sendSub()"` attribute to initiate the javascript function which we'll get to next.

Note that there is a simple `<div>` with `id="message"` below the form which will be used to show a message once we hear back from the endpoint. There is also a hidden input called `campaign_id` which has a value of `1`. The purpose of this is so that you can send a newsletter ID along with an email address, which allows you to create different email newsletter subscriptions easily by assigning a different number value.

``` html
<form id="subscription" name="subscription" action="javascript:;">
    Email:  <input type="text" id="email"><br>
    <input type="hidden" id="campaign_id" value="1">
    <input type="submit" value="Send" onclick="sendSub()">
</form>
<div id="message" style="color:green;"></div>
```

## Write the javascript function

Now we have the form ready to go, it's now time to write the function that submits the entered values above to an endpoint. The first thing we're doing here is including jQuery, which simplified the ajax call script for us. Ajax stands for "Asynchronous Javascript and XML." Essentially it allows you to run javascript without reloading a page, which is very common in todays applications.

Then we will create a `function` called `sendSub()`. We'll now create two variables which will inherit the values we assigned in the above form by using the `getElementById().value` method. Essentially, we're asking javascript to grab the elements input value from the form and store it in variables so that we can pass it along to the ajax call.

Every endpoint call you make is either `POST` or `GET`. `POST` calls pass along the data inside of the URL. You can often see these types of URL's with parameters such as `utm_campaign="christmas_shopping"`. The first part of this parameter is a key `utm_campaign` which you can think of as a variable. Essentially a mini container that can store a value which you can then recall to use somewhere else. `christmas_shopping` is the value. You'll often hear people refer to *"key value pairs"* which just means that you have data which can be accessed by calling the key, or variable name, to access the value stored in it.

Now that we've got the variables loaded with the values from the form and we've defined that this will be a `POST` request, we can now form the URL that will send the data.

Lastly, we will return a message if the endpoint/URL responds with a `200` code, which means that everything went well.

``` html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
<script type="text/javascript">
    function sendSub() {
        var email = document.getElementById('email').value;
        var campaign_id = document.getElementById('campaign_id').value;
        $.ajax({
            type: "POST",
            url: 'https://yourdomain.com/?email=' + email + '&campaign_id=' + campaign_id,
            cache: false,
            success: function() {
                $('#message').html("Success!");
            }
        });
    }
</script>
```

## Wrapping up

What's great about javascript is that you don't need a server to test this out. You can go ahead and create a local file using a [text editor](https://www.techradar.com/best/best-text-editors "best text editors for 2020") and run this on your browser for testing. However, we still need to make the endpoint that will receive the data, store it and then respond with a success or error code and message

## Part 2 coming soon

Watch out for part 2 where we'll build an endpoint for this service and store the data that is being transmitted.
