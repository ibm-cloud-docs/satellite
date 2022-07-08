---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-08"

keywords: satellite, hybrid, multicloud, satellite infrastructure service

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Understanding {{site.data.keyword.satelliteshort}} Infrastructure Service components
{: #satis-infra-about}

{{site.data.keyword.satelliteshort}} Infrastructure Service is a fully integrated solution that includes the necessary compute, storage, and network infrastructure to run {{site.data.keyword.cloud_notm}} services with your application workloads in your data center. You provide the floor space, power, cooling, and network connections. {{site.data.keyword.IBM_notm}} provides the rest of the infrastructure.
{: shortdesc}

{{site.data.keyword.satelliteshort}} Infrastructure Service includes redundant infrastructure components to meet the {{site.data.keyword.cloud_notm}} service level agreement. You can work with {{site.data.keyword.IBM_notm}} to expand your location for more compute and storage as your demand increases. The minimum configuration shown in Figure 1 is described as follows.

- 24 compute blades that delivers 1000 vCPU of compute capacity for your {{site.data.keyword.satelliteshort}} Infrastructure Service location. Installed compute capacity can be increased in increments of 8 blades.
- 120TB of flash storage. Installed storage capacity can be increased in increments of 30TB.  

![An example minimum configuration rack of {{site.data.keyword.satelliteshort}} Infrastructure Service.](images/satellite-is-ov.svg "Minimum configuration rack of {{site.data.keyword.satelliteshort}} Infrastructure Service."){: caption="Figure 1. An example deployment of {{site.data.keyword.satelliteshort}} Infrastructure Service." caption-side="bottom"}

## Compute resources
{: #satis-infra-compute}

Your {{site.data.keyword.satelliteshort}} Infrastructure Service location always maintains a selection of unallocated hosts.  Additional unallocated hosts will be created as you assign hosts in your location.  You can customize the quantity and size of the unallocated hosts in your {{site.data.keyword.satelliteshort}} location during the planning process to better fit the mix of workloads you deploy.  It can also be adjusted after deployment as your needs change.

To use your hosts, create {{site.data.keyword.satelliteshort}} resources such as {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services or clusters. Then, assign the hosts to these resources, either [automatically](/docs/satellite?topic=satellite-assigning-hosts#host-autoassign-ov) or [manually](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual).

The following table outlines a sample selection of hosts in a {{site.data.keyword.satelliteshort}} Infrastructure Service location. To scale the type and number of {{site.data.keyword.satelliteshort}} Infrastructure Service hosts in your location, work with your {{site.data.keyword.IBM_notm}} contact.

| Host size | vCPU | RAM (GB) | Primary storage (GB) | Auxiliary storage (GB) | Tertiary storage (GB) | Pool size |
| --- | --- | --- | --- | --- | --- | --- |
| Small | 4   | 32  | 25  | 100 | 0   | 5   |
| Medium | 8   | 64  | 25  | 100 | 0   | 5   |
| Large | 16  | 128 | 25  | 100 | 0   | 5   |
| Tertiary-Medium | 8   | 64  | 100 | 500 | 1000 | 0  |
| Tertiary-Large | 16  | 128 | 100 | 1000 | 2000 | 5  |
| Tertiary-XLarge | 16  | 128 | 100 | 1000 | 4000 | 0  |
{: caption="{{site.data.keyword.satelliteshort}} Infrastructure Service host pool sizes."}

## Required license for operating system and container platform
{: #satis-infra-license}

As described in the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs), your hosts must run the Red Hat Enterprise Linux operating system and OpenShift Container Platform. You must provide an activation key for the required software during the planning phase of your location.
{: shortdesc}

## Storage for persistent volumes
{: #satis-infra-storage}

{{site.data.keyword.satelliteshort}} Infrastructure Service provides flash storage that can be used for Kubernetes persistent volumes (PVs). Your apps can save data to these volumes by using persistent volume claims (PVCs). You can review the attached storage through the {{site.data.keyword.satelliteshort}} management interface.
{: shortdesc}

## Networking
{: #satis-infra-network}

{{site.data.keyword.satelliteshort}} Infrastructure Service networking components connect to your data center's network infrastructure. You must have your networking infrastructure and ports ready before the {{site.data.keyword.satelliteshort}} Infrastructure Service hardware is installed.
{: shortdesc}

### Required connections to your LAN
{: #satis-infra-network-lan}

{{site.data.keyword.satelliteshort}} Infrastructure Service requires two types of connections to your local area network (LAN): data and remote management. You decide the number of ports when you plan the setup with {{site.data.keyword.IBM_notm}}.  Rack domains can be split across multiple racks to accommodate power and cooling density requirements.
{: shortdesc}

| Connection | Type | Speed (Gbps) | Number of ports |
| --- | --- | --- | --- |
| Data | `Eth` | 10 Gbps | 2 - 28 per rack domain |
| Remote Management | `Eth` | 1 Gbps  | 2 - 4 per location |
{: caption="{{site.data.keyword.satelliteshort}} Infrastructure Service required data and remote management connections."}

### IP addresses
{: #satis-infra-network-ip}

You must provide an IP address range that is reserved for your {{site.data.keyword.satelliteshort}} Infrastructure Service location and integrated into your existing data center network.
{: shortdesc}

### Distance between compute domains
{: #satis-infra-racks-distance}

In medium and larger installations, you must spread the hardware equipment across multiple racks as described in the following table.
{: shortdesc}

| Rack placement | Cable type | Max distance |
| --- | --- | --- |
| Close proximity | Copper | Racks must be adjacent |
| In same data room | Fiber | 30 meters |
| Different buildings | Fiber | Contact {{site.data.keyword.IBM_notm}} |
{: summary="The table describes the distance between multiple racks. Rows are to be read from the left to right. The first column describes the placement of the racks. The second column is the type of cable that connects to the rack. The third column is the maximum distance that the racks can be apart."}
{: caption="Distance between multiple racks."}

## Power and cooling
{: #satis-infra-env}

During the planning phase, you can customize your {{site.data.keyword.satelliteshort}} Infrastructure Service installation to match the power and cooling density requirements of your data center. Typical power densities are 11kW, 17kW, or 22kW per rack. For lower densities, hardware components can be spread across more racks. Components are grouped into rack domains that occupy two or three racks, depending on the power density. Racks within a single domain must be adjacent.
{: shortdesc}

| Configuration | Power | Cooling (BTU/hr) | Dimensions (H x W x D) | Weight |
| --- | --- | --- | --- | --- |
| First rack | 17 kW  | 41,278.58  | 2,013 mm x 597 mm x 1,266 mm (79.3 in. x 23.5 in. x 49.8 in.) | 689.66 kg (1,520 lb)  |
| Second rack | 9 kW  | 22,329.09  | 2,013 mm x 597 mm x 1,266 mm (79.3 in. x 23.5 in. x 49.8 in.)  | 471 kg (1,037.87 lb) |
{: caption="Power and cooling guidance for different rack configurations." caption-side="bottom"}
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
{: caption="Power and cooling guidance for different rack configurations." caption-side="bottom"}
{: summary="The rows are read from left to right. The first column describes the size of the configuration. The second column is the required power. The third column is the required cooling. The fourth column is the dimensions of the rack. The fifth column is the weight of the rack."}
{: class="simple-tab-table"}
{: #three-rack}
{: tab-title="Three racks"}
{: tab-group="rack-power-cooling"}
