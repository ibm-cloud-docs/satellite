---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-04"

keywords: satellite, hybrid, multicloud, satellite infrastructure service

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
{:c#: .ph data-hd-programlang='c#'}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: #curl .ph data-hd-programlang='curl'}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: .external target="_blank"}
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
{:middle: .ph data-hd-position='middle'}
{:navgroup: .navgroup}
{:new_window: target="_blank"}
{:node: .ph data-hd-programlang='node'}
{:note: .note}
{:objectc: .ph data-hd-programlang='Objective C'}
{:org_name: data-hd-keyref="org_name"}
{:php: .ph data-hd-programlang='PHP'}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:right: .ph data-hd-position='right'}
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
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:topicgroup: .topicgroup}
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


# {{site.data.keyword.satellitelong_notm}} Infrastructure Service
{: #infrastructure-service}

With {{site.data.keyword.satelliteshort}} Infrastructure Service, you can order managed infrastructure from IBM to create an on-premises location in {{site.data.keyword.satellitelong}}.
{: shortdesc}

{{site.data.keyword.satelliteshort}} Infrastructure Service comes in varying target sizes with elastic capacity that scales to your computing needs. As a managed infrastructure service, IBM deploys {{site.data.keyword.satellitelong_notm}} for you in your data center, so you can focus on maximizing your application workloads with the capabilities of {{site.data.keyword.cloud_notm}}.

## Getting started with {{site.data.keyword.satelliteshort}} Infrastructure Service
{: #satis-getting-started}

You can request a {{site.data.keyword.satelliteshort}} Infrastructure Service environment by using the {{site.data.keyword.satellitelong_notm}} console.
{: shortdesc}

1.  Log in to your [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.
2.  From the Navigation Menu, click **Satellite > Locations > Create location**.
3.  Click the **Satellite Infrastructure Service** tile.
4.  Fill out the **Location details** form with information such as the country, city, and requested computing and storage capacity.

Your request is complete! Now, review the three-staged setup process to understand happens next.

### Stage 1: Planning your setup
{: #satis-gs-plan}

After you submit the {{site.data.keyword.satelliteshort}} Infrastructure Service location details form, the IBM team begins to tailor your infrastructure to your request.
{: shortdesc}

1.  The IBM team contacts you to confirm the details of the order and contact information for your technical and facilities staff.
2.  The IBM team works with your technical staff to confirm location details and prepare for the onsite installation including:
    * Workload details to confirm the capacity of your {{site.data.keyword.satelliteshort}} Infrastructure Service location
    * Power, cooling, and physical environment details
    * Networking setup including ports and IP address ranges
    * Coordinating physical access
    * Installation setup dates
3.  You and IBM review the [shared responsibilities](#satis-responsibilities) that you have with {{site.data.keyword.satelliteshort}} Infrastructure Service.

### Stage 2: Installing your on-prem infrastructure
{: #satis-gs-install}

After you and IBM agree to the installation plan, your {{site.data.keyword.satelliteshort}} Infrastructure Service location is set up.
{: shortdesc}

1.  IBM delivers the infrastructure to your physical location.
2.  IBM specialists work onsite to install the infrastructure, connect the power and network, validate the configuration, and establish remote access.
3.  The IBM operations team creates the {{site.data.keyword.satellitelong_notm}} location with the {{site.data.keyword.satelliteshort}} Infrastructure Service hardware that was set up.

Now that your {{site.data.keyword.satelliteshort}} location is created, you can view the location from the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}.

### Stage 3: Transitioning to daily operations
{: #satis-gs-ops}

With your {{site.data.keyword.satelliteshort}} Infrastructure Service location up and running, you can use {{site.data.keyword.cloud_notm}} to deploy and manage your application workloads.
{: shortdesc}

**Application workloads**

1.  As needed, [create clusters](/docs/satellite?topic=openshift-satellite-clusters) in your {{site.data.keyword.satelliteshort}} location to run your apps. The clusters use the available hosts as worker nodes.
2.  Use tools such as [{{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-cluster-config), [{{site.data.keyword.satelliteshort}} Link](/docs/satellite?topic=satellite-link-location-cloud), and [{{site.data.keyword.satelliteshort}} storage](/docs/satellite?topic=satellite-config-storage-local-block) to consistently deploy and manage your apps across the clusters in your {{site.data.keyword.satelliteshort}} location.

**Usage and capacity planning**

1.  Monitor daily usage such as [platform metrics](/docs/satellite?topic=satellite-monitor#setup-mon) or the [health of your location, hosts, and clusters](/docs/satellite?topic=satellite-monitor#view-health).
2.  Contact the IBM team for exceptional changes to capacity needs in your {{site.data.keyword.satelliteshort}} location.

**Support requests**

See [Getting help](/docs/satellite?topic=satellite-get-help).

<br />

## Understanding {{site.data.keyword.satelliteshort}} Infrastructure Service components
{: #satis-infra-about}

{{site.data.keyword.satelliteshort}} Infrastructure Service is a fully integrated solution that includes the necessary compute, storage, and network infrastructure to run {{site.data.keyword.cloud_notm}} services with your application workloads in your data center. You provide the floor space, power, cooling, and network connections. IBM provides the rest of the infrastructure.
{: shortdesc}

{{site.data.keyword.satelliteshort}} Infrastructure Service includes redundant infrastructure components to meet the {{site.data.keyword.cloud_notm}} service level agreement. You can work with IBM to expand your location for more compute and storage as your demand increases. The minimum configuration shown in Figure 1 is described as follows.
* 24 compute blades that delivers 1000 vCPU of compute capacity for your {{site.data.keyword.satelliteshort}} Infrastructure Service location. Installed compute capacity can be increased in increments of 8 blades.
* 120TB of flash storage. Installed storage capacity can be increased in increments of 30TB.  

<img src="/images/satellite-is-ov.png" alt="Figure 1. An example minimum configuration rack of {{site.data.keyword.satelliteshort}} Infrastructure Service." width="400" style="width: 400px; border-style: none"/>

_Figure 1. An example deployment of {{site.data.keyword.satelliteshort}} Infrastructure Service._

<br />

### Compute resources
{: #satis-infra-compute}

Your {{site.data.keyword.satelliteshort}} Infrastructure Service location will maintain a selection of unallocated hosts at all times.  Additional unallocated hosts will be created as you assign hosts in your location.  You can customize the quantity and size of the unallocated hosts in your {{site.data.keyword.satelliteshort}} location during the planning process to better fit the mix of workloads you deploy.  It can also be adjusted after deployment as your needs change.

To use your hosts, create {{site.data.keyword.satelliteshort}} resources such as {{site.data.keyword.satelliteshort}}-enabled services or clusters. Then, assign the hosts to these resources, either [automatically](/docs/satellite?topic=satellite-hosts#host-autoassign-ov) or [manually](/docs/satellite?topic=satellite-hosts#host-assign).

The following table outlines a sample selection of hosts in a {{site.data.keyword.satelliteshort}} Infrastructure Service location. To scale the type and number of {{site.data.keyword.satelliteshort}} Infrastructure Service hosts in your location, work with your IBM contact.

| Host size | vCPU | RAM (GB) | Primary storage (GB) | Secondary storage (GB) | Tertiary storage (GB) | Pool size |
| --- | --- | --- | --- | --- | --- | --- |
| Small | 4   | 32  | 25  | 100 | 0   | 5   |
| Medium | 8   | 64  | 25  | 100 | 0   | 5   |
| Large | 16  | 128 | 25  | 100 | 0   | 5   |
| Tertiary-Medium | 8   | 64  | 100 | 500 | 1000 | 0   |
| Tertiary-Large | 16  | 128 | 100 | 1000 | 2000 | 5   |
| Tertiary-XLarge | 16  | 128 | 100 | 1000 | 4000 | 0   |
{: summary="The table shows available host pools. Rows are to be read from the left to right. The first column is a size for the host pool. The second column is the number of vCPU in each host (each vCPU is roughly equivalent to a logical core on the underlying server). The third column is the amount of memory in gigabytes for the host machines. The third column is the size in gigabytes of the primary (operating system) storage that is attached to the host machines. The fourth column is the size in gigabytes of the secondary (ephemeral) storage that is attached to the host machines. The fifth column is the size in gigabytes of the tertiary (persistent) storage that is attached to the host machines. The fifth column is the number of unassigned hosts of that type."}
{: caption="{{site.data.keyword.satelliteshort}} Infrastructure Service host pool sizes."}

### Required license for operating system and container platform
{: #satis-infra-license}

As described in the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs), your hosts must run the Red Hat Enterprise Linux operating system and OpenShift Container Platform. You must provide an activation key for the required software during the planning phase of your location.
{: shortdesc}


### Storage for persistent volumes
{: #satis-infra-storage}

{{site.data.keyword.satelliteshort}} Infrastructure Service provides flash storage that can be used for Kubernetes persistent volumes (PVs). Your apps can save data to these volumes by using persistent volume claims (PVCs). You can review the attached storage through the {{site.data.keyword.satelliteshort}} management interface.
{: shortdesc}

<br />

### Networking
{: #satis-infra-network}

{{site.data.keyword.satelliteshort}} Infrastructure Service networking components connect to your data center's network infrastructure. You must have your networking infrastructure and ports ready before the {{site.data.keyword.satelliteshort}} Infrastructure Service hardware is installed.
{: shortdesc}

#### Required connections to your LAN
{: #satis-infra-network-lan}

{{site.data.keyword.satelliteshort}} Infrastructure Service requires two types of connections to your local area network (LAN): data and remote management. You decide the number of ports when you plan the setup with IBM.  Rack domains can be split across multiple racks to accommodate power and cooling density requirements.
{: shortdesc}

| Connection | Type | Speed (Gbps) | Number of ports |
| --- | --- | --- | --- |
| Data | Eth | 10 Gbps | 2 - 28 per rack domain |
| Remote Management | Eth | 1 Gbps  | 2 - 4 per location |
{: summary="The table shows the required connections. Rows are to be read from the left to right. The first column is the required connection. The second column is the type of connection. The third column is the speed in gigabits per second. The fourth column is a range of the number of required ports."}
{: caption="{{site.data.keyword.satelliteshort}} Infrastructure Service required data and remote management connections."}

#### IP addresses
{: #satis-infra-network-ip}

You must provide an IP address range that is reserved for your {{site.data.keyword.satelliteshort}} Infrastructure Service location and integrated into your existing data center network.
{: shortdesc}

#### Distance between compute domains
{: #satis-infra-racks-distance}

In medium and larger installations, you must spread the hardware equipment across multiple racks as described in the following table.
{: shortdesc}

| Rack placement | Cable type | Max distance |
| --- | --- | --- |
| Close proximity | Copper | Racks must be adjacent |
| In same data room | Fiber | 30 meters |
| Different buildings | Fiber | Contact IBM |
{: summary="The table describes the distance between multiple racks. Rows are to be read from the left to right. The first column describes the placement of the racks. The second column is the type of cable that connects to the rack. The third column is the maximum distance that the racks can be apart."}
{: caption="Distance between multiple racks."}

<br />

### Power and cooling
{: #satis-infra-env}

During the planning phase, you can customize your {{site.data.keyword.satelliteshort}} Infrastructure Service installation to match the power and cooling density requirements of your data center. Typical power densities are 11kW, 17kW, or 22kW per rack. For lower densities, hardware components can be spread across more racks. Components are grouped into rack domains that occupy two or three racks, depending on the power density. Racks within a single domain must be adjacent.
{: shortdesc}

| Configuration | Power | Cooling (BTU/hr) | Dimensions (H x W x D) | Weight |
| --- | --- | --- | --- | --- |
| First rack | 17 kW  | 41,278.58  | 2,013 mm x 597 mm x 1,266 mm (79.3 in. x 23.5 in. x 49.8 in.) | 689.66 kg (1,520 lb)  |
| Second rack | 9 kW  | 22,329.09  | 2,013 mm x 597 mm x 1,266 mm (79.3 in. x 23.5 in. x 49.8 in.)  | 471 kg (1,037.87 lb) |
{: caption="Power and cooling guidance for different rack configurations." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the size of the configuration. The second column is the required power. The third column is the required cooling. The fourth column is the dimensions of the rack. The fifth column is the weight of the rack."}
{: class="simple-tab-table"}
{: #two-racks}
{: tab-title="Two racks"}
{: tab-group="rack-power-cooling"}

| Configuration | Power | Cooling (BTU/hr) | Dimensions (H x W x D) | Weight |
| --- | --- | --- | --- | --- |
| First rack | 8 kW  | 18,852.2  | 2,013 mm x 597 mm x 1,266 mm (79.3 in. x 23.5 in. x 49.8 in.) | 426 kg (938.66 lb)  |
| Second rack | 9 kW  | 25,903.27  | 2,013 mm x 597 mm x 1,266 mm (79.3 in. x 23.5 in. x 49.8 in.)  | 462.66 kg (1,019.55 lb)  |
| Third rack | 8 kW  | 18,852.2  | 2,013 mm x 597 mm x 1,266 mm (79.3 in. x 23.5 in. x 49.8 in.)  | 426 kg (938.66 lb)  |
{: caption="Power and cooling guidance for different rack configurations." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the size of the configuration. The second column is the required power. The third column is the required cooling. The fourth column is the dimensions of the rack. The fifth column is the weight of the rack."}
{: class="simple-tab-table"}
{: #three-rack}
{: tab-title="Three racks"}
{: tab-group="rack-power-cooling"}

<br />

## Responsibilities with {{site.data.keyword.satelliteshort}} Infrastructure Service
{: #satis-responsibilities}

Learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.satelliteshort}} Infrastructure Service. For a high-level view of the service types in {{site.data.keyword.cloud_notm}} and the breakdown of responsibilities between you as the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities).
{:shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use {{site.data.keyword.satelliteshort}} Infrastructure Service. For the overall terms of use, see [{{site.data.keyword.cloud_notm}} Terms and Notices](/docs/overview/terms-of-use?topic=overview-terms). For responsibilities that you have for other {{site.data.keyword.cloud_notm}} services that you use with {{site.data.keyword.satelliteshort}}, refer to the documentation of those services, such as [{{site.data.keyword.openshiftlong_notm}} responsibilities](/docs/openshift?topic=openshift-responsibilities_iks).

These responsibilities differ from the [general {{site.data.keyword.satellitelong_notm}} responsibilities](/docs/satellite?topic=satellite-responsibilities) because {{site.data.keyword.satelliteshort}} Infrastructure Service is a managed infrastructure offering.
{: note}

### Incident and operations management
{: #satis-incident-and-ops}

Incident and operations management includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.
{: shortdesc}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
| General | <ul><li>Provide 24x7 customer support for {{site.data.keyword.satelliteshort}} locations.</li><li>Provide [customer support plans](/docs/get-support?topic=get-support-support-plans) to help resolve problems that you might encounter.</li><li>Procure and assemble infrastructure components with requested capacity.</li><li>Install at your data center or a data center you control.</li></ul> | <ul><li>Provide space for the {{site.data.keyword.satelliteshort}} Infrastructure Service racks with power, cooling, and LAN connectivity to your network.</li><li>Provide physical access to the infrastructure for IBM and IBM authorized representatives.</ul> |
|{{site.data.keyword.satelliteshort}} Location | <ul><li>Provide an interface to initiate operational activities, such as to create and delete locations.</li><li>Set up a highly available location control plane that is fully managed for you in an {{site.data.keyword.cloud_notm}} multizone metro.</li><li>Monitor the health of the location, automatically resolve issues when possible, and alert IBM site reliability engineers when manual intervention is required.</li><li>Automatically forward location events to your {{site.data.keyword.at_full_notm}} instance.</li><li>Use the provided tools to create a location, add hosts to the location, assign hosts with sufficient compute resources for the control plane worker components, and debug any issues that might happen in the location.</li></ul> | N/A |
| {{site.data.keyword.satelliteshort}} Host | <ul><li>Provide an interface to initiate operational activities, such as to attach and remove hosts to a location.</li><li>Set up a location-specific control plane that runs on your user-provided hosts with three replicas for high availability.</li><li>Generate a script that users can run on select hosts to attach them to a {{site.data.keyword.satelliteshort}} location.</li><li>[Add](/docs/satellite?topic=satellite-hosts#attach-hosts) and [assign](/docs/satellite?topic=satellite-hosts#host-assign) hosts that meet the [host](/docs/satellite?topic=satellite-host-reqs) and [provider-specific](/docs/satellite?topic=satellite-infrastructure-plan#create-options-cloud) requirements to {{site.data.keyword.satelliteshort}} locations as needed to support your application workloads.</li><li>Ensure at initial setup that the location has the agreed upon number of unassigned hosts pools.</li><li>Assign hosts as worker nodes to {{site.data.keyword.openshiftshort}} clusters that you designate.</li><li>Monitor the health of hosts that are attached to a location, automatically resolve issues when possible, and alert IBM site reliability engineers when manual intervention is required.</li><li>Automatically forward host events to your {{site.data.keyword.at_full_notm}} instance.</li></ul> | <ul><li>Assign hosts with [enough compute resources to support the location control plane](/docs/satellite?topic=satellite-locations#control-plane-scale). The minimum requirement for demonstration purposes is at least 3 hosts with at least 4 vCPU and 16 GB memory. When you need more capacity, you must increase the control plane evenly across zones and in multiples of 3, such as 6, 9, or 12 hosts.</li><li>Establish initial network configuration so that hosts can connect to {{site.data.keyword.satelliteshort}} locations, such as allowing connectivity through firewalls or virtual private networks (VPNs).</li><li>In the on-prem or other user-provided environment, set up hosts in a [highly available architecture](/docs/satellite?topic=satellite-ha#satellite-ha-setup), such as in three separate zones.</li><li>Use the provided tools to manage hosts and [debug](/docs/satellite?topic=satellite-ts-hosts-debug) any issues that might happen in the hosts.</li></ul> |
|{{site.data.keyword.satelliteshort}} Config| <ul><li>Provide a highly available configuration management service that you can use to manage the deployment of Kubernetes resources across clusters that are registered with the location.</li><li>Provide an interface to initiate operational activities, such as to create and delete configurations.</li><li>Provide a `kubectl` command that users can run in an {{site.data.keyword.openshiftshort}} cluster to register the cluster to {{site.data.keyword.satelliteshort}} Config.</li><li>Provide the ability to create Kubernetes resource configurations, upload new versions, and subscribe a subset of cluster to a version, including to a previous version.</li><li>Store app configuration files in a highly available, back-end data store (`etcd`).</li><li>Automatically forward configuration events to your {{site.data.keyword.at_full_notm}} instance.</li></ul> | <ul><li>Use the provided tools to [set up clusters](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig), upload your Kubernetes configuration file content as versions in the configuration, and subscribe your clusters to the [configuration](/docs/satellite?topic=satellite-cluster-config#create-satconfig-ui). Keep in mind that you are responsible for the apps that run in your clusters, but you can use {{site.data.keyword.satelliteshort}} Config to help you consistently deploy and update your apps.</li></ul> |
|{{site.data.keyword.satelliteshort}} Link | <ul><li>Set up the {{site.data.keyword.satelliteshort}} link connector in the {{site.data.keyword.satelliteshort}} location to connect the control plane worker nodes to the control plane master.</li><li>Provide an interface to allow connections between your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}} or any publicly accessible endpoint.</li><li>Provide the ability to enable and disable connections between your location and an endpoint.</li><li>Automatically collect incoming and outgoing network traffic for an endpoint.</li><li>Provide a dashboard to review endpoint metrics, and automatically send endpoint logs to your {{site.data.keyword.la_full_notm}} instance.</li><li>Automatically forward link events to your {{site.data.keyword.at_full_notm}} instance.</li></ul> | <ul><li>Use the provided tools to [create and manage](/docs/satellite?topic=satellite-link-location-cloud) {{site.data.keyword.satelliteshort}} location endpoints.</li><li>Ensure that the {{site.data.keyword.satelliteshort}} link connector in the {{site.data.keyword.satelliteshort}} control plane is [enabled](/docs/satellite?topic=satellite-link-location-cloud#enable_disable_endpoint) to allow network traffic between your location and endpoints outside your location.</li><li>Enable any connections that you need to successfully run the apps in your location and debug any connection issues for your endpoints.</li></ul> |
| {{site.data.keyword.satelliteshort}}-enabled services | <ul><li>Provide the ability to deploy a select group of {{site.data.keyword.cloud_notm}} services such as [{{site.data.keyword.openshiftshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters) to a {{site.data.keyword.satelliteshort}} location.</li><li>Review each service's documentation for additional responsibilities that IBM maintains.</li></ul> | <ul><li>Use the provided tools to set up additional services as needed.</li><li>Provide enough hosts for the services to use as compute capacity, per the service documentation.</li><li>Review each service's documentation for additional responsibilities that you fulfill when you use these services.</li></ul> |
{: caption="Responsibilities for incident and operations." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a party might have responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

### Change management
{: #satis-change-management}

Change management includes tasks such as deployment, configuration, upgrades, patching, configuration changes, and deletion.
{: shortdesc}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|{{site.data.keyword.satelliteshort}} Location| <ul><li>Provide an interface to initiate change management activities, such as to delete locations.</li><li>Monitor the pool of unassigned hosts for the location. Add hosts as needed to ensure agreed upon pool size.</li></ul> | <ul><li>Update the hosts that are assigned to the location control plane, and ensure the control plane has [enough compute resources to run](/docs/satellite?topic=satellite-locations#control-plane-scale).</li><li>Before you [delete any locations](/docs/satellite?topic=satellite-locations#location-remove), remove all associated hosts and clusters. Save any backup information that you want to keep about the location before you delete the location.</li></ul> |
| {{site.data.keyword.satelliteshort}} Host | <ul><li>Add hosts if the pool of unassigned hosts falls below the agreed upon level.</li><li>Delete hosts that are removed from the {{site.data.keyword.satelliteshort}} location control plane or clusters and in a `replacement required` state.</li><li>Provide an interface to initiate change management activities.</li><li>Monitor and report back the health of hosts with actions that you must complete, such as reloading a host operating system.</li><li>Disable the ability to SSH into hosts after you attach the hosts to a location, to enhance security.</li><li>Make major, minor, and fix pack version updates of the container platform and operating system software available for you to apply.</li></ul> | <ul><li>[Review the status of your hosts](/docs/satellite?topic=satellite-monitor#host-health) and take any actions required to resolve host infrastructure issues, such as operating system reloads or updates.</li><li>Before you update or [delete](/docs/satellite?topic=satellite-hosts#host-remove) any hosts, make sure that you have [enough additional hosts](/docs/satellite?topic=satellite-infrastructure-plan#location-sizing) in the cluster or location control plane to continue running any components that you must run. Save any backup information that you want to keep about the hosts before you update or delete.</li><li>Deleted hosts are not reused. Assign hosts from the pool of unassigned hosts to handle new workloads.</li></ul> |
|{{site.data.keyword.satelliteshort}} Config| <ul><li>Provide an interface to initiate change management activities, such as to update configurations or subscriptions.</li><li>Automatically initiate the roll out of changes to a configuration to subscribed clusters.</li><li>Automatically delete Kubernetes resources that run in subscribed clusters when you delete a configuration.</li></ul> | <ul><li>Use the provided [{{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-cluster-config) and [{{site.data.keyword.openshiftshort}} tools](/docs/openshift?topic=openshift-plan_deploy#updating) to manage all changes to your apps. You are completely responsible for your app lifecycle, including any downtime that might occur when you update an app version, depending on your update rollout strategy.</li></ul> |
|{{site.data.keyword.satelliteshort}} Link | <ul><li>Maintain {{site.data.keyword.satelliteshort}} link connector versions.</li></ul> | <ul><li>Use the provided tools to [create, update, or delete the endpoints](/docs/satellite?topic=satellite-link-location-cloud) that you need.</li><li>[Enable and disable endpoints](/docs/satellite?topic=satellite-link-location-cloud#enable_disable_endpoint) to allow or block network traffic between your location and a service's endpoint.</li></ul> |
| {{site.data.keyword.satelliteshort}}-enabled services | <ul><li>Review each service's documentation for additional responsibilities that IBM maintains. For example, with {{site.data.keyword.openshiftlong_notm}} clusters, IBM provides patch version updates for the masters automatically and for the worker nodes that you initiate.</li></ul> | <ul><li>Review each service's documentation for additional responsibilities that you fulfill when you use these services.</li></ul> |
{: caption="Responsibilities for change management." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a party might have responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

<br />

### Identity and access management
{: #satis-iam-responsibilities}

Identity and access management includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access. For more information, see [Managing access for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-iam).
{: shortdesc}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|{{site.data.keyword.satelliteshort}} Location | <ul><li>Provide an interface to assign access control to locations via IAM.</li><li>Use customer-provided access to query {{site.data.keyword.satelliteshort}} APIs for the status of hosts that are attached to the location.</li></ul> | <ul><li>Use the provided tools to [manage authentication, authorization, and access control policies](/docs/satellite?topic=satellite-iam).</li><li>Provide the IBM team access to run {{site.data.keyword.satelliteshort}} automation against the location to monitor the status of attached hosts.</li></ul> |
| {{site.data.keyword.satelliteshort}} Host | <ul><li>Disable the ability to SSH into hosts after you assign the hosts to a location control plane or cluster, to enhance security.</li><li>Add hosts to the {{site.data.keyword.satelliteshort}} location, with the agreed upon number of available host pools.</li><li>After setting up the hosts, IBM does not have access to the hosts unless you grant access.</li></ul> | <ul><li>[Assign](/docs/satellite?topic=satellite-hosts#host-assign) hosts to a cluster. After assigning the host, SSH access is disabled and access to the host is controlled via [{{site.data.keyword.cloud_notm}} IAM access](/docs/openshift?topic=openshift-users).</li></ul> |
|{{site.data.keyword.satelliteshort}} Config| <ul><li>Provide an interface to assign access control to configurations via IAM.</li></ul> | <ul><li>Use the provided tools to [manage authentication, authorization, and access control policies](/docs/satellite?topic=satellite-iam) to use {{site.data.keyword.satelliteshort}} configurations and subscriptions to create, update, and delete Kubernetes resources.<p class="note">Access in IAM to {{site.data.keyword.satelliteshort}} Config does not give users access to the clusters, nor the ability to log in and manage the Kubernetes resources from the cluster. Users with access to a cluster might log in and manually change the Kubernetes resources.</p></li></ul> |
|{{site.data.keyword.satelliteshort}} Link | <ul><li>Provide an interface to assign access control to endpoints via IAM.</li></ul> | <ul><li>Use the provided tools to [manage authentication, authorization, and access control policies](/docs/satellite?topic=satellite-iam).</li></ul> |
| {{site.data.keyword.satelliteshort}}-enabled services | <ul><li>Review each service's documentation for additional responsibilities that IBM maintains.</li></ul> | <ul><li>Review each service's documentation for additional responsibilities that you fulfill when you use these services.</li></ul> |
{: caption="Responsibilities for identity and access management." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a party might have responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

<br />

### Security and regulation compliance
{: #satis-security-compliance}

Security and regulation compliance includes tasks such as security controls implementation and compliance certification.
{: shortdesc}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
| General | <ul><li>Provide platform-level compliance to certain standards. For more information, see [{{site.data.keyword.cloud_notm}} compliance](/docs/overview?topic=overview-compliance).</li><li>Provide tools to manage billing, usage, and identity and access control (IAM).</li><li>Set default security settings for {{site.data.keyword.satelliteshort}} components. These settings do not guarantee security, and might be modified by the user.</li></ul> | <ul><li>Identify government, industry, and proprietary corporate standards that are required for the environment.</li><li>Review the physical premises that host the underlying infrastructure for security controls to protect the data center.</li></ul>  |
|{{site.data.keyword.satelliteshort}} Location| <ul><li>Maintain security and regulation compliance for the {{site.data.keyword.cloud_notm}}-managed location control plane.</li><li>Update the managed master components.</li><li>Provide patch updates for the control plane components that run in the location worker nodes.</li><li>Provide the ability to control access to locations via {{site.data.keyword.cloud_notm}} IAM.</li><li>Keep {{site.data.keyword.satelliteshort}} Infrastructure Service infrastructure components such as the compute, network, and storage hardware secure and compliant.</li></ul> | <ul><li>Apply worker node patch updates to the hosts that run the location control plane.</li></ul> |
| {{site.data.keyword.satelliteshort}} Host | <ul><li>Provide patch updates for the hosts that run as worker nodes in {{site.data.keyword.satelliteshort}} clusters.</li><li>Disable the ability to SSH into hosts after you assign the hosts to a location control plane or clusters, to enhance security.</li><li>Keep {{site.data.keyword.satelliteshort}} Infrastructure Service infrastructure components such as the compute, network, and storage hardware secure and compliant.</li></ul> | <ul><li>Apply worker node patch updates to ensure host compliance.</li></ul> |
|{{site.data.keyword.satelliteshort}} Config | <ul><li>Deploy apps consistently across clusters and locations.</li><li>Provide the ability to control access to configurations via {{site.data.keyword.cloud_notm}} IAM.</li></ul> | <ul><li>Create your Kubernetes configuration files by following the security standards that you want to comply to, such as by using security context constraints. You are responsible for the security and compliance of your apps.</li></ul> |
|{{site.data.keyword.satelliteshort}} Link | <ul><li>Establish a secure connection between {{site.data.keyword.cloud_notm}} and {{site.data.keyword.satelliteshort}} locations by using the {{site.data.keyword.satelliteshort}} link connector.</li><li>Provide the ability to control access to endpoints via {{site.data.keyword.cloud_notm}} IAM.</li><li>Provide the ability to monitor network traffic between your location and endpoints outside of your location.</li></ul> | <ul><li>Set up and audit [link endpoints](/docs/satellite?topic=satellite-link-location-cloud) across locations.</li></ul> |
| {{site.data.keyword.satelliteshort}}-enabled services | <ul><li>Review each service's documentation for additional responsibilities that IBM maintains.</li></ul> | <ul><li>Review each service's documentation for additional responsibilities that you fulfill when you use these services.</li></ul> |
{: caption="Responsibilities for security and regulation compliance." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a party might have responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

<br />

### Disaster recovery
{: #satis-disaster-recovery}

Disaster recovery includes tasks such as providing dependencies on disaster recovery sites, provision disaster recovery environments, data and configuration backup, replicating data and configurations to the disaster recovery environment, and failover on disaster events.
{: shortdesc}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|{{site.data.keyword.satelliteshort}} Location| <ul><li>Back up location information to recover {{site.data.keyword.satelliteshort}} location control plane components to an IBM-managed bucket in your {{site.data.keyword.cos_full_notm}} instance.</li><li>Monitor the health of the location, automatically resolve issues when possible, and alert IBM site reliability engineers when manual intervention is required.</li></ul> | <ul><li>Define the disaster recovery requirements for the environment.</li><li>Manage backups of applications and data stored in the location.</li><li>Provide access to the backup in your [{{site.data.keyword.cos_full_notm}} instance](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage) if you need to recover a location.</li></ul> |
| {{site.data.keyword.satelliteshort}} Host | <ul><li>Back up information for hosts that are assigned to the location control plane to an IBM-managed {{site.data.keyword.cos_full_notm}} instance.</li><li>Back up information for hosts that are assigned to {{site.data.keyword.satelliteshort}} clusters to an {{site.data.keyword.cos_full_notm}} instance in your account.</li><li>Maintain, repair, or replace hardware as needed.</li></ul> | N/A |
|{{site.data.keyword.satelliteshort}} Config| <ul><li>Back up information about saved {{site.data.keyword.satelliteshort}} Configs in etcd.</li><li>When service is restored, automatically deploy configuration files to available clusters.</li></ul> | <ul><li>Design, implement and test a disaster recovery plan for your [highly available apps](/docs/openshift?topic=openshift-plan_deploy#highly_available_apps).</li></ul> |
|{{site.data.keyword.satelliteshort}} Link | <ul><li>N/A</li></ul> | <ul><li>Reinstate any necessary [endpoints](/docs/satellite?topic=satellite-link-location-cloud) to your resources after recovering from a disaster.</ul> |
| {{site.data.keyword.satelliteshort}}-enabled services | <ul><li>Review each service's documentation for additional responsibilities that IBM maintains.</li></ul> | <ul><li>Review each service's documentation for additional responsibilities that you fulfill when you use these services.</li></ul> |
{: caption="Responsibilities for disaster recovery." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the task that a party might have responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}
