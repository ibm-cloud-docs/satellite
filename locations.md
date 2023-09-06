---

copyright:
  years: 2020, 2023
lastupdated: "2023-09-06"

keywords: satellite, hybrid, multicloud, location, satellite location, create location

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Create a {{site.data.keyword.satelliteshort}} location overview
{: #locations}

The first step to getting started with {{site.data.keyword.satelliteshort}} is to create a {{site.data.keyword.satelliteshort}} location. A {{site.data.keyword.satelliteshort}} location is a representation of an environment in your infrastructure provider, such as an on-prem data center or cloud. Locations are made up of compute sources, called hosts, from separate zones of your backing infrastructure environment. 
{: shortdesc}

The type of location that you create dictates the type of operating systems that can run on your hosts. If your location is RHCOS enabled, then you can attach hosts that are running either RHEL and RHCOS. If your location isn't RHCOS enabled, then you can attach only hosts that are running RHEL. You can check whether your [location is RHCOS enabled](/docs/satellite?topic=satellite-locations#verify-coreos-location).
{: note}

Before you create your location, check out the following topics.

- [Find out more about locations and hosts](/docs/satellite?topic=satellite-location-host).
- [Plan your infrastructure](/docs/satellite?topic=satellite-infrastructure-plan).
- [Verify your host setup](/docs/satellite?topic=satellite-host-network-check).

## Host operating system
{: #create-host-os}

Choose your operating system for your hosts. You can choose Red Hat Enterprise Linux or Red Hat CoreOS. If you want to use Red Hat CoreOS for your managed services, you must create and enable a location to use RHCOS hosts.

For more information, see [Planning your operating system](/docs/satellite?topic=satellite-infrastructure-plan#infras-plan-os).


## Options for creating your {{site.data.keyword.satelliteshort}} location
{: #create-options}

Your locations is made up of compute sources, called hosts, from separate zones of your backing infrastructure environment. Depending on your infrastructure provider, you have different options to create a {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

My infrastructure is a cloud provider.
:    You can create your locations [manually with the CLI or from the console](/docs/satellite?topic=satellite-loc-manual-create). For certain providers, you can set up {{site.data.keyword.satelliteshort}} locations with a {{site.data.keyword.bpshort}} template. 

:    - [AWS](/docs/satellite?topic=satellite-loc-aws-create-auto)
     - [Azure](/docs/satellite?topic=satellite-loc-azure-create-auto)
     - [GCP](/docs/satellite?topic=satellite-loc-gcp-template)

My infrastructure is on prem or edge.
:    You can create your locations [manually with the CLI or from the console](/docs/satellite?topic=satellite-loc-manual-create).  

I want to create an RHCOS enabled location.
:    To create an RHCOS enabled location, you must [create it manually](/docs/satellite?topic=satellite-loc-manual-create). 

I want to create a location with reduced firewall footprint
:    Depending on your setup, you might be able to connect your location to a single network destination instead of multiple destinations to reduce the number of outbound IP addresses to allow on your firewall. For more information, see [Creating Red Hat CoreOS enabled Locations with reduced firewall footprint](/docs/satellite?topic=satellite-coreos-reduced-firewall).


## Is my location enabled for Red Hat CoreOS?
{: #verify-coreos-location}

You can verify that your location is enabled for Red Hat CoreOS by running the **`location get`** command. Look for the `Ignition Server Port:` field to populate. Wait to check until after your location is provisioned. 

Red Hat CoreOS is available in all supported {{site.data.keyword.satelliteshort}} locations and for {{site.data.keyword.redhat_openshift_notm}} version 4.9 and later. Red Hat CoreOS hosts don't support all services. For more information, see [Supported {{site.data.keyword.satelliteshort}}-enabled IBM Cloud services](/docs/satellite?topic=satellite-managed-services).
{: note}


```sh
ibmcloud sat location get --location LOCATION
```
{: pre}

Example output

```sh
Name:                           my-coreos-test   
ID:                             <ID>   
Created:                        2022-03-26 15:02:00 +0000 (4 days ago)   
Managed From:                   dal   
State:                          action required   
Ready for deployments:          no   
Message:                        R0012: The location control plane does not have hosts in all 3 zones. Add available hosts to your location for the control plane.   
Hosts Available:                0   
Hosts Total:                    0   
Host Zones:                     us-south-1, us-south-2, us-south-3   
Provider:                       -   
Provider Region:                -   
Provider Credentials:           no   
Public Service Endpoint URL:    <ENDPOINT>   
Private Service Endpoint URL:   -   
OpenVPN Server Port:            -   
Ignition Server Port:           30119   
Konnectivity Server Port:       32157
```
{: screen}

To create a Red Hat CoreOS-enabled location, see [Manually creating {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-loc-manual-create).



