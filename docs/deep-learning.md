---
layout: tutorial
title: Deep Learning in the cloud
icon: "static/learning.png"
---

The cloud is particularly well suited to training neural networks (or any other form of ML model); it can provide as much computational power as you need, when you need it, and you pay for only what you use. To read about success stories, check out [&sect;Case Studies](#case-studies). To learn more, read on.

The major pieces of instracture to think about for such a workload include:

- **Development / Experimentation**

    ML models are usually developed in notebook environments like Jupyter, on smaller subsets of the complete data set. All major cloud platforms can host python notebooks for you; that is, you upload your notebook file and are given a URL to access the notebook interface without further setup on your end. That said, it may be more cost effective to do this on your own computer.

    See [&sect;Notebooks](#notebooks) for resources on running your notebooks in the cloud.

- **Job submission**

    Once you like how your model runs on subsets of the data, it's useful to train it on the full dataset in a non-interactive environment that may run for a couple of hours or days. This is where the cloud really shines, using container technologies like Docker or Singularity. On your local computer you package training script and any libraries it depends on into a container, and then submit the container to be run in the cloud like an HPC job. This is where you can ask for beefy compute power with GPUs attached, which are costly but only charged for the precise amount of time needed for the job to run.

    See [&sect;Containers](#containers) for resources on packaging your work into containers and submitting them to the cloud.

- **Dataset storage**

    In order to compute effectively, cloud platforms need your data sets to be stored in-cloud alongside your code. The most straight-forward way to do this is to use your cloud platform's file storage service, which behaves much like Google Drive or Dropbox. To dramatically reduce storage cost, you can instead use your platform's *object* storage service. This indexes your data into a way that is internally more efficient, but slightly less convenient to access.

    See [&sect;Storage](#storage) for resources on getting your data into file or object storage accounts.

- **Deployment**

    TODO

    See [&sect;Deployment](#deployment) for resources on making your models available to others as a service after they're mature.

## Guides

### Notebooks

There are two routes to go for serving notebooks from the cloud:

- **Notebook services**

    Most cloud platforms provide a notebook service that allows you to upload your python notebooks and work with them through your browser without any further configuration. These services are usually packaged within a larger Machine Learning-focused workspace, that tends to be cost-free to set up and intended to be an onramp to platform-specific ML tools. That said, the biggest benefit of these services are the cost savings; they have the potential to be [TODO: figure] cheaper.

    Guides:

    - AWS: [Notebooks with Sagemaker](https://docs.aws.amazon.com/sagemaker/latest/dg/gs-setup-working-env.html)

    - Azure: [Notebooks with Azure Machine Learning Studio](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-run-jupyter-notebooks)

    - GCP: [Notebooks with Jupyterlab](https://cloud.google.com/notebooks). Google also offers a separate service, [Google Colab](https://colab.research.google.com/notebooks/), which currently has a resource-limited free tier and a more feature-rich pro tier.

    - IBM: TODO


- **Virtual Machines**

    Like running Jupyter on your own local computer, you can create a virtual machine hosted in the cloud that runs a Jupyter Notebook server. Most cloud platforms provide Linux VMs that come pre-installed with Anaconda, Jupyter and other common data science libraries.

    This method is the most flexible and consistent across cloud platforms, but also the most costly; you're paying for the notebook server even when you're not using it. For ways to mitigate VM costs, check out our [cost mitigation guide](cost-mitigation.md#vms)

    Guides:

    - AWS: [Deep Learning AMI](https://docs.aws.amazon.com/dlami/latest/devguide/what-is-dlami.html)

    - Azure: [Data Science Virtual Machine (DSVM)](https://azure.microsoft.com/en-us/services/virtual-machines/data-science-virtual-machines/)

    - GCP: [Deep Learning VM Image](https://cloud.google.com/deep-learning-vm/docs/)

    - IBM: TODO


### Containers

Cloud platforms often provide services to run Docker containers without needing to set up a full virtual machine to do so (this is one of the common things referred to by "serverless" computing). Because Docker containers are often used for running web app infrastructure, this is what cloud services and documentation are geared towards. That said, they can just as well be used for long-running machine learning jobs. Cloud platforms are also beginning to offer ML-focused container services as well, usually under different names than the generic container jazz.

Guides:

- AWS: [Hands-on: train a deep learning model](https://aws.amazon.com/getting-started/hands-on/train-deep-learning-model-aws-ec2-containers/)

- Azure: [Train a model by using a custom Docker image](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-train-with-custom-image)

- GCP: [AI Platform Containers Overview](https://cloud.google.com/ai-platform/training/docs/containers-overview)

- IBM: TODO


### Storage

TODO

### Deployment

TODO

## Case Studies

- https://www.cloudbank.org/training/use-case/supervised-machine-learning