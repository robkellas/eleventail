---
layout: layouts/post.njk
title: "Javascript DOM Manipulation & Other Tricks"
subtitle: Some quick code samples of javascript dom manipulation and other handy snippets
date: 2015-09-01
tags:
    - javascript
    - post
---

## If URL param value is (x) then change the background image

The following code grabs the parameters of the URL, targets a parameter called "utm_content" and checks to see if the value is "e9150826b" and then adds a background image to the body if true.

``` js
function getURLParameter(name) {
  return decodeURIComponent((new RegExp('[?|&]' + name + '=' + '([^&;]+?)(&|#|;|$)').exec(location.search)||[,""])[1].replace(/\+/g, '%20'))||null;
}


targetVar = getURLParameter('utm_content');

if (targetVar == 'e9150826b') {
  document.getElementsByTagName('body')[0].style.background = 'url(/assets/img/content/stars2.jpg)';
}
```

Applications for this include A/B testing a campaign by changing the "utm-content" variable. In the case above "e9150826b" would display the background image, where any other utm_content variable wouldn't initiate anything (control). You can test this now by clicking: [/javascript-dom-manipulation?param=test1](/javascript-dom-manipulation?param=test1). There are a number of various applications in using this. For example you could load up and change multiple elements on the page when this variable is triggered in the URL. The same could be applied by using a cookie method instead of a URL param. However, I thought it is quite an interesting idea to base it off a Google Campaign URL, as it will auto-track results in Google Analytics. Play around by changing the param in the URL, it will only trigger when the parameter "param" is set to "test1".

___

## Change DOM element with javascript

Sometimes when I'm in a pinch I use Google Tag Manager to change a DOM element by using a javascript tag.

``` js
document.getElementsByTagName('article')[0].innerHTML = "<h1>Wow, now you've gone and done it! Where did all the content go?</h1><p><a href='/javascript-dom-manipulation'>click here to get back to the post</a></p>";
```

I'm going to add this to the code block I created above and change it so that it will only initiate where the "param" value is "test2". [Check it out](/javascript-dom-manipulation?param=test2).

So now my javascript looks like this:

``` js
function getURLParameter(name) {
  return decodeURIComponent((new RegExp('[?|&]' + name + '=' + '([^&;]+?)(&|#|;|$)').exec(location.search)||[,""])[1].replace(/\+/g, '%20'))||null;
}

targetVar = getURLParameter('param');

if (targetVar == 'test1') {
  document.getElementsByTagName('body')[0].style.background = 'url(/assets/img/content/green-bg.jpg)';
}

if (targetVar == 'test2') {
  document.getElementsByTagName('body')[0].style.background = 'url(/assets/img/content/stars2.jpg)';
  document.getElementsByTagName('article')[0].innerHTML = "Wow, now you've gone and done it! Where did all the content go?<a href='/javascript-dom-manipulation'>click here to get back to the post</a>";
}
```

You can also target CSS ID's using the following:

``` js
document.getElementById('name-of-id').innerHTML = 'you could change in image, text etc';
```

Or alternatively target a specific class:

``` js
document.getElementsByClassName('name-of-class')[0].innerHTML = 'you could change in image, text etc';
```

Note that in the case of a class, you can specify a specific instance of the class by changing the `[0]` to if the javascript should target the first, second, third etc instance of the class. Remember that 0, in this case refers to the first instance of the class.

___

## Create a DOM element via Javascript

Sometimes you may want to create a dom element in javascript from scratch.

``` js
var newDiv = document.createElement("div");
newDiv.id = "div1";
newDiv.setAttribute("data-name", "Rob Johnson");
newDiv.style.margin = "0px auto";
newDiv.style.textAlign = "center";
newDiv.className = "setclass";
newDiv.innerHTML = "<a href='#'>You can enter any HTML here</a>";
document.body.appendChild(newDiv);
```

___

## Another way to create a DOM element in Javascript

``` html
<script>
  // create element
  var newElement = document.createElement("div");
  // style this new element
  newElement.style.cssText = "margin: 20px auto; max-width: 600px;";
  // add class name
  newElement.className = "columns is-centered";
  // define the innerHTML of the new div
  newElement.innerHTML = "<a href='/destination-url'><img src='/img/my-super-awesome-image.jpg'></a>";
  // selet the element on the page
  var targetElement = document.querySelector('.class_of_target_div');
  // DO IT
  targetElement.parentNode.insertBefore(newElement, targetElement);
</script>
```

You can see above, that I'm also styling the object with `newElement.style.cssText`. There are many `.style` functions you can use to style up your HTML element. You can find a great list of all [javascript .style options here](https://www.w3schools.com/jsref/dom_obj_style.asp).

___

## Redirect page

Simple method of redirecting the browser

``` js
window.location = "https://google.com";
```


<script>
function getURLParameter(name) {
return decodeURIComponent((new RegExp('[?|&]' + name + '=' + '([^&;]+?)(&|#|;|$)').exec(location.search)||[,""])[1].replace(/\+/g, '%20'))||null;}

targetVar = getURLParameter('param');

if (targetVar == 'test1') {
  document.getElementsByTagName('body')[0].style.background = 'url(/assets/img/content/green-bg.jpg)';
}

if (targetVar == 'test2') {
  document.getElementsByTagName('body')[0].style.background = 'url(/assets/img/content/stars2.jpg)';
  document.getElementsByClass('content')[0].innerHTML = "<h1>Wow, now you've gone and done it! Where did all the content go?</h1><p><a href='/javascript-dom-manipulation'>click here to get back to the post</a></p>";
}
</script>
