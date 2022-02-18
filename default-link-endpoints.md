---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-18"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Default Link endpoints for {{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satelliteshort}} location
{: #default-link-endpoints}

Default {{site.data.keyword.satelliteshort}} Link endpoints are created for your location's control plane cluster and for any other {{site.data.keyword.satelliteshort}}-enabled services that you run in your location. These default {{site.data.keyword.satelliteshort}} Link endpoints are accessible only from within the {{site.data.keyword.cloud_notm}} private network.
{: shortdesc}

The following table describes the Link endpoints that are automatically created in your {{site.data.keyword.satelliteshort}} location.

| Name | Description | Type | Instances |
| ---- | ----------- | ---- | --------- |
| `satellite-healthcheck-<location_ID>` | Allows the {{site.data.keyword.satelliteshort}} control plane master to check the health of your location's control plane cluster. | `location` | One per location |
| `satellite-containersApi` | Allows your {{site.data.keyword.satelliteshort}} location to communicate with the {{site.data.keyword.cloud_notm}} containers API. | `cloud` | One per location |
| `satellite-cosCrossRegion-<location_ID>` | Allows the control plane data of your {{site.data.keyword.satelliteshort}} location to be backed up to your {{site.data.keyword.cos_full}} instance. Control plane master data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own. | `cloud` | One per location |
| `satellite-cosRegional-<location_ID>` | Allows the control plane data of your {{site.data.keyword.satelliteshort}} location to be backed up to your {{site.data.keyword.cos_full}} instance. Control plane master data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own. | `cloud` | One per location |
| `satellite-cosResConf-<location_ID>` | Allows the control plane data of your {{site.data.keyword.satelliteshort}} location to be backed up to your {{site.data.keyword.cos_full}} instance. Control plane master data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own. | `cloud` | One per location |
| `satellite-iam-<location_ID>` | Allows requests to your {{site.data.keyword.satelliteshort}} location in {{site.data.keyword.cloud_notm}} to be authenticated and user actions to be authorized by Identity and Access Management (IAM). | `cloud` | One per {{site.data.keyword.satelliteshort}} location |
| `satellite-kpRegional-<location_ID>` | Allows apps and services in the location to communicate with the {{site.data.keyword.keymanagementservicelong_notm}} service API | `cloud` | One per location |
| `satellite-logdna-<location_ID>` | Allows logs for your {{site.data.keyword.satelliteshort}} location to be sent to your {{site.data.keyword.la_full}} instance. | `cloud` | One per location |
| `satellite-logdnaapi-<location_ID>` | Allows your {{site.data.keyword.satelliteshort}} location to communicate with the {{site.data.keyword.la_full}} API. | `cloud` | One per {{site.data.keyword.satelliteshort}} location |
| `satellite-sysdig-<location_ID>` | Allows metrics for your {{site.data.keyword.satelliteshort}} location to be sent to your {{site.data.keyword.mon_full}} instance. | `cloud` | One per location |
| `satellite-sysdigapi-<location_ID>` | Allows your {{site.data.keyword.satelliteshort}} location to communicate with the {{site.data.keyword.mon_full_notm}} API. | `cloud` | One per {{site.data.keyword.satelliteshort}} location |
| `openshift-api-<cluster_ID>` | Allows the {{site.data.keyword.openshiftlong_notm}} API to communicate with the master for the service cluster. | `location` | One per [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services) in your location |
{: caption="Default Link endpoints." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the name of the default endpoint. The second column describes what the endpoint is for. The third column describes how many instances of the endpoint are created and for which component the endpoint is created."}

These endpoints are used to manage and update your location and are enabled by default. If you disable any of these endpoints, your client services that are running on your {{site.data.keyword.satelliteshort}} location can be negatively impacted. To avoid issues, do not disable these endpoints.
{: important}
