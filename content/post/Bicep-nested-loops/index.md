+++
author = "Kevin Oliver"
title = "Nested loops with Azure Bicep"
date = "2022-30-02"
draft = true
description = "Deploying Azure resource with Bicep"
tags = [ "Bicep", "Infrastructure as Code" ]
lastmod = "2022-10-08T22:33:02.994Z"
+++

### Working with nested loops in Azure Bicep

For the last several months I have been building ARM templates with the help if [Azure Bicep](https://github.com/Azure/bicep). Bicep is a new Domain Specific Language (DSL) for deploying Azure resource declaratively. It allows you to build complex ARM templates in a efficent resuable fashion with the help of modularized code. While Bicep is still in the early stages of development, it can handle a huge number of scenarios. A particular issue that has yet to be resolved is the simple application of nested loops in resource.

## Bicep resource overview

For those who have not worked with Bicep before, below is a basic bicep resource

```bicep
targetScope = 'subscription'

resource ResourceGroup 'Microsoft.Resources/resourceGroups@2021-04-01' = {
  name: 'ResourcegroupName'
  location: 'EastUS'
}
```

Each resource represents an Azure object that will be created as part of the deployment. Resources have numerous properties, some required and some optional. There are times where you would like to deploy more one copy of a resource. To do so we can use a for loop to deploy multiple copies of our resource. 

```bicep
targetScope = 'subscription'

var resources = [
  1
  2
  3
]

resource ResourceGroup 'Microsoft.Resources/resourceGroups@2021-04-01' = [for item in resources: {
  name: 'ResourcegroupName${item}'
  location: 'eastus'
}]
```

The above code will create 3 new resource groups; ResourcegroupName1, ResourcegroupName2, ResourcegroupName3. This allows great flexability for deploying the required number of resources that are needed for the given situation. Another situation that happens often is the need to deploy multiple properties to a resource. Many resources have properties that can be passed as arrays. An example resource with array properties is the [virtual network](https://docs.microsoft.com/en-us/azure/templates/microsoft.network/virtualnetworks?tabs=bicep). One of the primary properties is the subnets property. This property has multiple properties of it's own. Some of these properties are also arrays as well, the delegations property is an example of this. Each Virtual Network can have multiple subnets, each subnet can have multiple delegations. This issue that we run into is it currently is not possible to nest for loops inside a resource. This creates issues when trying to deploy more complex resource configurations. Below is a sample configuration of a Virtual network with subnets and delegations 

main.bicep




