---
title: Technical Resources
has_children: true
nav_order: 2
has_toc: false
---

# Technical Resources

Cloudbank's focus is on helping researchers and educators utilize the cloud as a tool for the work they're already doing. The resources in this section help you identify exactly how your work would look in the cloud, and the learning resources to get started on building it.

## Design Patterns

Here are some of the most common patterns we see appear in research cloud infrastructure. Follow the links for guides on how to map this work onto cloud services.

<div class="card-bin" markdown="0">

{% include icon_card.html
    link=""
    title="HPC"
    icon="pattern-hpc.png" %}

{% include icon_card.html
    link="patterns/deep-learning"
    title="ML and Deep Learning"
    icon="pattern-ml.png" %}

{% include icon_card.html
    link=""
    title="IOT"
    icon="pattern-iot.png" %}

{% include icon_card.html
    link=""
    title="Genomics"
    icon="pattern-genom.png" %}


{% include icon_card.html
    link=""
    title="Science Gateways"
    icon="pattern-gateway.png" %}


{% include icon_card.html
    link=""
    title="Licensed software platforms"
    icon="pattern-sw.png" %}

{% include icon_card.html
    link=""
    title="Security-sensetive data"
    icon="pattern-security.png" %}

</div>

## Solutions

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

## Terms and Jargon

Although all cloud platforms offer roughly equivalent functionality, the exact terminology and services available for a given use case differ across them. Below are references to help fill in the gaps.

- [Vendor comparison chart](https://www.cloudbank.org/cloudbank-catalog)

    A chart laying out the equivalent product brand names for most common cloud serivces, across all platforms supported by CloudBank

- [Glossary](https://www.cloudbank.org/cloud-terms)

    A glossary of common terms that you'll see across clouds


