---
layout: layouts/post.njk
title: "Flamingo: A Free Responsive Theme for Get Simple CMS"
subtitle: When I was recovering from knee surgery I made this theme which has been downloaded over 5,000 times. 
date: 2012-04-07
tags:
    - projects
    - feature
    - post
---

A while ago I stumbled across the <a href="http://get-simple.info/">GetSimple CMS</a>. At the time I was working on developing my own CMS, which now exists only as a contacts application, because I found that <a href="http://chriscagle.me/">Chris Cagle</a> had done a bunch of work that would save me A LOT of time.

So I've been using it for about a year so far and I thought it was about time that I give back.

![Flamingo Responsive Theme](/assets/img/content/flamingo-screenshot.png)

<a href="http://get-simple.info/extend/theme/flamingo/410/" class="button download" id="Flamingo Theme" title="Download Flamingo Theme" target="_blank"><span class="icon is-danger"><strong>&#9660;</strong></span>Download Flamingo</a>

## Two Themes in One
Change the css file in inc.start.php from style1.css to style2.css. Style2.css changes the layout to allow for longer nav items. Both styles are as responsive as each other.

## Responsive
The theme is a responsive design with a <a href="http://stuffandnonsense.co.uk/projects/320andup/">320px and up</a> philosophy. That basically means that if you took out all of the @media calls in the CSS document. It would only render the mobile version. So browsers which do not support @media calls such as, IE 7 and below, will render the mobile version and not dynamically respond to the screen size.</p>

## HTML5 Boilerplate
That said, if a user comes to your site in IE6, or another seriously depreciated browser, they will receive an update message notifying that they should update. This is an inherent part of the <a href="http://html5boilerplate.com/">HTML5 Boilerplate</a> that is cooked into this theme. That means <a href="http://www.modernizr.com/">modernizer.js</a> is also part of the theme allowing HTML5 objects to be rendered properly in old browsers.

## Multiple Theme Pages
This theme comes with some extras that really help it stand out. First of all it comes with the following pages:

- Template.php (1-3 column layout)
- 2-column.php (1-2 column layout)
- 404.php (Set this as your default 404 page at /data/other/404.xml in template field)  - Blog.php (Blog tile index page)

## Code for image thumbnails

``` html
<ul class="thumbs">
  <li>
    <a class="fbox" href="javascript:;"><img src="/data/uploads/blog/flamingo/desert.jpg" /></li>
  <li>
    <a class="fbox" href="javascript:;"><img src="/data/uploads/blog/flamingo/sunflower.jpg" /></a>
  </li>
  <li>
    <a class="fbox" href="javascript:;"><img src="/data/uploads/blog/flamingo/jungle.jpg" /></a>
  </li>
  <li>
    <a class="fbox" href="javascript:;"><img src="/data/uploads/blog/flamingo/sun.jpg" /></a>
  </li>
  <li>
    <a class="fbox" href="javascript:;"><img src="/data/uploads/blog/flamingo/pelicans.jpg" /></a>
  </li>
  <li>
    <a class="fbox" href="javascript:;"><img src="/data/uploads/blog/flamingo/landscape.jpg" /></a>
  </li>
</ul>
```

## Style2 - add your tagline

For style2 you need to add a component and label it "tagline". Just enter in the text you wish to appear as your tagline, hit save and it will appear next to the site name you have given for your site (top left - mine is Portfolio &amp; Blog).

## Last but not least, charts

``` html
<dl class="chart">
  <dd>
    <p class="yellow" style="width:92%">
      <span class="label">Illustrator<span class="value">92%</span></span>
    </p>
  </dd>

  <dd>
    <p class="pink" style="width:84%">
      <span class="label">Photoshop<span class="value">83%></span>
    </p>
  </dd>

  <dd>
    <p class="yellow" style="width:80%">
      <span class="label">InDesign<span class="value">80%</span></span>
    </p>
  </dd>
</dl>
```
