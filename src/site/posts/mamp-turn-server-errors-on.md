---
layout: layouts/post.njk
title: Turning on Server Errors for MAMP
subtitle: How to turn server errors on Mamp
date: 2014-03-06
tags:
    - php
    - post
---

Debugging your apps can be a moneumental pain without proper logging or error reports. Mamp ships with errors turned off. Here is how you turn them back on.

Find out what version of PHP you are running on MAMP. You can find this by going to the MAMP preferences pain. Here you can see I'm using **5.5.3**

![MAMP preferences pain](/assets/img/content/mamp-preferences.jpg)

Now check your php.ini located at

``` ini
Applications/MAMP/bin/php/**php5.5.3**/conf/php.ini
```

Open it up in a text editor and search for `display_errors`

You are most likely seeing them turned "Off." Replace "Off" with "On" and you should be set.

Also, it's worth mentioning that you may have a ";" at the beginning of the line like so. ";" comments out lines which will set these preferences to defaults. What you should be ending up with is something like this.

```ini
; Print out errors (as a part of the output).  For production web sites,
; you're strongly encouraged to turn this feature off, and use error logging
; instead (see below).  Keeping display_errors enabled on a production web site
; may reveal security information to end users, such as file paths on your Web
; server, your database schema or other information.
display_errors = On
```

Now just restart your MAMP server and you should be good to go.
