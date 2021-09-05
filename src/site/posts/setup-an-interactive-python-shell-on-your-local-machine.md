---
layout: layouts/post.njk
title: Setup an Interactive Python Shell on Your Local Machine
subtitle: A tutorial that covers some of the basics to help you start playing around with Python
date: 2021-08-16
tags:
    - tutorial
    - python
    - aws
    - feature
    - post
---

Python has gained support and popularity in the development space due to its powerful ability to perform statistical calculations along with enterprise support from the likes of AWS in using it for ETL processes.

Below is an overview of the basics, along with a lambda script overview.

## Bash Terminal

First open up a [bash shell if you're on Windows](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/), or the "Terminal" application if you're on Mac OS.

Find out the version you're running by entering:

```bash
python --version
```

Currently, I'm using `Python 2.7.18`

Anything 2+ is fine for what we're doing. If you need to upgrade, [download the latest release here](https://www.python.org/downloads/).

## Keeping it Simple with iPython

iPython is an enhanced interactive shell for Python. It adds improved syntax highlighting and tools far beyong what we're doing today, but worth installing to improve the developer experience. Additional benefits include ([from project homepage](https://ipython.org)):

- A powerful interactive shell.
- A kernel for Jupyter.
- Support for interactive data visualization and use of GUI toolkits.
- Flexible, embeddable interpreters to load into your own projects.
- Easy to use, high performance tools for parallel computing.

Type the following to initiate the install

```bash
pip3 install ipython
```

Verify the location of your install

```bash
which ipython
```

You should see something like `~/local/bin/ipython`

Now start the shell by simply typing

```bash
ipython
````

This has now initiated an interactive shell for Python using iPython. You can enter the code you want to use in your script.

Now you could type Python as you would expect. Simple example below (hit enter after each line):

```python
order_1 = 120
order_2 = 300
total = order_1 + order_2
# total would now equal 420
# show the variable value by typing the variable name and hitting enter
total
```

This is a simple way to get started with Python and learn the basics.

You can also `import` packages that will help you do more such as [boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html), which is the Amazon Web Services (AWS) Software Development Kit (SDK) for Python.

```bash
import boto3
```

## Fun bonus for AWS

You can follow the same process here for an AWS Cloud Shell which is located at the top right of the AWS Console. This Cloud Shell will inherit all the same permissions that your AWS Console user has.
