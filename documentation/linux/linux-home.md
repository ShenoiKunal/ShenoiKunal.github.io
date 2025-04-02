---
layout: post
title: "Linux"
date: 2025-04-02
categories: [home]
tags: [linux]
author: Kunal Shenoi
---

# {{ page.title }}

*Published on {{ page.date | date: "%B %d, %Y" }}*

*Tags: {{ page.tags | join: ", "  }}*

## Introduction

Primary landing page for Linux related articles

## Articles

| Title | Description | Tags |
|-------|-------------|------|{% for page in site.pages %}{% if page.parent == "Linux" %}
| [{{ page.title }}]({{ page.url }}) | {{ page.description | default: "No description" }} | {% if page.tags %}{{ page.tags | join: ", " }}{% else %}No tags{% endif %} |{% endif %}{% endfor %}
