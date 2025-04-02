---
layout: post
title: "Windows"
date: 2025-04-02 14:30:00 -0500
categories: [home]
tags: [windows]
author: Kunal Shenoi
---

# {{ page.title }}

*Published on {{ page.date | date: "%B %d, %Y" }}*

*Tags: {{ page.tags }}*

## Introduction

Primary landing page for Windows-related articles

## Articles

| Title | Description | Tags |
|-------|-------------|------|{% for page in site.pages %}{% if page.parent == "Windows" %}
| [{{ page.title }}]({{ page.url }}) | {{ page.description | default: "No description" }} | {% if page.tags %}{{ page.tags | join: ", " }}{% else %}No tags{% endif %} |{% endif %}{% endfor %}
