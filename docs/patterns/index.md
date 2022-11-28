---
title: Design Patterns
parent: Technical Resources
nav_order: 2
has_children: true
has_toc: false
---

# Design Patterns

Here are some of the most common patterns we see appear in research cloud infrastructure. Follow the links for guides on how to map this work onto cloud services.

<div class="card-bin" markdown="0">
{% assign subpages = site.pages | where: 'parent', 'Design Patterns' %}
{% for subpage in subpages %}
    {% capture subpage_url %}{{ site.baseurl }}{{ subpage.url }}{% endcapture %}
    {% include icon_card.html
        link=subpage_url
        title=subpage.title
        icon=subpage.icon %}   
{% endfor %}
</div>
