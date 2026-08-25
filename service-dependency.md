---

copyright:
  years: 2024, 2026
lastupdated: "2026-08-25"

keywords:

subcollection: satellite


---




{{site.data.keyword.attribute-definition-list}}

# Service dependency map for IBM Cloud Satellite
{: #service-dependencies}

If a service depends on other {{site.data.keyword.cloud_notm}} services, there can be impacts if any of the dependent services are having issues. The dependency severity indicates the impact to the service when the dependency is down.
{: shortdesc}

Critical
:   When the the dependency is down, the service is down.

Significant
:   When the dependency is down, the service features are impacted.

Medium
:   When the dependency is down, the service can be impacted and a workaround is possible.

Minimal
:   When the dependency is down, the main service features are not impacted.



## Data and Control plane deployment for an MZR
{: #data-and-control-plane-deployment-for-an-mzr}

The following dependencies apply to the following deployment locations: Dallas (us-south), Frankfurt (eu-de), London (eu-gb), Madrid (eu-es), Osaka (jp-osa), Sao Paulo (br-sao), Sydney (au-syd), Tokyo (jp-tok), Toronto (ca-tor), Washington DC (us-east).


|Dependencies|Dependency impacts|Customer provided|Control or data plane|Location of dependency|
|:---|:---|:---|:---|:---|
| Akamai | Availability, Change management, Disaster recovery, Instance control, Security compliance | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| IBM Cloud Business Support Services | Availability, Security compliance | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| IBM Cloud Classic DNS Servers | Availability, Change management, Instance control | No | Both |  Same data center  |
| IBM Cloud Global Search and Tagging | Availability, Security compliance | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| {{site.data.keyword.BluVirtServers}} | Availability, Change management, Disaster recovery, Instance control | No | Both |  Same data center  |
| {{site.data.keyword.iamlong}} | Access management, Availability, Change management, Instance control, Security compliance | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| {{site.data.keyword.cos_full}} | Availability, Change management, Disaster recovery, Instance control | No | Both |  Same region  |
{: row-headers}
{: caption="IBM Cloud Satellite - Data and Control plane deployment for an MZR service dependency information - Critical dependencies" caption-side="top"}
{: tab-title="Critical dependencies"}
{: tab-group="service-dependency-data-for-satellite-Data-and-Control-plane-deployment-for-an-MZR"}
{: class="comparison-tab-table"}
{: #critical-deps-data-and-control-plane-deployment-for-an-mzr}
{: summary="Use the buttons for the dependency level to change the context of the table. This table has row and column headers. The row headers detail the specific dependent service. The column headers identify the details about the dependency and its impact. To understand the details about each dependency, navigate to the row to find the dependency that you need more information about interested in."}

|Dependencies|Dependency impacts|Customer provided|Control or data plane|Location of dependency|
|:---|:---|:---|:---|:---|
| IBM Cloud Service Endpoints | Availability, Change management, Disaster recovery, Instance control, Security compliance | No | Both |  Same data center  |
| {{site.data.keyword.registrylong}} | Availability, Change management, Disaster recovery, Instance control, Security compliance | No | Both |  Same region  |
| Amplitude | Availability | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| IBM Cloud Business Support Services | Availability | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| Let's Encrypt | Availability, Change management, Instance control, Security compliance | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| {{site.data.keyword.monitoringlong}} | Availability, Operations, Security compliance | No | Both |  Same region  |
| IBM Cloud CLI  | Availability, Change management, Instance control | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| {{site.data.keyword.keymanagementservicefull}} | Availability, Change management, Disaster recovery, Instance control, Security compliance | No | Both |  Same region  |
| {{site.data.keyword.atracker_full}} | Availability, Operations, Security compliance | No | Both |  Same region  |
| RedHat OpenShift Cluster Manager | Availability, Change management | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| IBM Cloud Console | Availability, Change management, Instance control, Operations, Security compliance | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| {{site.data.keyword.keymanagementservicefull}} | Availability, Change management, Disaster recovery, Instance control, Security compliance | Yes | Both |  Same region  |
| {{site.data.keyword.bplong}} | Availability, Change management, Disaster recovery | No | Both |  Same region  |
| OSS Platform | Availability, Operations | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| LaunchDarkly | Availability, Change management, Instance control, Security compliance | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| {{site.data.keyword.secrets-manager_full}} | Availability, Change management, Disaster recovery, Instance control, Security compliance | Yes | Both |  Same region  |
| IBM Cloud Classic NTP Servers | Availability, Change management, Instance control | No | Both |  Same data center  |
| PagerDuty | Availability, Operations | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| IBM Cloud Global Resource Catalog | Availability, Change management, Instance control | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| Slack | Availability, Instance control, Operations | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| Segment | Availability | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| {{site.data.keyword.logs_full}} | Access management, Availability, Operations, Security compliance | No | Both |  Same region  |
| IBM Cloud Databases | Availability, Change management, Disaster recovery, Instance control | No | Both |  Same region  |
{: row-headers}
{: caption="IBM Cloud Satellite - Data and Control plane deployment for an MZR service dependency information - Significant dependencies" caption-side="top"}
{: tab-title="Significant dependencies"}
{: tab-group="service-dependency-data-for-satellite-Data-and-Control-plane-deployment-for-an-MZR"}
{: class="comparison-tab-table"}
{: #significant-deps-data-and-control-plane-deployment-for-an-mzr}
{: summary="Use the buttons for the dependency level to change the context of the table. This table has row and column headers. The row headers detail the specific dependent service. The column headers identify the details about the dependency and its impact. To understand the details about each dependency, navigate to the row to find the dependency that you need more information about interested in."}

|Dependencies|Dependency impacts|Customer provided|Control or data plane|Location of dependency|
|:---|:---|:---|:---|:---|
| corporate-qradar| Operations, Security compliance | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| SOS File Integrity Monitoring| Security compliance | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| SOS SIEM| Security compliance | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| IBM Cloud Security and Compliance Center| Security compliance | No | Both |  Same region  |
| SOS Tenable| Security compliance | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
| SOS Inventory Management| Security compliance | No | Both |  [Global](/docs/resiliency?topic=resiliency-ha-redundancy#global-platform)  |
{: row-headers}
{: caption="IBM Cloud Satellite - Data and Control plane deployment for an MZR service dependency information - Minimal dependencies" caption-side="top"}
{: tab-title="Minimal dependencies"}
{: tab-group="service-dependency-data-for-satellite-Data-and-Control-plane-deployment-for-an-MZR"}
{: class="comparison-tab-table"}
{: #minimal-deps-data-and-control-plane-deployment-for-an-mzr}
{: summary="Use the buttons for the dependency level to change the context of the table. This table has row and column headers. The row headers detail the specific dependent service. The column headers identify the details about the dependency and its impact. To understand the details about each dependency, navigate to the row to find the dependency that you need more information about interested in."}


## Understanding service dependency data
{: #understand-dependency-data}



If you have any questions about the service dependency data as you review the service dependency information in the tables, you can refer to the following FAQ:

### What is the expected impact to the functions described?
{: #expected-impact}

Each severity tab in the table indicates the impact on your provisioned service if the dependency goes offline. The dependency's high availability and disaster recovery configuration influences the severity of that impact and provides general guidance about potential issues during an incident.

Services that are regional are not impacted by a severe outage of a single zone because of the built-in failover to another zone. During failover, a slight performance impact may occur while the system switches to the other location. Global services reduce impact further by failing over across regions, which decreases the frequency of the impacts shown in the table.
{: note}

### What services does my service depend on?
{: #dependent-services}

The **Dependencies** column lists the services. These are the major service-to-service dependencies, including major internal dependencies that are not visible externally.

### What function does the dependency impact?
{: #function-impact}

Functions include access management, availability, change management, configuration management, customer responsibility, disaster recovery, instance control, operations, security compliance, or none. If the dependency goes offline, these functions are impacted. Definitions for each available value are as follows:

Access management
:   Authentication, authorization, and governance of the customer users access to the service and service instances.

Availability
:   Availability of the service and service instances.

Change management
:   Deployment, upgrade, patch, and so on, of the service and service instances.

Configuration management
:   Deployment, upgrade, patch, and so on, of the service and service instances.

Customer responsibility
:   Functions provided by customers to support specific service and service instances function. For example, {{site.data.keyword.keymanagementservicefull_notm}} instances provided by customer to support service BYOK encryption.

Disaster recovery
:   Backup, recovery, restart of the service and service instances in the case of a disruption.

Instance control
:   Creation, deletion, start, stop actions on the lifecycle of the service instances.

Operations
:   Monitoring, troubleshooting, and so on, of the service and service instances.

Security compliance
:   Vulnerability management and other security and compliance management of the service and service instances.

None
:   No function impacted.

### What does customer provided mean for dependencies?
{: #customer-provided-dep}

The **Customer provided** column reports if there is any dependency that has been provided by the customer to enable specific functionality. One such example is to properly configure and set up using BYOK into a service, the customer would provision a service like {{site.data.keyword.keymanagementservicefull_notm}}. For more information about how to enable the features and which services you need to provision, see the documentation for the service.

### Where do dependency services need to be deployed regarding my service?
{: #deploy-dependencies}

In the **Location of dependency** column, you can view if the dependency is located in the same region or deployed to a specific data center. You can use this data with the data in the **Control or data plane** column for a quick reference to identify if your data leaves the region or not in a standard setup.

To find where your service can be deployed, see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region).

The table shows a standard cloud deployment. If a special deployment is used like FedRAMP or other region-bound deployment models, the data differs from the details available in the table. Refer to the specific deployment that you are using for that information.
{: note}

### Where are the separate control plane and data plane located, if applicable?
{: #separate-plans}

Sometimes, the dependency has a separate control plane and data plane. In these cases, there are separate lines that show the location in relation to the deployed customer instance of the service where these will be provisioned. The lines can have different impacts and different functions. See the **Control or data plane** column to understand what possible impact this type of outage can have.

`Same region` means that the dependent services are in the same region as the provisioned instance. Other values show `data center` or region names if the service must be used from a specific region, zone, or set of zones. If a service is tied to a specific region or site and that region goes offline, the service goes offline as well. Review the high availability and disaster recovery documentation of each dependency to determine mitigation steps.

## Additional resources
{: #additional-resources}

For more information about the policies that are related to the services, you can refer to the following resources:

* [Service Level Agreement](https://www.ibm.com/support/customer/csol/terms/?id=i126-6605&lc=en){: external}
* [Shared responsibilities for using {{site.data.keyword.cloud_notm}} products](/docs/overview?topic=overview-shared-responsibilities)
* [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region)
