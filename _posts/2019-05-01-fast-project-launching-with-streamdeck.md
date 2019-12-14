---
layout: post
title: "Fast project launching with Stream Deck"
tags: [productivity, tutorial, hardware]
comments: true
---

Getting started is the hardest part.

This is something that rings true with our team. With a default to action mindset, even _starting_ to work on a project can be a task. We've built a boilerplate to quickly launch SaaS applications -- here's a quick guide to make managing and working on multiple applications a bit easier right away.

## Elgaot Stream Deck

Using Elgato's Stream Deck, we get a sweet piece of hardware with great capabilities. Using their software, you can map system functions to specific keys, one of which being launch an application.

For this tutorial, we'll build a small application written in `applescript`, using Mac's `Automator`. Our `applescript` will also leverage [iTermocil](https://github.com/TomAnthony/itermocil) to manage our project's system processes in `iTerm`.

The goal of our application is to

* Start our application's local dev server
* Start our `app` `firebase` cloud functions
* Start our `app` `Storybook` instance
* Open our project in `SublimeText`

## Applescript

```
on run {input, parameters}
	set projectDir to "~/Desktop/stuff/01_projects/04_one-a-day"
	set {width, height, scale} to words of (do shell script "system_profiler SPDisplaysDataType | awk '/Built-In: Yes/{found=1} /Resolution/{width=$2; height=$4} /Retina/{scale=($2 == \"Yes\" ? 2 : 1)} /^ {8}[^ ]+/{if(found) {exit}; scale=1} END{printf \"%d %d %d\\n\", width, height, scale}'")
	
	tell application "iTerm"
		set newWindow to (create window with default profile)
		tell current session of current window
			write text "cd " & projectDir & "; itermocil --here " & projectDir & "/oad-iTermocil; code " & projectDir & "/oad-web"
		end tell
	end tell
	tell application "iTerm"
		set bounds of front window to {0, 0, width, height}
	end tell
	return input
end run
```

## [iTermocil]()
```
windows:
  - name: _main_horizontal_4_panes
    root: ~/Desktop/stuff/01_projects/01_stockstreamtv/web-app
    layout: main-horizontal
    panes:
      - git status
      - npm run start
      - npm run emulate:fbcf
      - npm run storybook
```

## Stream Deck Setup

1) Install & Run Stream Deck's software
2) 
