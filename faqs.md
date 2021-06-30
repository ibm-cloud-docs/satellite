---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-30"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: faq

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
{:terraform: .ph data-hd-interface='terraform'}
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



# FAQs
{: #faqs}

Review frequently asked questions (FAQs) for using {{site.data.keyword.satellitelong}}.
{: shortdesc}

## What is {{site.data.keyword.satellitelong_notm}} and how does it work?
{: #what-is-satellite}
{: faq}
{: support}

With {{site.data.keyword.satellitelong_notm}}, you can create a hybrid environment that brings the scalability and on-demand flexibility of public cloud services to the applications and data that run in your secure private cloud. To achieve this distributed cloud architecture, {{site.data.keyword.satelliteshort}} provides an API-based suite of tools that let you represent your on-premises data center, another cloud provider, or an edge network as a {{site.data.keyword.satelliteshort}} location. You fill the {{site.data.keyword.satelliteshort}} location with your own host machines that meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-system). Then, these hosts provide the compute power to run {{site.data.keyword.cloud_notm}} services, such as workloads in managed {{site.data.keyword.openshiftshort}} clusters or data and artificial intelligence (AI) tools like {{site.data.keyword.watson}}.

Your {{site.data.keyword.satelliteshort}} location includes tools like {{site.data.keyword.satelliteshort}} Link and {{site.data.keyword.satelliteshort}} config to provide additional capabilities for securing and auditing network connections in your location and consistently deploying, managing, and controlling your apps and policies across clusters in the location.

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
|Securely access {{site.data.keyword.cloud_notm}} services.|Every {{site.data.keyword.satelliteshort}} location is securely connected to {{site.data.keyword.cloud_notm}} through an encrypted TLS tunnel that is provided with the {{site.data.keyword.satelliteshort}} Link component. By using this TLS tunnel, you can securely access {{site.data.keyword.cloud_notm}} services in your location and use them with the same security and compliance standards as in {{site.data.keyword.cloud_notm}}.  For more information, see [Connecting {{site.data.keyword.satelliteshort}} locations with services outside of locations by using endpoints](/docs/satellite?topic=satellite-link-location-cloud).  |
|Control and monitor network traffic to services.|With {{site.data.keyword.satelliteshort}} Link, you can control network access to apps, services, and servers that run in {{site.data.keyword.cloud_notm}}, other cloud providers, or in your on-premises data center. Configure these apps, services, and servers as sources for your {{site.data.keyword.satelliteshort}} endpoints. All network traffic that is sent through an endpoint is automatically captured and can be reviewed by the user.  |
|Consistently deploy Kubernetes resources across multiple locations.|Use a single dashboard to manage the deployment of Kubernetes resources across cloud, on-premises, and edge environments with {{site.data.keyword.satelliteshort}} Config and gain global visibility into your apps and Kubernetes operations. For more information, see [Deploying Kubernetes resources across clusters](/docs/satellite?topic=satellite-cluster-config). |
|Centralize your monitoring and logging.|{{site.data.keyword.satellitelong_notm}} is integrated with {{site.data.keyword.mon_full_notm}}, {{site.data.keyword.la_full_notm}}, and {{site.data.keyword.at_full_notm}}. You can view the metrics and logs for the apps that run in your location, the {{site.data.keyword.cloud_notm}} services that you use, or the events that happen in your location from a single location. |
{: caption="Reasons to use {{site.data.keyword.satellitelong_notm}}." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the benefit of using {{site.data.keyword.satellitelong_notm}}. The second column is a description of the benefit."}

For more information about {{site.data.keyword.satelliteshort}}, how it works and the service benefits, see the [{{site.data.keyword.satelliteshort}} product page](https://www.ibm.com/cloud/satellite){: external}.

## Where is the service available?
{: #satellite-locations}
{: faq}
{: support}

Because you bring your own compute host infrastructure to your {{site.data.keyword.satelliteshort}} location, you can choose to host this infrastructure anywhere you need it. However, to monitor malicious activity and apply updates to your location, these compute hosts are managed by an {{site.data.keyword.cloud_notm}} multizone region that is supported by {{site.data.keyword.satellitelong_notm}}. You can choose any of the supported regions, but to reduce latency between {{site.data.keyword.cloud_notm}} and your {{site.data.keyword.satelliteshort}} location, choose the region that is closest to your compute hosts.

For more information, see [Supported {{site.data.keyword.cloud_notm}} locations](/docs/satellite?topic=satellite-sat-regions).

## Is my location setup highly available?
{: #satellite-ha}
{: faq}
{: support}

The {{site.data.keyword.satellitelong_notm}} service architecture and infrastructure is designed to ensure reliability, low processing latency, and a maximum uptime of the service. By default, every location is managed by a highly available {{site.data.keyword.satelliteshort}} control plane that consists of a control plane master and worker nodes. For an overview of potential points of failures and your options to increase the availability of your location and control plane, see [High availability for {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha).

## What happens if my {{site.data.keyword.satelliteshort}} control plane becomes unavailable?
{: #control-plane-unavailable}
{: faq}
{: support}

Every location is securely connected to the {{site.data.keyword.cloud_notm}} multizone region that manages your location by using the {{site.data.keyword.satelliteshort}} Link component. The link component runs in your control plane and is the main gateway for any communication between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}}. If your {{site.data.keyword.satelliteshort}} location cannot communicate with the {{site.data.keyword.cloud_notm}} multizone region anymore, your existing location workloads will continue to run, but you cannot make any configuration changes or roll out updates to the services and apps that run in your location.

For an overview of your options to make the {{site.data.keyword.satelliteshort}} control plane more highly available to prevent connectivity issues with your {{site.data.keyword.cloud_notm}} multizone region, see [High availability for {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha).

## Does IBM support third-party and open source tools that I use with {{site.data.keyword.satelliteshort}}?
{: #faq_thirdparty_oss}
{: faq}

See the [IBM open source and third-party policy](https://www.ibm.com/support/pages/node/737271){: external}.

## What am I charged for when I use {{site.data.keyword.satellitelong_notm}}?
{: #pricing}
{: support}
{: faq}

{{site.data.keyword.satellitelong_notm}} provides a convenient way for you to consume {{site.data.keyword.cloud_notm}} services in any location that you want, with visibility across your locations. Most of your costs are for the [{{site.data.keyword.cloud_notm}} services](#pricing-services) that you consume. Each location also has a [{{site.data.keyword.satelliteshort}} management](#pricing-satloc) fee.
{: shortdesc}

{{site.data.keyword.satelliteshort}} charges a flat management fee for all of the service benefits, such as the following.
* **Flexibile consumption**. By charging per vCPU hour only for assigned hosts, you have no upfront costs and no cancellation fees. No charges are incurred for hosts that are attached to a location but are not assigned to a resource. You can have as many hosts waiting in your location without charge for future growth. As soon as you unassign a host from a resource, you are no longer charged. Keep in mind that hosts might be automaticaly assigned, depending on your setup.
* **Application and networking capabilities at no additional charge**. You do not have separate charges for {{site.data.keyword.satelliteshort}} management capabilities for the locations, hosts, Link endpoints, configuration versions and subscriptions, or other {{site.data.keyword.satelliteshort}} resources.
* **Consistent {{site.data.keyword.cloud_notm}} experience**. The management fee includes benefits such as the managed {{site.data.keyword.satelliteshort}} master, the installation and security patch updates of OpenShift Container Platform on your {{site.data.keyword.satelliteshort}} location control plane; managing your {{site.data.keyword.satelliteshort}} resources with a suite of API, CLI, and UI tools; integration with {{site.data.keyword.cloud_notm}} platform tooling like IAM; continuous monitoring by IBM Site Reliability Engineers; access to {{site.data.keyword.cloud_notm}} support. For more information, see [the responsibilities topic](/docs/satellite?topic=satellite-responsibilities).

### {{site.data.keyword.satelliteshort}}-enabled services
{: #pricing-services}

Each {{site.data.keyword.cloud_notm}} service instance that you create in your {{site.data.keyword.satelliteshort}} location incurs charges. Review the following {{site.data.keyword.satelliteshort}}-enabled services.
{: shortdesc}

#### {{site.data.keyword.openshiftshort}} clusters
{: #pricing-services-clusters}

Get the benefits of a [managed {{site.data.keyword.openshiftshort}} service](/docs/openshift?topic=openshift-cs_ov#compare_ocp) on any compatible infrastructure that you want.
{: shortdesc}

| Type of charge | How the charge is applied | What the charge covers |
| -------------- | ------------------------- | ---------------------- |
| Cluster management fee | Per vCPU hour of the hosts that are assigned to the cluster as worker nodes | The benefits of {{site.data.keyword.openshiftlong_notm}}, such as installation and security patch updates of OpenShift Container Platform for your worker nodes; managing your cluster with a suite of API, CLI, and UI tools; integration with {{site.data.keyword.cloud_notm}} platform tooling like IAM; continuous monitoring by IBM Site Reliability Engineers; access to {{site.data.keyword.cloud_notm}} support; and more. |
| {{site.data.keyword.satelliteshort}} management fee | Per vCPU hour of the hosts that are assigned to the cluster as worker nodes | The benefits of {{site.data.keyword.satellitelong_notm}}, such as to create the cluster on any compatible infrastructure that you want; tooling to consistently deploy apps, storage drivers, and endpoints across the location; integration with {{site.data.keyword.cloud_notm}} platform tooling like IAM; continuous monitoring by IBM Site Reliability Engineers; access to {{site.data.keyword.cloud_notm}} support; and more. |
| OCP licensing fee | Red Hat charges a fee for Red Hat Enterprise Linux and OpenShift Container Platform per 2 vCPU hour. | This charge is not included in your {{site.data.keyword.cloud_notm}} bill. Instead, you cover this charge by [bringing your own license](#byo-ocp). |
| Infrastructure | Varies by provider | The underlying infrastructure that you bring to {{site.data.keyword.satelliteshort}} is your own, so it has its own charges. Consult your infrastructure provider for more details, such as about the storage, compute, and networking of the hosts in a cloud or on-prem environment. |
{: caption="{{site.data.keyword.openshiftshort}} cluster charges." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the type of charge for a {{site.data.keyword.openshiftshort}} cluster. The second column describes how the charge is applied. The third column describes what is included with the charge."}

#### Other services
{: #pricing-services-other}

Other {{site.data.keyword.satelliteshort}}-enabled services often set up a cluster in the location for you, to deliver their services. The services also might have their own charges. For more information, consult their documentation.
{: shortdesc}

### {{site.data.keyword.satelliteshort}} locations
{: #pricing-satloc}

When you create a location, you must create a {{site.data.keyword.satelliteshort}} location control plane to manage the location. You need only one control plane per location, although you might need to add more hosts depending on the [size of your location](/docs/satellite?topic=satellite-locations#control-plane-scale).
{: shortdesc}

| Type of charge | How the charge is applied | What the charge covers |
| -------------- | ------------------------- | ---------------------- |
| {{site.data.keyword.satelliteshort}} management fee | Per vCPU hour of the hosts that are assigned to the {{site.data.keyword.satelliteshort}} location control plane | The benefits of {{site.data.keyword.satellitelong_notm}}, such as to create the cluster on any compatible infrastructure that you want; tooling to consistently deploy apps, storage drivers, and endpoints across the location; integration with {{site.data.keyword.cloud_notm}} platform tooling like IAM; continuous monitoring by IBM Site Reliability Engineers; access to {{site.data.keyword.cloud_notm}} support; and more.  |
| Infrastructure | Varies by provider | The underlying infrastructure that you bring to {{site.data.keyword.satelliteshort}} is your own, so it has its own charges. Consult your infrastructure provider for more details, such as about the storage, compute, and networking of the hosts in a cloud or on-prem environment. |
{: caption="{{site.data.keyword.satelliteshort}} location control plane charges." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the type of charge for a {{site.data.keyword.openshiftshort}} cluster. The second column describes how the charge is applied. The third column describes what is included with the charge."}

## How do I bring my own OCP license?
{: #byo-ocp}
{: faq}

All clusters in your {{site.data.keyword.satelliteshort}} location are installed with OpenShift Container Platform, which incurs a licensing fee from Red Hat. However, you must bring your own OCP license, such as if you have an {{site.data.keyword.cloud_notm}} Pak or other OCP entitlement.
{: shortdesc}

When you create the cluster, make sure to include your Red Hat pull secret to entitle the cluster to run OCP, either by uploading the pull secret in the console or by including the `--pull-secret` flag in the `ibmcloud oc cluster create satellite` [command](/docs/openshift?topic=openshift-kubernetes-service-cli#cli_cluster-create-satellite).



## Can I estimate my costs?
{: #cost-estimate}
{: faq}

When you create a resource such as a location or cluster, you can review a cost estimate in the **Summary** pane of the console. For other types of estimates, see [Estimating your costs](/docs/billing-usage?topic=billing-usage-cost#cost).

Keep in mind that some charges are not reflected in the estimate, such as the costs for your underlying infrastructure.

## Can I view and control my current usage?
{: #usage}
{: faq}

See [View your usage](/docs/billing-usage?topic=billing-usage-viewingusage#viewingusage) and [Set spending notifications](/docs/billing-usage?topic=billing-usage-spending) for general {{site.data.keyword.cloud_notm}} account guidance.

## What are the terms of the service level agreement?
{: #sla}
{: faq}

See the [{{site.data.keyword.cloud_notm}} terms of service](/docs/overview?topic=overview-slas) and the [{{site.data.keyword.satelliteshort}} additional service description](http://www-03.ibm.com/software/sla/sladb.nsf/sla/bm-8913-01){: external}.

If you use {{site.data.keyword.satelliteshort}} Infrastructure Service, consult that product's service level agreement.
{: note}

## What compliance standards does the service meet?
{: #standards}
{: faq}

{{site.data.keyword.cloud_notm}} is built by following many data, finance, health, insurance, privacy, security, technology, and other international compliance standards. For more information, see [{{site.data.keyword.cloud_notm}} compliance](/docs/overview?topic=overview-compliance).

To view detailed system requirements, you can run a [software product compatibility report for {{site.data.keyword.satellitelong_notm}}](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=4440E450C2C811E6A98AAE81A233E762){: external}.

Note that compliance also might depend on the setup of the underlying infrastructure provider that you use for the {{site.data.keyword.satelliteshort}} location control plane and other resources.
{: important}

{{site.data.keyword.satellitelong_notm}} implements controls commensurate with the following security standards:
- International Organization for Standardization (ISO 27001, ISO 27017, ISO 27018)


## What {{site.data.keyword.cloud_notm}} services can I use with {{site.data.keyword.satelliteshort}}?
{: #supported-services}
{: faq}
{: support}

You can run the following {{site.data.keyword.cloud_notm}} services in your {{site.data.keyword.satelliteshort}} location. Keep in mind that each service might have its own [limitations for use in {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-requirements#reqs-services).  

|Service name|Description|
|------------|----------------------------------|
|{{site.data.keyword.openshiftlong_notm}}|Run {{site.data.keyword.openshiftlong_notm}} clusters on the infrastructure that you added to your {{site.data.keyword.satelliteshort}} location. For more information, see [Creating {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters). |
{: caption="{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the {{site.data.keyword.cloud_notm}} service that is enabled for use with {{site.data.keyword.satelliteshort}}. The second column is a description of the service."}

## What managed add-ons can I use with {{site.data.keyword.openshiftshort}} clusters in my {{site.data.keyword.satelliteshort}} location?
{: #faq-managed-addons}

See the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-managed-addons#addons-satellite).
