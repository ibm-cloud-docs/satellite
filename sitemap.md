---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-18"

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


# Sitemap
{: #sitemap}



## Getting started with {{site.data.keyword.satellitelong_notm}}
{: #sitemap_getting_started_with_}


[Getting started with {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-getting-started)

* [Video demonstration of setting up an on-prem location](/docs/satellite?topic=satellite-getting-started#gs-video-demo)

* [Start by considering your infrastructure](/docs/satellite?topic=satellite-getting-started#gs-start-here)

* [Step 1: Create your location](/docs/satellite?topic=satellite-getting-started#create-location)

* [Step 2: Attach compute hosts to your location](/docs/satellite?topic=satellite-getting-started#attach-hosts-to-location)
    * [Attaching hosts from cloud providers](/docs/satellite?topic=satellite-getting-started#gs-attach-hosts-cloud)
    * [Attaching hosts from on-premises data centers and edge networks](/docs/satellite?topic=satellite-getting-started#gs-attach-hosts-onprem)

* [Step 3: Assign your hosts to the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-getting-started#assign-hosts-to-cp)

* [What's next](/docs/satellite?topic=satellite-getting-started#whats-next)


## Understanding Satellite use cases
{: #sitemap_understanding_satellite_use_cases}


[About {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-about)
* [Benefits of using {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-about#about-benefits)
* [{{site.data.keyword.satelliteshort}} components](/docs/satellite?topic=satellite-about#components)
* [{{site.data.keyword.satelliteshort}} architecture](/docs/satellite?topic=satellite-about#architecture)
  * [Master and worker node components](/docs/satellite?topic=satellite-about#architecture-master-worker)
  * [Latency requirements](/docs/satellite?topic=satellite-about#architecture-latency)

[Securing access between {{site.data.keyword.cloud_notm}} and on-prem resources with {{site.data.keyword.satelliteshort}} Link](/docs/satellite?topic=satellite-sg-usecase)
* [{{site.data.keyword.satelliteshort}} as a Layer 4 connection solution](/docs/satellite?topic=satellite-sg-usecase#sg-alt)
* [Setting up a secure connection to {{site.data.keyword.cloud_notm}} with {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-sg-usecase#sg-alt-setup)
  * [Step 1: Deploy {{site.data.keyword.satelliteshort}} to your on-premises environment](/docs/satellite?topic=satellite-sg-usecase#sg-example-loc)
  * [Step 2: Set up secure communication channels by using {{site.data.keyword.satelliteshort}} Link](/docs/satellite?topic=satellite-sg-usecase#sg-example-link)

[Edge environments for AI, IoT, and machine learning](/docs/satellite?topic=satellite-edge-usecase)
* [Solving common edge workload challenges with {{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-edge-usecase#edge-challenges)
* [Setting up your edge solution with {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-edge-usecase#edge-solution)
  * [Step 1: Set up machine learning and model training for your data](/docs/satellite?topic=satellite-edge-usecase#edge-example-ml)
  * [Step 2: Deploy {{site.data.keyword.satelliteshort}} with a serverless component to your edge environment](/docs/satellite?topic=satellite-edge-usecase#edge-example-serverless)
  * [Step 3: Run model inferencing at the edge](/docs/satellite?topic=satellite-edge-usecase#edge-example-inferencing)


## Installing the CLI plug-in for {{site.data.keyword.satelliteshort}} commands
{: #sitemap_installing_the_cli_plug-in_for__commands}


[Installing the CLI plug-in for {{site.data.keyword.satelliteshort}} commands](/docs/satellite?topic=satellite-setup-cli)

* [Updating the CLI](/docs/satellite?topic=satellite-setup-cli#update-sat-cli)

* [Uninstalling the CLI](/docs/satellite?topic=satellite-setup-cli#uninstall-sat-cli)

* [CLI reference documentation](/docs/satellite?topic=satellite-setup-cli#cli-ref-docs)


## Preparing your infrastructure to use in Satellite
{: #sitemap_preparing_your_infrastructure_to_use_in_satellite}


[Planning your infrastructure environment for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-infrastructure-plan)
* [Planning your infrastructure zones and hosts to meet the {{site.data.keyword.satelliteshort}} requirements](/docs/satellite?topic=satellite-infrastructure-plan#plan-zone-host-reqs)
* [Sizing your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-infrastructure-plan#location-sizing)
* [Deciding how to create your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-infrastructure-plan#create-options)
  * [On-premises infrastructure](/docs/satellite?topic=satellite-infrastructure-plan#create-options-onprem)
  * [Cloud infrastructure like AWS, Azure, and GCP](/docs/satellite?topic=satellite-infrastructure-plan#create-options-cloud)
  * [IBM-managed infrastructure](/docs/satellite?topic=satellite-infrastructure-plan#create-options-sat-is)

[Host requirements](/docs/satellite?topic=satellite-host-reqs)
* [Host system requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-system)
  * [Computing characteristics](/docs/satellite?topic=satellite-host-reqs#reqs-host-compute)
  * [Packages and other machine configurations](/docs/satellite?topic=satellite-host-reqs#reqs-host-packages)
* [Host storage and attached devices](/docs/satellite?topic=satellite-host-reqs#reqs-host-storage)
* [Host network](/docs/satellite?topic=satellite-host-reqs#reqs-host-network)
  * [Networking configurations](/docs/satellite?topic=satellite-host-reqs#reqs-host-network-config)
  * [Host network bandwidth](/docs/satellite?topic=satellite-host-reqs#reqs-host-network-bandwidth)
  * [Network gateways and interfaces](/docs/satellite?topic=satellite-host-reqs#reqs-host-network-interface)
  * [Inbound connectivity](/docs/satellite?topic=satellite-host-reqs#reqs-host-network-firewall-inbound)
  * [Outbound connectivity](/docs/satellite?topic=satellite-host-reqs#reqs-host-network-firewall-outbound)
* [Host latency](/docs/satellite?topic=satellite-host-reqs#host-latency-test)
  * [Testing the latency between {{site.data.keyword.cloud_notm}} and the {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-host-reqs#host-latency-mzr)

[{{site.data.keyword.satellitelong_notm}} Infrastructure Service](/docs/satellite?topic=satellite-infrastructure-service)
* [Getting started with {{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-infrastructure-service#satis-getting-started)
  * [Stage 1: Planning your setup](/docs/satellite?topic=satellite-infrastructure-service#satis-gs-plan)
  * [Stage 2: Installing your on-prem infrastructure](/docs/satellite?topic=satellite-infrastructure-service#satis-gs-install)
  * [Stage 3: Transitioning to daily operations](/docs/satellite?topic=satellite-infrastructure-service#satis-gs-ops)
* [Understanding {{site.data.keyword.satelliteshort}} Infrastructure Service components](/docs/satellite?topic=satellite-infrastructure-service#satis-infra-about)
  * [Compute resources](/docs/satellite?topic=satellite-infrastructure-service#satis-infra-compute)
  * [Required license for operating system and container platform](/docs/satellite?topic=satellite-infrastructure-service#satis-infra-license)
  * [Storage for persistent volumes](/docs/satellite?topic=satellite-infrastructure-service#satis-infra-storage)
  * [Networking](/docs/satellite?topic=satellite-infrastructure-service#satis-infra-network)
  * [Power and cooling](/docs/satellite?topic=satellite-infrastructure-service#satis-infra-env)
* [Responsibilities with {{site.data.keyword.satelliteshort}} Infrastructure Service](/docs/satellite?topic=satellite-infrastructure-service#satis-responsibilities)
  * [Incident and operations management](/docs/satellite?topic=satellite-infrastructure-service#satis-incident-and-ops)
  * [Change management](/docs/satellite?topic=satellite-infrastructure-service#satis-change-management)
  * [Identity and access management](/docs/satellite?topic=satellite-infrastructure-service#satis-iam-responsibilities)
  * [Security and regulation compliance](/docs/satellite?topic=satellite-infrastructure-service#satis-security-compliance)
  * [Disaster recovery](/docs/satellite?topic=satellite-infrastructure-service#satis-disaster-recovery)

[Amazon Web Services (AWS)](/docs/satellite?topic=satellite-aws)
* [Manually adding AWS hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-aws#aws-host-attach)
* [Supported AWS instance types](/docs/satellite?topic=satellite-aws#aws-instance-types)
* [Security group settings](/docs/satellite?topic=satellite-aws#aws-reqs-secgroup)
* [Access to {{site.data.keyword.satelliteshort}} clusters and the {{site.data.keyword.openshiftshort}} web console](/docs/satellite?topic=satellite-aws#aws-reqs-console-access)

[Google Cloud Platform (GCP)](/docs/satellite?topic=satellite-gcp)
* [Adding GCP hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-gcp#gcp-host-attach)
* [Network firewall settings](/docs/satellite?topic=satellite-gcp#gcp-reqs-firewall)
* [Access to {{site.data.keyword.satelliteshort}} clusters and the {{site.data.keyword.openshiftshort}} web console](/docs/satellite?topic=satellite-gcp#gcp-reqs-console-access)

[Microsoft Azure](/docs/satellite?topic=satellite-azure)
* [Manually adding Azure hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-azure#azure-host-attach)
* [Security group settings](/docs/satellite?topic=satellite-azure#azure-reqs-firewall)
* [Access to {{site.data.keyword.satelliteshort}} clusters and the {{site.data.keyword.openshiftshort}} web console](/docs/satellite?topic=satellite-azure#azure-reqs-console-access)

[{{site.data.keyword.cloud_notm}} for tests](/docs/satellite?topic=satellite-ibm)
* [Adding {{site.data.keyword.cloud_notm}} hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-ibm#ibm-host-attach)


## Setting up {{site.data.keyword.satelliteshort}} locations
{: #sitemap_setting_up__locations}


[Setting up {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-locations)

* [Automating your location setup with a {{site.data.keyword.bpshort}} template](/docs/satellite?topic=satellite-locations#satloc-template)

* [Manually creating {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-locations#location-create)
    * [Creating locations from the console](/docs/satellite?topic=satellite-locations#location-create-console)
    * [Creating locations from the CLI](/docs/satellite?topic=satellite-locations#locations-create-cli)

* [Setting up the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane)
    * [Setting up the control plane from the console](/docs/satellite?topic=satellite-locations#control-plane-ui)
    * [Setting up the control plane from the CLI](/docs/satellite?topic=satellite-locations#control-plane-cli)
    * [What's next?](/docs/satellite?topic=satellite-locations#location-control-plane-next)

* [Adding capacity to your {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#control-plane-scale)

* [Copying a location](/docs/satellite?topic=satellite-locations#location-copy)

* [Monitoring location health](/docs/satellite?topic=satellite-locations#location-monitor-health)

* [Removing locations](/docs/satellite?topic=satellite-locations#location-remove)
    * [Removing locations from the console](/docs/satellite?topic=satellite-locations#location-remove-console)
    * [Removing locations from the CLI](/docs/satellite?topic=satellite-locations#location-remove-cli)


## Setting up {{site.data.keyword.satelliteshort}} hosts
{: #sitemap_setting_up__hosts}


[Setting up {{site.data.keyword.satelliteshort}} hosts](/docs/satellite?topic=satellite-hosts)

* [Understanding {{site.data.keyword.satelliteshort}} hosts](/docs/satellite?topic=satellite-hosts#host-concept)

* [Attaching hosts to your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-hosts#attach-hosts)
    * [Attaching hosts from the console](/docs/satellite?topic=satellite-hosts#attach-hosts-console)
    * [Attaching hosts from the CLI](/docs/satellite?topic=satellite-hosts#attach-hosts-cli)

* [Using host autoassignment](/docs/satellite?topic=satellite-hosts#host-autoassign-ov)
    * [About host labels](/docs/satellite?topic=satellite-hosts#host-autoassign-about)
    * [Automatically assigning hosts](/docs/satellite?topic=satellite-hosts#host-autoassign)
    * [Disabling host autoassignment](/docs/satellite?topic=satellite-hosts#host-autoassign-disable)
    * [Re-enabling host autoassignment](/docs/satellite?topic=satellite-hosts#host-autoassign-enable)

* [Manually assigning hosts to {{site.data.keyword.satelliteshort}} resources](/docs/satellite?topic=satellite-hosts#host-assign)
    * [Prerequisites](/docs/satellite?topic=satellite-hosts#host-assign-prereq)
    * [Assigning hosts from the console](/docs/satellite?topic=satellite-hosts#host-assign-ui)
    * [Assigning hosts from the CLI](/docs/satellite?topic=satellite-hosts#host-assign-cli)

* [Updating hosts that are assigned as worker nodes to {{site.data.keyword.satelliteshort}}-enabled services like clusters](/docs/satellite?topic=satellite-hosts#host-update-workers)
    * [Checking if a version update is available for worker node hosts](/docs/satellite?topic=satellite-hosts#host-update-workers-check)
    * [Reviewing the changelog for version updates](/docs/satellite?topic=satellite-hosts#host-update-workers-changelog)
    * [Applying version updates to worker node hosts](/docs/satellite?topic=satellite-hosts#host-update-workers-apply)

* [Updating {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-hosts#host-update-location)
    * [Considerations before you update control plane hosts](/docs/satellite?topic=satellite-hosts#host-update-considerations)
    * [Procedure to update control plane hosts](/docs/satellite?topic=satellite-hosts#host-update-cp-procedure)

* [Updating host metadata](/docs/satellite?topic=satellite-hosts#host-update-metadata)

* [Resetting the host key](/docs/satellite?topic=satellite-hosts#host-key-reset)

* [Monitoring host health](/docs/satellite?topic=satellite-hosts#host-monitor-health)

* [Removing hosts](/docs/satellite?topic=satellite-hosts#host-remove)
    * [Removing hosts from the console](/docs/satellite?topic=satellite-hosts#host-remove-console)
    * [Removing hosts from the CLI](/docs/satellite?topic=satellite-hosts#host-remove-cli)


## Creating {{site.data.keyword.openshiftshort}} clusters in {{site.data.keyword.satelliteshort}}
{: #sitemap_creating__clusters_in_{{site.data.keyword.satelliteshort}}}


[Creating {{site.data.keyword.openshiftshort}} clusters in {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=openshift-satellite-clusters)

* [Prerequisites](/docs/satellite?topic=openshift-satellite-clusters#satcluster-prereqs)

* [Creating {{site.data.keyword.openshiftshort}} clusters on {{site.data.keyword.satelliteshort}} from the console](/docs/satellite?topic=openshift-satellite-clusters#satcluster-create-console)

* [Creating {{site.data.keyword.openshiftshort}} clusters on {{site.data.keyword.satelliteshort}} from the CLI](/docs/satellite?topic=openshift-satellite-clusters#satcluster-create-cli)

* [Accessing and working with your {{site.data.keyword.openshiftshort}} clusters](/docs/satellite?topic=openshift-satellite-clusters#satcluster-access)

* [Setting up the internal container image registry](/docs/satellite?topic=openshift-satellite-clusters#satcluster-internal-registry)

* [Managing {{site.data.keyword.satelliteshort}} worker pools](/docs/satellite?topic=openshift-satellite-clusters#satcluster-worker-pools)
    * [Creating {{site.data.keyword.satelliteshort}} worker pools with host labels for autoassignment](/docs/satellite?topic=openshift-satellite-clusters#sat-pool-create-labels)
    * [Maintaining {{site.data.keyword.satelliteshort}} worker pools](/docs/satellite?topic=openshift-satellite-clusters#sat-pool-maintenance)

* [Exposing apps](/docs/satellite?topic=openshift-satellite-clusters#satcluster-expose-apps)

* [Storing application data in persistent storage](/docs/satellite?topic=openshift-satellite-clusters#satcluster-storage)

* [Removing {{site.data.keyword.satelliteshort}} worker nodes or clusters](/docs/satellite?topic=openshift-satellite-clusters#satcluster-rm)

* [Limitations for {{site.data.keyword.openshiftshort}} clusters in {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=openshift-satellite-clusters#satcluster-limitations)


## Deploying Kubernetes resources across clusters with {{site.data.keyword.satelliteshort}} configurations
{: #sitemap_deploying_kubernetes_resources_across_clusters_with__configurations}


[Deploying Kubernetes resources across clusters with {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-cluster-config)

* [Understanding {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-cluster-config#understanding-satcon)
    * [How {{site.data.keyword.satelliteshort}} configurations work](/docs/satellite?topic=satellite-cluster-config#satcon-flow)
    * [Key concepts](/docs/satellite?topic=satellite-cluster-config#satcon-terminology)

* [Setting up clusters to use with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig)
    * [Prerequisites](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig-prereq)
    * [Setting up cluster groups](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig-groups)
    * [Granting {{site.data.keyword.satelliteshort}} Config access to your clusters](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig-access)

* [Creating {{site.data.keyword.satelliteshort}} configurations from the console](/docs/satellite?topic=satellite-cluster-config#create-satconfig-ui)

* [Creating {{site.data.keyword.satelliteshort}} configurations from the CLI](/docs/satellite?topic=satellite-cluster-config#create-satconfig-cli)

* [Reviewing resources that are managed by {{site.data.keyword.satelliteshort}} config](/docs/satellite?topic=satellite-cluster-config#satconfig-resources)

* [Using {{site.data.keyword.satelliteshort}} config with existing {{site.data.keyword.openshiftlong_notm}} clusters in {{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-cluster-config#manage-existing-openshift-clusters)
    * [Registering existing {{site.data.keyword.openshiftshort}} clusters with {{site.data.keyword.satelliteshort}} config](/docs/satellite?topic=satellite-cluster-config#existing-openshift-clusters)
    * [Removing {{site.data.keyword.satelliteshort}} config from your cluster](/docs/satellite?topic=satellite-cluster-config#remove-satconfig)


## Connecting {{site.data.keyword.satelliteshort}} locations with external services using Link endpoints
{: #sitemap_connecting__locations_with_external_services_using_link_endpoints}


[Connecting {{site.data.keyword.satelliteshort}} locations with external services using Link endpoints](/docs/satellite?topic=satellite-link-location-cloud)

* [About {{site.data.keyword.satelliteshort}} endpoints](/docs/satellite?topic=satellite-link-location-cloud#link-about)
    * [Architecture](/docs/satellite?topic=satellite-link-location-cloud#link-architecture)
    * [External network requirements and security](/docs/satellite?topic=satellite-link-location-cloud#link-security)
    * [Encryption protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols)
    * [Access and audit controls](/docs/satellite?topic=satellite-link-location-cloud#link-audit-about)
    * [Default Link endpoints for {{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-link-location-cloud#default-link-endpoints)
    * [Use cases](/docs/satellite?topic=satellite-link-location-cloud#link-usecases)

* [Creating `cloud` endpoints to connect to resources outside of the location](/docs/satellite?topic=satellite-link-location-cloud#link-cloud)
    * [Creating cloud endpoints by using the console](/docs/satellite?topic=satellite-link-location-cloud#link-cloud-ui)
    * [Creating cloud endpoints by using the CLI](/docs/satellite?topic=satellite-link-location-cloud#link-cloud-cli)
    * [Testing connections through cloud endpoints](/docs/satellite?topic=satellite-link-location-cloud#link-cloud-test)

* [Creating `location` endpoints to connect to resources in a location](/docs/satellite?topic=satellite-link-location-cloud#link-location)
    * [Creating location endpoints by using the console](/docs/satellite?topic=satellite-link-location-cloud#link-location-ui)
    * [Creating location endpoints by using the CLI](/docs/satellite?topic=satellite-link-location-cloud#link-location-cli)
    * [Setting up source lists to limit access to endpoints](/docs/satellite?topic=satellite-link-location-cloud#link-sources)

* [Auditing events for endpoint actions](/docs/satellite?topic=satellite-link-location-cloud#link-audit)

* [Logging and monitoring network traffic for endpoints](/docs/satellite?topic=satellite-link-location-cloud#link-health)
    * [Setting up {{site.data.keyword.mon_short}} for {{site.data.keyword.satelliteshort}} Link metrics](/docs/satellite?topic=satellite-link-location-cloud#link-mon)
    * [Running a packet capture of endpoint traffic](/docs/satellite?topic=satellite-link-location-cloud#link-packet-capture)

* [Enabling and disabling endpoints](/docs/satellite?topic=satellite-link-location-cloud#enable_disable_endpoint)


## Logging and monitoring
{: #sitemap_logging_and_monitoring}


[Logging for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-health)
* [Setting up {{site.data.keyword.la_short}} for {{site.data.keyword.satelliteshort}} location platform logs](/docs/satellite?topic=satellite-health#setup-la)
  * [Enabling platform logs](/docs/satellite?topic=satellite-health#enable-la)
  * [Viewing logs for your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-health#view-la)
  * [Analyzing logs for your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-health#analyze-la)
  * [`R00XX` error logs](/docs/satellite?topic=satellite-health#logs-error)
  * [Enablement of resource deployment logs](/docs/satellite?topic=satellite-health#logs-deploy)
  * [Endpoint health status logs](/docs/satellite?topic=satellite-health#logs-link)
* [Setting up {{site.data.keyword.at_short}} for {{site.data.keyword.satelliteshort}} location events](/docs/satellite?topic=satellite-health#setup-at)
* [Setting up logging for clusters](/docs/satellite?topic=satellite-health#setup-clusters)

[Monitoring for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-monitor)
* [Understanding what is logged and monitored by default](/docs/satellite?topic=satellite-monitor#health-default)
  * [Auditing events for {{site.data.keyword.satelliteshort}} actions](/docs/satellite?topic=satellite-monitor#audit-events)
  * [IBM monitoring to resolve and report location alerts](/docs/satellite?topic=satellite-monitor#monitoring-default)
* [Viewing location, host, and cluster health](/docs/satellite?topic=satellite-monitor#view-health)
  * [Viewing location health](/docs/satellite?topic=satellite-monitor#location-health)
  * [Viewing host health](/docs/satellite?topic=satellite-monitor#host-health)
  * [Viewing cluster health](/docs/satellite?topic=satellite-monitor#cluster-health)
  * [Viewing Kubernetes resources in clusters](/docs/satellite?topic=satellite-monitor#kubernetes-resources-health)
  * [Viewing {{site.data.keyword.satelliteshort}} config registration status for clusters](/docs/satellite?topic=satellite-monitor#satconfig-registration-status)
* [Setting up {{site.data.keyword.mon_short}} for {{site.data.keyword.satelliteshort}} location platform metrics](/docs/satellite?topic=satellite-monitor#setup-mon)
  * [Available metrics](/docs/satellite?topic=satellite-monitor#available-metrics)
  * [Attributes for segmentation](/docs/satellite?topic=satellite-monitor#attributes)
* [Setting up monitoring for clusters](/docs/satellite?topic=satellite-monitor#setup-clusters)


## Setting up storage
{: #sitemap_setting_up_storage}


[Understanding {{site.data.keyword.satelliteshort}} storage templates](/docs/satellite?topic=satellite-sat-storage-template-ov)
* [How do I configure storage on {{site.data.keyword.satelliteshort}}?](/docs/satellite?topic=satellite-sat-storage-template-ov#storage-sat-configure)
* [What are the benefits of using templates?](/docs/satellite?topic=satellite-sat-storage-template-ov#storage-template-benefits)
* [How do storage templates work?](/docs/satellite?topic=satellite-sat-storage-template-ov#storage-template-flow)
* [Which storage providers have {{site.data.keyword.satelliteshort}} storage templates?](/docs/satellite?topic=satellite-sat-storage-template-ov#storage-template-ov-providers)


## AWS storage templates
{: #sitemap_aws_storage_templates}


[AWS EBS](/docs/satellite?topic=satellite-config-storage-ebs)
* [Prerequisites](/docs/satellite?topic=satellite-config-storage-ebs#aws-ebs-prereq)
* [Creating an AWS EBS storage configuration](/docs/satellite?topic=satellite-config-storage-ebs#sat-storage-aws-ebs)
* [Deploying an app that uses AWS EBS storage](/docs/satellite?topic=satellite-config-storage-ebs#sat-storage-ebs-deploy)
* [Removing AWS EBS storage from your apps](/docs/satellite?topic=satellite-config-storage-ebs#aws-ebs-rm)
* [Removing the AWS EBS storage configuration from your cluster](/docs/satellite?topic=satellite-config-storage-ebs#aws-ebs-template-rm)
* [AWS EBS storage configuration CLI parameter reference](/docs/satellite?topic=satellite-config-storage-ebs#sat-storage-aws-ebs-params-cli)
* [Storage class reference](/docs/satellite?topic=satellite-config-storage-ebs#sat-ebs-sc-reference)

[AWS EFS](/docs/satellite?topic=satellite-config-storage-efs)
* [Prerequisites](/docs/satellite?topic=satellite-config-storage-efs#sat-storage-efs-prereqs)
* [Creating an AWS EFS storage configuration](/docs/satellite?topic=satellite-config-storage-efs#sat-storage-aws-efs)
* [Deploying an app that uses AWS EFS storage](/docs/satellite?topic=satellite-config-storage-efs#sat-storage-efs-deploy)
* [Removing AWS EFS storage from your apps](/docs/satellite?topic=satellite-config-storage-efs#aws-efs-rm)
* [Removing the AWS EFS storage configuration from your cluster](/docs/satellite?topic=satellite-config-storage-efs#aws-efs-template-rm)
* [AWS EFS storage configuration parameter reference](/docs/satellite?topic=satellite-config-storage-efs#sat-storage-aws-efs-params-cli)
* [Storage class reference](/docs/satellite?topic=satellite-config-storage-efs#efs-sc-reference)


## IBM storage templates
{: #sitemap_ibm_storage_templates}


[IBM Spectrum Scale driver](/docs/satellite?topic=satellite-config-storage-spectrum-scale)
* [Prerequisites](/docs/satellite?topic=satellite-config-storage-spectrum-scale#sat-storage-spectrum-scale-prereq)
* [Mapping IBM Spectrum Scale hosts to worker node names](/docs/satellite?topic=satellite-config-storage-spectrum-scale#sat-storage-spectrum-scale-ts-mapping)
* [Creating an Spectrum Scale storage configuration in the command line](/docs/satellite?topic=satellite-config-storage-spectrum-scale#sat-storage-spectrum-scale-cli)
* [Assigning your Spectrum Scale storage configuration to a cluster](/docs/satellite?topic=satellite-config-storage-spectrum-scale#assign-storage-spectrum-scale)
  * [Assigning a storage configuraton in the command line](/docs/satellite?topic=satellite-config-storage-spectrum-scale#assign-storage-spectrum-scale-cli)
  * [Changing the Spectrum Scale CSI driver mount point](/docs/satellite?topic=satellite-config-storage-spectrum-scale#ess-change-mount-point)
* [Deploying an app that uses your Spectrum Scale storage](/docs/satellite?topic=satellite-config-storage-spectrum-scale#storage-spectrum-app-deploy)
* [Removing the Spectrum Scale storage configuration from the CLI](/docs/satellite?topic=satellite-config-storage-spectrum-scale#sat-storage-spectrum-rm-cli)
* [Troubleshooting](/docs/satellite?topic=satellite-config-storage-spectrum-scale#ess-ts)
  * [Rebuilding the portability layer](/docs/satellite?topic=satellite-config-storage-spectrum-scale#ess-ts-rebuilding)
* [Storage class reference](/docs/satellite?topic=satellite-config-storage-spectrum-scale#sat-storage-spectrum-scale-sc-ref)
* [Additional references](/docs/satellite?topic=satellite-config-storage-spectrum-scale#sat-storage-spectrum-scale-ref)
* [Limitations](/docs/satellite?topic=satellite-config-storage-spectrum-scale#sat-storage-spectrum-scale-limits)
  * [Spectrum Scale configuration parameter reference](/docs/satellite?topic=satellite-config-storage-spectrum-scale#sat-storage-spectrum-scale-params-cli)

[IBM Systems {{site.data.keyword.blockstorageshort}} CSI driver](/docs/satellite?topic=satellite-config-storage-block-csi)
* [Prerequisites](/docs/satellite?topic=satellite-config-storage-block-csi#sat-storage-block-csi-prereq)
* [Creating a {{site.data.keyword.blockstorageshort}} configuration in the command line](/docs/satellite?topic=satellite-config-storage-block-csi#sat-storage-block-csi-cli)
* [Assigning your {{site.data.keyword.blockstorageshort}} configuration to a cluster](/docs/satellite?topic=satellite-config-storage-block-csi#assign-storage-block-csi)
  * [Assigning a storage configuraton in the command line](/docs/satellite?topic=satellite-config-storage-block-csi#assign-storage-block-csi-cli)
* [Deploying an app that uses your IBM {{site.data.keyword.blockstorageshort}}](/docs/satellite?topic=satellite-config-storage-block-csi#storage-block-csi-app-deploy)


## NetApp storage templates
{: #sitemap_netapp_storage_templates}


[NetApp Trident Operator](/docs/satellite?topic=satellite-config-storage-netapp-trident)
* [Creating a NetApp Trident storage configuration in the command line](/docs/satellite?topic=satellite-config-storage-netapp-trident#sat-storage-netapp-cli)
* [Assigning your NetApp storage configuration to a cluster](/docs/satellite?topic=satellite-config-storage-netapp-trident#assign-storage-netapp)
  * [Assigning a storage configuraton in the command line](/docs/satellite?topic=satellite-config-storage-netapp-trident#assign-storage-netapp-cli)
  * [Removing the NetApp Trident storage assignment and configuration from the CLI](/docs/satellite?topic=satellite-config-storage-netapp-trident#netapp-trident-template-rm-cli)

[NetApp ONTAP-NAS](/docs/satellite?topic=satellite-config-storage-netapp-nas)
* [Creating a NetApp ONTAP-NAS storage configuration in the command line](/docs/satellite?topic=satellite-config-storage-netapp-nas#sat-storage-netapp-cli-nas)
* [Assigning your NetApp ONTAP-NAS storage configuration to a cluster](/docs/satellite?topic=satellite-config-storage-netapp-nas#assign-storage-netapp-nas)
  * [Assigning a storage configuration in the command line](/docs/satellite?topic=satellite-config-storage-netapp-nas#assign-storage-netapp-cli-nas)
* [Deploying an app that uses ONTAP-NAS storage](/docs/satellite?topic=satellite-config-storage-netapp-nas#sat-storage-netapp-nas-deploy)
* [Removing NetApp ONTAP-NAS storage from your apps](/docs/satellite?topic=satellite-config-storage-netapp-nas#netapp-nas-rm)
  * [Removing the NetApp ONTAP-NAS storage assignment and configuration from the CLI](/docs/satellite?topic=satellite-config-storage-netapp-nas#netapp-nas-template-rm-cli)
* [NetApp ONTAP-NAS storage configuration parameter reference](/docs/satellite?topic=satellite-config-storage-netapp-nas#sat-storage-netapp-params-cli-nas)
* [Storage class reference](/docs/satellite?topic=satellite-config-storage-netapp-nas#netapp-sc-reference-nas)

[NetApp ONTAP-SAN](/docs/satellite?topic=satellite-config-storage-netapp)
* [Creating a NetApp Trident SAN storage configuration in the command line](/docs/satellite?topic=satellite-config-storage-netapp#sat-storage-netapp-cli-san)
* [Assigning your NetApp storage configuration to a cluster](/docs/satellite?topic=satellite-config-storage-netapp#assign-storage-netapp-san)
  * [Assigning a storage configuration in the command line](/docs/satellite?topic=satellite-config-storage-netapp#assign-storage-netapp-cli-san)
* [NetApp Trident storage configuration parameter reference](/docs/satellite?topic=satellite-config-storage-netapp#sat-storage-netapp-params-cli-san)
* [Storage class reference](/docs/satellite?topic=satellite-config-storage-netapp#netapp-sc-reference-san)


## Red Hat storage templates
{: #sitemap_red_hat_storage_templates}


[Local Storage Operator - Block](/docs/satellite?topic=satellite-config-storage-local-block)
* [Prerequisites](/docs/satellite?topic=satellite-config-storage-local-block#sat-storage-local-prereqs)
  * [Getting the device details for your local block storage configuration](/docs/satellite?topic=satellite-config-storage-local-block#sat-storage-block-local-devices)
  * [Labeling your worker nodes](/docs/satellite?topic=satellite-config-storage-local-block#sat-storage-block-local-labels)
* [Creating a local block storage configuration in the command line](/docs/satellite?topic=satellite-config-storage-local-block#sat-storage-local-block-cli)
* [Assigning your storage configuration to a cluster](/docs/satellite?topic=satellite-config-storage-local-block#assign-storage-local-block)
  * [Assigning a storage configuration in the command line](/docs/satellite?topic=satellite-config-storage-local-block#assign-storage-local-block-cli)
* [Deploying an app that uses your local block storage](/docs/satellite?topic=satellite-config-storage-local-block#deploy-app-local-block)
* [Removing the local block storage configuration from your cluster](/docs/satellite?topic=satellite-config-storage-local-block#sat-storage-remove-local-block-config)
* [Local block storage configuration parameter reference](/docs/satellite?topic=satellite-config-storage-local-block#sat-storage-local-block-params-cli)
* [Storage class reference](/docs/satellite?topic=satellite-config-storage-local-block#local-block-sc-ref)

[Local Storage Operator - File](/docs/satellite?topic=satellite-config-storage-local-file)
* [Prerequisites](/docs/satellite?topic=satellite-config-storage-local-file#prerequisites)
  * [Getting the device details for your local file storage configuration](/docs/satellite?topic=satellite-config-storage-local-file#sat-storage-file-local-devices)
  * [Labeling your worker nodes](/docs/satellite?topic=satellite-config-storage-local-file#sat-storage-file-local-labels)
* [Creating a local file storage configuration in the command line](/docs/satellite?topic=satellite-config-storage-local-file#sat-storage-local-file-cli)
* [Assigning your storage configuration to a cluster](/docs/satellite?topic=satellite-config-storage-local-file#assign-storage-local-file)
  * [Assigning a storage configuraton in the command line](/docs/satellite?topic=satellite-config-storage-local-file#assign-storage-local-file-cli)
* [Deploying an app that uses your local file storage](/docs/satellite?topic=satellite-config-storage-local-file#deploy-app-local-file)
* [Removing the local file storage configuration from your cluster](/docs/satellite?topic=satellite-config-storage-local-file#sat-storage-remove-local-file-config)
* [Local file storage configuration parameter reference](/docs/satellite?topic=satellite-config-storage-local-file#sat-storage-local-file-params-cli)
* [Storage class reference](/docs/satellite?topic=satellite-config-storage-local-file#local-file-sc-reference)

[OpenShift Container Storage using local disks](/docs/satellite?topic=satellite-config-storage-ocs-local)
* [Prerequisites](/docs/satellite?topic=satellite-config-storage-ocs-local#sat-storage-ocs-local-prereq)
* [Optional: Setting up an {{site.data.keyword.cos_full_notm}} backing store](/docs/satellite?topic=satellite-config-storage-ocs-local#sat-storage-ocs-local-cos)
* [Getting the device details for your OCS configuration](/docs/satellite?topic=satellite-config-storage-ocs-local#sat-storage-ocs-local-devices)
* [Creating an OpenShift Container Storage configuration in the command line](/docs/satellite?topic=satellite-config-storage-ocs-local#sat-storage-ocs-local-cli)
* [Assigning your OCS storage configuration to a cluster](/docs/satellite?topic=satellite-config-storage-ocs-local#assign-storage-ocs-local)
* [Deploying an app that uses OpenShift Container Storage](/docs/satellite?topic=satellite-config-storage-ocs-local#sat-storage-ocs-local-deploy)
* [Upgrading your OCS version](/docs/satellite?topic=satellite-config-storage-ocs-local#ocs-local-upgrade)
* [Removing OpenShift Container Storage from your apps](/docs/satellite?topic=satellite-config-storage-ocs-local#ocs-local-rm)
* [Removing the OCS local storage configuration from your cluster](/docs/satellite?topic=satellite-config-storage-ocs-local#ocs-local-template-rm)
* [OpenShift Container Storage configuration parameter reference](/docs/satellite?topic=satellite-config-storage-ocs-local#sat-storage-ocs-local-params-cli)
* [Storage class reference](/docs/satellite?topic=satellite-config-storage-ocs-local#sat-storage-ocs-local-sc-ref)

[OpenShift Container Storage for remote devices](/docs/satellite?topic=satellite-config-storage-ocs-remote)
* [Prerequisites](/docs/satellite?topic=satellite-config-storage-ocs-remote#sat-storage-ocs-remote-prereq)
  * [Optional: Creating the {{site.data.keyword.cos_full_notm}} service instance](/docs/satellite?topic=satellite-config-storage-ocs-remote#sat-storage-ocs-remote-cos)
* [Creating an OpenShift Container Storage configuration in the command line](/docs/satellite?topic=satellite-config-storage-ocs-remote#sat-storage-ocs-remote-cli)
* [Assigning your OCS storage configuration to a cluster](/docs/satellite?topic=satellite-config-storage-ocs-remote#assign-storage-ocs-remote)
  * [Assigning a storage configuraton in the command line](/docs/satellite?topic=satellite-config-storage-ocs-remote#assign-storage-ocs-remote-cli)
* [Upgrading your OCS configuration](/docs/satellite?topic=satellite-config-storage-ocs-remote#sat-storage-ocs-remote-upgrade-config)
  * [Removing the OCS remote storage assignment from the command line](/docs/satellite?topic=satellite-config-storage-ocs-remote#ocs-remote-template-rm-cli)
* [OpenShift Container Storage configuration parameter reference](/docs/satellite?topic=satellite-config-storage-ocs-remote#sat-storage-ocs-remote-params-cli)
* [Storage class reference](/docs/satellite?topic=satellite-config-storage-ocs-remote#sat-storage-ocs-remote-sc-ref)


## Storage class reference
{: #sitemap_storage_class_reference}


[Storage class reference](/docs/satellite?topic=satellite-storage-class-ref)

* [AWS EBS](/docs/satellite?topic=satellite-storage-class-ref#ebs-ref)

* [AWS EFS](/docs/satellite?topic=satellite-storage-class-ref#efs-ref)

* [Local block storage](/docs/satellite?topic=satellite-storage-class-ref#local-block-ref)

* [Local file storage](/docs/satellite?topic=satellite-storage-class-ref#local-file-ref)

* [NetApp Trident NAS](/docs/satellite?topic=satellite-storage-class-ref#netapp-nas-ref)

* [NetApp Trident SAN](/docs/satellite?topic=satellite-storage-class-ref#netapp-san-ref)

* [OpenShift Container Storage for local volumes](/docs/satellite?topic=satellite-storage-class-ref#ocs-local-ref)

* [OpenShift Container Storage for remote volumes](/docs/satellite?topic=satellite-storage-class-ref#ocs-remote-ref)


## Enhancing security
{: #sitemap_enhancing_security}


[Managing access](/docs/satellite?topic=satellite-iam)
* [Understanding {{site.data.keyword.satelliteshort}} resource types in IAM](/docs/satellite?topic=satellite-iam#iam-resource-types)
  * [Location](/docs/satellite?topic=satellite-iam#iam-resource-loc)
  * [Configuration, subscription, cluster, cluster group, and resource](/docs/satellite?topic=satellite-iam#iam-resource-config)
  * [Link](/docs/satellite?topic=satellite-iam#iam-resource-link)
  * [Other services](/docs/satellite?topic=satellite-iam#iam-resource-services)
* [Assigning access with {{site.data.keyword.cloud_notm}} IAM](/docs/satellite?topic=satellite-iam#iam-assign)
  * [Overview of the process to set up access to {{site.data.keyword.satellitelong_notm}} in {{site.data.keyword.cloud_notm}} IAM](/docs/satellite?topic=satellite-iam#iam-assign-overview)
  * [Example UI steps](/docs/satellite?topic=satellite-iam#iam-assign-ui)
  * [Example CLI steps](/docs/satellite?topic=satellite-iam#iam-assign-cli)
* [IAM platform and service roles](/docs/satellite?topic=satellite-iam#iam-roles)
  * [Access policies](/docs/satellite?topic=satellite-iam#iam-roles-policies)
  * [Platform access roles](/docs/satellite?topic=satellite-iam#iam-roles-platform)
  * [Service access roles](/docs/satellite?topic=satellite-iam#iam-roles-service)
  * [Platform and service roles for {{site.data.keyword.openshiftshort}} clusters](/docs/satellite?topic=satellite-iam#iam-roles-clusters)
* [Common use cases and roles in {{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-iam#iam-roles-usecases)
* [API keys in {{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-iam#sat-api-keys)
  * [Container service API key](/docs/satellite?topic=satellite-iam#api-keys-containers)
  * [Template API key](/docs/satellite?topic=satellite-iam#api-keys-templates)
* [Common permissions in other cloud providers](/docs/satellite?topic=satellite-iam#permissions-other-clouds)
  * [AWS permissions](/docs/satellite?topic=satellite-iam#permissions-aws)

[Learning about {{site.data.keyword.satelliteshort}} architecture, workload isolation, and dependencies](/docs/satellite?topic=satellite-service-architecture)
* [{{site.data.keyword.satelliteshort}} architecture](/docs/satellite?topic=satellite-service-architecture#architecture)
  * [Master and worker node components](/docs/satellite?topic=satellite-service-architecture#architecture-master-worker)
  * [Latency requirements](/docs/satellite?topic=satellite-service-architecture#architecture-latency)
* [Workload isolation in {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-service-architecture#workload-isolation)
  * [Workload isolation in the {{site.data.keyword.cloud_notm}} multizone metro that manages your location](/docs/satellite?topic=satellite-service-architecture#workload-isolation-cloud)
  * [Workload isolation in your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-service-architecture#workload-isolation-location)
* [Dependencies to other {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-service-architecture#cloud-service-dependencies)
* [Dependencies to third-party services](/docs/satellite?topic=satellite-service-architecture#3rd-party-dependencies)

[High availability and disaster recovery](/docs/satellite?topic=satellite-ha)
* [About high availability and disaster recovery](/docs/satellite?topic=satellite-ha#ha-about)
* [Understanding high availability in {{site.data.keyword.satellitelong_notm}}](/docs/satellite?topic=satellite-ha#ha-understand)
  * [High availability of the {{site.data.keyword.satelliteshort}} control plane master](/docs/satellite?topic=satellite-ha#ha-control-plane-master)
  * [High availability of the {{site.data.keyword.satelliteshort}} control plane worker nodes](/docs/satellite?topic=satellite-ha#ha-control-plane-worker)
  * [High availability of {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-ha#ha-cloud-services)
* [Basic control plane worker setup](/docs/satellite?topic=satellite-ha#satellite-basic-setup)
* [Highly available control plane worker setup](/docs/satellite?topic=satellite-ha#satellite-ha-setup)
  * [Example for a high availability setup in an on-premises data center](/docs/satellite?topic=satellite-ha#example-ha-onprem)
  * [Example for a high availability setup in another cloud provider](/docs/satellite?topic=satellite-ha#example-ha-cloudprovider)

[Security and compliance](/docs/satellite?topic=satellite-compliance)
* [Data security](/docs/satellite?topic=satellite-compliance#secure-data)
  * [What data is stored when I use {{site.data.keyword.satelliteshort}}? How can I use my own keys to encrypt my data?](/docs/satellite?topic=satellite-compliance#secure-data-store-encrypt)
  * [How do I make my data secure over {{site.data.keyword.satelliteshort}} Link?](/docs/satellite?topic=satellite-compliance#secure-data-link)
  * [What measures can I take to secure user access to data in my location?](/docs/satellite?topic=satellite-compliance#secure-data-access)
* [IBM operational access](/docs/satellite?topic=satellite-compliance#operational-access)
  * [What automated access does IBM have to my location?](/docs/satellite?topic=satellite-compliance#operational-access-automated)
  * [What access do IBM SREs have to my location control plane, including the masters of {{site.data.keyword.satelliteshort}}-enabled service clusters?](/docs/satellite?topic=satellite-compliance#operational-access-control-plane)
  * [What access do IBM SREs have to my data and workloads that run in my {{site.data.keyword.satelliteshort}}-enabled service clusters?](/docs/satellite?topic=satellite-compliance#operational-access-workloads)
  * [How can I monitor and manage IBM access into my location? How can I know that there are no backdoor access points on the hosts?](/docs/satellite?topic=satellite-compliance#operational-access-monitor)
  * [What happens if {{site.data.keyword.satelliteshort}} Link becomes unavailable? Can IBM still maintain my {{site.data.keyword.satelliteshort}} location?](/docs/satellite?topic=satellite-compliance#operational-access-availability)
* [Digital certificates for {{site.data.keyword.satelliteshort}} hosts and domains](/docs/satellite?topic=satellite-compliance#certs-hosts-domains)
* [Platform compliance and certification](/docs/satellite?topic=satellite-compliance#platform-compliance)
  * [What compliance standards does the service meet?](/docs/satellite?topic=satellite-compliance#compliance-standards)
  * [Which areas of security compliance am I responsible for?](/docs/satellite?topic=satellite-compliance#compliance-responsibilities)
  * [What are the security compliance responsibilities of {{site.data.keyword.satelliteshort}}-enabled services?](/docs/satellite?topic=satellite-compliance#compliance-services)

[Securing your connection](/docs/satellite?topic=satellite-service-connection)
* [User access to resources that run in your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-service-connection#user-access)
  * [Service-instance clusters](/docs/satellite?topic=satellite-service-connection#user-access-service)
  * [IBM private network access with {{site.data.keyword.satelliteshort}} Link](/docs/satellite?topic=satellite-service-connection#user-access-loc-ep)
  * [Application workloads that run in clusters](/docs/satellite?topic=satellite-service-connection#user-access-apps)
* [{{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-service-connection#ibm-cloud-access)

[Securing your data](/docs/satellite?topic=satellite-data-security)
* [What information is stored with IBM when using {{site.data.keyword.satelliteshort}}?](/docs/satellite?topic=satellite-data-security#sat-sensitive-data)
  * [Stored information when you create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-data-security#sat-sensitive-data-default)
  * [Stored information from resources that you create](/docs/satellite?topic=satellite-data-security#sat-sensitive-data-user-added)
* [How is my information stored, backed up, and encrypted?](/docs/satellite?topic=satellite-data-security#sat-data-encryption)
* [Which {{site.data.keyword.cloud_notm}} region is my information stored in?](/docs/satellite?topic=satellite-data-security#sat_data-location)
* [How can I remove my information?](/docs/satellite?topic=satellite-data-security#sat-data-removal)

[Auditing events](/docs/satellite?topic=satellite-at_events)
* [Events for {{site.data.keyword.satelliteshort}} clusters](/docs/satellite?topic=satellite-at_events#at_actions_clusters)
* [Events for the {{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-at_events#at_actions_link)
* [Events for the {{site.data.keyword.satelliteshort}} Config component](/docs/satellite?topic=satellite-at_events#at_actions_config)
* [Viewing events](/docs/satellite?topic=satellite-at_events#at_ui)


## API reference

[API reference](https://containers.cloud.ibm.com/global/swagger-global-api/#/satellite-beta-cluster){: external}


## CLI reference for {{site.data.keyword.satelliteshort}} commands
{: #sitemap_cli_reference_for__commands}


[CLI reference for {{site.data.keyword.satelliteshort}} commands](/docs/satellite?topic=satellite-satellite-cli-reference)

* [`ibmcloud sat` commands](/docs/satellite?topic=satellite-satellite-cli-reference#satellite-cli-map)

* [Cluster commands](/docs/satellite?topic=satellite-satellite-cli-reference#sat-cluster-commands)
    * [`ibmcloud sat cluster get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-get)
    * [`ibmcloud sat cluster ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-ls)
    * [`ibmcloud sat cluster register`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-register)
    * [`ibmcloud sat cluster unregister`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-cluster-unregister)

* [Cluster group commands](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-commands)
    * [`ibmcloud sat group attach`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-attach)
    * [`ibmcloud sat group create`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-create)
    * [`ibmcloud sat group detach`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-detach)
    * [`ibmcloud sat group get`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-get)
    * [`ibmcloud sat group ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-ls)
    * [`ibmcloud sat group rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-group-rm)

* [Config commands](/docs/satellite?topic=satellite-satellite-cli-reference#sat-config-configuration-commands)
    * [`ibmcloud sat config create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-create)
    * [`ibmcloud sat config get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-get)
    * [`ibmcloud sat config ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-ls)
    * [`ibmcloud sat config rename`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-rename)
    * [`ibmcloud sat config rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-rm)
    * [`ibmcloud sat config version create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-version-create)
    * [`ibmcloud sat config version get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-version-get)
    * [`ibmcloud sat config version rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-configuration-version-rm)

* [Endpoint commands](/docs/satellite?topic=satellite-satellite-cli-reference#sat-endpoint-commands)
    * [`ibmcloud sat endpoint create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-create)
    * [`ibmcloud sat endpoint get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-get)
    * [`ibmcloud sat endpoint ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-ls)
    * [`ibmcloud sat endpoint rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-rm)
    * [`ibmcloud sat endpoint update`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-endpoint-update)

* [Host commands](/docs/satellite?topic=satellite-satellite-cli-reference#sat-host-commands)
    * [`ibmcloud sat host assign`](/docs/satellite?topic=satellite-satellite-cli-reference#host-assign)
    * [`ibmcloud sat host attach`](/docs/satellite?topic=satellite-satellite-cli-reference#host-attach)
    * [`ibmcloud sat host get`](/docs/satellite?topic=satellite-satellite-cli-reference#host-get)
    * [`ibmcloud sat host ls`](/docs/satellite?topic=satellite-satellite-cli-reference#host-ls)
    * [`ibmcloud sat host rm`](/docs/satellite?topic=satellite-satellite-cli-reference#host-rm)
    * [`ibmcloud sat host update`](/docs/satellite?topic=satellite-satellite-cli-reference#host-update)

* [Location commands](/docs/satellite?topic=satellite-satellite-cli-reference#sat-location-commands)
    * [`ibmcloud sat location create`](/docs/satellite?topic=satellite-satellite-cli-reference#location-create)
    * [`ibmcloud sat location dns ls`](/docs/satellite?topic=satellite-satellite-cli-reference#location-dns-ls)
    * [`ibmcloud sat location dns register`](/docs/satellite?topic=satellite-satellite-cli-reference#location-dns-register)
    * [`ibmcloud sat location get`](/docs/satellite?topic=satellite-satellite-cli-reference#location-get)
    * [`ibmcloud sat location ls`](/docs/satellite?topic=satellite-satellite-cli-reference#location-ls)
    * [`ibmcloud sat location rm`](/docs/satellite?topic=satellite-satellite-cli-reference#location-rm)

* [Resource commands](/docs/satellite?topic=satellite-satellite-cli-reference#sat-resource-commands)
    * [`ibmcloud sat resource get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-get)
    * [`ibmcloud sat resource ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-ls)

* [Service commands](/docs/satellite?topic=satellite-satellite-cli-reference#sat-service-commands)
    * [`ibmcloud sat service ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-service-ls)

* [Storage commands](/docs/satellite?topic=satellite-satellite-cli-reference#sat-storage-commands)
    * [`ibmcloud sat storage assignment create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create)
    * [`ibmcloud sat storage assignment get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-get)
    * [`ibmcloud sat storage assignment ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-ls)
    * [`ibmcloud sat storage assignment rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-rm)
    * [`ibmcloud sat storage assignment update`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-update)
    * [`ibmcloud sat storage config create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create)
    * [`ibmcloud sat storage config get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-get)
    * [`ibmcloud sat storage config ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-ls)
    * [`ibmcloud sat storage config rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-rm)
    * [`ibmcloud sat storage config sc add`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-sc-add)
    * [`ibmcloud sat storage config sc get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-sc-get)
    * [`ibmcloud sat storage config sc ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-sc-ls)
    * [`ibmcloud sat storage template get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-template-get)
    * [`ibmcloud sat storage template ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-template-ls)

* [Subscription commands](/docs/satellite?topic=satellite-satellite-cli-reference#sat-config-subscription-commands)
    * [`ibmcloud sat subscription create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-create)
    * [`ibmcloud sat subscription get`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-get)
    * [`ibmcloud sat subscription ls`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-ls)
    * [`ibmcloud sat subscription rm`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-rm)
    * [`ibmcloud sat subscription update`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-config-subscription-update)

* [Other commands](/docs/satellite?topic=satellite-satellite-cli-reference#other-commands)
    * [`ibmcloud sat messages`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-messages)
    * [{{site.data.keyword.openshiftlong_notm}} commands (`ibmcloud oc`)](/docs/satellite?topic=satellite-satellite-cli-reference#cluster-create)


## CLI changelog for {{site.data.keyword.satelliteshort}} commands
{: #sitemap_cli_changelog_for__commands}


[CLI changelog for {{site.data.keyword.satelliteshort}} commands](/docs/satellite?topic=satellite-satellite-cli-changelog)

* [Version 1.0](/docs/satellite?topic=satellite-satellite-cli-changelog#10)


## Usage requirements
{: #sitemap_usage_requirements}


[Usage requirements](/docs/satellite?topic=satellite-requirements)

* [Locations](/docs/satellite?topic=satellite-requirements#reqs-locations)

* [Hosts](/docs/satellite?topic=satellite-requirements#reqs-host)

* [Clusters](/docs/satellite?topic=satellite-requirements#reqs-clusters)

* [Link and endpoints](/docs/satellite?topic=satellite-requirements#reqs-link)

* [Config](/docs/satellite?topic=satellite-requirements#reqs-config)

* [{{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-requirements#reqs-services)


## Supported {{site.data.keyword.cloud_notm}} locations
{: #sitemap_supported__locations}


[Supported {{site.data.keyword.cloud_notm}} locations](/docs/satellite?topic=satellite-sat-regions)

* [About {{site.data.keyword.cloud_notm}} regions for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-sat-regions#understand-supported-regions)


## Your responsibilities
{: #sitemap_your_responsibilities}


[Your responsibilities](/docs/satellite?topic=satellite-responsibilities)

* [Overview of shared responsibilities](/docs/satellite?topic=satellite-responsibilities#overview-by-resource)

* [Tasks for shared responsibilities by area](/docs/satellite?topic=satellite-responsibilities#task-responsibilities)
    * [Incident and operations management](/docs/satellite?topic=satellite-responsibilities#incident-and-ops)
    * [Change management](/docs/satellite?topic=satellite-responsibilities#change-management)
    * [Identity and access management](/docs/satellite?topic=satellite-responsibilities#iam-responsibilities)
    * [Security and regulation compliance](/docs/satellite?topic=satellite-responsibilities#security-compliance)
    * [Disaster recovery](/docs/satellite?topic=satellite-responsibilities#disaster-recovery)


## {{site.data.keyword.satellitelong_notm}} notices
{: #sitemap__notices}


[{{site.data.keyword.satellitelong_notm}} notices](/docs/satellite?topic=satellite-sat-notices)

* [Creative Commons Attribution Share Alike 4.0 Generic](/docs/satellite?topic=satellite-sat-notices#cc-40-generic)


## FAQs
{: #sitemap_faqs}


[FAQs](/docs/satellite?topic=satellite-faqs)

* [What is {{site.data.keyword.satellitelong_notm}} and how does it work?](/docs/satellite?topic=satellite-faqs#what-is-satellite)

* [What are the reasons to use {{site.data.keyword.satellitelong_notm}}?](/docs/satellite?topic=satellite-faqs#satellite-benefits)

* [Where is the service available?](/docs/satellite?topic=satellite-faqs#satellite-locations)

* [Is my location setup highly available?](/docs/satellite?topic=satellite-faqs#satellite-ha)

* [What happens if my {{site.data.keyword.satelliteshort}} control plane becomes unavailable?](/docs/satellite?topic=satellite-faqs#control-plane-unavailable)

* [Does IBM support third-party and open source tools that I use with {{site.data.keyword.satelliteshort}}?](/docs/satellite?topic=satellite-faqs#faq_thirdparty_oss)

* [What am I charged for when I use {{site.data.keyword.satellitelong_notm}}?](/docs/satellite?topic=satellite-faqs#pricing)
    * [{{site.data.keyword.satelliteshort}}-enabled services](/docs/satellite?topic=satellite-faqs#pricing-services)
    * [{{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-faqs#pricing-satloc)

* [How do I bring my own OCP license?](/docs/satellite?topic=satellite-faqs#byo-ocp)

* [Can I estimate my costs?](/docs/satellite?topic=satellite-faqs#cost-estimate)

* [Can I view and control my current usage?](/docs/satellite?topic=satellite-faqs#usage)

* [What are the terms of the service level agreement?](/docs/satellite?topic=satellite-faqs#sla)

* [What compliance standards does the service meet?](/docs/satellite?topic=satellite-faqs#standards)

* [What {{site.data.keyword.cloud_notm}} services can I use with {{site.data.keyword.satelliteshort}}?](/docs/satellite?topic=satellite-faqs#supported-services)

* [What managed add-ons can I use with {{site.data.keyword.openshiftshort}} clusters in my {{site.data.keyword.satelliteshort}} location?](/docs/satellite?topic=satellite-faqs#faq-managed-addons)


## Troubleshooting errors
{: #sitemap_troubleshooting_errors}


[Getting help](/docs/satellite?topic=satellite-get-help)
* [General ways to resolve issues](/docs/satellite?topic=satellite-get-help#help-general)
* [Reviewing cloud issues and status](/docs/satellite?topic=satellite-get-help#help-cloud-status)
* [Identifying issues for {{site.data.keyword.satelliteshort}}-enabled services](/docs/satellite?topic=satellite-get-help#help-services)
* [Using {{site.data.keyword.la_short}} to review {{site.data.keyword.satelliteshort}} location logs](/docs/satellite?topic=satellite-get-help#review-logs)
  * [Enabling platform logs](/docs/satellite?topic=satellite-get-help#enable-la)
  * [Viewing logs for your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-get-help#view-la)
  * [Analyzing logs for your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-get-help#analyze-la)
  * [`R00XX` error logs](/docs/satellite?topic=satellite-get-help#logs-error)
  * [Enablement of resource deployment logs](/docs/satellite?topic=satellite-get-help#logs-deploy)
  * [Endpoint health status logs](/docs/satellite?topic=satellite-get-help#logs-link)
* [Feedback and questions](/docs/satellite?topic=satellite-get-help#feedback-qs)
* [Contacting support](/docs/satellite?topic=satellite-get-help#help-support)

[Location error messages](/docs/satellite?topic=satellite-ts-locations-debug)
* [Reviewing error messages and logs](/docs/satellite?topic=satellite-ts-locations-debug#review-messages-logs)
* [R0001: Ready location](/docs/satellite?topic=satellite-ts-locations-debug#R0001)
* [R0002, R0018, R0020, R0023, R0029, R0037, R0039, R0042: Wait for location to be ready](/docs/satellite?topic=satellite-ts-locations-debug#R0002)
* [R0009: Unable to recover](/docs/satellite?topic=satellite-ts-locations-debug#R0009)
* [R0010, R0030, R0031, R0032: Control plane needs hosts](/docs/satellite?topic=satellite-ts-locations-debug#R0010)
* [R0011, R0040, R0041: Issues with the control plane hosts](/docs/satellite?topic=satellite-ts-locations-debug#R0011)
* [R0012: Hosts are needed in all 3 zones](/docs/satellite?topic=satellite-ts-locations-debug#R0012)
* [R0013: Unavailable zone](/docs/satellite?topic=satellite-ts-locations-debug#R0013)
* [R0014: DNS record for control plane](/docs/satellite?topic=satellite-ts-locations-debug#R0014)
* [R0015, R0016: Host issues](/docs/satellite?topic=satellite-ts-locations-debug#R0015)
* [R0024, R0025, R0038: Cluster issues](/docs/satellite?topic=satellite-ts-locations-debug#R0024)
* [R0026: Host disk space](/docs/satellite?topic=satellite-ts-locations-debug#R0026)
* [R0033, R0034, R0035: Control plane capacity issues](/docs/satellite?topic=satellite-ts-locations-debug#R0033)
* [R0036: Location subdomain traffic routing](/docs/satellite?topic=satellite-ts-locations-debug#R0036)
* [R0043: Layer 3 connectivity](/docs/satellite?topic=satellite-ts-locations-debug#R0043)
* [R0044: DNS issues](/docs/satellite?topic=satellite-ts-locations-debug#R0044)
* [R0045: Host read-only file system issues](/docs/satellite?topic=satellite-ts-locations-debug#R0045)
* [R0046: NTP issues](/docs/satellite?topic=satellite-ts-locations-debug#R0046)
* [R0047: Location health checking](/docs/satellite?topic=satellite-ts-locations-debug#R0047)
* [R0048: Etcd backup failure](/docs/satellite?topic=satellite-ts-locations-debug#R0048)
* [R0049, R0050, R0051: {{site.data.keyword.satelliteshort}} Link connector issues](/docs/satellite?topic=satellite-ts-locations-debug#R0049)


## Locations
{: #sitemap_locations}


[Debugging the health of the location control plane](/docs/satellite?topic=satellite-ts-locations-control-plane)

[Why does the location subdomain not route traffic to control plane hosts?](/docs/satellite?topic=satellite-ts-location-subdomain)

[Why is {{site.data.keyword.cloud_notm}} unable to check my location's health?](/docs/satellite?topic=satellite-ts-location-healthcheck)

[Why can't I see a location that another user gave me access to?](/docs/satellite?topic=satellite-ts-location-missing-location)
* [Target the regional endpoint](/docs/satellite?topic=satellite-ts-location-missing-location#ts-location-missing-location-target)
* [Ask the location owner to update your permissions](/docs/satellite?topic=satellite-ts-location-missing-location#ts-location-missing-location-perms)


## Hosts
{: #sitemap_hosts}


[Debugging host health](/docs/satellite?topic=satellite-ts-hosts-debug)

[Logging in to a host machine to debug](/docs/satellite?topic=satellite-ts-hosts-login)

[Why can't I SSH into my host machines?](/docs/satellite?topic=satellite-ssh-login-denied)

[Why does the host registration script fail?](/docs/satellite?topic=satellite-host-registration-script-fails)

[Why can't I assign hosts to a cluster?](/docs/satellite?topic=satellite-assign-fails)
* [Debugging hosts for connectivity issues](/docs/satellite?topic=satellite-assign-fails#debug-host-connectivity)
* [Endpoints to verify connectivity by {{site.data.keyword.cloud_notm}} multizone metro](/docs/satellite?topic=satellite-assign-fails#endpoints-to-verify)


## Clusters
{: #sitemap_clusters}


[Debugging clusters](/docs/satellite?topic=satellite-ts-clusters-debug)

[Why does the list of Kubernetes resources not show up or update after registering my cluster with {{site.data.keyword.satelliteshort}} Config?](/docs/satellite?topic=satellite-satconfig-cluster-access-error)

[Why can't I access the {{site.data.keyword.openshiftshort}} console?](/docs/satellite?topic=satellite-ts-console-fail)

[Why can't I update or complete other actions with my cluster?](/docs/satellite?topic=satellite-ts-cluster-operations-blocked)

[Why doesn't my cluster add-on work?](/docs/satellite?topic=satellite-addon-errors)


## Why is the namespace where my storage operator was deployed stuck in **Terminating** status?
{: #sitemap_why_is_the_namespace_where_my_storage_operator_was_deployed_stuck_in_**terminating**_status?}


[Why is the namespace where my storage operator was deployed stuck in **Terminating** status?](/docs/satellite?topic=satellite-storage-namespace-terminating)


## Release notes
{: #sitemap_release_notes}


[Release notes](/docs/satellite?topic=satellite-release-notes)

* [June 2021](/docs/satellite?topic=satellite-release-notes#june21)

* [May 2021](/docs/satellite?topic=satellite-release-notes#may21)

* [April 2021](/docs/satellite?topic=satellite-release-notes#apr21)

* [March 2021](/docs/satellite?topic=satellite-release-notes#mar21)

* [February 2021](/docs/satellite?topic=satellite-release-notes#february21)

* [January 2021](/docs/satellite?topic=satellite-release-notes#january21)

* [December 2020](/docs/satellite?topic=satellite-release-notes#december20)

* [November 2020](/docs/satellite?topic=satellite-release-notes#november20)

* [October 2020](/docs/satellite?topic=satellite-release-notes#october20)

* [September 2020](/docs/satellite?topic=satellite-release-notes#september20)

* [August 2020](/docs/satellite?topic=satellite-release-notes#august20)