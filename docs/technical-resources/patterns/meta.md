---
title: Meta
parent: Design Patterns
icon: pattern-security.png
grand_parent: Technical Resources
---

# Meta

We include this page for common meta-practice / meta-design. We try to describe *what* is useful and *how* one learns 
to use it. The theme is collaborative data science irrespective of cloud computing. Research computing methods advocacy
is a broader topic includes cloud but many other tools and technologies and methods besides. 


## Learning curves and time investment

These topics may seem daunting. They tend to make sense with some dedicated time and effort. As with
research there is no substitute for clearing away distractions and focusing for an 
extended period (from minutes to months depending) developing new skills. The good news is that
we are in a "boom" era for documentation, tutorials, knowledge bases and debugging tools. 
For example it is now common practice -- given a bug some code -- to paste the error message into a browser 
and shortly arrive at 
a good solution to the problem on Stack Overflow. YouTube abounds with walk-throughs and tutorials; 
and resource books such as the Python Data Science Handbook can be found online at no cost. 
 

## Help us curate good training resources

Suppose you are interested in analyzing dolphin vocalizations. 
You may find a training series on YouTube; or you may create one. 
As a more concrete example we found
[this](https://regexone.com/) training resource on learning how to work with **regular expressions**. Super powerful
if you work with text interpretation and parsing, say in NLP. So we put that link below. If you 
find a good resource: Tell us so we can put it here!

## Linux

## The shell

## Software versioning and `git`

## GitHub and GitLab

## Jupyter notebooks

## Environments and Environment Bootstrapping

## Binder sandboxes

## Integrating and harmonizing notebooks: Jupyter Books

## More on git: Clones in relation to Forks

## More on Jupyter 1: Commonly used Python libraries for meta-tasks

- Pickle: An easy way to share data structures across notebooks. [Here is an introductory blog.](https://betterprogramming.pub/dont-fear-the-pickle-using-pickle-dump-and-pickle-load-5212f23dbbce)
    - Notebooks tend to grow until they become unwieldy. A natural step is to break them down into smaller conceptual blocks. 
    - Pickle facilitates 'picking up where the previous notebook left off'.
    - Warning: Not necessarily cross-platform
    - Rejoice: Checkpointing! Cloud cost savings!
- Up your game to JSON!

## More on Jupyter 2: Magic

## More on Jupyter 3: Hubs, Littlest

## More on Jupyter 4: How the cloud plays in

## Jupyter for Education

[Here](https://jupyter4edu.github.io/jupyter-edu-book/case-studies.html)

## Natural Language Processing 1: Where to begin

- The Jupyter environment as noted is suitable for many data science sub-disciplines. Natural Language Processing (NLP) is no exception.
    - [This tutorial video provides an excellent introduction to the topic]

- 

## Regular Expressions

A regular expression is a formalism for parsing text. Learn how to do it [here](https://regexone.com/).

## The Cloud 1: What are the key differences?

## The Cloud 2: How do I move data between clouds?

* [AWS-to-Azure via AzCopy](https://azure.microsoft.com/en-us/blog/move-your-data-from-aws-s3-to-azure-storage-using-azcopy/)
and [via Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/data-migration-guidance-s3-azure-storage)
* [GCP-to-Azure via AzCopy](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-google-cloud) and 
[via Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/connector-google-cloud-storage?tabs=data-factory)
* [GCP-to-AWS](https://aws.amazon.com/blogs/big-data/migrate-terabytes-of-data-quickly-from-google-cloud-to-amazon-s3-with-aws-glue-connector-for-google-bigquery/)
* [Azure-to-AWS](https://aws.amazon.com/blogs/storage/one-way-to-migrate-data-from-azure-blob-storage-to-amazon-s3/)
* [AWS-to-GCP](https://cloud.google.com/architecture/transferring-data-from-amazon-s3-to-cloud-storage-using-vpc-service-controls-and-storage-transfer-service)
* [Azure-to-GCP](https://cloud.google.com/migrate/compute-engine/docs/4.5/how-to/migrate-azure-to-gcp/overview)
