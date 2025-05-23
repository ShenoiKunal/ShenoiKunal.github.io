---
layout: default
title: Your Post Title Here
date: {{date}}
categories:
  - category1
  - category2
tags:
  - tag1
  - tag2
  - tag3
author: Your Name
image: /assets/images/featured-image.jpg
description: A brief description of your post that will appear in search engines and social media shares.
permalink: /custom-url-slug/
published: true
comments: true
nav_exclude: true
---

# {{ page.title }}

*Published on {{ page.date | date: "%B %d, %Y" }}*

## Introduction

Write your introduction here. This is where you should hook your readers and provide a brief overview of what the post will cover.

## Main Content Section

This is where the main content of your post goes. You can use standard markdown formatting:

### Subheading

Text with **bold**, *italic*, or ***bold and italic*** formatting.

> This is a blockquote. Great for highlighting important information or quotes.

#### Lists

Unordered list:
- Item 1
- Item 2
- Item 3

Ordered list:
1. First item
2. Second item
3. Third item

#### Code Blocks

Inline code: `var example = "hello world";`

```javascript
// Code block with syntax highlighting
function exampleFunction() {
  console.log("Hello, Jekyll!");
  return true;
}
```

#### Links and Images

[Link text](https://example.com)

![Alt text for image]({{ site.baseurl }}/assets/images/example-image.jpg)
*Caption for the image*

#### Tables

| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Cell 1   | Cell 2   | Cell 3   |
| Cell 4   | Cell 5   | Cell 6   |

## Another Section

Continue with more sections as needed.

### Including Jekyll Variables and Liquid Tags

You can use Jekyll's Liquid templating:

{% if site.related_posts.size > 0 %}
<h2>Related Posts</h2>
<ul>
  {% for post in site.related_posts limit:3 %}
  <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
{% endif %}

## Conclusion

Summarize your post and possibly include a call to action or questions to engage readers.

---

{% include author-bio.html %}

{% if page.comments %}
  {% include comments.html %}
{% endif %}

{% include related-posts.html %}

<!-- Additional scripts or styles specific to this post -->
<script src="{{ site.baseurl }}/assets/js/specific-script.js"></script>
