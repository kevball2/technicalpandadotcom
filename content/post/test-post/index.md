+++
author = "Kevin Oliver"
title = "Test Post"
date = "2021-01-15"
description = "What is my plan for transitioning to the Machine Learning space"
tags = [
    "Blog", "Github", "Machine Learning", "Azure", "featured"
]
featured = true

categories = [
    "Azure",
    "Infra as Code",
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]
thumbnail = "images/azure/bicep_thumbnail.png"
+++


A sample blog post to see for effect


<!--more-->


```Bicep
resource pipFirewall 'Microsoft.Network/publicIPAddresses@2019-11-01' = {
  name: 'pip-firewall-${environment}'
  location: location
  sku: {
    name: 'Standard'
  }
  properties: {
    publicIPAllocationMethod: 'Static'
  }
}
```