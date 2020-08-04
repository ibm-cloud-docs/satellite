---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-04"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:step: data-tutorial-type='step'}


# FAQs
{: #faqs}

Review frequently asked questions (FAQs) for using {{site.data.keyword.satellitelong}}.
{: shortdesc}

## What is {{site.data.keyword.satellitelong_notm}} and how does it work?
{: #what-is-satellite}
{: faq}
{: support}

With {{site.data.keyword.satellitelong_notm}}, you can bring your own compute infrastructure that resides in your on-premises data center, at other cloud providers, or in edge networks as a {{site.data.keyword.satelliteshort}} Location to {{site.data.keyword.cloud_notm}}. Then, you use the capabilities of {{site.data.keyword.satelliteshort}} to run {{site.data.keyword.cloud_notm}} services on this infrastructure, and consistently deploy, manage, and control your app workloads.  

For more information, see the [{{site.data.keyword.satelliteshort}} product page](https://www.ibm.com/cloud/satellite){: external}.

## How can I get access to the {{site.data.keyword.satelliteshort}} beta?
{: #satellite-beta-access}
{: faq}
{: support}

{{site.data.keyword.satellitelong_notm}} is currently available as a tech preview and subject to change. To register for the beta, see [{{site.data.keyword.satellitelong_notm}} beta](https://cloud.ibm.com/satellite/beta){: external}.
{: preview}

## Where is the service available?
{: #satellite-locations}
{: faq}
{: support}

Because you bring your own compute host infrastructure to your {{site.data.keyword.satelliteshort}} location, you can choose to host this infrastructure anywhere you need it. However, to monitor malicious activity and apply updates to your location, these compute hosts are managed by an {{site.data.keyword.cloud_notm}} region that is supported by {{site.data.keyword.satellitelong_notm}}. You can choose any of the supported regions, but to reduce latency between {{site.data.keyword.cloud_notm}} and your {{site.data.keyword.satelliteshort}} location, choose the region that is closest to your compute hosts.

For more information about supported {{site.data.keyword.satelliteshort}} regions, see [Supported {{site.data.keyword.cloud_notm}} regions](/docs/satellite?topic=satellite-sat-regions).

## Is my location set up highly available?
{: #satellite-ha}
{: faq}
{: support}

The {{site.data.keyword.satellitelong_notm}} service architecture and infrastructure is designed to ensure reliability, low processing latency, and a maximum uptime of the service. By default, every location is managed by a highly available {{site.data.keyword.satelliteshort}} control plane that consists of a control plane master and worker nodes. For an overview of potential points of failures and your options to increase the availability of your location and control plane, see [High availability for {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha).

## What happens if my {{site.data.keyword.satelliteshort}} control plane becomes unavailable?
{: #control-plane-unavailable}
{: faq}
{: support}

Every location is securely connected to the {{site.data.keyword.cloud_notm}} region that manages your location by using the {{site.data.keyword.satelliteshort}} Link component. The link component runs in your control plane and is the main gateway for any communication between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}. If your {{site.data.keyword.satelliteshort}} location cannot communicate with the {{site.data.keyword.cloud_notm}} region anymore, your existing location workloads continue to run, but you cannot make any configuration changes or roll out updates to the services and apps that run in your location.

For an overview of your options to make the {{site.data.keyword.satelliteshort}} control plane more highly available to prevent connectivity issues with your {{site.data.keyword.cloud_notm}} region, see [High availability for {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha). 

## Does IBM support third-party and open source tools that I use with {{site.data.keyword.satelliteshort}}?
{: #faq_thirdparty_oss}
{: faq}

See the [IBM Open Source and Third Party policy](https://www.ibm.com/support/pages/node/737271){: external}.



## What {{site.data.keyword.cloud_notm}} services can I use with my {{site.data.keyword.satelliteshort}} cluster?
{: #supported-services}
{: faq}
{: support}

During the {{site.data.keyword.satelliteshort}} beta, you can run the following {{site.data.keyword.cloud_notm}} services in your {{site.data.keyword.satelliteshort}} location:

|Service name|Description|
|------------|----------------------------------|
|{{site.data.keyword.openshiftlong_notm}}|Run {{site.data.keyword.openshiftlong_notm}} clusters on the infrastructure that you added to your {{site.data.keyword.satelliteshort}} location. For more information, see [Creating {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters). |
