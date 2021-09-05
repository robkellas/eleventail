---
layout: layouts/post.njk
title: Create a Better Workflow To Capture and Organize Screenshots with Alfred for Mac
subtitle: How to change your default location for screenshots and create a better workflow to retrieve them with Alfred.
date: 2015-10-08
tags:
    - productivity
    - feature
    - post
---

> Update: Looks like an update that came out in August 2019 requires a redo of this process. Added another step to remove the screenshot delay caused by the screenshot floating thumbnail. See new optional last step.

If you find yourself taking copious amounts of screenshots on a Mac, you'll often find yourself with a desktop full of screenshot files. Not only that, but if you order your files by name and kind, you'll find that your most recent screenshots aren't always at the bottom of the list of files. Here's a workflow that has helped me using [Alfred](https://www.alfredapp.com).

## End Result: Type "SS " and get a quick list of your most recent screenshots

![Recent Screenshots Demo](/assets/img/content/screenshot-workflow.gif)

You can optionally continue to search by date. Hitting enter on any of these results will reveal it in Finder.

___

## 3 Step How-to

### 1. Create a new folder for your screenshots

First, create a folder on your mac where you'd like to store all of your screenshots. I've created a folder called "Screenshots" in the same directory as "Documents", "Music", Movies" etc folders.

![Screenshots folder location](/assets/img/content/screenshot-folder-location.png "screenshots folder location on mac")

### 2. Change the default location of where your screenshots get stored by entering the following commands in Terminal

Then open the Terminal app and enter in the text below and hit enter

``` bash
defaults write com.apple.screencapture location ~/Screenshots/
```

I like to put a name to my screenshots. To do this replace "Rob Johnson" with your name and paste that command line into terminal and hit enter.

``` bash
defaults write com.apple.screencapture name "Rob Johnson"
```

Now you just need to restart SystemUIServer to have the changes take effect.

``` bash
killall SystemUIServer
```

### 3. Download and add the Screenshot Workflow for Alfred

I've adapted a workflow that was created by [Vítor Galvão](http://vitorgalvao.com/), that lists the most recent download files in the "Screenshot" directory in descending order. You can also search for a specific date if you need to. For example, if I wanted to search for screenshots I took on Feb 21, I would type "SS 2016-02-21".

<a href="/assets/files/Recent-Screenshots.alfredworkflow" class="button download" id="Alfred Screenshot Workflow"><span class="icon is-danger"><strong>&#9660;</strong></span>Download Workflow</a>

### 4. Remove Delay from Screenshot Capture (optional)

Mac OSX has a new feature where you can markup your screenshots. Pretty great, but it does cause a delay in the screenshot appearing in your screenshot folder as it waits to see if you want to edit it before it saves.

To turn this off open the "Screenshot" app and deselect the option "Show Floating Thumbnail."

End result: Quickly retrievable screenshots that don't clutter your desktop! Hopefully this will help a few folks. If you have any feedback, you can contact me [@robkellas](https://twitter.com/robkellas).