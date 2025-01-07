+++
author = "Kevin Oliver"
title = "Accelerating AzApi Adoption with Newres"
date = "2025-01-06T13:57:01.161Z"
draft = false
description = ""
tags = ["Terraform", "Infrastructure as Code", "Azure Verified Modules"]
lastmod = "2022-10-08T22:33:02.994Z"
slug = "accelerating-azpai-adoption-with-newres"
+++


 Earlier this year Microsoft released version [2.0](https://techcommunity.microsoft.com/blog/azuretoolsblog/announcing-azapi-2-0/4275733) of the AzApi Terraform provider. My favorite feature of this release was the implementation of [Dynamic Properties](https://techcommunity.microsoft.com/blog/azuretoolsblog/announcing-azapi-dynamic-properties/4121855), no more Json Encoding and Decoding! Removing the requirement for JSON formatting in favor of HCL (HashiCorp Configuration Language) was the final push I needed to start module development with AzAPI. Unlike the AzureRM provider, AzAPI resource [templates](https://learn.microsoft.com/en-us/azure/templates/) are found on Microsoft Learn. AzAPI resource formats are different than those used by AzureRM as shown below.

 ``` Terraform
 #AzAPI
resource "azapi_resource" "symbolicname" {
  type = "Microsoft.Resources/resourceGroups@2024-11-01"
  name = "string"
  location = "string"
  managedBy = "string"
  tags = {
    {customized property} = "string"
  }
  body = {
    properties = {
    }
  }
}

#AzureRM
resource "azurerm_resource_group" "example" {
  name     = "example"
  location = "West Europe"
}
```

The differences between the provider can be quite intimidating if you are used AzureRM exclusively. To help ease the transition to AzAPI, we turn on a slick tool called [newres](https://github.com/lonegunmanb/newres). `Newres` is a command-line tool that provides the ability to create boilerplate module resources for several Terraform providers. Recently `newres` has been updated to allow the generation of resources for the AzAPI provider. 

## AzureRM Resource Creation with Newres

`Newres` makes it easy to generate boilerplate code when starting your module development. The command below will generate the following `main.tf` and `variables.tf` file for the deployment on an Azure Resource Group.

``` bash
newres -dir ./ -r azurerm_resource_group
```

```  Terraform
#Main.tf

resource "azurerm_resource_group" "this" {
  location = var.resource_group_location
  name     = var.resource_group_name
  tags     = var.resource_group_tags

  dynamic "timeouts" {
    for_each = var.resource_group_timeouts == null ? [] : var.resource_group_timeouts
    content {
      create = timeouts.value.create
      delete = timeouts.value.delete
      read   = timeouts.value.read
      update = timeouts.value.update
    }
  }
}

```

``` terraform
#Variables.tf

variable "resource_group_location" {
  type        = string
  description = "(Required) The Azure Region where the Resource Group should exist. Changing this forces a new Resource Group to be created."
  nullable    = false
}

variable "resource_group_name" {
  type        = string
  description = "(Required) The Name which should be used for this Resource Group. Changing this forces a new Resource Group to be created."
  nullable    = false
}

variable "resource_group_tags" {
  type        = map(string)
  default     = null
  description = "(Optional) A mapping of tags which should be assigned to the Resource Group."
}

variable "resource_group_timeouts" {
  type = object({
    create = optional(string)
    delete = optional(string)
    read   = optional(string)
    update = optional(string)
  })
  default     = null
  description = <<-EOT
 - `create` - (Defaults to 1 hour and 30 minutes) Used when creating the Resource Group.
 - `delete` - (Defaults to 1 hour and 30 minutes) Used when deleting the Resource Group.
 - `read` - (Defaults to 5 minutes) Used when retrieving the Resource Group.
 - `update` - (Defaults to 1 hour and 30 minutes) Used when updating the Resource Group.
EOT
}
```


This code generation saves a considerable amount of time during initial module development. This time saving allows developers to focus more on the specific customizations required to meet their environment security and compliance requirements. For the development of a complex module, this can be a significant time savings. 

## AzAPI Resource Generation with Newres

Baseline AzApi resource generation can now be done with a simple command to `newres`.

``` bash
newres --azapi-resource-type "Microsoft.Resources/resourceGroups@2021-04-01" -r azapi_resource --dir .
```

This will generate the require main.tf and variables.tf for the resource provided. 

main.tf
``` Terraform
resource "azapi_resource" "this" {
  type      = "Microsoft.Resources/resourceGroups@2021-04-01"
  body      = var.resource_body
  location  = var.resource_location
  name      = var.resource_name
  parent_id = var.resource_parent_id
  tags      = var.resource_tags
}

```

Variables.tf

``` Terraform
variable "resource_body" {
  type = object({
    managedBy = optional(string)
  })
  description = <<-EOT
 - `managedBy` - 

 ---
 `properties` block supports the following:
EOT
  nullable    = false
}

variable "resource_location" {
  type        = stringhttps://www.bing.com/ck/a?!&&p=a51fc3e7a50e6b8f30ff9ae9e13d8458f17711626ca396bd655755ff6a557498JmltdHM9MTczNTYwMzIwMA&ptn=3&ver=2&hsh=4&fclid=29395f82-9cea-661a-2925-4c6b9d1a6749&psq=backup+movies+with+mkv&u=a1aHR0cHM6Ly93d3cuaG93dG9nZWVrLmNvbS8xNjE0OTgvaG93LXRvLWJhY2t1cC15b3VyLWR2ZC1hbmQtYmx1LXJheS1tb3ZpZS1jb2xsZWN0aW9uLw&ntb=1
  description = "The location of the resource group. It cannot be changed after the resource group has been created. It must be one of the supported Azure locations."
  nullable    = false
}

variable "resource_name" {
  type        = string
  description = "The resource name"
  nullable    = false
}

variable "resource_parent_id" {
  type        = string
  description = "The ID of the azure resource in which this resource is created."
  nullable    = false
}

variable "resource_tags" {
  type        = map(string)
  default     = null
  description = "The tags attached to the resource group."
}
```

## Caveats When Using Newres



## Conclusion
Support for AzApi resource modules with `newres` enables our team to investigate the replacement of our azureRM based modules knowing we will continue to have the same development speed and experience we have come to rely on for module creation. It is a difficult task to ask your team to try and develop new resource modules with the tools that introduce time savings to the process. Immediately there is an natural opposition to a process that feels slower and more time consuming then your current process. Even if the results provide additional benefits, the adoption of that process will be slow and difficult. 
