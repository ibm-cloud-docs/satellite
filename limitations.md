---

copyright:
  years: 2020, 2020
lastupdated: "2020-09-01"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
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
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
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
{:swift: #swift .ph data-hd-programlang='swift'}
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
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}



# Usage requirements
{: #requirements}

{{site.data.keyword.satellitelong}} comes with usage requirements, default service settings, and limitations to ensure security, convenience, and basic functionality. Requirements are subject to change, and might differ from beta to generally available releases.
{: shortdesc}

{{site.data.keyword.satellitelong_notm}} is available as a closed beta and is subject to change. To register for the beta, see the [product details page](https://cloud.ibm.com/satellite/beta){: external}.
{: beta}



## Locations
{: #reqs-locations}

You can create up to 20 {{site.data.keyword.satelliteshort}} locations per {{site.data.keyword.cloud_notm}} multizone metro that the location is managed from.
{: shortdesc}

Do not reuse the name of a previously deleted location. If you do reuse the name, the location subdomains might still use the IP addresses of the previous location's hosts. To resolve that issue, see [Location subdomain not routing traffic to control plane hosts](/docs/satellite?topic=satellite-ts-locations#ts-location-subdomain).

<br />


## Hosts
{: #reqs-host}

See [Host requirements](/docs/satellite?topic=satellite-host-reqs#host-reqs). For provider-specific requirements, see [Provider requirements](/docs/satellite?topic=satellite-providers).
{: shortdesc}

<br />


## Clusters
{: #reqs-clusters}

See [{{site.data.keyword.satelliteshort}} cluster limitations](/docs/openshift?topic=openshift-openshift_limitations#satellite_limits) in the {{site.data.keyword.openshiftlong_notm}} documentation. The limitations include information related to the following components.
{: shortdesc}

* {{site.data.keyword.openshiftlong_notm}} clusters that you create in your {{site.data.keyword.satelliteshort}} location.
* Storing data in Kubernetes persistent volumes for apps that run in your clusters.
* Cluster networking, such as Kubernetes load balancers.
* Using your hosts as the worker nodes in the cluster.

<br />


## Link and endpoints
{: #reqs-link}

The {{site.data.keyword.satelliteshort}} Link connector instances that run in your [{{site.data.keyword.satelliteshort}} location control plane worker nodes](/docs/satellite?topic=satellite-service-architecture) are limited to 3 instances, one per host. Even if you add hosts to the location control plane, network traffic that is routed through the {{site.data.keyword.satelliteshort}} Link connector is sent only over 3 hosts.

<br />


## Config
{: #reqs-config}

Review the following application configuration requirements for {{site.data.keyword.satelliteshort}} Config.
{: shortdesc}

**{{site.data.keyword.satelliteshort}} Config access to modify Kubernetes resources within a cluster**<br>
By default, {{site.data.keyword.satelliteshort}} Config is limited to what Kubernetes resources it can read and modify in your clusters. You must grant {{site.data.keyword.satelliteshort}} Config access in each cluster where you want to use {{site.data.keyword.satelliteshort}} Config to manage your Kubernetes resources.

Choose from the following options:
*   **Cluster admin access**: Create a cluster role binding to grant {{site.data.keyword.satelliteshort}} Config access to the appropriate service accounts.
    ```
    kubectl create clusterrolebinding razee-cluster-admin --clusterrole=razee-cluster-admin --serviceaccount=razeedeploy:razee-viewer --serviceaccount=razeedeploy:razee-editor --serviceaccount=razeedeploy:razee-satcon
    ```
    {: pre}
*   **Custom access, cluster-wide or scoped to a project**: You can create custom RBAC policies to grant {{site.data.keyword.satelliteshort}} Config access only to the projects (namespaces), actions, and resources that you want {{site.data.keyword.satelliteshort}} Config to manage. For more information and examples, see [Granting {{site.data.keyword.satelliteshort}} Config access to your clusters](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig-access).

**{{site.data.keyword.satelliteshort}} Config and {{site.data.keyword.cloud_notm}} IAM**<br>
You cannot scope access policies for {{site.data.keyword.satelliteshort}} Config resources (configuration, subscription, cluster, or cluster group) to an {{site.data.keyword.cloud_notm}} resource group. {{site.data.keyword.satelliteshort}} Config uses the open source Razee project, which authenticates users by using the organization. The organization supports only the account ID, not resource groups.

**Configuration files in {{site.data.keyword.satelliteshort}} Config**
* You can upload only an individual configuration file of Kubernetes resources per release version. You cannot upload a directory or several different configuration files.
* Configuration files are subject to Kubernetes requirements, such as that the manifest must be expressed in YAML format.
