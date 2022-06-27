---

copyright:
  years: 2020, 2022
lastupdated: "2022-06-27"

keywords: satellite storage, storage template, satellite config, block, file, ocs

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Understanding {{site.data.keyword.satelliteshort}} storage
{: #sat-storage-template-ov}

Before you can decide what type of storage is the right solution for your {{site.data.keyword.satelliteshort}} clusters, you must understand the infrastructure provider, your app requirements, the type of data that you want to store, and how often you want to access this data. For more information, see [choosing a storage solution](/docs/openshift?topic=openshift-storage_planning#choose_storage_solution).

Before you can use storage templates and configurations to manage your storage resources across locations and clusters, make sure you [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig) in your location.
{: note}


## What are my options for deploying storage to {{site.data.keyword.satelliteshort}}?
{: #storage-sat-configure}

You have two options when configuring storage in {{site.data.keyword.satelliteshort}}. The first is by using one of the provided storage templates. The second is by bringing your own storage drivers (BYOD), Helm charts, or Operators from the IBM Cloud Catalog, OperatorHub, or GitHub repo. 
{: shortdesc}

{{site.data.keyword.satelliteshort}} storage templates
:   You can create storage configurations by [using the {{site.data.keyword.satelliteshort}} storage template for the storage provider or driver](#storage-template-ov-providers) that you want to use. After you create a storage configuration by using a template, you can assign your storage configuration to your clusters or services. By using storage templates, you can create storage configurations that can be consistently assigned, updated, and managed across the clusters, service clusters, and cluster groups in your location.

Bring your own Operators, drivers, and plug-ins
:   You can also bring your own storage drivers to {{site.data.keyword.satelliteshort}} by installing them from the Catalog, OperatorHub, Helm charts, or by using your preferred method of deploying images to your clusters including images and drivers from various {{site.data.keyword.cloud_notm}} storage and partner solutions, for example Portworx. Bringing your own storage driver is functionally supported, but you are responsible for the entire lifecycle operations, installation, troubleshooting, and support. Note that bringing your own drivers or operators to your {{site.data.keyword.satelliteshort}} clusters is possible, if you want to deploy services that use storage make sure you use one of the provided storage templates.

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
{: caption="Comparison of supported features between using {{site.data.keyword.satelliteshort}} storage templates and bringing your own storage drivers" caption-side="top"}
{: summary="The rows are read from left to right. The first column is a description of the feature. The second column is the feature support on Satellite. The third column is the feature support for bring your own drivers."}

## How do templates work?
{: #storage-template-flow}

When you create a configuration by using a template, you specify a set of parameters for the storage provider that you want to use. These parameters preset values that are used when you deploy storage drivers, create persistent volumes, provision instances, or use other functions depending on the provider and the type of storage that you want to use. For more information, see the [list of available storage templates](#storage-template-ov-providers).
{: shortdesc}

The following image depicts the workflow for creating a {{site.data.keyword.satelliteshort}} storage configuration by using a storage template.

1. Select the storage template that you want to use for your configuration.
2. Specify the storage provider specific parameters and create a {{site.data.keyword.satelliteshort}} storage configuration.
3. Assign your {{site.data.keyword.satelliteshort}} storage configuration to your clusters.
4. {{site.data.keyword.satelliteshort}} deploys the storage drivers and any solution-specific resources for the provider that you selected to the clusters that you assigned the storage configuration to.

![Concept overview of Satellite storage templates](/images/storage-template.png){: caption="Figure 1. A conceptual overview of creating a storage configuration by using a template." caption-side="bottom"}


## Which storage templates are available?
{: #storage-template-ov-providers}

You can create a {{site.data.keyword.satelliteshort}} storage configuration by using a template for the storage provider that you want to use. If your preferred storage provider does not have a template, you can [create your own configuration template](https://github.com/{{site.data.keyword.IBM_notm}}/ibm-satellite-storage){: external} or you can manually deploy storage drivers. The following list includes the storage templates are currently available to deploy to your {{site.data.keyword.satelliteshort}} clusters.
{: shortdesc}


* [Amazon Elastic Block Storage (EBS)](/docs/satellite?topic=satellite-config-storage-ebs)
* [Amazon Elastic File System (EFS)](/docs/satellite?topic=satellite-config-storage-efs)
* **Deprecated** [{{site.data.keyword.IBM_notm}} Spectrum Scale driver](/docs/satellite?topic=satellite-config-storage-spectrum-scale)
* [{{site.data.keyword.IBM_notm}} Systems block storage CSI driver](/docs/satellite?topic=satellite-config-storage-block-csi)
* [NetApp Trident](/docs/satellite?topic=satellite-config-storage-netapp-trident)
* [NetApp ONTAP-NAS](/docs/satellite?topic=satellite-config-storage-netapp-nas)
* [NetApp ONTAP-SAN](/docs/satellite?topic=satellite-config-storage-netapp)
* [Red Hat Local Storage Operator - Block](/docs/satellite?topic=satellite-config-storage-local-block)
* [Red Hat Local Storage Operator - File](/docs/satellite?topic=satellite-config-storage-local-file)
* [Red Hat OpenShift Data Foundation using local disks](/docs/satellite?topic=satellite-config-storage-odf-local)
* [Red Hat OpenShift Data Foundation using remote volumes](/docs/satellite?topic=satellite-config-storage-odf-remote)
* [Google Compute Engine persistent disk CSI driver](/docs/satellite?topic=satellite-config-storage-gcp-csi) 

For a list of available storage templates and storage classes, review the [site map](/docs/satellite?topic=satellite-storage-class-ref#storage-class-ref).

