---
layout: post
title: Automated Blogging
cover: cave-house.jpg
date: 2014-08-17
categories: scripting
---


<s>Lazy</s> Good Coders Automate Everything

I have an aversion to any sort of repetitive task. I generally start to worry that I will not repeat the task in exactly the same way, resulting in minor variations from the procedure. Inconsistency bothers me. That probably says a lot about me. Probably more than I should be telling...

So after a couple of iterations of anything I will try to find a way to script it or automate it in some way.

After writing two blog posts about my automated writing workflow, I realised that my blogging was very repetitive. Typical!

So I applied the same logic to the blogging that I had applied to the writing and came up with this

{% highlight Bash %}

#!/bin/bash
# open or create blog post for today
# uses or creates markdown file for current date with supplied title.

rootPath=~/code/blog/
draftPath=$rootPath/_drafts
postPath=$rootPath/_posts

if [ $# -eq 0 ]
then
  echo "No post name specified. Current drafts are:"
  ls $draftPath
  exit 1
fi

thisYear=$postPath/$(date +"%Y")
thisMonth=$thisYear-$(date +"%m")
fileName=$thisMonth-$(date +"%d")-$1.md

if [ ! -e $fileName ]
then
  cp $draftPath/new-post.md $fileName
fi

echo "Opening $fileName in WriteRoom..."
open -a WriteRoom $fileName

{% endhighlight %}

<br/>

### Conclusion

Now I have a nifty  method of getting a new post up and running.

{% highlight Bash %}
~ new-post my-post-title
{% endhighlight %}

