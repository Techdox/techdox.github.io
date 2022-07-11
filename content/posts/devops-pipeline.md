---
title: "Devops Pipeline"
date: 2022-07-11T17:18:50+12:00
draft: false
description: "My Azure DevOps Terraform Pipeline"
tags: [terraform,automation,azure,devops]
categories: [Azure DevOps]
ShowToc: true
---
When it comes to deploying Terraform via Pipelines your choices are almost limitless, you can go about it in so many ways, I personally have been deploying a lot of work based projects via Azure DevOps which has led me to also use it for personal projects.

# Pipeline Setup
The pipeline setup I use is pretty basic and I won't go into the weeds of the yaml file etc, but I will cover what it's doing and how I deploy it.

![Pipeline](/pipeline.png)

# The Process
So as you can see from the diagram above, it's a rather simple concept. Work is done on the feature branch then pushed to Azure DevOps, the Terraform pipeline it run to check it's correct and working and then a PR is raised to merge it into the main branch.

When this PR is raised, the Infracost pipeline is triggered which this will show you the esitmated cost changes

![infracost](/infracost-estimate.jpeg)

Once the PR is approved, the full pipeline can be deployed, which looks like this.

![pipeline-run](/pipeline-run.png)

# Final notes
This is a pretty high level view on how I am deploying Terraform + Infracost with Azure DevOps and I aim to do a walkthrough on the actual pipeline files, but I'm looking to make it a bit more simple first before I do that.

