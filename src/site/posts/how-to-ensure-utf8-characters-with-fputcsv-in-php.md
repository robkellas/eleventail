---
layout: layouts/post.njk
title: How to Ensure UTF8 Characters with fputcsv in PHP
subtitle: Even when you config php.ini to output UTF8 characters for your PHP instance, you still need to ensure the fputcsv() function is outputting UTF8 characters
date: 2019-11-25
tags:
    - tutorial
    - php
    - post
---

I was working on a project where I transform data, create and load a CSV for another system to process. The issue was non-conforming characters were breaking the endpoint system. I ensured that my php.ini configuration had the UTF8 character set defined as default, however the output from fputcsv() was still outputting non-UTF8 characters.

I found the a [Stack Overflow conversation](https://stackoverflow.com/questions/21988581/write-utf-8-characters-to-file-with-fputcsv-in-php) and saw that the fix was to issue the following definitions in the fputcsv() function.

```php
fprintf($df, chr(0xEF).chr(0xBB).chr(0xBF));
```

The line `fprintf($df, chr(0xEF).chr(0xBB).chr(0xBF));` writes file header for correct encoding.

Thanks goes to [Hardy](https://stackoverflow.com/users/2611927/hardy).
