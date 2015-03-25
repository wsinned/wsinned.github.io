---
layout: post
title: Dated File Generation
cover: cave-house.jpg
date: 2014-08-05 12:00:00
categories: scripting
---


## Reason

I like to keep things organised and I like to write regularly. Sometimes just my diary, sometimes blog posts, sometimes just brain dumping to clear my head.

One of my pet hates is having to create folders and files to match my preferred organising scheme: /2014/08/05-August.txt

## Solution

I wrote a simple bash script to automate this task. Bash isn't usually my first port of call, but ruby or any other scripting language seemed a little too much for such a simple task.

{% gist wsinned/2152f751a6158f18f2d0 %}

## Explanation

My favourite text editor is Vim, but when I am writing prose of any sort I much prefer [WriteRoom](http://www.hogbaysoftware.com/products/writeroom) (OSX only, sorry).

I keep all of my writing in my [Copy](https://copy.com?r=C3q4at) (similar to Dropbox but with more free space) folder.

The script is wrapped in a block redirecting to /dev/null to prevent it from echoing every step back to the command prompt.
