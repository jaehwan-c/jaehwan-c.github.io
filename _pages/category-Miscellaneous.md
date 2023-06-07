---
title: "Miscellaneous"
layout: archive
permalink: /category/miscellaneous
author_profile: true
sidebar:
    nav: "docs"
---


{% assign posts = site.categories.Miscellaneous %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}