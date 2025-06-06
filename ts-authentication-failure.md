---


copyright:
  years: 2022, 2025
lastupdated: "2025-06-06"

keywords: satellite, node, azure, file, disk

subcollection: satellite
content-type: troubleshoot

---
{{site.data.keyword.attribute-definition-list}}

# Why am I getting an authentication failure when logging in to my Azure Disk or Azure File {{site.data.keyword.satelliteshort}} configuration?
{: #ts-authentication-failure}

You are unable to access your storage configuration due to an `authentication error`. 
{: tsSymptoms}

Verify that your `service principal` is configured correctly. 
{: tsResolve}

To view your `service principal` configuration, follow the steps in the [Azure CLI documentation](https://learn.microsoft.com/en-us/cli/azure/azure-cli-sp-tutorial-1?view=azure-cli-latest&tabs=bash#create-a-service-principal){: external}. 
