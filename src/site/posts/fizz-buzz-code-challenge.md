---
layout: layouts/post.njk
title: How to Solve the Fizz Buzz Code Challenge
subtitle: How to solve a common programming question often brought up during the interview process
date: 2014-10-06
tags:
    - tutorial
    - javascript
    - post
---

Below is a common problem that has been used to test developer candidates during job interviews. It's a good problem to review and check to see if you understand some of the basic javascript concepts. Heads up, this is probably very old and not likely asked anymore in 2020 :)

## Problem

> Write a program that prints the numbers from 1 to 100. But for multiples of three print 'Fizz' instead of the number and for the multiples of five print 'Buzz'. For numbers which are multiples of both three and five print 'FizzBuzz'.

## Helpful Resources

1. Fizz Buzz tutorial via [c2.com](http://www.c2.com/cgi/wiki?FizzBuzzTest)
2. The FizzBuzz Kata, [using Ruby](https://www.youtube.com/watch?v=CHTep2zQVAc)

___

## Answer
``` javascript
// Start the count from 1. Limit to 100.
for (var  i = 1; i <= 100;) {
  // Define write conditions based off divisibility
  document.write((i % 3 ? "" : "Fizz") + (i % 5 ? "" : "Buzz") || i)
  // Add line breaks for display purposes
  + document.write("<br>");
  //Increment i for next loop
  i++;
}
```

## Example output of the above code
``` text
1
2
Fizz
4
Buzz
Fizz
7
8
Fizz
Buzz
11
Fizz
13
14
FizzBuzz
16
17
Fizz
19
Buzz
Fizz
22
23
Fizz
Buzz
26
Fizz
28
29
FizzBuzz
31
32
Fizz
34
Buzz
Fizz
37
38
Fizz
Buzz
41
Fizz
43
44
FizzBuzz
46
47
Fizz
49
Buzz
Fizz
52
53
Fizz
Buzz
56
Fizz
58
59
FizzBuzz
61
62
Fizz
64
Buzz
Fizz
67
68
Fizz
Buzz
71
Fizz
73
74
FizzBuzz
76
77
Fizz
79
Buzz
Fizz
82
83
Fizz
Buzz
86
Fizz
88
89
FizzBuzz
91
92
Fizz
94
Buzz
Fizz
97
98
Fizz
Buzz
```