---
layout: page
class: home
css: ['pages/index.css']
title: About
description: 码农
keywords: Meng
comments: true
menu: 关于
permalink: /about/
---

红豆生南国，
春来发几枝，
愿君多采撷，
此物最相思。

## Contact

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
