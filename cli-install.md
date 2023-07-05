---

copyright: 
  years: 2023, 2023
lastupdated: "2023-07-05"

keywords: satellite, install cli, install sat, set up satellite command line, satellite command line, satellite cli, sat commands


subcollection: satellite

 


---


{{site.data.keyword.attribute-definition-list}}

# Installing the CLI
{: #cli-install}


You can use the following tools to manage your {{site.data.keyword.satelliteshort}} locations and clusters. While you can install a subset of the tools, all of the following tools are recommended.
{: shortdesc}


## Understanding the CLI tools
{: #cli-understand}

| CLI | Description |
| --- | --- |
| `ibmcloud` | You can use the `ibmcloud` CLI to login to your account, add users, manage your catalogs and more. |
| `ks` plug-in | You can use the `ks` plug-in to create and manage clusters and to manage your {{site.data.keyword.satelliteshort}} resources. |
{: caption="Table 1: CLI tools" caption-side="bottom"}


{{../cli/index.md#step1-install-idt}}

For more installation methods, see [IBM Cloud CLI](/docs/cli?topic=cli-getting-started).

{{../cli/index.md#step2-verify-idt}}

{{../cli/index.md#step3-install-idt-manually}}


To install the `container-service` or `ks` plugin, which includes all `ibmcloud sat` commands, run the following command.

```sh
ibmcloud plugin install ks
```
{: pre}



