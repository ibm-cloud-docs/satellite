---


copyright:
  years: 2023, 2023
lastupdated: "2023-01-10"

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Setting up Terraform for {{site.data.keyword.satelliteshort}}
{: #terraform}


Terraform enables predictable and consistent creation of {{site.data.keyword.cloud_notm}} services so that you can rapidly build complex, multi-tier cloud environments following Infrastructure as Code (IaC) principles. Similar to using the {{site.data.keyword.cloud_notm}} CLI or API and SDKs, you can automate the creation, update, and deletion of your {{site.data.keyword.satelliteshort}} instances by using HashiCorp Configuration Language (HCL).
{: shortdesc}

Looking for a managed Terraform on {{site.data.keyword.cloud}} solution? Try out [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-getting-started). With {{site.data.keyword.bpshort}}, you can use the Terraform scripting language that you are familiar with, but you don't have to worry about setting up and maintaining the Terraform command line and the {{site.data.keyword.cloud}} Provider plug-in. {{site.data.keyword.bpshort}} also provides pre-defined Terraform templates that you can easily install from the {{site.data.keyword.cloud}} catalog.
{: tip}

## Installing Terraform and configuring resources for {{site.data.keyword.satelliteshort}}
{: #install-terraform}

Before you can create an authorization by using Terraform, make sure that you have completed the following tasks.

- Make sure that you have the [required access](/docs/satellite?topic=satellite-iam) to create and work with {{site.data.keyword.satelliteshort}} resources.
- Install the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. For more information, see the tutorial for [Getting started with Terraform on {{site.data.keyword.cloud}}](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started). The plug-in abstracts the {{site.data.keyword.cloud_notm}} APIs that are used to complete this task.
- Create a Terraform configuration file that is named `main.tf`. In this file, you define resources by using HashiCorp Configuration Language. For more information, see the [Terraform documentation](https://developer.hashicorp.com/terraform/language){: external}.
- To create and manage the underlying infrastructure in other cloud providers, you must have the appropriate permissions. Review [common permissions in other cloud providers](/docs/satellite?topic=satellite-iam-common).

## Creating a {{site.data.keyword.satelliteshort}} location by using Terraform
{: #terraform-location-create}

{{site.data.keyword.satelliteshort}} provides Terraform templates for creating a {{site.data.keyword.satelliteshort}} location in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}. 

You can clone and modify these Terraform templates from the [Satellite Terraform GitHub repository](https://github.com/terraform-ibm-modules/terraform-ibm-satellite/tree/main/examples){: external}. 

For more information about creating a Satellite location by using Terraform, see [`ibm_satellite_location`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/satellite_location){: external}.


## What's next?
{: #terraform-setup-next}

Now that you successfully created your first {{site.data.keyword.satelliteshort}} location with Terraform on {{site.data.keyword.cloud_notm}}, you can choose from the following tasks:

- [Create, update, or delete {{site.data.keyword.satelliteshort}} Host](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/satellite_host){: external}.
- [Create, update, or delete {{site.data.keyword.satelliteshort}} Cluster](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/satellite_cluster){: external}.
- [Create, update, or delete {{site.data.keyword.satelliteshort}} Link](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/satellite_link){: external}.
- [Create, update, or delete {{site.data.keyword.satelliteshort}} Endpoint](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/satellite_enpoint){: external}.

