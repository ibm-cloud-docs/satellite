---

copyright:
  years: 2020, 2021
lastupdated: "2021-04-09"

keywords: satellite storage, storage template, satellite config, block, file, ocs

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Understanding {{site.data.keyword.satelliteshort}} storage templates
{: #sat-storage-template-ov}

Before you can decide what type of storage is the right solution for your {{site.data.keyword.satelliteshort}} clusters, you must understand the infrastructure provider, your app requirements, the type of data that you want to store, and how often you want to access this data. For more information, see [choosing a storage solution](/docs/openshift?topic=openshift-storage_planning#choose_storage_solution).


<br />

## How do I configure storage on {{site.data.keyword.satelliteshort}}?
{: #storage-sat-configure}

You can configure storage on {{site.data.keyword.satelliteshort}} by using one of the provided storage configuration templates or by manually installing your own storage drivers.
{: shortdesc}

The {{site.data.keyword.satelliteshort}} storage templates are currently available in beta and should not be used for production workloads.
{: beta}

**Automatic installation with templates**: You can create storage configurations by [using the {{site.data.keyword.satelliteshort}} storage template for your storage provider](#storage-template-ov-providers). After you create a storage configuration by using a template, you can assign your storage configuration to your clusters. By using storage templates, you can create storage configurations that can be consistently assigned across your clusters.

**Manual installation**: You can also bring your own storage drivers to {{site.data.keyword.satelliteshort}} by installing them from OperatorHub, installing Helm charts, creating configurations, or by using your preferred method of deploying images to your clusters including images and drivers from various {{site.data.keyword.cloud_notm}} storage and partner solutions. Bringing your own storage driver is functionally supported, but you are responsible for the entire lifecycle operations, installation, troubleshooting, and support.

<br />

## What are the benefits of using templates?
{: #storage-template-benefits}

With {{site.data.keyword.satelliteshort}} storage templates, you can create a storage configuration that can be deployed across your clusters without the need to re-create the configuration for each cluster.
{: shortdesc}

The {{site.data.keyword.satelliteshort}} storage templates are provided and tested by IBM or third-party vendors. IBM provides the configuration tooling to install the storage drivers on clusters in your {{site.data.keyword.satelliteshort}} location as described in the templates. For issues with the installation process, you can contact IBM. However, for issues with lifecycle operations of the storage devices, contact the storage provider.

When you use a {{site.data.keyword.satelliteshort}} storage template to create a configuration, you include the details for the storage provider that you want to use. Each template contains a set of parameters that is specific to the storage provider. These parameters include things like credentials, device information, worker nodes, and more. After you use a template to create a storage configuration, you can reuse that configuration to assign storage across your {{site.data.keyword.satelliteshort}} clusters.

| Feature | {{site.data.keyword.satelliteshort}} storage templates | Bring your own drivers |
| --- | --- | --- |
| Integrated with the {{site.data.keyword.satelliteshort}} interface. | <img src="images/icon-checkmark-filled.svg" width="32" alt="Feature available" style="width:32px;" /> |  |
| Quickly and consistently install storage drivers across multiple clusters. | <img src="images/icon-checkmark-filled.svg" width="32" alt="Feature available" style="width:32px;" /> |  |
| Repeatable, site-specific or storage-specific configurations. | <img src="images/icon-checkmark-filled.svg" width="32" alt="Feature available" style="width:32px;" /> |  |
| Certified and tested with {{site.data.keyword.satelliteshort}}. | <img src="images/icon-checkmark-filled.svg" width="32" alt="Feature available" style="width:32px;" /> |  |
{: caption="Comparison of supported features between using {{site.data.keyword.satelliteshort}} storage templates and bringing your own storage drivers" caption-side="top"}
{: summary="The rows are read from left to right. The first column is a description of the feature. The second column is the feature support on Satellite. The third column is the feature support for bring your own drivers."}

<br />

## How do storage templates work?
{: #storage-template-flow}

When you create a configuration by using a template, you specify a set of parameters for the storage provider that you want to use. These parameters preset values that are used when you deploy storage drivers, create persistent volumes, provision instances, or use other functions depending on the provider and the type of storage that you want to use. For more information, see the [list of available storage templates](#storage-template-ov-providers).
{: shortdesc}

The following image depicts the workflow for creating a {{site.data.keyword.satelliteshort}} storage configuration by using a storage template.

1. Select the storage template that you want to use for your configuration.
2. Specify the storage provider specific parameters and create a {{site.data.keyword.satelliteshort}} storage configuration.
3. Assign your {{site.data.keyword.satelliteshort}} storage configuration to your clusters.
4. {{site.data.keyword.satelliteshort}} deploys the storage drivers and any solution-specific resources for the provider that you selected to the clusters that you assigned the storage configuration to.

![Concept overview of Satellite storage templates](/images/storage-template.png){: caption="Figure 1. A conceptual overview of creating a storage configuration by using a template." caption-side="bottom"}


<br />

## Which storage providers have {{site.data.keyword.satelliteshort}} storage templates?
{: #storage-template-ov-providers}

You can create a {{site.data.keyword.satelliteshort}} storage configuration by using a template for the storage provider that you want to use. If your preferred storage provider does not have a template, you can [create your own configuration template](https://github.com/IBM/ibm-satellite-storage){: external} or you can manually deploy storage drivers. The following list includes the storage templates are currently available to deploy to your {{site.data.keyword.satelliteshort}} clusters.
{: shortdesc}

* [Amazon Elastic Block Storage (EBS)](/docs/satellite?topic=satellite-config-storage-ebs)
* [Amazon Elastic File System (EFS)](/docs/satellite?topic=satellite-config-storage-efs)
* [NetApp Trident](/docs/satellite?topic=satellite-config-storage-netapp-trident)
* [NetApp ONTAP-NAS](/docs/satellite?topic=satellite-config-storage-netapp-nas)
* [NetApp ONTAP-SAN](/docs/satellite?topic=satellite-config-storage-netapp)
* [Red Hat Local Storage Operator - Block](/docs/satellite?topic=satellite-config-storage-local-block)
* [Red Hat Local Storage Operator - File](/docs/satellite?topic=satellite-config-storage-local-file)
* [Red Hat OpenShift Container Storage using local disks](/docs/satellite?topic=satellite-config-storage-ocs-local)
