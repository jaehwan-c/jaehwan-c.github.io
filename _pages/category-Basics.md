---
title: "Basics"
layout: archive
permalink: /categories/basics
author_profile: true
sidebar:
    nav: "docs"
---


{% assign posts = site.categories.Basics %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}