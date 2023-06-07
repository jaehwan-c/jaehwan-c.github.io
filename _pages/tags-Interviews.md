---
title: "Interviews"
layout: archive
permalink: /categories/interview
author_profile: true
sidebar:
    nav: "docs"
---

{% assign posts = site.categories.Interviews %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}