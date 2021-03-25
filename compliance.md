---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-25"

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


# Security and compliance
{: #compliance}

You can use built-in security features in {{site.data.keyword.satellitelong}} for risk analysis and security protection. These features help you to protect your {{site.data.keyword.satelliteshort}} workloads that run on your location infrastructure and network communication.
{: shortdesc}

## Data security
{: #secure-data}

Learn more about options to secure the data that you use for workloads in {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

### What data is stored when I use {{site.data.keyword.satelliteshort}}? How can I use my own keys to encrypt my data?
{: #secure-data-store-encrypt}

See [Securing your data in {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-data-security).

### How do I make my data secure over {{site.data.keyword.satelliteshort}} Link?
{: #secure-data-link}

Your {{site.data.keyword.satelliteshort}} location infrastructure is a part of your local network (on-prem hosts) or the network of another cloud provider, but is managed remotely via secure {{site.data.keyword.satelliteshort}} Link access from {{site.data.keyword.cloud_notm}}. All communication over {{site.data.keyword.satelliteshort}} Link is encrypted by IBM. When you create an endpoint, you can optionally specify an additional data encryption protocol for the endpoint connection between the client source and destination resource. For example, even if the traffic is not encrypted on the source side, you can specify your own additional TLS encryption for the connection that goes over the internet.

To review information about how {{site.data.keyword.satelliteshort}} Link handles each type of connection protocol, see [Encryption protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols). To review other frequently asked questions about {{site.data.keyword.satelliteshort}} Link network security, see [External network requirements and security](/docs/satellite?topic=satellite-link-location-cloud#link-security).

### What measures can I take to secure user access to data in my location?
{: #secure-data-access}

Review the following ways that you can secure access to your location:
* Consider the types of [user access to resources that run in your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-service-connection#user-access)
* [Manage access for {{site.data.keyword.satelliteshort}} by using {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)](/docs/satellite?topic=satellite-iam)
* Monitor user-initiated activities by [Setting up {{site.data.keyword.at_short}} for {{site.data.keyword.satelliteshort}} location events](/docs/satellite?topic=satellite-health#setup-at)
* In the case of a potential security incident, [reset the key that the control plane uses to communicate with all of the hosts in the Satellite location](/docs/satellite?topic=satellite-hosts#host-key-reset)

<br />

## IBM operational access
{: #operational-access}

Learn more about IBM operational access to your {{site.data.keyword.satelliteshort}} location and how to monitor IBM-initiated activities.
{: shortdesc}

### What automated access does IBM have to my location?
{: #operational-access-automated}

Operations such as host attachment, host assignment, and major and minor version updates for the {{site.data.keyword.satelliteshort}} location control plane hosts are controlled by automation through the {{site.data.keyword.satellitelong_notm}} API server in {{site.data.keyword.cloud_notm}}. The {{site.data.keyword.satelliteshort}} API server communicates with the control plane master, which also exists in {{site.data.keyword.cloud_notm}}, to make these changes in your location.

Regular maintenance and automation tooling accesses the masters of {{site.data.keyword.satelliteshort}}-enabled service clusters in your location through the default `openshift-api-<cluster_ID>` Link endpoint. This endpoint allows the {{site.data.keyword.openshiftlong_notm}} API to communicate with the master for the service cluster. For example, if you create an {{site.data.keyword.openshiftshort}} cluster to run applications in your location, all version updates for that cluster's master are automatically applied through the default `openshift-api-<cluster_ID>` Link endpoint for that cluster.

Updates to {{site.data.keyword.satelliteshort}}-enabled services that are running on hosts in your location are initiated by the {{site.data.keyword.cloud_notm}} team for that service. When a change is ready to be rolled out to a service, the {{site.data.keyword.satelliteshort}}-enabled service team uses [{{site.data.keyword.satelliteshort}} config](/docs/satellite?topic=satellite-cluster-config) to upload a new version of the service to the subscription that the {{site.data.keyword.satelliteshort}}-enabled service cluster is included in. To apply the service update through {{site.data.keyword.satelliteshort}} config, the control plane automation for the {{site.data.keyword.satellitelong_notm}} API server, which exists in {{site.data.keyword.cloud_notm}}, deploys the update to the master of the {{site.data.keyword.satelliteshort}}-enabled service cluster in your location through the default `openshift-api-<cluster_ID>` Link endpoint. The cluster master then applies the updates to the worker nodes in the cluster.

### What access do IBM SREs have to my location control plane, including the masters of {{site.data.keyword.satelliteshort}}-enabled service clusters?
{: #operational-access-control-plane}

The default `satellite-healthcheck-<location_ID>` Link endpoint allows the {{site.data.keyword.satelliteshort}} control plane master to check the health of your location's control plane cluster, and alerts IBM Site Reliability Engineers (SREs) when manual intervention is required.

* To manually resolve issues with your {{site.data.keyword.satelliteshort}} location infrastructure management, such as host assignment or attachment, IBM SREs use tooling to access the {{site.data.keyword.satellitelong_notm}} API server. The {{site.data.keyword.satelliteshort}} API server communicates with the {{site.data.keyword.satelliteshort}} control plane master in {{site.data.keyword.cloud_notm}}.
* To manually resolve issues with masters of {{site.data.keyword.satelliteshort}}-enabled service clusters in your location, such as if the master fails to correctly deploy, IBM SREs use tooling to access the default `openshift-api-<cluster_ID>` Link endpoint for the master of the service cluster.

Note that tooling is controlled through the {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from.

### What access do IBM SREs have to my data and workloads that run in my {{site.data.keyword.satelliteshort}}-enabled service clusters?
{: #operational-access-workloads}

{{site.data.keyword.satelliteshort}} Link uses a zero-trust model: {{site.data.keyword.cloud_notm}} has no access to your workloads by default. Any management of infrastructure in your location that is initiated by IBM SREs occurs through the {{site.data.keyword.satelliteshort}} API server, which exists in {{site.data.keyword.cloud_notm}}, and any management of {{site.data.keyword.satelliteshort}}-enabled service cluster masters occurs through the default `openshift-api-<cluster_ID>` Link endpoint for that cluster.

The access through the `openshift-api-<cluster_ID>` endpoint is isolated from your workloads and the network connections, such as the Link endpoints, that your workloads use. The `openshift-api-<cluster_ID>` provides access only to the {{site.data.keyword.satelliteshort}} location control plane. IBM SREs have no access to the hosts that are assigned as worker nodes of your {{site.data.keyword.satelliteshort}}-enabled service clusters, where you run workloads in your location, because no default Link endpoints are created for workloads that run on these worker nodes, and all SSH access is disabled as part of the host bootstrapping process.

### How can I monitor and manage IBM access into my location? How can I know that there are no backdoor access points on the hosts?
{: #operational-access-monitor}

Default {{site.data.keyword.satelliteshort}} Link endpoints are created for your location's control plane cluster and for any other {{site.data.keyword.satelliteshort}}-enabled services that you run in your location. These default {{site.data.keyword.satelliteshort}} Link endpoints are accessible only from within the {{site.data.keyword.cloud_notm}} private network. To review these endpoints, see [Default Link endpoints for {{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-link-location-cloud#default-link-endpoints). You have full control over these default endpoints, including the ability to disable them. However, disabling these endpoints prevents your location from being fully managed and updated, and the endpoints cannot be removed.

{{site.data.keyword.satelliteshort}} Link provides built-in controls to help you restrict which clients can access endpoints. For more information, see [Access and audit controls](/docs/satellite?topic=satellite-link-location-cloud#link-audit-about).

Additionally, you can configure auditing to monitor user-initiated events for Link endpoints. {{site.data.keyword.satellitelong_notm}} integrates with {{site.data.keyword.at_full}} to collect and send audit events for all link endpoints in your location to your {{site.data.keyword.at_short}} instance. For example, after you set up event auditing, you can review all events that relate to the masters for the clusters that run in your location, including events that are initiated by {{site.data.keyword.cloud_notm}}. To get started with auditing, see [Auditing events for endpoint actions](/docs/satellite?topic=satellite-link-location-cloud#link-audit).

### What happens if {{site.data.keyword.satelliteshort}} Link becomes unavailable? Can IBM still maintain my {{site.data.keyword.satelliteshort}} location?
{: #operational-access-availability}

{{site.data.keyword.satelliteshort}} Link depends on the underlying connectivity of your hosts' local network to monitor and maintain the managed services for your {{site.data.keyword.satelliteshort}} location. If {{site.data.keyword.satelliteshort}} Link become unavailable, any requested changes to your {{site.data.keyword.satelliteshort}} location, such as adding hosts or access control requests to IBM services through {{site.data.keyword.iamshort}}, are disrupted. After connectivity is restored, logs and events are sent to your [{{site.data.keyword.la_full_notm}} and {{site.data.keyword.at_full_notm}} instances](/docs/satellite?topic=satellite-health).

Note that your on-location workloads continue to run independently even if the location's connectivity to {{site.data.keyword.cloud_notm}} is unavailable. However, if any applications use a Link endpoint to communicate with {{site.data.keyword.cloud_notm}}, communication between those apps and {{site.data.keyword.cloud_notm}} is disrupted.

For more information about making your {{site.data.keyword.satelliteshort}} location highly available, see [High availability and disaster recovery for {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha).

<br />

## Platform compliance and certification
{: #platform-compliance}

Learn more about compliance standards for the {{site.data.keyword.satellitelong_notm}} service.
{: shortdesc}

### What compliance standards does the service meet?
{: #compliance-standards}

See the [{{site.data.keyword.satelliteshort}} FAQ for compliance standards](/docs/satellite?topic=satellite-faqs#standards).

### Which areas of security compliance am I responsible for?
{: #compliance-responsibilities}

For an overview of who is responsible for particular cloud resources when using {{site.data.keyword.satellitelong_notm}}, see the table in [Overview of shared responsibilities](/docs/satellite?topic=satellite-responsibilities#overview-by-resource). For a detailed description of areas in which responsibilities are shared between you and IBM for security and compliance, see [Tasks for shared responsibilities: Security and regulation compliance](/docs/satellite?topic=satellite-responsibilities#security-compliance). If you use {{site.data.keyword.satelliteshort}} Infrastructure Service as your infrastructure provider, [review the {{site.data.keyword.satelliteshort}} Infrastructure Service responsibilities instead](/docs/satellite?topic=satellite-infrastructure-service#satis-responsibilities).

### What are the security compliance responsibilities of {{site.data.keyword.satelliteshort}}-enabled services?
{: #compliance-services}

To see the security options and compliance standards of a {{site.data.keyword.satelliteshort}}-enabled service, review the documentation for each {{site.data.keyword.satelliteshort}}-enabled service. For example, for {{site.data.keyword.openshiftshort}} clusters, see [What compliance standards does the service meet?](/docs/openshift?topic=openshift-faqs#standards) in the {{site.data.keyword.openshiftlong_notm}} documentation.
