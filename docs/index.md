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

<!-- - **Job-based computation**

  Submitting computational workloads to run on powerful computers; HPC clusters

- **Data exploration**

  Working with code and data interactively to explore the properties of a data set

- **ML and Deep Learning**

  TODO: Basically data exploration

- **Data dissemination**

  Making a dataset available for colleagues outside of your lab to use; science gateways

- **Web applications**

  Self-hosting web applications for your lab or research community to use; ML annotation software

- **Working with privacy-sensitive data**

  Government compliance and standards; securing data
 -->

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

## Best Practices

Once you have access to a cloud account, it can feel like there is a lot to keep track of! To help out your transition from cloud novice to master, Cloudbank has put together a downloadable onboarding document that organizes the salient points that cloud users and administrators should keep in mind throughout their work:

<div class="icon-buffeted-list" markdown="1">
<img alt="Cloud checklist icon" src="{{ site.baseurl }}/static/checklist.png" style="width: 5em">

- [Cloudbank Onboarding Protocol](https://www.cloudbank.org/docs/onboarding) _(pdf)_
    - [Appendix: Operations](https://www.cloudbank.org/docs/onboarding/ops) _(pdf)_
    - [Appendix: Security and Costing](https://www.cloudbank.org/docs/onboarding/costsec) _(pdf)_

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

## Platform-specific resources

Although all cloud platforms offer roughly equivalent functionality, the exact terminology and services available for a given use case differ across them. Below are references and guides tailored to the specific platforms supported by CloudBank.

<div class="card-bin" markdown="0">

    {% include icon_card.html
        link="platforms/aws"
        title="Amazon AWS"
        subclass="icon-borderless"
        icon="aws-logo.png" %}   


    {% include icon_card.html
        link="platforms/gcp"
        title="Google GCP"
        subclass="icon-borderless"
        icon="google-cloud-platform.png" %}   


    {% include icon_card.html
        link="platforms/aws"
        title="Microsoft Azure"
        subclass="icon-borderless"
        icon="microsoft-azure.png" %}   


    {% include icon_card.html
        link="platforms/ibm"
        title="IBM Cloud"
        subclass="icon-borderless"
        icon="ibm-cloud.png" %}   


</div>

