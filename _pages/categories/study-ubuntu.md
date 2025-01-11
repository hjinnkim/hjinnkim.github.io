---
title: "Study / Ubuntu"
permalink: /categories/studies/ubuntu
layout: archive
author_profile: true
sidebar_main: true
---

{% assign posts = site.tags['Ubuntu'] %}
{% for post in posts %} 
        {% include archive-single.html type=page.entries_layout %}
{% endfor %}