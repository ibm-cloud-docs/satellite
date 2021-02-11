---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-11"

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



# Securing your data in {{site.data.keyword.satellitelong_notm}}
{: #data-security}

Review what information is stored with IBM when you use {{site.data.keyword.satellitelong}}, how this data is stored and encrypted, and how you can permanently remove this information.
{: shortdesc}

## What information is stored with IBM when using {{site.data.keyword.satelliteshort}}?
{: #sat-sensitive-data}

For each location that you create with {{site.data.keyword.satelliteshort}}, IBM stores the following information:

|Type of information|Description|
|--------------|---------------------------------|
|Personal information|<ul><li>The email address of the {{site.data.keyword.cloud_notm}} account that created the location.</li></ul>|
|Sensitive information|<ul><li>The TLS certificate and secret that is used for the assigned {{site.data.keyword.satelliteshort}} subdomain.</li><li>The Certificate Authority that is used for the TLS certificate.</li><li>An IBM-owned encryption key for each location that is used to encrypt the TLS certificates, secrets, and Certificate Authority in an IBM-owned etcd data store.</li><li>A customer root key in {{site.data.keyword.keymanagementservicelong_notm}} for each {{site.data.keyword.cloud_notm}} account that is used to encrypt the backup of the etcd data store.</li><li>{{site.data.keyword.satelliteshort}} control plane data that can be used to restore the control plane in case of a disaster.</li></ul>|
{: caption="Information that is stored when you use {{site.data.keyword.satellitelong_notm}}." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the type of information. The second column is a description of the information."}

## How is my information stored and encrypted?
{: #sat-data-encryption}

All personal and sensitive information is stored in an etcd database and backed up every 8 hours to {{site.data.keyword.cos_full_notm}}. The etcd database and {{site.data.keyword.cos_full_notm}} service instance are owned and managed by the {{site.data.keyword.satelliteshort}} service team. To protect your data in the etcd database, IBM creates an encryption key for each location by using an IBM-owned encryption mechanism. The etcd backup data that is stored in {{site.data.keyword.cos_full_notm}} are protected in transit and at rest by a root key that IBM creates and stores in an IBM-owned {{site.data.keyword.keymanagementservicelong_notm}} service instance. Access to this service instance is controlled by {{site.data.keyword.iamshort}} (IAM) and granted to the {{site.data.keyword.satelliteshort}} service team and IBM Site Reliability Engineers (SRE) only.

In addition to the data that IBM stores on behalf of the customer, all {{site.data.keyword.satelliteshort}} control plane data is backed up to a customer-owned {{site.data.keyword.cos_full_notm}} bucket. Access to this bucket is controlled by IAM and the customer can grant or revoke access to this data for the {{site.data.keyword.satelliteshort}} service team. All data in the bucket is protected in transit and at rest by using a customer-owned root key in {{site.data.keyword.keymanagementservicelong_notm}}.

## Where is my information stored?
{: #sat_data-location}

The location where your information is stored depends on the {{site.data.keyword.cloud_notm}} multizone metro that manages the control plane of your {{site.data.keyword.satelliteshort}} location. By selecting the {{site.data.keyword.cloud_notm}} multizone metro that is closest to your {{site.data.keyword.satelliteshort}} location, your data is automatically spread across a multizone metro area for high availability. Because the {{site.data.keyword.cloud_notm}} multizone metro might be in a different city or country than the infrastructure hosts that you bring to your {{site.data.keyword.satelliteshort}} location, make sure that your data can be stored in the selected {{site.data.keyword.cloud_notm}} multizone metro.

## How can I remove my information?
{: #sat-data-removal}

Review your options to remove your personal and sensitive information from {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

Removing personal and sensitive information is permanent and non-reversible. Make sure that you want to permanently remove your information before you proceed.
{: important}

[Deleting a location](/docs/satellite?topic=satellite-locations#location-remove) does not remove all information from {{site.data.keyword.satellitelong_notm}}. When you delete a location, location-specific information is removed from the etcd instance that is managed by IBM. However, your information still exists in the following places.

* **Data that IBM manages**: A backup of the {{site.data.keyword.satelliteshort}} location is in {{site.data.keyword.cos_full_notm}} and can still be accessed by the IBM service team. To remove all data that IBM stores, choose between the following options. Note that removing your personal and sensitive information requires all of your {{site.data.keyword.satelliteshort}} locations to be deleted as well. Make sure that you backed up your data before your proceed.
  - **Open an {{site.data.keyword.cloud_notm}} support case**: Contact IBM Support to remove your personal and sensitive information from {{site.data.keyword.satellitelong_notm}}. For more information, see [Getting support](/docs/get-support?topic=get-support-using-avatar).
  - **End your {{site.data.keyword.cloud_notm}} subscription**: After you end your {{site.data.keyword.cloud_notm}} subscription, all personal and sensitive information is permanently removed.
* **Cluster data in {{site.data.keyword.cos_full_notm}}**: When you create a {{site.data.keyword.openshiftlong_notm}} cluster, some cluster data is backed up to an {{site.data.keyword.cos_short}} instance in your account. To delete the data, review the [{{site.data.keyword.cos_short}} documentation](/docs/cloud-object-storage?topic=cloud-object-storage-security).
* **Cluster data on the local host**: Because the cluster masters run on your {{site.data.keyword.satelliteshort}} location control plane hosts, the data is still available on the physical hosts in your infrastructure provider after you delete the {{site.data.keyword.satelliteshort}} location. To delete the data, consult your infrastructure provider documentation to reload the operating system or delete the host.


