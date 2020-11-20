---
---

{% for post in site.posts %}
{% capture postdate %}{{ post.date | date: '%Y-%m-%d'}}{% endcapture %}

{% if prepostdate != postdate %}

{{ post.date | date:"%b %-d, %Y"}}

{% endif %}

### [{{ post.title }}]({{ post.url | relative_url }})

{% capture prepostdate %}{{post.date | date: '%Y-%m-%d'}}{% endcapture %}

{% endfor %}
