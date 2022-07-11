---
title: "Terraform File Structure For Azure DevOps"
date: 2022-07-11T10:01:08+12:00
draft: false
description: "My basic layout of Terraform when using it with Azure DevOps"
tags: [terraform,automation,azure,devops]
categories: [terraform]
ShowToc: true
---
This post is mainly a post for myself so that I remember how I structure Terraform for when I am deploying it in Azure DevOps.

# File Structure

- main.tf
- providers.tf
- backend.tf
- variables.tf
- variables.tfvars
- modules

## main.tf
The main file just contains your terraform resource and module blocks, not much needs to be explained here.

## providers.tf
Here is where we can set the version of terraform if we want to ensure that only a certain version of Terraform is used.

Azure and Random are two other providers I am using since I am deploying to Azure and I am using random for generating my passwords that are stored in Keyvault, other providers such as AWS and Google can also be used in one file which would allow you to deploy to AWS and Azure incase you are wanting to create a VPN between them or something.

    terraform {
      required_providers {
        azurerm = {
          source  = "hashicorp/azurerm"
          version = "~> 2.94.0"
        }
        random = {
          source  = "hashicorp/random"
          version = "3.1.0"
        }
      }
    }

    provider "random" {
      # Configuration options
    }

    # Configure the Microsoft Azure Provider
    provider "azurerm" {
      features {}
    }

## backend.tf
My backend is in Azure as is being stored in a Storage Account.

    terraform {
      backend "azurerm" {
        resource_group_name = "terraformState" #Resource group name
        storage_account_name = "pocterraformstate" #Storage account name
        container_name = "tfstate" #Container name in the storage account
        key = "platformstate" #File name of the state file
      }
    }

## variables.tf and tfvars
These files are basic terraform files that I don't think need to be explained, but I use variables.tf to set my variables with any descriptions they need and the tfvars to assign the value.

The use of tfvars is not needed as you can assign the value of the variable in the variables.tf file but I like using tfvars as it most projects we are deploying to many environments like prod, test etc and this keeps me in a good habit.

### Example of variables.tf
    #------------------------------ Subscription Variables ------------------------------
    variable "subscription" {
      description = "Name for Subscription"
    }
    #------------------------------ Subscription Variables End ------------------------------

    #------------------------------ Platform Variables ------------------------------

    variable "platform_resource" {
    
      type = map(object({
          rg_name = string
          rg_location = string
      }))
    }

### Example of tfvars

    #Subscriptions
    subscription      = "40ba55ea-xxxxxxx"

    platform_resource = {
        base_resources = {
            rg_name = "xxxx-xxx"
            rg_location = "Australia East"
        }
    }

## Module folder
Here is where I keep all my modules, not again this is the way it's done and if you are used to using Terraform this is pretty standard, but here it is.

- modules
  - platform
    - main.tf
    - outputs.tf
    - variables.tf
  - virtualMachine
    - ''
    - ''
    - ''
  - vnets
    - ''
    - ''
    - ''

I am planning to do a write up on how I deploy Terraform using Azure DevOps and pipelines and how all this ties in, so stay tuned.