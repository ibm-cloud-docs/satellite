---


copyright:
  years: 2020, 2025
lastupdated: "2025-04-17"

keywords: satellite, hybrid, multicloud, getting started, {{site.data.keyword.satellitelong}}, hosts, host

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

  
# Getting started with {{site.data.keyword.satellitelong_notm}} 
{: #getting-started}

{{site.data.keyword.satellitelong}} provides a distributed cloud architecture that brings the scalability and flexibility of public cloud services to the applications and data that run in your secure private cloud. With {{site.data.keyword.satellitelong}}, you can use your own compute infrastructure that is in your on-premises data center, other cloud providers, or edge networks to create a {{site.data.keyword.satelliteshort}} location. Then, you can use the capabilities of {{site.data.keyword.satelliteshort}} to run {{site.data.keyword.cloud_notm}} services on your infrastructure, and consistently deploy, manage, and control your app workloads. From a single pane of glass, you can manage workloads that run across the infrastructure from your {{site.data.keyword.satelliteshort}} locations.
{: shortdesc}

Your {{site.data.keyword.satelliteshort}} location includes tools such as {{site.data.keyword.satelliteshort}} Link and {{site.data.keyword.satelliteshort}} Config to provide capabilities for securing and auditing network connections in your location and consistently deploying, managing, and controlling your apps and policies across clusters in the location.

![Deploy apps and run anywhere with IBM Cloud Satellite](https://www.kaltura.com/p/1773841/sp/177384100/embedIframeJs/uiconf_id/27941801/partner_id/1773841?iframeembed=true&entry_id=1_030mi9vp){: video output="iframe" data-script="#video-transcript-sat" id="mediacenterplayer" frameborder="0" width="560" height="315" allowfullscreen webkitallowfullscreen mozAllowFullScreen}

## Video transcript
{: #video-transcript-sat}
{: notoc}

Many enterprises today are getting advantages out of moving workloads into the public cloud. They’re building cognitive applications, they’re scaling them on demand, and they’re improving the speed of their development through as a service API-based consumption on the public cloud. But the reality is many workloads have not moved to the public cloud yet many of those workloads are from regulated industries; they have security and compliance requirements, or performance and latency requirements that make it challenging to physically move those applications into a public cloud data center. And what many organizations really want is all the agility benefits of public cloud, but the flexibility to run those workloads wherever they need them: on-premises, in their existing data centers, in multiple public cloud providers around the world, or at the edge of the network closer to the applications and data sources that they need to process.  Public cloud is evolving to satisfy this requirement with a new concept called “Distributed Cloud”. So, the heart of {{site.data.keyword.satellitelong_notm}} is the notion of {{site.data.keyword.cloud_notm}} services managed anywhere. When you consume capabilities on the {{site.data.keyword.cloud_notm}} today, you have a catalog of over 130 services that you can use to build and run your applications - everything from infrastructure to Kubernetes and OpenShift, databases, DevOps tools, and frameworks for AI machine learning and IOT. All those services are available as APIs that you can provision on demand, and you can consume and use to build your applications. With {{site.data.keyword.satellitelong_notm}}, we’re extending that catalog of services and enabling it to be consumed in the exact same way, through the same APIs, in locations outside of {{site.data.keyword.cloud_notm}} regions. So, you can now consume an OpenShift cluster in your data center, or an AI machine learning framework in your factory. You can use those capabilities in a consistent way wherever the application workload needs to run. You can also use software - things like {{site.data.keyword.cloud_notm}} Paks and open-source capabilities - and deploy them in a consistent way across any environment. One of the advantages of this approach is that the public cloud becomes a kind of the central management, or control plane, for all your distributed workloads. You have a single console that you can log in to with {{site.data.keyword.cloud_notm}} that allows you to provision resources, provision cloud services, configure them, provision your applications, and manage them in a consistent way across this diverse set of environments. You get a single way to do security. You also get, of course, common observability - logging and monitoring, and dashboards and alerts, that allow you to monitor the workloads that you’re running in a consistent way across all these environments. We’re also doing some work in {{site.data.keyword.satelliteshort}} to help you with inventory and change management. So, part of the power of Satellite is this common control plane or single pane of glass that allows you to manage your applications across a diverse set of environments. Now, how does this work? Kind of the key idea within Satellite that we’re introducing is the notion of a location. A location is a way to define, to {{site.data.keyword.cloud_notm}}, a place outside of {{site.data.keyword.cloud_notm}} where you want to be able to deploy and consume cloud services. It is a collection of infrastructure that you own that we’re going to use on your behalf to run cloud services. That location is essentially a collection of Linux hosts, of virtual machines or physical machines that get arranged together into a pool of resources that get managed automatically by Satellite, and that get used, by us, to provision services. So, when you, let’s say, you want to create a Satellite location in your data center, you can provision a set of Linux machines within that data center. You register those Linux machines with {{site.data.keyword.cloud_notm}} as a location, and once that collection is registered with {{site.data.keyword.satellitelong_notm}}, you now see that location kind of like a custom region within your cloud account where you can now deploy cloud services. So, if you want to create a Postgres database, define a DevOps tool chain, or create an OpenShift cluster - when you provision that cluster through services in the cloud, that custom location you just defined in your data center will be a location that you can select when you provision that resource. And so, location is a really simple concept where you’re able to take any Linux infrastructure and make it available as a place to run cloud services. Now, one of the advantages of this idea of a location is it really provides a tremendous flexibility. {{site.data.keyword.satellitelong_notm}} is built on top of Kubernetes and OpenShift as the common infrastructure abstraction that we use to allow you to consume cloud services in any infrastructure. And by using OpenShift as that abstraction layer, we can support a variety of infrastructures underneath a Satellite location. You can bring your own custom infrastructure in your data center, physical or virtual servers. You can use your account on another public cloud like Amazon, or Azure, or Google - and consume that infrastructure, and arrange it into a location that’s used by {{site.data.keyword.cloud_notm}} for running cloud services. Now, since those locations those services and applications, are managed by {{site.data.keyword.cloud_notm}}, we, of course, need a connection back to the cloud to help us manage those things - and that connection is provided by a component called “Satellite Link”. And the idea of Satellite Link is to give you the transparent visibility to all the data that’s flowing back and forth between the cloud and that location, and to give you control over what applications and resources are exposed in those locations. So, these two ideas, of location and link, provide the fundamental new concepts that Satellite introduces to {{site.data.keyword.cloud_notm}} to give you the power of services anywhere. We’re also going to provide some optimized solutions. Fully integrated rack systems, both from IBM and from partners, and as-a-service infrastructure capabilities where we can run the entire stake for you from hardware through infrastructure and up into the platform and SaaS applications. And so, you can consume infrastructure either with what you have, you can build new environments, or you can run multi-cloud environments across {{site.data.keyword.cloud_notm}} and other public cloud providers. So, those are kind of the core ideas in Satellite: location and link allow us to extend our cloud catalog services to any location, giving you the power of cloud, and the power of {{site.data.keyword.cloud_notm}}, anywhere in the world that you need it. All you need is some Linux infrastructure and IBM does the rest. Thanks a lot. 



## What are {{site.data.keyword.satelliteshort}} locations, hosts, and so on?
{: #sat-key-terms}

Before you get started, become familiar with some key terms for {{site.data.keyword.satelliteshort}}. Afterward, you can test your knowledge and [take a quiz!](https://ibm.biz/BdPUqR){: external}

| Term | Description |
| ------ | ------------------------- |
| {{site.data.keyword.satelliteshort}} location | A {{site.data.keyword.satelliteshort}} location is a representation of an environment in your infrastructure provider, such as an on-prem data center or cloud. Locations are made up of compute sources, called hosts, from separate zones of your backing infrastructure environment. For more information, see [Understanding {{site.data.keyword.satelliteshort}} location and hosts](/docs/satellite?topic=satellite-location-host). |
| {{site.data.keyword.satelliteshort}} Connector | A {{site.data.keyword.satelliteshort}} Connector is a deployment model that enables only the secure communications from {{site.data.keyword.cloud_notm}} to on-prem resources with a light-weight container that is deployed on your container platform hosts, such as Docker hosts. This option brings all the security and auditability of {{site.data.keyword.satelliteshort}} communication, but with fewer resources required. For more information, see [{{site.data.keyword.satelliteshort}} Connector overview](/docs/satellite?topic=satellite-understand-connectors). |
| {{site.data.keyword.satelliteshort}} Connector Agent | Each Connector needs an agent running on your destination to establish the connection. For more information, [Understanding Connectors](/docs/satellite?topic=satellite-understand-connectors). |
| {{site.data.keyword.satelliteshort}} hosts | A {{site.data.keyword.satelliteshort}} host is a compute source that resides in your infrastructure provider or even locally. After you attach your hosts to a {{site.data.keyword.satelliteshort}} location, assign the hosts to the location control plane or to a {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service to provide the computing power to run your service workloads. For more information, see [Attaching hosts to your location](/docs/satellite?topic=satellite-attach-hosts). |
| {{site.data.keyword.satelliteshort}}-enabled service | An {{site.data.keyword.cloud_notm}} service that you can set up in a Satellite location, such as a {{site.data.keyword.redhat_openshift_notm}} cluster. The service is managed from the {{site.data.keyword.cloud_notm}} region that your location is managed from, but you provide the infrastructure hosts to run the service's resources in your location. For more information, see [Supported {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-managed-services). |
| {{site.data.keyword.satelliteshort}} Config | Based on the [Razee open source project](https://github.com/razee-io/Razee){: external}, {{site.data.keyword.satelliteshort}} Config is a continuous delivery tool that you can use to consistently roll out versions of your apps across clusters in your {{site.data.keyword.satelliteshort}} location. For more information, see [Deploying {{site.data.keyword.redhat_openshift_notm}} resources across clusters with {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-setup-clusters-satconfig). |
| {{site.data.keyword.satelliteshort}} Link | {{site.data.keyword.satelliteshort}} Link securely connects your {{site.data.keyword.satelliteshort}} location to the {{site.data.keyword.cloud_notm}} region that your location is managed from. Communication to and from your location is proxied by the Link tunnel server, and network traffic on this connection can be monitored and audited. For more information, see [Connecting {{site.data.keyword.satelliteshort}} locations with external services using Link endpoints](/docs/satellite?topic=satellite-link-location-cloud).|
| {{site.data.keyword.satelliteshort}} storage | {{site.data.keyword.satelliteshort}} storage uses {{site.data.keyword.satelliteshort}} Config to provide a convenient way to install various storage drivers in {{site.data.keyword.redhat_openshift_notm}} clusters across your {{site.data.keyword.satelliteshort}} locations, by using storage templates. The storage templates are provided and tested by the vendors. After you install {{site.data.keyword.satelliteshort}} storage, your cluster users can use Kubernetes persistent volume claims (PVCs) to order and save their application data in persistent storage. For more information, see [Understanding {{site.data.keyword.satelliteshort}} storage templates](/docs/satellite?topic=satellite-storage-template-ov). |
{: caption="Satellite terminology" caption-side="bottom"}


## Choose your infrastructure for {{site.data.keyword.satelliteshort}}
{: #gs-start-here}

To get started with {{site.data.keyword.satelliteshort}}, decide what type of infrastructure you want to use. Then, create a location by attaching hosts and creating a location control plane. For some cloud providers, you can use a Terraform template to create your location and attach your hosts. Otherwise, you can manually attach your hosts. For more information about your options, see [Planning your environment for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-infrastructure-plan).
{: shortdesc}

I want to try out {{site.data.keyword.satelliteshort}}.
:    You can try out {{site.data.keyword.satelliteshort}} with our [{{site.data.keyword.satelliteshort}} guided tour](https://www.ibm.com/products/satellite#demo){: external}. You can also create a Satellite location by using [{{site.data.keyword.cloud_notm}} for tests](/docs/satellite?topic=satellite-ibm). This setup is not intended for use with production systems.

I'm planning to use my on-prem or edge infrastructure. 
:    For on-prem infrastructure, you can [manually set up a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-loc-manual-create). 

I want to use a different cloud provider for my infrastructure.
:    Choose from [Amazon Web Services (AWS)](/docs/satellite?topic=satellite-aws), [Google Cloud Platform (GCP)](/docs/satellite?topic=satellite-gcp), [Microsoft Azure](/docs/satellite?topic=satellite-azure), or [Alibaba Cloud](/docs/satellite?topic=satellite-alibaba). Many of these providers include Terraform-based automation.

I want to create a {{site.data.keyword.satelliteshort}} Connector.
:    With connectors, you can create a secure connection between a specific remote location and {{site.data.keyword.cloud_notm}}. To create one, see [Creating a Connector](/docs/satellite?topic=satellite-create-connector). You can also learn more [about connectors](/docs/satellite?topic=satellite-understand-connectors). 

### Minimum requirements for hosts
{: #gs-min-reqs}

In each of these cases, make sure that your infrastructure meets the minimum requirements for hosts.

- [Host systems](/docs/satellite?topic=satellite-host-reqs)
- [Host storage options](/docs/satellite?topic=satellite-reqs-host-storage)
- [Host network configuration](/docs/satellite?topic=satellite-reqs-host-network)
- [Host outbound connectivity](/docs/satellite?topic=satellite-reqs-host-network-outbound)
- [Host latency](/docs/satellite?topic=satellite-host-latency-test)

You can validate your host set up by running the `satellite-host-check` script. For more information, see [Checking your host set up](/docs/satellite?topic=satellite-host-network-check).
{: tip}

## I created a {{site.data.keyword.satelliteshort}} location, what comes next?
{: #whats-next}

Now that your {{site.data.keyword.satelliteshort}} location is set up, you are ready to start using {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Add compute capacity to your location by [attaching more hosts to the location](/docs/satellite?topic=satellite-attach-hosts) so that you can run [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster. You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.
