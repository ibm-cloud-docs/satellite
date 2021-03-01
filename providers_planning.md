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



# Planning your infrastructure environment for {{site.data.keyword.satelliteshort}}
{: #infrastructure-plan}

Plan how to set up your infrastructure environment to use with {{site.data.keyword.satellitelong}}. Your infrastructure environment can be an on-premises data center, in another cloud provider, or on compatible edge devices anywhere.
{: shortdesc}

Don't have your own infrastructure or want a managed solution? [Check out {{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-infrastructure-service).
{: tip}


## Planning your infrastructure zones and hosts to meet the {{site.data.keyword.satelliteshort}} requirements
{: #plan-zone-host-reqs}

Your {{site.data.keyword.satelliteshort}} location starts with your actual infrastructure that runs somewhere outside {{site.data.keyword.cloud_notm}}, such as another cloud provider or on-prem. Your infrastructure provides the basis for the hosts and zones that you use to build out your {{site.data.keyword.satelliteshort}} location. For more details on the different responsibilities for your infrastructure and {{site.data.keyword.satelliteshort}} resources, see [Your responsibilities](/docs/satellite?topic=satellite-responsibilities).
{: shortdesc}

![Concept overview of planning your infrastructure](/images/satellite-infra-plan.png){: caption="Figure 1. Your {{site.data.keyword.satelliteshort}} location is built atop the zones and hosts in your infrastructure provider." caption-side="bottom"}

1.  Choose the infrastructure provider that you want to use to create a {{site.data.keyword.satelliteshort}} location.
    * **On-premises**: You can use a data center with existing infrastructure, or order infrastructure from IBM with [{{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-infrastructure-service). You might not even have a data center, but rather an edge location that meets the minimum hardware requirements, such as three racks at one of your company's local sites.
    * **Cloud provider**: You can use a cloud provider of your choice, such as Amazon Web Services (AWS), Google Cloud Platform (GCP), or Microsoft Azure. With the AWS cloud provider, you can also use a {{site.data.keyword.bpshort}} template to quickly create the location.
2.  In your infrastructure provider, identify a location with at least three zones that are physically separate. You need three zones to spread out hosts evenly across the zones to increase [high availability](/docs/satellite?topic=satellite-ha). For example, your cloud provider might have three different zones within the same region, or you might use three racks with three separate networking and power supply systems in an on-prem environment.
3.  In each of the three zones in your infrastructure provider, plan to create compatible hosts to add to {{site.data.keyword.satelliteshort}}. The host instances in your infrastructure provider become the compute hosts to run the services in your {{site.data.keyword.satelliteshort}} location, like the worker nodes in a {{site.data.keyword.openshiftlong_notm}} cluster.
    * Each host must meet the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs) for {{site.data.keyword.satelliteshort}}, such as RHEL 7 operating system; at least 4 CPU, 16 RAM, and 100GB storage per host; full network connectivity between hosts in the same location; and more.
    * To calculate how many hosts you need, see [Sizing your {{site.data.keyword.satelliteshort}} location](#location-sizing).
4.  Use your infrastructure to power your {{site.data.keyword.satelliteshort}} resources.
    1.  Create your {{site.data.keyword.satelliteshort}} location based on the zones and hosts in your infrastructure provider. For more information, see [Deciding how to create your {{site.data.keyword.satelliteshort}} location](#create-options).
    2.  [Assign hosts](/docs/satellite?topic=satellite-hosts) to {{site.data.keyword.satelliteshort}}-enabled services like {{site.data.keyword.openshiftlong_notm}} clusters. The hosts become the worker nodes that provide the compute power for the service.

## Sizing your {{site.data.keyword.satelliteshort}} location
{: #location-sizing}

Because your {{site.data.keyword.satelliteshort}} location represents your own data center and infrastructure resources, the size of the location can be flexible according to what you want. You are not limited in the number of hosts that you attach to a location. However, as you plan your {{site.data.keyword.satelliteshort}} strategy, keep in mind the following sizing considerations.
{: shortdesc}

1. Set up a highly available {{site.data.keyword.satelliteshort}} location control plane with enough compute capacity to manage the resources in your {{site.data.keyword.satelliteshort}} location.
   *  **Minimum size**: To get started, you must attach and assign at least 3 hosts that meet the [minimum requirements](/docs/satellite?topic=satellite-host-reqs) of 4 vCPU, 16 GB memory, and 100 GB storage. You assign these hosts to 3 separate zones. The minimum of 3 hosts for the location control plane is for demonstration purposes. To continue to use the location for production workloads, [attach more hosts to the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#control-plane-scale) in multiples of 3, such as 6, 9, or 12 hosts.
   *  **High availability**: When you assign hosts to the {{site.data.keyword.satelliteshort}} location control plane, assign the hosts evenly across each of the 3 available zones of your {{site.data.keyword.cloud_notm}} multizone metro that you selected during location creation. To make the control plane highly available, make sure that the underlying hosts are in separate zones in your physical infrastructure environment. For example, you might assign 2 hosts each that run in 3 separate availability zones in your cloud provider, or that run in 3 separate physical systems in your own data center. You do not have to meet specific requirements for a "zone," but the separate zones must provide availability for system maintenance operations. For example, if 1 zone becomes unavailable due to a failure, or if 1 host becomes unavailable due to updating, the remaining 2 zones are still available to run control plane operations. A poor high availability setup is 2 hosts that are virtual machines on the same hypervisor, because servicing the underlying hardware such as to update the machine would make both hosts become unavailable. For more information, see [High availability for {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha).
   *  **Compute capacity**: {{site.data.keyword.satelliteshort}} monitors the available compute capacity of your location. When the location reaches 70% capacity, you see a warning status to notify you to attach more hosts to the location. If the location reaches 80% capacity, the status changes to **critical** and you see another warning that tells you to attach more hosts to the location.
2. Plan to keep **at least 3 extra hosts** attached and unassigned to your location. When you have extra hosts, then IBM can assign the hosts to your {{site.data.keyword.satelliteshort}} location control plane automatically when the location reaches the warning capacity threshold or an unhealthy host needs to be replaced.
3. To decide on the size and number of hosts to attach to your clusters, consider the workloads that you want to run in the location. Review the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-strategy#sizing) for guidance about the following considerations:
   * How many resources does my app require?
   * What else besides my app might use resources in the cluster?
   * What type of availability do I want my workload to have?
   * How many worker nodes (hosts) do I need to handle my workload?
   * How do I monitor resource usage and capacity in my cluster?

## Deciding how to create your {{site.data.keyword.satelliteshort}} location
{: #create-options}

Depending on your infrastructure provider, you have different options to create a {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

### On-premises infrastructure
{: #create-options-onprem}

For on-prem infrastructure, you can manually setup a {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

1. To understand how your infrastructure is used in {{site.data.keyword.satelliteshort}} location, [review the planning steps](#plan-zone-host-reqs).
2. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations#location-create).
3. [Attach 6 hosts to the {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-hosts#attach-hosts) that meet the [minimum requirements](/docs/satellite?topic=satellite-host-reqs).
4. [Assign the 6 hosts to the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).
5. [Attach at least 3 more hosts to the {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-hosts#attach-hosts) that meet the [minimum requirements](/docs/satellite?topic=satellite-host-reqs).
6. [Create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) and assign the hosts as worker nodes to the cluster, so that you can run {{site.data.keyword.openshiftshort}} workloads in your location.

### Cloud infrastructure like AWS, Azure, and GCP
{: #create-options-cloud}

For cloud provider infrastructure, you can follow provider-specific guides.
{: shortdesc}

**AWS**: You can choose between manually setting up an AWS location, or using a {{site.data.keyword.bpshort}} template to automatically create and set up the location.
*   Automatic: [Automating your location setup with a {{site.data.keyword.bpshort}} template](/docs/satellite?topic=satellite-locations#satloc-template).
*   Manual: [Adding AWS hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-aws).

**Azure**: [Adding Azure hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-azure).

**GCP**: [Adding GCP hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-gcp)

**{{site.data.keyword.cloud_notm}}**: For testing and demonstration purposes only, see [Adding {{site.data.keyword.cloud_notm}} hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-ibm).


### IBM-managed infrastructure
{: #create-options-sat-is}

IBM can ship infrastructure and set up a {{site.data.keyword.satelliteshort}} location for you. See [Getting started with {{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-infrastructure-service).
{: shortdesc}


