---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-08"

keywords: satellite, hybrid, multicloud, faq, service, host, location

subcollection: satellite
content-type: faq

---

{{site.data.keyword.attribute-definition-list}}


# FAQs
{: #faqs}

Review frequently asked questions (FAQs) for using {{site.data.keyword.satellitelong}}.
{: shortdesc}


## What is {{site.data.keyword.satellitelong_notm}} and how does it work?
{: #what-is-satellite}
{: faq}
{: support}

With {{site.data.keyword.satellitelong_notm}}, you can create a hybrid environment that brings the scalability and flexibility of public cloud services on demand to the applications and data that run in your secure private cloud. To achieve this distributed cloud architecture, {{site.data.keyword.satelliteshort}} provides an API-based suite of tools that you can use to represent your on-premises data center, a public cloud provider, or an edge network as a {{site.data.keyword.satelliteshort}} location. You fill the {{site.data.keyword.satelliteshort}} location with your own host machines that meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs). Then, these hosts provide the compute power to run {{site.data.keyword.cloud_notm}} services, such as workloads in managed {{site.data.keyword.redhat_openshift_notm}} clusters or data and artificial intelligence (AI) tools like {{site.data.keyword.watson}}.

Your {{site.data.keyword.satelliteshort}} location includes tools such as {{site.data.keyword.satelliteshort}} Link and {{site.data.keyword.satelliteshort}} Config to provide additional capabilities for securing and auditing network connections in your location and consistently deploying, managing, and controlling your apps and policies across clusters in the location.

For more information, see the [{{site.data.keyword.satelliteshort}} product page](https://www.ibm.com/cloud/satellite){: external}.

## Where is Satellite available?
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

## Does {{site.data.keyword.IBM_notm}} support third-party and open source tools that I use with {{site.data.keyword.satelliteshort}}?
{: #faq_thirdparty_oss}
{: faq}

See the [{{site.data.keyword.IBM_notm}} open source and third-party policy](https://www.ibm.com/support/pages/node/737271){: external}.

## Why can't I install extra software like vulnerability scanning tools on my host?
{: #host-software}
{: faq}

To add your own server as a host in your {{site.data.keyword.satelliteshort}} location, the host must meet certain [compute, storage, networking, and system requirements](/docs/satellite?topic=satellite-host-reqs). These requirements specify the Red Hat software packages that must be installed on the Red Hat Enterprise Linux hosts. Other software packages that make modifications to the hosts, including vulnerability scanning tools such as McAfee or Qualys, cannot be installed on the hosts. But you can install read-only software such as OpenSCAP on the hosts before attaching them to your location.

The reasons that you cannot install extra software on the hosts relate to [{{site.data.keyword.IBM_notm}} 's responsibilities](/docs/satellite?topic=satellite-responsibilities) to manage multiple aspects of the {{site.data.keyword.satelliteshort}} hosts for you, such as installation, access, and maintenance.

**Installation**: The {{site.data.keyword.satelliteshort}} team tries to keep the host requirements to a minimal level so that many servers across infrastructure providers can meet the requirements to become {{site.data.keyword.satelliteshort}} hosts. By limiting the number of possible software packages, {{site.data.keyword.satelliteshort}} reduces instability and conflicts during installation tasks such as [bootstrapping](/docs/satellite?topic=satellite-host-concept) each host so that all hosts across {{site.data.keyword.satelliteshort}} locations have a consistent set of images and container platform software. This consistency also helps you develop applications and deploy {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services that work across your environments.

**Access**: For security purposes, {{site.data.keyword.satelliteshort}} restricts external access to hosts, including SSH. Many extra software packages require access to or from the host, so extra software packages are not allowed to be installed.

**Maintenance**: {{site.data.keyword.IBM_notm}} provides software updates that you choose when to apply to the host. Because {{site.data.keyword.IBM_notm}} is responsible for providing these updates, you cannot install extra software that is not managed by {{site.data.keyword.IBM_notm}}. Extra software also uses mores CPU, memory, and disk storage resources on the host, which impacts the amount available to your {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services and applications that run on the hosts.

## What am I charged for when I use {{site.data.keyword.satellitelong_notm}}?
{: #pricing}
{: support}
{: faq}

{{site.data.keyword.satellitelong_notm}} provides a convenient way for you to consume {{site.data.keyword.cloud_notm}} services in any location that you want, with visibility across your locations. Most of your costs are for the [{{site.data.keyword.cloud_notm}} services](#pricing-services) that you consume. Each location also has a [{{site.data.keyword.satelliteshort}} management](#pricing-satloc) fee.
{: shortdesc}

{{site.data.keyword.satelliteshort}} charges a flat management fee for all the service benefits, such as the following.
* **Flexible consumption**. By charging per vCPU hour only for assigned hosts, you have no upfront costs and no cancellation fees. No charges are incurred for hosts that are attached to a location but are not assigned to a resource. You can have as many hosts waiting in your location without charge for future growth. As soon as you unassign a host from a resource, you are no longer charged. Keep in mind that hosts might be automatically assigned, depending on your setup.
* **Application and networking capabilities at no additional charge**. You do not have separate charges for {{site.data.keyword.satelliteshort}} management capabilities for the locations, hosts, Link endpoints, configuration versions and subscriptions, or other {{site.data.keyword.satelliteshort}} resources.
* **Consistent {{site.data.keyword.cloud_notm}} experience**. The management fee includes benefits such as the managed {{site.data.keyword.satelliteshort}} master, the installation and security patch updates of OpenShift Container Platform on your {{site.data.keyword.satelliteshort}} location control plane; managing your {{site.data.keyword.satelliteshort}} resources with a suite of API, CLI, and UI tools; integration with {{site.data.keyword.cloud_notm}} platform tooling like IAM; continuous monitoring by {{site.data.keyword.IBM_notm}} Site Reliability Engineers; access to {{site.data.keyword.cloud_notm}} support. For more information, see [the responsibilities topic](/docs/satellite?topic=satellite-responsibilities).

### {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services
{: #pricing-services}

Each {{site.data.keyword.cloud_notm}} service instance that you create in your {{site.data.keyword.satelliteshort}} location incurs charges. Review the following {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

#### {{site.data.keyword.redhat_openshift_notm}} clusters
{: #pricing-services-clusters}

Get the benefits of a [managed {{site.data.keyword.redhat_openshift_notm}} service](/docs/openshift?topic=openshift-cs_ov#compare_ocp) on any compatible infrastructure that you want.
{: shortdesc}

| Type of charge | How the charge is applied | What the charge covers |
| -------------- | ------------------------- | ---------------------- |
| Cluster management fee | Per vCPU hour of the hosts that are assigned to the cluster as worker nodes | The benefits of {{site.data.keyword.openshiftlong_notm}}, such as installation and security patch updates of OpenShift Container Platform for your worker nodes; managing your cluster with a suite of API, CLI, and UI tools; integration with {{site.data.keyword.cloud_notm}} platform tooling like IAM; continuous monitoring by {{site.data.keyword.IBM_notm}} Site Reliability Engineers; access to {{site.data.keyword.cloud_notm}} support; and more. |
| {{site.data.keyword.satelliteshort}} management fee | Per vCPU hour of the hosts that are assigned to the cluster as worker nodes | The benefits of {{site.data.keyword.satellitelong_notm}}, such as to create the cluster on any compatible infrastructure that you want; tooling to consistently deploy apps, storage drivers, and endpoints across the location; integration with {{site.data.keyword.cloud_notm}} platform tooling like IAM; continuous monitoring by {{site.data.keyword.IBM_notm}} Site Reliability Engineers; access to {{site.data.keyword.cloud_notm}} support; and more. |
| OCP licensing fee | Red Hat charges a fee for Red Hat Enterprise Linux and OpenShift Container Platform per 2 vCPU hour. | This charge is not included in your {{site.data.keyword.cloud_notm}} bill. Instead, you cover this charge by [bringing your own license](#byo-ocp). |
| Infrastructure | Varies by provider | The underlying infrastructure that you bring to {{site.data.keyword.satelliteshort}} is your own, so it has its own charges. Consult your infrastructure provider for more details, such as about the storage, compute, and networking of the hosts in a cloud or on-prem environment. |
{: caption="{{site.data.keyword.redhat_openshift_notm}} cluster charges." caption-side="top"}

#### Other services
{: #pricing-services-other}

Other {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services often set up a cluster in the location for you, to deliver their services. The services also might have their own charges. For more information, consult their documentation.
{: shortdesc}

### {{site.data.keyword.satelliteshort}} locations
{: #pricing-satloc}

When you create a location, you must create a {{site.data.keyword.satelliteshort}} location control plane to manage the location. You need only one control plane per location, although you might need to add more hosts depending on the [size of your location](/docs/satellite?topic=satellite-location-sizing).
{: shortdesc}

| Type of charge | How the charge is applied | What the charge covers |
| -------------- | ------------------------- | ---------------------- |
| {{site.data.keyword.satelliteshort}} management fee | Per vCPU hour of the hosts that are assigned to the {{site.data.keyword.satelliteshort}} location control plane | The benefits of {{site.data.keyword.satellitelong_notm}}, such as to create the cluster on any compatible infrastructure that you want; tooling to consistently deploy apps, storage drivers, and endpoints across the location; integration with {{site.data.keyword.cloud_notm}} platform tooling like IAM; continuous monitoring by {{site.data.keyword.IBM_notm}} Site Reliability Engineers; access to {{site.data.keyword.cloud_notm}} support; and more.  |
| Infrastructure | Varies by provider | The underlying infrastructure that you bring to {{site.data.keyword.satelliteshort}} is your own, so it has its own charges. Consult your infrastructure provider for more details, such as about the storage, compute, and networking of the hosts in a cloud or on-prem environment. |
{: caption="{{site.data.keyword.satelliteshort}} location control plane charges." caption-side="top"}

## How do I bring my own OCP license?
{: #byo-ocp}
{: faq}

All user clusters in your {{site.data.keyword.satelliteshort}} location are installed with OpenShift Container Platform, which incurs a licensing fee from Red Hat. However, you can bring your own OpenShift Container Platform license for clusters created using your on-premises infrastructure or for clusters created by using IBM Cloud Paks.
{: shortdesc}

When you create the cluster, make sure to include your Red Hat pull secret to entitle the cluster to run OCP, either by uploading the pull secret in the console or by including the `--pull-secret` option in the `ibmcloud oc cluster create satellite` [command](/docs/openshift?topic=openshift-kubernetes-service-cli#cli_cluster-create-satellite).

Service clusters, which are the underlying platform for all {{site.data.keyword.cloud_notm}} services are created by services such as {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.cos_full_notm}} and do not require a license.



## Can I estimate my costs?
{: #cost-estimate}
{: faq}

When you create a resource such as a location or cluster, you can review a cost estimate in the **Summary** pane of the console. For other types of estimates, see [Estimating your costs](/docs/billing-usage?topic=billing-usage-cost#cost).

Keep in mind that some charges are not reflected in the estimate, such as the costs for your underlying infrastructure.

## Can I view and control my current usage?
{: #usage}
 

See [View your usage](/docs/billing-usage?topic=billing-usage-viewingusage#viewingusage) and [Set spending notifications](/docs/billing-usage?topic=billing-usage-spending) for general {{site.data.keyword.cloud_notm}} account guidance.

## What are the terms of the service level agreement?
{: #sla}
{: faq}

See the [{{site.data.keyword.cloud_notm}} terms of service](/docs/overview?topic=overview-slas) and the [{{site.data.keyword.satelliteshort}} additional service description](http://www.ibm.com/software/sla/sladb.nsf/sla/bm-8913-01){: external}.

 
{{site.data.keyword.satelliteshort}} Infrastructure Service is IBM-operated and as such, is covered in the {{site.data.keyword.satellitelong_notm}} cloud service terms with additional information outlined in the [{{site.data.keyword.cloud_notm}} Additional Services Description](http://www.ibm.com/support/customer/csol/terms/?id=i126-8979){: external}.

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

For a complete list of {{site.data.keyword.cloud_notm}} services that you can deploy to your {{site.data.keyword.satelliteshort}} location, see {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services. 

Keep in mind that each service might:
* Be available for {{site.data.keyword.satelliteshort}} locations that are managed from select {{site.data.keyword.cloud_notm}} regions only, such as from Washington, DC or London.
* Have its own [limitations for use in {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-requirements#reqs-services).


## What managed add-ons can I use with {{site.data.keyword.redhat_openshift_notm}} clusters in my {{site.data.keyword.satelliteshort}} location?
{: #faq-managed-addons}

See the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-managed-addons#addons-satellite).
