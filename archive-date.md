---
layout: page
title: Blog Archive
---

<a href="/archive">Sort by category</a>

{%- assign allposts = site.posts | sort_natural: "date" | reverse %}

{%- assign postsByYearMonth = allposts | group_by_exp:"allposts", "allposts.date | date: '%B %Y'"  %}

{%- for yearMonth in postsByYearMonth %}
<h3>{{ yearMonth.name }}</h3>
<ul>
  {%- for post in yearMonth.items %}
  <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {%- endfor %}
</ul>
{%- endfor %}
