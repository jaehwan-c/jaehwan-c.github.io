---
title: "Basics"
layout: archive
permalink: /tags/Basics
author_profile: true
sidebar:
    nav: "docs"
---

{% assign posts = site.tags.Basics %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}