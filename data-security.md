---

copyright:
  years: 2020, 2021
lastupdated: "2021-09-07"

keywords: satellite, hybrid, multicloud

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
{:audio: .audio}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: .ph data-hd-programlang='c#'}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: #curl .ph data-hd-programlang='curl'}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: .external target="_blank"}
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
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:middle: .ph data-hd-position='middle'}
{:navgroup: .navgroup}
{:new_window: target="_blank"}
{:node: .ph data-hd-programlang='node'}
{:note: .note}
{:objectc: .ph data-hd-programlang='Objective C'}
{:objectc: data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: .ph data-hd-programlang='PHP'}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:release-note: data-hd-content-type='release-note'}
{:right: .ph data-hd-position='right'}
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
{:step: data-tutorial-type='step'} 
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:topicgroup: .topicgroup}
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



# Securing your data
{: #data-security}

Review what personal and sensitive information is stored when you use {{site.data.keyword.satellitelong}}, how this data is stored and encrypted, and how you can permanently remove this information.
{: shortdesc}

## What information is stored with {{site.data.keyword.IBM_notm}} when using {{site.data.keyword.satelliteshort}}?
{: #sat-sensitive-data}

For every location that you create, {{site.data.keyword.IBM_notm}} stores certain personal and sensitive information. Depending on the type of information, {{site.data.keyword.IBM_notm}} or you are responsible to store this information and protect it. For more information, see [How is my information stored, backed up, and encrypted?](#sat-data-encryption).
{: shortdesc}

### Stored information when you create a {{site.data.keyword.satelliteshort}} location
{: #sat-sensitive-data-default}

The following information is stored when you create a {{site.data.keyword.satelliteshort}} location:

**Personal information:**
- The email address of the {{site.data.keyword.cloud_notm}} account that created the location.

**Sensitive information:**
- The TLS certificate and secret that is used for the assigned {{site.data.keyword.satelliteshort}} control plane domain.
- The Certificate Authority that is used for the TLS certificate.
- An {{site.data.keyword.IBM_notm}}-owned encryption key for each location that is used to encrypt the TLS certificates, secrets, and Certificate Authority of the {{site.data.keyword.satelliteshort}} control plane domain.
- {{site.data.keyword.satelliteshort}} control plane and {{site.data.keyword.satelliteshort}} cluster data that can be used to restore the control plane and clusters in case of a disaster.

### Stored information from resources that you create
{: #sat-sensitive-data-user-added}

Because {{site.data.keyword.satelliteshort}} is an extension of {{site.data.keyword.cloud_notm}} to your own environment, you create many resources whose metadata might be stored, backed up, and encrypted in {{site.data.keyword.satelliteshort}}.
{: shortdesc}

Do not use sensitive or personally identifiable information for the names, labels, tags, or other metadata for the following items.
{: important}

* {{site.data.keyword.satelliteshort}} resources, such as the names of locations, hosts, {{site.data.keyword.satelliteshort}} Link endpoints, {{site.data.keyword.satelliteshort}} Config configurations, versions, subscriptions, cluster group names, or storage configurations.
* {{site.data.keyword.satelliteshort}}-enabled service resources, such as the names of service instances or clusters.
* Kubernetes resources that run in clusters in your {{site.data.keyword.satelliteshort}} location, such as the names of deployments, pods, services, or config maps.
* Any other resources that run in your {{site.data.keyword.satelliteshort}} location.

## How is my information stored, backed up, and encrypted?
{: #sat-data-encryption}

Review the following image to see how your personal and sensitive information is stored, backed up, and encrypted.
{: shortdesc}

![Satellite data security](images/satellite_data_security.png)

|&nbsp;|Information|Location|Access and data management|Backup|Encryption|
|--|--|--|--|--|--|
|1|All personal and sensitive information.|All data is stored in a {{site.data.keyword.satelliteshort}} persistent storage instance that is set up in the location's {{site.data.keyword.satelliteshort}} control plane master.|The persistent storage instance is owned and managed by the {{site.data.keyword.satelliteshort}} control plane service team. You cannot access the data that is stored in the persistent storage instance.|See 2 and 3 to see how data is backed up.|Data is encrypted at rest with a customer root key from an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.keymanagementservicelong_notm}} service instance.|
|2|TLS certificate, TLS secret, and Certificate Authority to encrypt the {{site.data.keyword.satelliteshort}} control plane domain.|Data is backed up from the {{site.data.keyword.satelliteshort}} persistent storage instance to an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_full_notm}} instance.|Access to the {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_full_notm}} service instance is controlled by {{site.data.keyword.iamshort}} (IAM) and granted to the {{site.data.keyword.satelliteshort}} service team and {{site.data.keyword.IBM_notm}} Site Reliability Engineers (SRE) only.|Every hour|All backup data is protected in transit and at rest by a root key that {{site.data.keyword.IBM_notm}} creates and stores in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.keymanagementservicelong_notm}} service instance.|
|3|All {{site.data.keyword.satelliteshort}} control plane and cluster data.|Data is backed up from the {{site.data.keyword.satelliteshort}} persistent storage instance to a customer-owned {{site.data.keyword.cos_full_notm}} instance. You must have an existing {{site.data.keyword.cos_full_notm}} instance when you create the location. You can specify an existing bucket in the {{site.data.keyword.cos_full_notm}} instance that you want {{site.data.keyword.satelliteshort}} to use. Otherwise, a new bucket is automatically created in your {{site.data.keyword.cos_short}} instance on your behalf.|Access to the customer-owned {{site.data.keyword.cos_full_notm}} service instance is controlled by IAM.|Every 8 hours|Data is automatically encrypted by using the default built-in encryption mechanisms in {{site.data.keyword.cos_full_notm}}. You can further choose to protect your data by using a root key in {{site.data.keyword.keymanagementservicelong_notm}} and use the key to encrypt the data in your bucket. For more information, see the [{{site.data.keyword.cos_full_notm}} documentation](/docs/cloud-object-storage?topic=cloud-object-storage-encryption). |


## Which {{site.data.keyword.cloud_notm}} region is my information stored in?
{: #sat_data-location}

Where your {{site.data.keyword.satelliteshort}} information is stored depends on the {{site.data.keyword.cloud_notm}} region that manages the control plane of your {{site.data.keyword.satelliteshort}} location. By selecting the {{site.data.keyword.cloud_notm}} region that is closest to the infrastructure provider for your {{site.data.keyword.satelliteshort}} location, your data is automatically spread across zones in that region for high availability. Because the zones of an {{site.data.keyword.cloud_notm}} region might be in a different city or country than the infrastructure hosts that you bring to your {{site.data.keyword.satelliteshort}} location, make sure that your data can be stored in the selected {{site.data.keyword.cloud_notm}} region.

## How can I remove my information?
{: #sat-data-removal}

Review your options to remove your personal and sensitive information from {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

Removing personal and sensitive information is permanent and non-reversible. Make sure that you want to permanently remove your information before you proceed.
{: important}

[Deleting a location](/docs/satellite?topic=satellite-locations#location-remove) does not remove all information from {{site.data.keyword.satellitelong_notm}}. When you delete a location, location-specific information is removed from the etcd instance that is managed by {{site.data.keyword.IBM_notm}}. However, your information still exists in the following places.

* **Data that {{site.data.keyword.IBM_notm}} manages**: A backup of the {{site.data.keyword.satelliteshort}} location is in {{site.data.keyword.cos_full_notm}} and can still be accessed by the {{site.data.keyword.IBM_notm}} service team. To remove all data that {{site.data.keyword.IBM_notm}} stores, choose between the following options. Note that removing your personal and sensitive information requires all of your {{site.data.keyword.satelliteshort}} locations to be deleted as well. Make sure that you backed up your data before your proceed.
    - **Open an {{site.data.keyword.cloud_notm}} support case**: Contact {{site.data.keyword.IBM_notm}} Support to remove your personal and sensitive information from {{site.data.keyword.satellitelong_notm}}. For more information, see [Getting support](/docs/get-support?topic=get-support-using-avatar).
    - **End your {{site.data.keyword.cloud_notm}} subscription**: After you end your {{site.data.keyword.cloud_notm}} subscription, all personal and sensitive information is permanently removed.
* **Cluster data in {{site.data.keyword.cos_full_notm}}**: When you create a {{site.data.keyword.openshiftlong_notm}} cluster, some cluster data is backed up to an {{site.data.keyword.cos_short}} instance in your account. To delete the data, review the [{{site.data.keyword.cos_short}} documentation](/docs/cloud-object-storage?topic=cloud-object-storage-security).
* **Cluster data on the local host**: Because the cluster masters run on your {{site.data.keyword.satelliteshort}} location control plane hosts, the data is still available on the physical hosts in your infrastructure provider after you delete the {{site.data.keyword.satelliteshort}} location. To delete the data, consult your infrastructure provider documentation to reload the operating system or delete the host.


