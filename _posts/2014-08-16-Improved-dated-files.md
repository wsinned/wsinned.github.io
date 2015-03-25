---
layout: post
title: Improved Dated Files
cover: cave-house.jpg
date: 2014-08-16 12:00:00
categories: scripting
---


## It got me thinking

I also like to keep things simple, and after publishing the
last post I got thinking about how the script looked like my thought
process of how to solve the problem. Good start, but there are always
tricks up the sleeve of the \*nix toolkit.

Opening up the man page for mkdir led me to the forgotten -p option.
This will create the directory structure required, and not complain if
it already exists.

## Improved solution

I also realised that I was suppressing output when I didn't need to. I had added the -x switch to the shebang line, causing bash to echo every command. After removing the switch I then got rid of the redirection to /dev/null

{% gist wsinned/4eb0a6262b2550ed3d6b %}

## Result!

This is simpler and still does exactly what I wanted. A single command
in my ~/bin folder means I can start or continue today's entry from
anywhere in the shell whenever I feel the need to write.
