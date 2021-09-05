---
layout: layouts/post.njk
title: Spree Conf NYC 2014 Notes
subtitle: Notes taken from Spree Conf in 2014. A two-day conference in New York City focused on Spree, an open-sourced Ruby on Rails E-commerce platform
date: 2014-03-03
tags:
    - commerce
    - post
---

The company I work for is currently looking at alternative solutions for an E-commerce platform. Currently we are using a custom solution that is brittle from which we incur a lot of maintenance cost. Some of our shopping requisites for a potential new platform include the avoidance of: hefty monthly license fees, pay-per-order models, dependence on third-party developers for feature development. Yet maintain our current feature set and ideally stay within the rails environment. We're not picky at all...

## Looking for a "solved problem" solution

Spree has been growing for a number of years in popularity and claims to power [45,000 online stores](http://spreecommerce.com). With recent news that it has received a [$5 million in its latest round of funding](http://venturebeat.com/2014/02/25/spree-commerce-raises-5-million-for-open-source-e-tailer-software/), it would be daft to ignore the platform as a viable solution.

![SVA Theatre - Location of Spreeconf](/assets/img/content/sva-theatre.jpg)

[@thereddshift](https://twitter.com/thereddshift) and I attended the latest Spree conference in NYC to meet the Spree community and talk with the developers and companies that were using it. After all we want to ensure that this platform has a bright future and an invested developer community so we don't have to revisit this costly exercise in the near future.

## Top Tweets

<blockquote class="twitter-tweet" lang="en"><p>For good PR, your company needs a good story. No story = no PR, regardless of what agency you choose. <a href="https://twitter.com/search?q=%23spreeconf&amp;src=hash">#spreeconf</a> <a href="https://twitter.com/search?q=%23AndyDunn&amp;src=hash">#AndyDunn</a></p>&mdash; Rob Johnson (@robkellas) <a href="https://twitter.com/robkellas/statuses/438781547957997570">February 26, 2014</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p>&quot;Right decisions don&#39;t necessarily lead to good outcomes&quot; - Jeff Ma at <a href="https://twitter.com/search?q=%23spreeconf&amp;src=hash">#spreeconf</a></p>&mdash; Matthew Redd (@mingusredd) <a href="https://twitter.com/mingusredd/statuses/439091621830086656">February 27, 2014</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p>Amazon has attributed a 29% increase in sales since introducing product recommendation sections throughout the site. <a href="https://twitter.com/search?q=%23spreeconf&amp;src=hash">#spreeconf</a></p>&mdash; Rob Johnson (@robkellas) <a href="https://twitter.com/robkellas/statuses/438795568161513473">February 26, 2014</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p>Page speed matters: Yahoo improved page load time by 300ms and saw 9% increase in rev <a href="https://twitter.com/search?q=%23spreeconf&amp;src=hash">#spreeconf</a></p>&mdash; Rob Johnson (@robkellas) <a href="https://twitter.com/robkellas/statuses/438747310663954434">February 26, 2014</a></blockquote>

<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

## Day 1 (Wednesday)

### Sales tax

Use TaxCloud or AvaTax to calculate tax on orders

Tax is calculated per line item

Difference between tax. Included vs additional

###  Adjustments

Spree will automatically adjust your order to the max value promotion for the customer

###  Promotions

- You can combine promotions
- Not got credits yet
- Not got group discounts yet
- Supports vat refunding
- Almost 2000 test in spree

### Spree hub

A platform to decouple the backend processed from the storefront. 

###  The Hub

Working to make it easier
Want a plug and play solution for all platforms

  - Object
    - core of the system customers, carts, addresses etc
    - each object just requires an ID field.
  - Event
  - Webhook

###  Simplified API

>it's not about catching demand but creating demand. 

Delighted.com a customer opinion site that shows customer awareness??

Guide shops: build stores that you size up and then they ship to you. Hope to grow to 50 stores within three years. 

___

## Day 2 (Thursday)

### Daniel Honig (railsdog) & Thomas Von Deyen (Alchemy)

- E-commerce is a hard problem because everyone is doing something different.
- Grid based e-commerce is over
- Four pillars of ecommerce

  1. Commerce - this is 2014 and everything needs to be integrated. Flexible shopping cart and checkout experience supporting all channels
  2. Content - management systems built for rich storytelling and interactive experience
  3. Community
  4. Context - True personalization of experience, based on location, channel and relationship

#### Priorities for retailers

- Drive excitement for products
- Top brands must build a global halo for the brand thorough rich storytelling and address issues of heritage, craftsmanship and cultural relevance
- Create an unforgettable shopping experience

#### Why does storytelling matter

Customers are using more information than at any other time in history to make purchase decisions

A good storyteller has the power to transport audiences to another world
What I like is to feel emotion... and I agree even more when I see the sales results.

Why do we all like a good story?
- Escapism
- Exploration
- Identification
- Simulation
- Intimacy
- Comprehension
  - Stories run on the same hardware that process real events (only the frontal cortex differentiates it)
- Resolution

#### McKenzie Funnel (old model)

Awareness -> Familiarity -> Consideration -> Purchase -> Loyalty

#### McKenzie new model

Google ZMOT (zero model of truth) - http://www.thinkwithgoogle.com/collections/zero-moment-truth.html

New model:
- Stimulus
- zero moment of truth
- point of shelf
- purchase experience (missed this)

#### What's changed
- Noise - lots of noise, more than ever
- Two-way - social
- Consistency/Alignment
- Loyalty

Another step has been added

Emotion can trigger sales and loyalty

People will remember your story and forget your product

https://alchemy-cms.com/about

---

### Jeff Ma - basis for the main character for the book and movie of 21

Separate your decision from the outcome. You can make the right decision, and the outcome is irrelevant to whether that decision was correct or not.

> Group think: Making a decision based of consensus of the group in order to avoid conflict

Innovation requires making unpopular decisions

> Loss aversion: We are more impacted by a loss than a gain of the same amount.

> Endowment bias: When you become protective of what you have over getting more

---

### 

> Imagine spree as a service using the API.

Spree can just be a service

### Design and Designers

Decouple business logic from the experience is the next step

Going 100% JS (client-side)
SEO Implications:
Issues with indexing:

Prerender.io - gem "prerender_rails" every time it knows a bot is requesting a page it will prerender the page via phantom js and spit out the html server side.

---

### godynamo.com

- If people are clicking it, make it tappable
- Outbound links are a point of no return. Keep people in your site as much as possible.
- Instead of saying "cart" show a cart. 20% more clicks on images than words.
- Whats your favorite responsive framework? Bootstrap
- Why not native site verses a responsive site
- A/B testing with Optimizely

___

A random pic taken at the top of the Empire State Building.

![Empire State Plaza, Capitol](/assets/img/content/empire-state-plaza.jpg)
