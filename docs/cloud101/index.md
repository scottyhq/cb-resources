---
title: Cloud 101
has_children: true
nav_order: 1
has_toc: false
---

# Cloud 101

Our Cloud 101 video series is a conceptual introduction to the cloud from a researcher's perspective. The series is split into six parts, reflecting at a coarse grain the topics that we keep in mind when using the cloud for a project.

These videos cover concrete examples, but stay at a relatively high level to keep at an informative pace. For more detailed walkthroughs of technical content, see our [Cloudbank Solutions](../technical-resources/solutions).

<div class="card-bin" markdown="0">
{% assign subpages = site.pages | where: 'parent', 'Cloud 101' | sort: "nav_order" %}
{% for subpage in subpages %}
    {% capture subpage_url %}{{ site.baseurl }}{{ subpage.url }}{% endcapture %}
    {% include icon_card.html
        link=subpage_url
        title=subpage.title
        icon=subpage.icon %}   
{% endfor %}
</div>
