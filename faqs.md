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



# FAQs
{: #faqs}

Review frequently asked questions (FAQs) for using {{site.data.keyword.satellitelong}}.
{: shortdesc}

## What is {{site.data.keyword.satellitelong_notm}} and how does it work?
{: #what-is-satellite}
{: faq}
{: support}

With {{site.data.keyword.satellitelong_notm}}, you can bring your own compute infrastructure that meets the [{{site.data.keyword.satelliteshort}} host requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-system) to {{site.data.keyword.cloud_notm}}. You can add infrastructure from your on-premises data center, at other cloud providers, or in edge networks as a {{site.data.keyword.satelliteshort}} host in your {{site.data.keyword.satelliteshort}} location. Then, you use the capabilities of {{site.data.keyword.satelliteshort}} to run {{site.data.keyword.cloud_notm}} services on this infrastructure, and consistently deploy, manage, and control your app workloads.  

For more information, see the [{{site.data.keyword.satelliteshort}} product page](https://www.ibm.com/cloud/satellite){: external}.

## What are the reasons to use {{site.data.keyword.satellitelong_notm}}?
{: #satellite-benefits}
{: faq}
{: support}

Review some of the key benefits for using {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

|Benefits|Description|
|----------|----------------------------------|
|Bring your own infrastructure to {{site.data.keyword.cloud_notm}}.|With {{site.data.keyword.satellitelong_notm}}, you can bring your own infrastructure that is in your on-premises data center, in other cloud providers, or in edge environments to {{site.data.keyword.cloud_notm}} by creating a {{site.data.keyword.satelliteshort}} location. With this setup, you can run your workloads in the physical location of your choice to meet legal requirements, compliance standards, data speeds, and network latency requirements while securely using {{site.data.keyword.cloud_notm}} services. For more information, see [Setting up {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-locations). |
|Securely access {{site.data.keyword.cloud_notm}} services.|Every {{site.data.keyword.satelliteshort}} location is securely connected to {{site.data.keyword.cloud_notm}} through an encrypted VPN tunnel that is provided with the {{site.data.keyword.satelliteshort}} Link component. By using this VPN tunnel, you can securely access {{site.data.keyword.cloud_notm}} services in your location and use them with the same security and compliance standards as in {{site.data.keyword.cloud_notm}}.  For more information, see [Connecting {{site.data.keyword.satelliteshort}} locations with services outside of locations by using endpoints](/docs/satellite?topic=satellite-link-location-cloud).  |
|Control and monitor network traffic to services.|With {{site.data.keyword.satelliteshort}} Link, you can control network access to apps, services, and servers that run in {{site.data.keyword.cloud_notm}}, other cloud providers, or in your on-premises data center. Configure these apps, services, and servers as sources for your {{site.data.keyword.satelliteshort}} endpoints. All network traffic that is sent through an endpoint is automatically captured and can be reviewed by the user.  |
|Consistently deploy Kubernetes resources across multiple locations.|Use a single dashboard to manage the deployment of Kubernetes resources across cloud, on-premises, and edge environments with {{site.data.keyword.satelliteshort}} Config and gain global visibility into your apps and Kubernetes operations. For more information, see [Deploying Kubernetes resources across clusters](/docs/satellite?topic=satellite-cluster-config). |
|Centralize your monitoring and logging.|{{site.data.keyword.satellitelong_notm}} is integrated with {{site.data.keyword.mon_full_notm}}, {{site.data.keyword.la_full_notm}}, and {{site.data.keyword.at_full_notm}}. You can view the metrics and logs for the apps that run in your location, the {{site.data.keyword.cloud_notm}} services that you use, or the events that happen in your location from a single location. |
{: caption="Reasons to use {{site.data.keyword.satellitelong_notm}}." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the benefit of using {{site.data.keyword.satellitelong_notm}}. The second column is a description of the benefit."}

For more information about {{site.data.keyword.satelliteshort}}, how it works and the service benefits, see the [{{site.data.keyword.satelliteshort}} product page](https://www.ibm.com/cloud/satellite){: external}.

## How can I get access to the {{site.data.keyword.satelliteshort}} beta?
{: #satellite-beta-access}
{: faq}
{: support}

{{site.data.keyword.satellitelong_notm}} is available as a closed beta and subject to change. To register for the beta, see [{{site.data.keyword.satellitelong_notm}} beta](https://cloud.ibm.com/satellite/beta){: external}.
{: beta}

## Where is the service available?
{: #satellite-locations}
{: faq}
{: support}

Because you bring your own compute host infrastructure to your {{site.data.keyword.satelliteshort}} location, you can choose to host this infrastructure anywhere you need it. However, to monitor malicious activity and apply updates to your location, these compute hosts are managed by an {{site.data.keyword.cloud_notm}} multizone metro that is supported by {{site.data.keyword.satellitelong_notm}}. You can choose any of the supported metros, but to reduce latency between {{site.data.keyword.cloud_notm}} and your {{site.data.keyword.satelliteshort}} location, choose the metro that is closest to your compute hosts.

For more information about supported {{site.data.keyword.satelliteshort}} metros, see [Supported {{site.data.keyword.cloud_notm}} locations](/docs/satellite?topic=satellite-sat-regions).

## Is my location setup highly available?
{: #satellite-ha}
{: faq}
{: support}

The {{site.data.keyword.satellitelong_notm}} service architecture and infrastructure is designed to ensure reliability, low processing latency, and a maximum uptime of the service. By default, every location is managed by a highly available {{site.data.keyword.satelliteshort}} control plane that consists of a control plane master and worker nodes. For an overview of potential points of failures and your options to increase the availability of your location and control plane, see [High availability for {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha).

## What happens if my {{site.data.keyword.satelliteshort}} control plane becomes unavailable?
{: #control-plane-unavailable}
{: faq}
{: support}

Every location is securely connected to the {{site.data.keyword.cloud_notm}} multizone metro that manages your location by using the {{site.data.keyword.satelliteshort}} Link component. The link component runs in your control plane and is the main gateway for any communication between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}. If your {{site.data.keyword.satelliteshort}} location cannot communicate with the {{site.data.keyword.cloud_notm}} multizone metro anymore, your existing location workloads continue to run, but you cannot make any configuration changes or roll out updates to the services and apps that run in your location.

For an overview of your options to make the {{site.data.keyword.satelliteshort}} control plane more highly available to prevent connectivity issues with your {{site.data.keyword.cloud_notm}} multizone metro, see [High availability for {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha). 

## Does IBM support third-party and open source tools that I use with {{site.data.keyword.satelliteshort}}?
{: #faq_thirdparty_oss}
{: faq}

See the [IBM open source and third-party policy](https://www.ibm.com/support/pages/node/737271){: external}.

## What am I charged for when I use {{site.data.keyword.satellitelong_notm}}?
{: support}

During the beta, you are not charged for using {{site.data.keyword.satellitelong_notm}}. However, because you own the infrastructure that you bring to your {{site.data.keyword.satelliteshort}} location, you might be charged for hosting or using this infrastructure.



## What {{site.data.keyword.cloud_notm}} services can I use with my {{site.data.keyword.satelliteshort}} cluster?
{: #supported-services}
{: faq}
{: support}

During the {{site.data.keyword.satelliteshort}} beta, you can run the following {{site.data.keyword.cloud_notm}} services in your {{site.data.keyword.satelliteshort}} location:

|Service name|Description|
|------------|----------------------------------|
|{{site.data.keyword.openshiftlong_notm}}|Run {{site.data.keyword.openshiftlong_notm}} clusters on the infrastructure that you added to your {{site.data.keyword.satelliteshort}} location. For more information, see [Creating {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters). |
{: caption="{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the {{site.data.keyword.cloud_notm}} service that is enabled for use with {{site.data.keyword.satelliteshort}}. The second column is a description of the service."}
