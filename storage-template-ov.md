---

copyright:
  years: 2020, 2025
lastupdated: "2025-08-28"

keywords: satellite storage, storage template, satellite config, block, file, ocs

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Understanding {{site.data.keyword.satelliteshort}} storage
{: #storage-template-ov}

Before you can decide what type of storage is correct for your {{site.data.keyword.satelliteshort}} clusters, you must understand the infrastructure provider, your app requirements, the type of data that you want to store, and how often you want to access this data. For more information, see [choosing a storage solution](/docs/openshift?topic=openshift-storage-plan).

Before you can use storage templates and configurations to manage your storage resources across locations and clusters, make sure you [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig) in your location.
{: note}

## What are {{site.data.keyword.satelliteshort}} storage templates?
{: #storage-sat-templates}

Satellite storage templates are the framework for a storage configuration. When you use a Satellite storage template to create a configuration, you include the details for your selected storage provider. Each template contains a set of parameters that is specific to the storage provider. These parameters include user credentials, device information, worker nodes, and more. After you use a template to create a storage configuration, you can reuse that configuration to assign storage across your Satellite clusters. Each template is will receive revision and version updates.

Storage template
:   Satellite storage templates are preset storage formats that enable you to create storage configurations that can be applied across clusters without the need to re-create the configuration for each cluster.

Storage configuration
:    A storage configuration contains the necessary information to provision your storage resources in your cluster.

Storage assignment
:    A storage assignment is a storage configuration applied to your cluster. 

Revision
:    A revision update resolves breaking issues and includes security patches. For example, updating from version `1.5` to `1.6`.

Version
:    A major update to the template version. For example, updating from version `1.0` to `2.0`. Version updates can add new template parameters, change storage resources, and are more disruptive than revision updates.

## What are my options for deploying storage to {{site.data.keyword.satelliteshort}}?
{: #storage-sat-configure}

You have two options when configuring storage in {{site.data.keyword.satelliteshort}}. The first is by using one of the provided storage templates. The second is by bringing your own storage drivers (BYOD), Helm charts, or Operators from the IBM Cloud Catalog, OperatorHub, or GitHub repo. 
{: shortdesc}

{{site.data.keyword.satelliteshort}} storage templates
:   You can create storage configurations by [using the {{site.data.keyword.satelliteshort}} storage template for the storage provider or driver](#storage-template-ov-providers) that you want to use. After you create a storage configuration by using a template, you can assign your storage configuration to your clusters or services. By using storage templates, you can create storage configurations that can be consistently assigned, updated, and managed across the clusters, service clusters, and cluster groups in your location.

Bring your own Operators, drivers, and plug-ins
:   You can also bring your own storage drivers to {{site.data.keyword.satelliteshort}} by installing them from the Catalog, OperatorHub, Helm charts, or by using your preferred method of deploying images to your clusters including images and drivers from various {{site.data.keyword.cloud_notm}} storage and partner solutions, for example Portworx. Bringing your own storage driver is functionally supported, but you are responsible for the entire lifecycle operations, installation, troubleshooting, and support.

While bringing your own drivers or operators to your {{site.data.keyword.satelliteshort}} clusters is possible, if you want to deploy {{site.data.keyword.cloud_notm}} services that use storage make sure you use one of the provided storage templates.
{: note}

## Which deployment option should I use?
{: #storage-template-services}

Since {{site.data.keyword.satelliteshort}} storage templates are supported by {{site.data.keyword.cloud_notm}} and the configurations that you create by deploying templates can be used across your {{site.data.keyword.satelliteshort}} clusters, service clusters, and cluster groups, templates are the preferred option when setting up storage for services in {{site.data.keyword.satelliteshort}}.

If the storage solution that you want to use does not have a {{site.data.keyword.satelliteshort}} template available, check the IBM Cloud Catalog for supported Helm charts and Operators.


## What are the benefits of using templates?
{: #storage-template-benefits}

With {{site.data.keyword.satelliteshort}} storage templates, you can create a storage configuration that can be deployed across your clusters without the need to re-create the configuration for each cluster.
{: shortdesc}

The {{site.data.keyword.satelliteshort}} storage templates are provided and tested by {{site.data.keyword.IBM_notm}} or third-party vendors. {{site.data.keyword.IBM_notm}} provides the configuration tooling to install the storage drivers on clusters in your {{site.data.keyword.satelliteshort}} location as described in the templates. For issues with the installation process, you can contact {{site.data.keyword.IBM_notm}}. However, for issues with lifecycle operations of the storage devices, contact the storage provider.

When you use a {{site.data.keyword.satelliteshort}} storage template to create a configuration, you include the details for the storage provider that you want to use. Each template contains a set of parameters that is specific to the storage provider. These parameters include things like credentials, device information, worker nodes, and more. After you use a template to create a storage configuration, you can reuse that configuration to assign storage across your {{site.data.keyword.satelliteshort}} clusters.

You can also quickly update the configuration parameters and apply them automatically across clusters, service clusters, and cluster groups in your locations.

| Feature or benefit | {{site.data.keyword.satelliteshort}} storage templates | Bring your own drivers |
| --- | --- | --- |
| Integrated with the {{site.data.keyword.satelliteshort}} interface. | Yes | No |
| Quickly and consistently install storage drivers across multiple clusters. | Yes| No |
| Repeatable, site-specific or storage-specific configurations. | Yes | No |
| Certified and tested with {{site.data.keyword.satelliteshort}}. | Yes| No |
{: caption="Comparison of supported features between using {{site.data.keyword.satelliteshort}} storage templates and bringing your own storage drivers" caption-side="bottom"}


## How do templates work?
{: #storage-template-flow}

When you create a configuration by using a template, you specify a set of parameters for the storage provider that you want to use. These parameters preset values that are used when you deploy storage drivers, create persistent volumes, provision instances, or use other functions depending on the provider and the type of storage that you want to use. For more information, see the [list of available storage templates](#storage-template-ov-providers).
{: shortdesc}

The following image depicts the workflow for creating a {{site.data.keyword.satelliteshort}} storage configuration by using a storage template.

1. Select the storage template that you want to use for your configuration.
2. Create a {{site.data.keyword.satelliteshort}} cluster. Make sure that you select the **Enable cluster admin access for {{site.data.keyword.satelliteshort}} Config** option when you create the cluster. If you don't enable Administrator (admin) access for {{site.data.keyword.satelliteshort}} Config when creating your cluster, you must re-create your cluster and enable admin access before you can deploy storage.
3. Assign your {{site.data.keyword.satelliteshort}} storage configuration to your clusters.
4. {{site.data.keyword.satelliteshort}} deploys the storage drivers and any solution-specific resources for the provider that you selected to the clusters that you assigned the storage configuration to.

![Concept overview of Satellite storage templates](/images/storage-template.svg){: caption="A conceptual overview of creating a storage configuration by using a template." caption-side="bottom"}

## How do I manage updates?
{: #storage-template-updates}

{{site.data.keyword.satelliteshort}} storage template updates include major version updates or patch updates, also called revisions.

Version
:    A major update to the template version. For example, updating from version `1.0` to `2.0`. Version updates can add new template parameters, change storage resources, and are more disruptive than revision updates. Version updates may require you to reload or attach new hosts, redeploy services, 

Patch (Revision)
:    Patch updates, also called revisions, contain bug fixes and security vulnerability patches. You can apply patches manually to each of your storage configurations and assignments or you enable automatic patch updates to ensure you get the latest bug fixes and security updates. For more information about enabling automatic patches, see the `ibmcloud sat storage assignment autopatch` [command reference](/docs/satellite?topic=satellite-satellite-cli-reference#storage-assignment-autopatch-enable-cli).


## Which storage templates are available?
{: #storage-template-ov-providers}

You can create a {{site.data.keyword.satelliteshort}} storage configuration by using a template for the storage provider that you want to use. If your preferred storage provider does not have a template, you can [create your own configuration template](https://github.com/{{site.data.keyword.IBM_notm}}/ibm-satellite-storage){: external} or you can manually deploy storage drivers. The following list includes the storage templates are currently available to deploy to your {{site.data.keyword.satelliteshort}} clusters.
{: shortdesc}

| Template name | Template CLI name | Version, Patch | Supported status | Change log |
| --- | --- | --- | --- | --- |
| [AWS EBS CSI Driver](/docs/satellite?topic=satellite-storage-aws-ebs-csi-driver) | `aws-ebs-csi-driver` | Version: 1.1.0, Patch: 5 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-aws-ebs-csi-driver) 
| [AWS EBS CSI Driver](/docs/satellite?topic=satellite-storage-aws-ebs-csi-driver) | `aws-ebs-csi-driver` | Version: 1.5.1, Patch: 5 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-aws-ebs-csi-driver) 
| [AWS EBS CSI Driver](/docs/satellite?topic=satellite-storage-aws-ebs-csi-driver) | `aws-ebs-csi-driver` | Version: 1.12.0 (Default), Patch: 3 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-aws-ebs-csi-driver) 
| [AWS EBS CSI Driver](/docs/satellite?topic=satellite-storage-aws-ebs-csi-driver) | `aws-ebs-csi-driver` | Version: 1.31.0, Patch: 1 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-aws-ebs-csi-driver) 
| [AWS EFS CSI Driver](/docs/satellite?topic=satellite-storage-aws-efs-csi-driver) | `aws-efs-csi-driver` | Version: 1.3.1, Patch: 4 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-aws-efs-csi-driver) 
| [AWS EFS CSI Driver](/docs/satellite?topic=satellite-storage-aws-efs-csi-driver) | `aws-efs-csi-driver` | Version: 1.3.7, Patch: 4 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-aws-efs-csi-driver) 
| [AWS EFS CSI Driver](/docs/satellite?topic=satellite-storage-aws-efs-csi-driver) | `aws-efs-csi-driver` | Version: 1.4.2 (Default), Patch: 1 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-aws-efs-csi-driver) 
| [AWS EFS CSI Driver](/docs/satellite?topic=satellite-storage-aws-efs-csi-driver) | `aws-efs-csi-driver` | Version: 2.0.3, Patch: 1 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-aws-efs-csi-driver) 
| [Azure Disk CSI Driver](/docs/satellite?topic=satellite-storage-azuredisk-csi-driver) | `azuredisk-csi-driver` | Version: 1.4.0, Patch: 5 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-azuredisk-csi-driver) 
| [Azure Disk CSI Driver](/docs/satellite?topic=satellite-storage-azuredisk-csi-driver) | `azuredisk-csi-driver` | Version: 1.18.0, Patch: 5 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-azuredisk-csi-driver) 
| [Azure Disk CSI Driver](/docs/satellite?topic=satellite-storage-azuredisk-csi-driver) | `azuredisk-csi-driver` | Version: 1.23.0, Patch: 1 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-azuredisk-csi-driver) 
| [Azure Disk CSI Driver](/docs/satellite?topic=satellite-storage-azuredisk-csi-driver) | `azuredisk-csi-driver` | Version: 1.30.3 (Default), Patch: 1 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-azuredisk-csi-driver) 
| [Azure File CSI Driver](/docs/satellite?topic=satellite-storage-azurefile-csi-driver) | `azurefile-csi-driver` | Version: 1.9.0, Patch: 4 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-azurefile-csi-driver) 
| [Azure File CSI Driver](/docs/satellite?topic=satellite-storage-azurefile-csi-driver) | `azurefile-csi-driver` | Version: 1.18.0, Patch: 4 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-azurefile-csi-driver) 
| [Azure File CSI Driver](/docs/satellite?topic=satellite-storage-azurefile-csi-driver) | `azurefile-csi-driver` | Version: 1.22.0, Patch: 1 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-azurefile-csi-driver) 
| [Azure File CSI Driver](/docs/satellite?topic=satellite-storage-azurefile-csi-driver) | `azurefile-csi-driver` | Version: 1.31.2 (Default), Patch: 1 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-azurefile-csi-driver) 
| [GCP Compute Persistent Disk CSI Driver](/docs/satellite?topic=satellite-storage-gcp-compute-persistent-disk-csi-driver) | `gcp-compute-persistent-disk-csi-driver` | Version: 1.0.4, Patch: 4 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-gcp-compute-persistent-disk-csi-driver) 
| [GCP Compute Persistent Disk CSI Driver](/docs/satellite?topic=satellite-storage-gcp-compute-persistent-disk-csi-driver) | `gcp-compute-persistent-disk-csi-driver` | Version: 1.7.1, Patch: 5 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-gcp-compute-persistent-disk-csi-driver) 
| [GCP Compute Persistent Disk CSI Driver](/docs/satellite?topic=satellite-storage-gcp-compute-persistent-disk-csi-driver) | `gcp-compute-persistent-disk-csi-driver` | Version: 1.8.0 (Default), Patch: 2 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-gcp-compute-persistent-disk-csi-driver) 
| [IBM Object Storage Plugin](/docs/satellite?topic=satellite-storage-ibm-object-storage-plugin) | `ibm-object-storage-plugin` | Version: 2.2 (Default), Patch: 30 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-ibm-object-storage-plugin) |
| [IBM System Storage Block CSI driver](/docs/satellite?topic=satellite-storage-ibm-system-storage-block-csi-driver) | `ibm-system-storage-block-csi-driver` | Version: 1.10.0, Patch: 1 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-ibm-system-storage-block-csi-driver) 
| [IBM System Storage Block CSI driver](/docs/satellite?topic=satellite-storage-ibm-system-storage-block-csi-driver) | `ibm-system-storage-block-csi-driver` | Version: 1.11.1 (Default), Patch: 1 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-ibm-system-storage-block-csi-driver) 
| [IBM System Storage Block CSI driver](/docs/satellite?topic=satellite-storage-ibm-system-storage-block-csi-driver) | `ibm-system-storage-block-csi-driver` | Version: 1.11.2, Patch: 1 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-ibm-system-storage-block-csi-driver) 
| [[Beta] IBM VPC Block CSI driver](/docs/satellite?topic=satellite-storage-ibm-vpc-block-csi-driver) | `ibm-vpc-block-csi-driver` | Version: 5.1 (Default), Patch: 12 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-ibm-vpc-block-csi-driver) 
| [[Beta] Local Storage File and/or Block](/docs/satellite?topic=satellite-storage-local-storage) | `local-storage` | Version: 1.0.0 (Default), Patch: 2 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-local-storage) 
| [Local Storage Operator](/docs/satellite?topic=satellite-storage-local-storage-operator) | `local-storage-operator` | Version: 1.0.0 (Default), Patch: 2 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-local-storage-operator) 
| [[Deprecated] Local Storage Operator - Block](/docs/satellite?topic=satellite-storage-local-volume-block) | `local-volume-block` | Version: 4.9, Patch: 4 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-local-volume-block) 
| [[Deprecated] Local Storage Operator - Block](/docs/satellite?topic=satellite-storage-local-volume-block) | `local-volume-block` | Version: 4.10, Patch: 4 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-local-volume-block) 
| [[Deprecated] Local Storage Operator - Block](/docs/satellite?topic=satellite-storage-local-volume-block) | `local-volume-block` | Version: 4.11, Patch: 1 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-local-volume-block) 
| [[Deprecated] Local Storage Operator - Block](/docs/satellite?topic=satellite-storage-local-volume-block) | `local-volume-block` | Version: 4.12, Patch: 1 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-local-volume-block) 
| [[Deprecated] Local Storage Operator - Block](/docs/satellite?topic=satellite-storage-local-volume-block) | `local-volume-block` | Version: 4.13 (Default), Patch: 1 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-local-volume-block) 
| [[Deprecated] Local Storage Operator - File](/docs/satellite?topic=satellite-storage-local-volume-file) | `local-volume-file` | Version: 4.9, Patch: 4 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-local-volume-file) 
| [[Deprecated] Local Storage Operator - File](/docs/satellite?topic=satellite-storage-local-volume-file) | `local-volume-file` | Version: 4.10, Patch: 4 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-local-volume-file) 
| [[Deprecated] Local Storage Operator - File](/docs/satellite?topic=satellite-storage-local-volume-file) | `local-volume-file` | Version: 4.11, Patch: 1 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-local-volume-file) 
| [[Deprecated] Local Storage Operator - File](/docs/satellite?topic=satellite-storage-local-volume-file) | `local-volume-file` | Version: 4.12, Patch: 1 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-local-volume-file) 
| [[Deprecated] Local Storage Operator - File](/docs/satellite?topic=satellite-storage-local-volume-file) | `local-volume-file` | Version: 4.13 (Default), Patch: 1 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-local-volume-file) 
| [NetApp Ontap-NAS Driver](/docs/satellite?topic=satellite-storage-netapp-ontap-nas) | `netapp-ontap-nas` | Version: 22.04, Patch: 29 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-netapp-ontap-nas) 
| [NetApp Ontap-NAS Driver](/docs/satellite?topic=satellite-storage-netapp-ontap-nas) | `netapp-ontap-nas` | Version: 22.10, Patch: 16 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-netapp-ontap-nas) 
| [NetApp Ontap-NAS Driver](/docs/satellite?topic=satellite-storage-netapp-ontap-nas) | `netapp-ontap-nas` | Version: 24.02 (Default), Patch: 5 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-netapp-ontap-nas) 
| [NetApp Ontap-SAN Driver](/docs/satellite?topic=satellite-storage-netapp-ontap-san) | `netapp-ontap-san` | Version: 22.04, Patch: 29 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-netapp-ontap-san) 
| [NetApp Ontap-SAN Driver](/docs/satellite?topic=satellite-storage-netapp-ontap-san) | `netapp-ontap-san` | Version: 22.10, Patch: 16 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-netapp-ontap-san) 
| [NetApp Ontap-SAN Driver](/docs/satellite?topic=satellite-storage-netapp-ontap-san) | `netapp-ontap-san` | Version: 24.02 (Default), Patch: 5 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-netapp-ontap-san) 
| [NetApp Trident Operator](/docs/satellite?topic=satellite-storage-netapp-trident) | `netapp-trident` | Version: 22.04, Patch: 4 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-netapp-trident) 
| [NetApp Trident Operator](/docs/satellite?topic=satellite-storage-netapp-trident) | `netapp-trident` | Version: 22.10, Patch: 2 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-netapp-trident) 
| [NetApp Trident Operator](/docs/satellite?topic=satellite-storage-netapp-trident) | `netapp-trident` | Version: 24.02 (Default), Patch: 3 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-netapp-trident) 
| [OpenShift Data Foundation for local devices](/docs/satellite?topic=satellite-storage-odf-local) | `odf-local` | Version: 4.12, Patch: 20 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-odf-local) 
| [OpenShift Data Foundation for local devices](/docs/satellite?topic=satellite-storage-odf-local) | `odf-local` | Version: 4.13, Patch: 18 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-odf-local) 
| [OpenShift Data Foundation for local devices](/docs/satellite?topic=satellite-storage-odf-local) | `odf-local` | Version: 4.14, Patch: 16 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-odf-local) 
| [OpenShift Data Foundation for local devices](/docs/satellite?topic=satellite-storage-odf-local) | `odf-local` | Version: 4.15, Patch: 13 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-odf-local) 
| [OpenShift Data Foundation for local devices](/docs/satellite?topic=satellite-storage-odf-local) | `odf-local` | Version: 4.16 (Default), Patch: 8 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-odf-local) 
| [OpenShift Data Foundation for local devices](/docs/satellite?topic=satellite-storage-odf-local) | `odf-local` | Version: 4.17, Patch: 6 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-odf-local) 
| [OpenShift Data Foundation for local devices](/docs/satellite?topic=satellite-storage-odf-local) | `odf-local` | Version: 4.18, Patch: 2 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-odf-local) 
| [OpenShift Data Foundation for remote storage](/docs/satellite?topic=satellite-storage-odf-remote) | `odf-remote` | Version: 4.12, Patch: 20 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-odf-remote) 
| [OpenShift Data Foundation for remote storage](/docs/satellite?topic=satellite-storage-odf-remote) | `odf-remote` | Version: 4.13, Patch: 18 | Deprecated | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-odf-remote) 
| [OpenShift Data Foundation for remote storage](/docs/satellite?topic=satellite-storage-odf-remote) | `odf-remote` | Version: 4.14, Patch: 16 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-odf-remote) 
| [OpenShift Data Foundation for remote storage](/docs/satellite?topic=satellite-storage-odf-remote) | `odf-remote` | Version: 4.15, Patch: 13 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-odf-remote) 
| [OpenShift Data Foundation for remote storage](/docs/satellite?topic=satellite-storage-odf-remote) | `odf-remote` | Version: 4.16 (Default), Patch: 8 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-odf-remote) 
| [OpenShift Data Foundation for remote storage](/docs/satellite?topic=satellite-storage-odf-remote) | `odf-remote` | Version: 4.17, Patch: 6 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-odf-remote) 
| [OpenShift Data Foundation for remote storage](/docs/satellite?topic=satellite-storage-odf-remote) | `odf-remote` | Version: 4.18, Patch: 2 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-odf-remote) 
| [VMware CSI Driver](/docs/satellite?topic=satellite-storage-vsphere-csi-driver) | `vsphere-csi-driver` | Version: 2.5.1, Patch: 6 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-vsphere-csi-driver) 
| [VMware CSI Driver](/docs/satellite?topic=satellite-storage-vsphere-csi-driver) | `vsphere-csi-driver` | Version: 2.7.0 (Default), Patch: 3 | Supported | [Change log](/docs/satellite?topic=satellite-cl-storage-templates-vsphere-csi-driver) 
{: caption="Storage template versions" caption-side="bottom"}
