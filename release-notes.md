---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-10"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Release notes
{: #release-notes}

Use the release notes to learn about the latest changes to the {{site.data.keyword.satelliteshort}} documentation that are grouped by month.
{: shortdesc}

## February 2022
{: #release-feb-2022}

### 10 Februrary 2022
{: #10feb2022}
{: release-note}

New! Beta support for the Azure File CSI driver template
:    You can use the Azure File CSI driver template to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [Azure File CSI driver template documentation](/docs/satellite?topic=satellite-config-storage-azurefile-csi).

## January 2022
{: #release-jan-2022}

Review the release notes for January 2022.
{: shortdesc}

### 27 January 2022
{: #27jan2022}
{: release-note}

{{site.data.keyword.cos_full_notm}} 
:   {{site.data.keyword.cos_full_notm}} is added as a supported managed service for {{site.data.keyword.satelliteshort}}. For more information, see [About {{site.data.keyword.cos_short}} for {{site.data.keyword.satelliteshort}}](/docs/cloud-object-storage?topic=cloud-object-storage-about-cos-satellite).

### 25 January 2022
{: #25jan2022}
{: release-note}

OpenShift Data Foundation
:   New and updated template parameters for the OpenShift Data Foundation {{site.data.keyword.satelliteshort}} templates.
    - Automatic disk discovery is now available for the `odf-local` template version 4.8. Enable this feature by setting the `auto-disk-discovery=true` parameter. For more information, see [ODF using local disks](/docs/satellite?topic=satellite-config-storage-odf-local).
    - The `monDevicePaths` and `monSize` parameters are no longer required for the `odf-local` template version 4.8.
    - The `monStorageClassName` and `monSize` parameters are no longer required for the `odf-remote` template version 4.8.


### 18 January 2022
{: #18jan2022}
{: release-note}

**New!** {{site.data.keyword.satellitelong_notm}} CLI Map
:    The [{{site.data.keyword.satellitelong_notm}} CLI Map](/docs/satellite?topic=satellite-icsat_map) lists all `ibmcloud sat` commands as they are structured in the CLI. Use this page as a visual reference for how ibmcloud sat commands are organized, or to quickly find a specific command. 


**New!** Site map
:   A site map is available for the {{site.data.keyword.satelliteshort}} [documentation](/docs/satellite?topic=satellite-sitemap).

## December 2021
{: #release-dec-2021}

Review the release notes for December 2021.
{: shortdesc}


### 15 December 2021
{: #15dec2021}
{: release-note}

**New! Sydney location**
:   You can now manage {{site.data.keyword.satelliteshort}} locations from the **Sydney** (`syd`, `au-syd`) [{{site.data.keyword.cloud_notm}} region](/docs/satellite?topic=satellite-sat-regions). Note that {{site.data.keyword.satelliteshort}} Link flow logs for Location endpoints created in the Sydney (`au-syd`) region are inconsistent or might not work.


### 3 December 2021
{: #3dec2021}
{: release-note}

**New! Satellite Mesh**!
: Satellite Mesh is available for select customers in beta. For more information, see [Setting up a service mesh with Satellite Mesh](/docs/satellite?topic=satellite-sat-mesh).

CLI changelog
:    Updated the CLI plug-in changelog page for the [release of version 1.0.353](/docs/satellite?topic=satellite-satellite-cli-changelog).



## November 2021
{: #release-nov-2021}

Review the release notes for November 2021.
{: shortdesc}

### 29 November 2021
{: #29nov2021}
{: release-note}


{{site.data.keyword.openshiftshort}} Data Foundation
:   Updated deployment parameters for {{site.data.keyword.openshiftshort}} Data Foundation. The `container-private-endpoint` parameter is no longer required.

{{site.data.keyword.at_short}}
:   New {{site.data.keyword.at_short}} events are available for {{site.data.keyword.satelliteshort}} storage. For more information, see [Auditing events](/docs/satellite?topic=satellite-at_events).

### 16 November 2021
{: #16nov2021}
{: release-note}

**New! Sao Paulo location**
:   You can now manage {{site.data.keyword.satelliteshort}} locations from the **Sao Paolo** (`sao`, `br-sao`) [{{site.data.keyword.cloud_notm}} region](/docs/satellite?topic=satellite-sat-regions). 

### 11 November 2021
{: #11nov2021}
{: release-note}

New location error message is added. 
:    For more information see [R0052: Ingress certificate generation issues](/docs/satellite?topic=satellite-ts-locations-debug#R0052).

### 10 November 2021
{: #10nov2021}
{: release-note}

Troubleshooting a `certificate expired` error for the {{site.data.keyword.openshiftshort}} console
:   For more information see [Why can't I access the {{site.data.keyword.openshiftshort}} web console?](/docs/satellite?topic=satellite-ts-sat-ocp-console).

### 9 November 2021
{: #9nov2021}
{: release-note}

Azure Disk CSI driver template
:   Previously, when you created a storage configuration by using the Azure Disk storage template, you passed your Azure configuration parameters as one base64 encoded string. Now, each of the Azure configuration parameters is exposed as a command-line option. For more information, see [Azure Disk CSI driver](/docs/satellite?topic=satellite-config-storage-azure-csi).

### 8 November 2021
{: #8nov2021}
{: release-note}

Troubleshooting a degraded Ingress operator
:   For more information, see [Why is my Ingress operator in a warning state?](/docs/satellite?topic=satellite-ts-degraded-ingress).


## October 2021
{: #release-oct-2021}

Review the release notes for October 2021.
{: shortdesc}

### 12 October 2021
{: #12oct2021}
{: release-note}

Review the release notes for 12 October 2021.
{: shortdesc}

CLI changelog
:    Updated the CLI plug-in changelog page for the [release of version 1.0.327](/docs/satellite?topic=satellite-satellite-cli-changelog).


## September 2021
{: #release-sep-2021}

Review the release notes for September 2021.
{: shortdesc}

### 29 September 2021
{: #29sep2021}
{: release-note}

Review the release notes for 29 September 2021.
{: shortdesc}

**New! Alibaba host support**
:   Alibaba Cloud hosts are now supported for your {{site.data.keyword.satelliteshort}} location.

**New! Toronto location**
:   You can now manage {{site.data.keyword.satelliteshort}} locations from the **Toronto** (`tor`, `ca-tor`) [{{site.data.keyword.cloud_notm}} region](/docs/satellite?topic=satellite-sat-regions). 

### 23 September 2021
{: #23sep2021}
{: release-note}

Review the release notes for 23 September 2021.
{: shortdesc}

**New! Tokyo location**
:   You can now manage {{site.data.keyword.satelliteshort}} locations from the **Tokyo** (`tok`, `jp-tok`) [{{site.data.keyword.cloud_notm}} region](/docs/satellite?topic=satellite-sat-regions). 


### 15 September 2021
{: #15sep2021}
{: release-note}

Review the release notes for 15 September 2021.
{: shortdesc}

OpenShift Data Foundation
:   OpenShift Data Foundation is generally available for {{site.data.keyword.satelliteshort}} cluster. For more information see, the following links.
    - [Understanding OpenShift Data Foundation](/docs/openshift?topic=openshift-ocs-storage-prep).
    - [Deploying OpenShift Data Foundation using local disks](/docs/satellite?topic=satellite-config-storage-odf-local).
    - [Deploying OpenShift Data Foundation using remote, dynamically provisioned disks](/docs/satellite?topic=satellite-config-storage-odf-remote).


## August 2021
{: #aug21}

Review the release notes for August 2021.
{: shortdesc}

### 9 August 2021
{: #9aug2021}
{: release-note}

Review the release notes for 9 August 2021.
{: shortdesc}

CLI changelog
:    Updated the CLI plug-in changelog page for the [release of version 1.0.312](/docs/satellite?topic=satellite-satellite-cli-changelog).

### 3 August 2021
{: #3aug2021}
{: release-note}

Review the release notes for 3 August 2021.
{: shortdesc}

New! Dallas location
:    You can now manage {{site.data.keyword.satelliteshort}} locations from the **Dallas** (`dal`, `us-south`) [{{site.data.keyword.cloud_notm}} region](/docs/satellite?topic=satellite-sat-regions).

## July 2021
{: #july21}

Review the release notes for July 2021.
{: shortdesc}

### 19 July 2021
{: #19jul2021}
{: release-note}

Review the release notes for 19 July 2021.
{: shortdesc}

End of UDP endpoint support
:    The UDP protocol is no longer supported for `cloud` and `location` endpoints that you create in {{site.data.keyword.satelliteshort}} Link. For more information, see [Encryption protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols).

### 16 July 2021
{: #16jul2021}
{: release-note}

Review the release notes for 16 July 2021.
{: shortdesc}

New! Beta support for the Azure Disk CSI driver template
:    You can use the Azure Disk CSI driver template to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [Azure Disk CSI driver template documentation](/docs/satellite?topic=satellite-config-storage-azure-csi).

### 8 July 2021
{: #8jul2021}
{: release-note}

Review the release notes for 8 July 2021.
{: shortdesc}

DNS provider change from Cloudflare to Akamai
:    On 08 July, the DNS provider is changed from Cloudflare to Akamai for all `containers.cloud.ibm.com` domains. Previously, to use {{site.data.keyword.satelliteshort}} Config and to access the {{site.data.keyword.satelliteshort}} Link API, you were required to allow outbound connectivity from your control plane hosts to the IP addresses for Cloudflare's proxied load balancers. Now, you must instead allow access to [Akamai's source IP addresses](https://github.com/{{site.data.keyword.IBM_notm}}-Cloud/kube-samples/tree/master/akamai/gtm-liveness-test){: external} on TCP ports 80 and 443. After the migration completes, you can remove the Cloudflare IP address rules.

## June 2021
{: #june21}

Review the release notes for June 2021.
{: shortdesc}

### 30 June 2021
{: #30jun2021}
{: release-note}

Review the release notes for 30 June 2021.
{: shortdesc}

New! General availability of the {{site.data.keyword.satelliteshort}} storage templates. 
:    For more information about storage templates, see [Understanding storage templates](/docs/satellite?topic=satellite-sat-storage-template-ov).

New! [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services)s</strong>: Many {{site.data.keyword.IBM_notm}} {{site.data.keyword.databases-for}} are now available to deploy to your {{site.data.keyword.satelliteshort}} location. 
:    For more information, see the [{{site.data.keyword.cloud_notm}} blog](https://www.ibm.com/cloud/blog/announcements/deploy-managed-cloud-native-databases-anywhere-with-ibm-cloud-satellite){: external}.

### 24 June 2021
{: #24jun2021}
{: release-note}

Review the release notes for 24 June 2021.
{: shortdesc}

Azure template
:    Automate the setup of a {{site.data.keyword.satelliteshort}} location in Microsoft Azure cloud infrastructure by using a [{{site.data.keyword.bpshort}}](/docs/satellite?topic=satellite-azure#azure-template).

CLI changelog
:    Updated the CLI plug-in changelog page for the [release of version 1.0.295](/docs/satellite?topic=satellite-satellite-cli-changelog).

### 18 June 2021
{: #18jun2021}
{: release-note}

Review the release notes for 18 June 2021.
{: shortdesc}

IP addresses for Link tunnel server endpoints
:    Updates the [required outbound connectivity for hosts](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-outbound) to include IP addresses for Link tunnel server endpoints.

### 9 June 2021
{: #9jun2021}
{: release-note}

Review the release notes for 9 June 2021.
{: shortdesc}

New! Frankfurt location
:    You can now manage {{site.data.keyword.satelliteshort}} locations from the Frankfurt (`fra`, `eu-de`) [{{site.data.keyword.cloud_notm}} region](/docs/satellite?topic=satellite-sat-regions).

Digital certificates
:    Added information about [certificates for various {{site.data.keyword.satellitelong_notm}} domains and hosts](/docs/satellite?topic=satellite-compliance#certs-hosts-domains).

## May 2021
{: #may21}

Review the release notes for May 2021.
{: shortdesc}

### 26 May 2021
{: #26may2021}
{: release-note}

Review the release notes for 26 May 2021.
{: shortdesc}

CLI changelog
:    Updated the CLI plug-in changelog page for the [release of version 1.0.258](/docs/satellite?topic=satellite-satellite-cli-changelog).

### 19 May 2021
{: #19may2021}
{: release-note}

Review the release notes for 19 May 2021.
{: shortdesc}

New! NetApp ONTAP-NAS storage driver template
:    You can use the NetApp ONTAP-NAS storage driver to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [NetApp ONTAP-NAS storage driver documentation](/docs/satellite?topic=satellite-config-storage-netapp-nas).

### 12 May 2021
{: #12may2021}
{: release-note}

Review the release notes for 12 May 2021.
{: shortdesc}

New! {{site.data.keyword.IBM_notm}} Systems block storage CSI driver template
:    You can use the {{site.data.keyword.IBM_notm}} Systems block storage CSI driver to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [{{site.data.keyword.IBM_notm}} Systems block storage CSI driver documentation](/docs/satellite?topic=satellite-config-storage-block-csi).

### 7 May 2021
{: #7may2021}
{: release-note}

Review the release notes for 7 May 2021.
{: shortdesc}

New! Spectrum Scale CSI driver template
:    You can use the {{site.data.keyword.IBM_notm}} Spectrum Scale Container Storage Interface (CSI) driver to create persistent storage for stateful apps that run in {{site.data.keyword.satelliteshort}} clusters. For more information, see the [{{site.data.keyword.IBM_notm}} Spectrum Scale driver documentation](/docs/satellite?topic=satellite-config-storage-spectrum-scale).

## April 2021
{: #apr21}

Review the release notes for April 2021.
{: shortdesc}

### 26 April 2021
{: #26apr2021}
{: release-note}

Review the release notes for 26 April 2021.
{: shortdesc}

CLI changelog
Updated the CLI plug-in changelog page for the [release of version 1.0.258](/docs/satellite?topic=satellite-satellite-cli-changelog).

### 15 April 2021
{: #15apr2021}
{: release-note}

Review the release notes for 15 April 2021.
{: shortdesc}

Cluster add-ons
:    Added the [list](/docs/satellite?topic=satellite-addon-errors) of supported add-ons for clusters in {{site.data.keyword.satelliteshort}} locations.

Use case
:    Added an example use case for using [{{site.data.keyword.satelliteshort}} edge environments for AI, IoT, and machine learning](/docs/satellite?topic=satellite-edge-usecase).

:    {{site.data.keyword.cos_short}} buckets
When you create a location from the console, you can now [enter the name of an existing {{site.data.keyword.cos_full_notm}} bucket](/docs/satellite?topic=satellite-locations#location-create-console) that you want to use to back up {{site.data.keyword.satelliteshort}} location control plane data.

### 9 April 2021
{: #9apr2021}
{: release-note}

Review the release notes for 9 April 2021.
{: shortdesc}

Logging
:    Expanded information for collecting and analyzing logs for your {{site.data.keyword.satelliteshort}} location, including the types of available logs, how to set up log alerts, and how to use logs to troubleshoot location and host errors.

New! Storage template for remote OpenShift Data Foundation
:    The {{site.data.keyword.satelliteshort}} storage template for [using OpenShift Container Storage with remote devices](/docs/satellite?topic=satellite-config-storage-odf-remote) is now available.

## March 2021
{: #mar21}

Review the release notes for March 2021.
{: shortdesc}

### 25 March 2021
{: #25mar2021}
{: release-note}

Review the release notes for 25 March 2021.
{: shortdesc}

Security and compliance
:    Added an [overview of security and compliance](/docs/satellite?topic=satellite-compliance) information for {{site.data.keyword.satelliteshort}}.

### 23 March 2021
{: #23mar2021}
{: release-note}

Review the release notes for 23 March 2021.
{: shortdesc}

OpenShift Data Foundation using local disks
:    Added steps for removing an ODF configuration and updated the [configuration parameter reference](/docs/satellite?topic=satellite-config-storage-odf-local#sat-storage-odf-local-params-cli).

### 12 March 2021
{: #12mar2021}
{: release-note}

Review the release notes for 12 March 2021.
{: shortdesc}

Resetting the host key
:    In the event of a potential security violation, you can [reset the key](/docs/satellite?topic=satellite-host-update-location#host-key-reset) that the control plane uses to communicate with all the hosts in the {{site.data.keyword.satelliteshort}} location.

Troubleshooting location health checks
:    Added steps for troubleshooting when [{{site.data.keyword.cloud_notm}} is unable to check a location's health](/docs/satellite?topic=satellite-ts-location-healthcheck).

### 10 March 2021
{: #10mar2021}
{: release-note}

Review the release notes for 10 March 2021.
{: shortdesc}

New `health-pending` host state
:    To review the state of your hosts, see [Viewing host health](/docs/satellite?topic=satellite-monitor#host-health).

### 1 March 2021
{: #01mar2021}
{: release-note}

Review the release notes for 1 March 2021.
{: shortdesc}

New! General availability of {{site.data.keyword.satellitelong}}
:    {{site.data.keyword.satellitelong_notm}} is now generally available. If you created locations during the closed beta, you must [re-create your locations](/docs/satellite?topic=satellite-location-copy) for the general availability release. For more information about what you are charged for when you use {{site.data.keyword.satellitelong_notm}}, see the [FAQs](/docs/satellite?topic=satellite-faqs#pricing).

CLI changelog
:    Updated the CLI plug-in changelog page for the [release of version 1.0.233](/docs/satellite?topic=satellite-satellite-cli-changelog).

Cloud provider guides
:    Added instructions to set up {{site.data.keyword.satelliteshort}} locations that use infrastructure from [AWS](/docs/satellite?topic=satellite-aws), [GCP](/docs/satellite?topic=satellite-gcp), [Azure](/docs/satellite?topic=satellite-azure), and [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm).

Exposing apps in {{site.data.keyword.satelliteshort}} clusters
:    Added an overview of the [options for exposing apps in Satellite clusters](/docs/openshift?topic=openshift-sat-expose-apps) and steps for setting up each option.

{{site.data.keyword.satelliteshort}} Infrastructure Service
:    [Order managed infrastructure from {{site.data.keyword.IBM_notm}}](/docs/satellite?topic=satellite-infrastructure-service) to create a {{site.data.keyword.satelliteshort}} location for you in your on-premises data center.
  
New! Template for fast provisioning on AWS
:    [Automate your {{site.data.keyword.satelliteshort}} location setup with a {{site.data.keyword.bpshort}} template](/docs/satellite?topic=satellite-locations#satloc-template), available for AWS infrastructure.

## February 2021
{: #february21}

Review the release notes for February 2021.
{: shortdesc}

### 25 February 2021
{: #25feb2021}
{: release-note}

Review the release notes for 25 February 2021.
{: shortdesc}

Auditing events
:    Added a topic for [auditing {{site.data.keyword.satelliteshort}} events with {{site.data.keyword.cloudaccesstrailshort}}](/docs/satellite?topic=satellite-at_events).

CLI changelog
:    Updated the CLI plug-in changelog page for the [release of version 1.0.223](/docs/satellite?topic=satellite-satellite-cli-changelog).

### 23 February 2021
{: #23feb2021}
{: release-note}

Review the release notes for 23 February 2021.
{: shortdesc}

Accessing clusters
:    Added steps for testing [access to a {{site.data.keyword.satelliteshort}} cluster by using the public network](/docs/openshift?topic=openshift-access_cluster#sat_public_access).

Host requirements
:    Added the minimum required disk storage, storage device IOPS, and network bandwidth to the [hosts requirements](/docs/satellite?topic=satellite-host-reqs).
  
Link endpoints
:    Added FAQs about [{{site.data.keyword.satelliteshort}} Link network security](/docs/satellite?topic=satellite-link-location-cloud#link-security).

Monitoring
:    Added information about how to [set up {{site.data.keyword.mon_short}} for {{site.data.keyword.satelliteshort}} location platform metrics](/docs/satellite?topic=satellite-monitor).

Reviewing resources
:    Added a topic for [reviewing resources that are managed by {{site.data.keyword.satelliteshort}} config](/docs/satellite?topic=satellite-satcon-manage#satconfig-resources).

### 18 February 2021
{: #18feb2021}
{: release-note}

Review the release notes for 18 February 2021.
{: shortdesc}

Securing service connections
:    Added a [{{site.data.keyword.satelliteshort}} connection overview](/docs/satellite?topic=satellite-service-connection) to describe user access to resources that run in your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satelliteshort}} location.

### 12 February 2021
{: #12feb2021}
{: release-note}

Review the release notes for 12 February 2021.
{: shortdesc}

Link endpoints
:    Added information about [default Link endpoints](/docs/satellite?topic=satellite-default-link-endpoints) that are automatically created for {{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satelliteshort}} location.

Securing service connections
:    Added a topic to explain all points of access to your {{site.data.keyword.satelliteshort}} location. For more information, see [Securing your connection to Satellite](/docs/satellite?topic=satellite-service-connection).

### 8 February 2021
{: #08feb2021}
{: release-note}

Review the release notes for 8 February 2021.
{: shortdesc}

CLI changelog
:    Updated the CLI plug-in changelog page for the [release of version 1.0.223](/docs/satellite?topic=satellite-satellite-cli-changelog).

Host autoassignment
:    Added information about how [{{site.data.keyword.satelliteshort}} can automatically assign hosts](/docs/satellite?topic=satellite-assigning-hosts#host-autoassign-ov) to worker pools in clusters or [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services)s that use host labels to request compute capacity.

Logging and monitoring
:    Added information about how to set up [logging and monitoring for {{site.data.keyword.satelliteshort}} health](/docs/satellite?topic=satellite-health).

Worker pool management
:    Described how to manage [worker pools in {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters#satcluster-worker-pools-sat), such as host labels for host autoassignment.

## January 2021
{: #january21}

Review the release notes for January 2021.
{: shortdesc}

### 29 January 2021
{: #29jan2021}
{: release-note}

Review the release notes for 29 January 2021.
{: shortdesc}

AWS hosts
:    Added the list of supported AWS EC2 instance types that can be used as hosts.

{{site.data.keyword.satelliteshort}} Link
:    Added information about the [maximum number of endpoints](/docs/satellite?topic=satellite-requirements#reqs-link) that you can create for one location.

### 25 January 2021
{: #25jan2021}
{: release-note}

Review the release notes for 25 January 2021.
{: shortdesc}

{{site.data.keyword.satelliteshort}} Link
:    Expanded information about endpoint security and access controls, and added example use cases.

### 15 January 2021
{: #15jan2021}
{: release-note}

Review the release notes for 15 January 2021.
{: shortdesc}

Host requirements
:    Updated the [{{site.data.keyword.redhat_notm}} package repositories](/docs/satellite?topic=satellite-host-reqs) that you must enable on hosts.

### 12 January 2021
{: #12jan2021}
{: release-note}

Review the release notes for 12 January 2021.
{: shortdesc}



Host updates
:    Added how to update hosts that are used as worker nodes in [clusters](/docs/satellite?topic=satellite-host-update-workers) and the [{{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-host-update-location).

## December 2020
{: #december20}

Review the release notes for December 2020.
{: shortdesc}

### 9 December 2020
{: #09dec2020}
{: release-note}

Review the release notes for 9 December 2020.
{: shortdesc}

Private hosts
:    Hosts with only private network connectivity are now supported for your {{site.data.keyword.satelliteshort}} location. If hosts have multiple IPv4 network interfaces, the lowest-order, non-loopback network interface with a valid IPv4 address is used as the primary network interface for the hosts. Note that if your hosts have private network connectivity only, authorized users must be in the private host network directly or through a VPN connection to access services that run in your location, such as {{site.data.keyword.openshiftlong_notm}} clusters.

Azure host support
:    Microsoft Azure hosts are now supported for your {{site.data.keyword.satelliteshort}} location.

Physical machine support
:    Physical machine hosts are now supported for your {{site.data.keyword.satelliteshort}} location.

Host network requirements
:    Updated the required ports and subnets that must be allowed for [inbound](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-inbound) and [outbound connectivity](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-outbound) on hosts' primary networks.

AWS and GCP host DNS
:    When you use AWS and GCP hosts for your {{site.data.keyword.satelliteshort}} location, the requirement to manually configure DNS for the location control plane and for cluster load balancing is removed. The hosts' private IP addresses are automatically registered in DNS.

## November 2020
{: #november20}

Review the release notes for November 2020.
{: shortdesc}

### 18 November 2020
{: #18nov2020}
{: release-note}

Review the release notes for 18 November 2020.
{: shortdesc}

Internal registry
:    Added information on how to [Set up the internal container image registry](/docs/openshift?topic=openshift-satellite-clusters#satcluster-internal-registry) for {{site.data.keyword.openshiftlong_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} locations.

Service overview
:    Added an [About {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-about) topic to help you learn about {{site.data.keyword.satelliteshort}} terminology, service architecture, and components.

## October 2020
{: #october20}

Review the release notes for October 2020.
{: shortdesc}

### 8 October 2020
{: #08oct2020}
{: release-note}

Review the release notes for 8 October 2020.
{: shortdesc}

Host network requirements
:    Added the required ports and subnets that must be allowed for [inbound](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-inbound) and [outbound connectivity](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-outbound) on hosts' public networks.

### 6 October 2020
{: #06oct2020}
{: release-note}

Review the release notes for 6 October 2020.
{: shortdesc}

CLI changelog
:    Added a CLI plug-in changelog page for the [release of version 1.0.178](/docs/satellite?topic=satellite-satellite-cli-changelog).

Location subdomain troubleshooting
:    Added steps for further troubleshooting when [the location subdomain does not route traffic to control plane hosts](/docs/satellite?topic=satellite-ts-location-subdomain).

## September 2020
{: #september20}

Review the release notes for September 2020.
{: shortdesc}

### 14 September 2020
{: #14sep2020}
{: release-note}

Review the release notes for 14 September 2020.
{: shortdesc}

Host requirements
:    Clarified that the `localhost` value must resolve to a valid local IP address, typically `127.0.0.1`, on a [host](/docs/satellite?topic=satellite-reqs-host-network).

IAM
:    Updated [assigning access to {{site.data.keyword.satelliteshort}} Config in {{site.data.keyword.cloud_notm}} IAM](/docs/satellite?topic=satellite-iam#iam-resource-types) to note that you cannot scope access policies to specific configurations, subscriptions, or {{site.data.keyword.cloud_notm}} resource group.

Provider documentation
:    Updated the provider requirements, such as examples for installing RHEL packages on GCP hosts before you attach the host to your {{site.data.keyword.satelliteshort}} location.
  
Service requirements
:    Added that you can provision up to [40 {{site.data.keyword.cloud_notm}} service instances per {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-requirements#reqs-services).

## August 2020
{: #august20}

Review the release notes for August 2020.
{: shortdesc}

### 21 August 2020
{: #21aug2020}
{: release-note}

Review the release notes for 21 August 2020.
{: shortdesc}

New! {{site.data.keyword.satellitelong_notm}} is now available as a closed beta.
:    For more information about the service and how it works, see the [{{site.data.keyword.satelliteshort}} product page](https://www.ibm.com/cloud/satellite){: external}. To register for the beta, go [here](https://cloud.ibm.com/satellite/overview){: external}.


