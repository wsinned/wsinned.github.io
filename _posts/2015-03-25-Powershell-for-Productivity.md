---
layout: post
title: Draft Post
cover: cave-house.jpg
date: 2000-01-01
categories: 
---


## PowerShell for Productivity

I have been meaning to write a post on PowerShell for a while now. I have been using it for a few years, and while there is a bit of a learning curve to it, there is a lot to be said for replacing your standard command prompt for a PowerShell prompt on Windows and diving in.
hiphip
I spend a lot of time in the Windows command line as part of my job, and because I prefer terminal driven development to GUI driven development a lot of the time. Visual Studio is a great and powerful tool, but there a re plenty of things I can do without paying the tax of opening such a large application and loading the bits and pieces (Resharper) that make it so useful. 

So, on the getting productive with PowerShell. Firstly, there are aliases. Nothing too special here, lots of environments have them, it is just good to point it out to anyone who didn't realise that they existed, because it beats creating batch files with easy to type names that just wrap a difficult to type command. 

My most productive aliases are v to open GVim and g to run git commands. I really do type g so many times a day that the saving of an extra 2 keystrokes makes a difference, to my sanity at least, if not to my productivity. The next most important one is e to open explorer. I can type e . And open it in the current folder for those times when a GUI view of the file system is essential. 

To add aliases, simply open your PowerShell profile ...add default path...  And add a line per alias just like this...


Sine I have already mentioned git, it is time to introduce PoshGit. Life in the Windows command line would be unbearable without it. Apart from ....

Setting up PoshGit is as simple as using PsGet. What? You don't have PsGet yet?

Getting PsGet is as simple as....

Now that we have PoshGit installed, and have opened a new PowerShell session we can tweak how we see our git repos in PowerShell. ....

The next area that PowerShell gives and keeps on giving, is with access to things that were just more of a pain in the old fashioned command line. I start and stop services regularly during development and testing, and I used to either have to opend services.msc and have yet one more window cluttering my working environment, or to type this a lot

Net start my.very.long.fullyqualified.service.name

The same goes for stopping. 

In PowerShell there are a few ways to do this, and depending on your workflow, they all make life a little easier. 

If I need to reference a service and work with it a lot I can use:

$s = get-service my.very.....

$s.Start(), $s.Stop() work now as very handy persistent shortcut for the remainder of the session. Don't be tempted to run $s on its own, to be the status of the service. The state is captured at the time of assigning the variable and doesn't update after other operations are run on it. 

The trick is that I used the built in tab completion to complete the get-service command so only typed get-s[tab]. Depending on what you have installed in PowerShell, you may have to press tab a few times to get to the command you need, but if you use it a lot you can alias it! After my command was completed, I typed enough of my service name for it to be unique and pressed tab again, completing the name. This only works if you have PoshGit installed as it includes some great tab-completion extras. It may be possible to get this without installing the whole of PoshGit, but I am assuming that developers will be reading this and everyone's using git, right?

If you won't be starting and stopping the same service frequently, then you can make use of the start-, stop-, and restart- verbs in front of service to do one off operations, still benefiting from the tab completion to make life simpler. 

Finally, there is get-child-item, aliased as gci by PowerShell. This is a command with many facets, and is one of the most versatile parts of PowerShell. Type it on its own, and it is the humble dir or ls command form DOS or Linux, in fact both of those are aliases for gci. The power appears when you want to recursively find all of the json files in a project folder, including the subfolders. 

Gci -recursive -include *.json . Returns a list of file system objects that can be captured in a variable, iterated over, manipulated, you name it. You can even just write the list to screen if you are old fashioned. 





