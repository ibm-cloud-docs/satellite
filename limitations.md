---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-01"

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

<br />

## Hosts
{: #reqs-host}

See [Host requirements](/docs/satellite?topic=satellite-host-reqs#host-reqs).
{: shortdesc}

For cloud provider-specific configurations, see the following topics:
* [Amazon Web Services (AWS)](/docs/satellite?topic=satellite-aws)
* [Google Cloud Platform (GCP)](/docs/satellite?topic=satellite-gcp)
* [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm) (for testing and demonstration purposes only)
* [Microsoft Azure](/docs/satellite?topic=satellite-azure). 


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

**Link connector instances**

The {{site.data.keyword.satelliteshort}} Link connector instances that run in your [{{site.data.keyword.satelliteshort}} location control plane worker nodes](/docs/satellite?topic=satellite-service-architecture) are limited to 3 instances, one per host. Even if you attach hosts to the location control plane, network traffic that is routed through the {{site.data.keyword.satelliteshort}} Link connector is sent only over 3 hosts.

**Cloud and location endpoints**

Review the maximum number of each type of Link endpoint that you can create for one {{site.data.keyword.satelliteshort}} location.
* `cloud` endpoints: 1000 total, TCP and UDP combined. For example, you might create up to 650 TCP endpoints and 350 UDP endpoints through which clients in your location can connect to resources outside of the location network.
* `location` endpoints: 100 TCP and 100 UDP. For example, you might create up to 100 TCP endpoints and 100 UDP endpoints through which clients outside of your location network can connect to resources inside the location.

<br />

## Config
{: #reqs-config}

Review the following application configuration requirements for {{site.data.keyword.satelliteshort}} Config.
{: shortdesc}

**{{site.data.keyword.satelliteshort}} Config access to modify Kubernetes resources within a cluster**

By default, {{site.data.keyword.satelliteshort}} Config is limited to what Kubernetes resources it can read and modify in your clusters. You must grant {{site.data.keyword.satelliteshort}} Config access in each cluster where you want to use {{site.data.keyword.satelliteshort}} Config to manage your Kubernetes resources.

Choose from the following options:
* Opt-in to cluster admin access when you create the cluster in the [console](/docs/openshift?topic=openshift-satellite-clusters#satcluster-create-console) or [CLI](/docs/openshift?topic=openshift-satellite-clusters#satcluster-create-cli) with the `--enable-admin-agent` flag. Note that you must perform a one-time `oc login` in each cluster to synchronize the admin permissions.
* To opt in after creating a cluster or to scope the access, see [Granting {{site.data.keyword.satelliteshort}} config access to your clusters](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig-access).

**{{site.data.keyword.satelliteshort}} Config and {{site.data.keyword.cloud_notm}} IAM**

You cannot scope access policies for {{site.data.keyword.satelliteshort}} Config resources (configuration, subscription, cluster, or cluster group) to an {{site.data.keyword.cloud_notm}} resource group. {{site.data.keyword.satelliteshort}} Config uses the open source Razee project, which authenticates users by using the organization. The organization supports only the account ID, not resource groups.

You cannot scope access policies to particular configuration or subscription resources. When you assign a policy in the {{site.data.keyword.cloud_notm}} IAM console, leave the **Resource** field blank for configurations or subscriptions. Instead, you can scope the access policy to a cluster group for more control of how your {{site.data.keyword.satelliteshort}} Config resources are deployed.

To let users view the Kubernetes resources that run in clusters with {{site.data.keyword.satelliteshort}} Config, you must assign an access policy with the appropriate role (Administrator, Manager, or Reader) to {{site.data.keyword.satelliteslong_notm}} (and not scoped to a particular resource or resource type).

**Configuration files in {{site.data.keyword.satelliteshort}} Config**
* You can upload only an individual configuration file of Kubernetes resources per release version. You cannot upload a directory or several different configuration files.
* Configuration files are subject to Kubernetes requirements, such as that the manifest must be expressed in YAML format.

## {{site.data.keyword.cloud_notm}} services
{: #reqs-services}

You can have up to 40 instances of a supported {{site.data.keyword.cloud_notm}} service per {{site.data.keyword.satelliteshort}} location. For example, you might have a {{site.data.keyword.satelliteshort}} location that has 40 {{site.data.keyword.openshiftlong_notm}} clusters in it.
{: shortdesc}

Each supported service might have its own limitations to run in {{site.data.keyword.satelliteshort}}.
* {{site.data.keyword.openshiftlong_notm}}: See [{{site.data.keyword.satelliteshort}} cluster limitations](/docs/openshift?topic=openshift-openshift_limitations#satellite_limits) in the {{site.data.keyword.openshiftlong_notm}} documentation.
