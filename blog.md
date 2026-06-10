---
layout: page
title: Blog
permalink: /blog/
---

Notes and longer thoughts.

<table class="post-table">
  <thead>
    <tr><th>Date</th><th>Post</th></tr>
  </thead>
  <tbody>
    {% for post in site.posts %}
    <tr>
      <td style="white-space: nowrap; padding-right: 1.5em; color: #828282;">
        {{ post.date | date: "%b %-d, %Y" }}
      </td>
      <td><a href="{{ post.url | relative_url }}">{{ post.title }}</a></td>
    </tr>
    {% endfor %}
  </tbody>
</table>

{% if site.posts.size == 0 %}
*No posts yet.*
{% endif %}
