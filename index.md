---
layout: default
title: Welcome!
description: This is my site. Welcome.
---

{% assign page = site.posts.first %}
{% assign content = page.content %}
{% include post.html %}
