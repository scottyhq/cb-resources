---
title: Solutions
parent: Technical Resources
has_children: true
nav_order: 4
has_toc: false
---

# Solutions

Cloudbank Solutions are researcher-oriented tutorials written by the Cloudbank team. Follow the links below to access hands-on guides for how to get the most out of your cloud access.

<div class="card-bin" markdown="0">
{% assign subpages = site.pages | where: 'parent', 'Solutions' %}
{% for subpage in subpages %}
    {% capture subpage_url %}{{ site.baseurl }}{{ subpage.url }}{% endcapture %}
    {% include icon_card.html
        link=subpage_url
        title=subpage.title
        icon=subpage.icon %}   
{% endfor %}
</div>
