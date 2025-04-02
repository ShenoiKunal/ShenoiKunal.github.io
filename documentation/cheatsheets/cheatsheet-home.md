---
layout: post
title: Cheatsheets
date: 2025-04-02
tags:
  - cheatsheets
author: Kunal Shenoi
description: Collection of cheatsheets and references for services and languages
---


# {{ page.title }}

*Published on {{ page.date | date: "%B %d, %Y" }}*

*Tags: {{ page.tags | join: ", " }}*

## Introduction

Primary landing page for a collection of cheatsheets and references for different services and languages.

## Directions

| Title | Description | Tags |
|-------|-------------|------|{% for page in site.pages %}{% if page.parent == "Cheatsheets" %}
| [{{ page.title }}]({{ page.url }}) | {{ page.description | default: "No description" }} | {% if page.tags %}{{ page.tags | join: ", " }}{% else %}No tags{% endif %} |{% endif %}{% endfor %}
