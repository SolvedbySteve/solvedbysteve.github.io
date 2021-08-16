---
layout: tags
title: "Tags"
permalink: /tags/
header:
  image: "/images/nola-about2.jpg"
---
{% for tag in site.tags %} [{{ tag\[0\] }}](#{{ tag[0] | slugify }}) {% endfor %}

* * *

{% for tag in site.tags %}

{{ tag\[0\] }}
--------------

{% for post in tag\[1\] %}[*   {{ post.title }} {{ post.date | date\_to\_string }}]({{ site.baseurl }}{{ post.url }}){% endfor %}

{% endfor %}
