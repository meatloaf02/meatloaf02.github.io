---
layout: home
title: Welcome to My Blog
subtitle: Insights on AI, Consulting, Higher Ed
author: Marco Siliezar
pagination:
  enabled: true
  collection: posts
  per_page: 5
---

# Latest Blog Posts

Welcome to my personal blog where I share insights.

{% for post in paginator.posts %}
## [{{ post.title }}]({{ post.url }})
*Posted on {{ post.date | date: "%Y-%m-%d" }}*

{{ post.excerpt }}

[Read more]({{ post.url }})
---
{% endfor %}

## Featured Topics
- [Cloud Computing](/tags/cloud)
- [Artificial Intelligence](/tags/ai)
- [Web Development](/tags/dev)

[View All Archives](/archives)
