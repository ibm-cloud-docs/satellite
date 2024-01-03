---


copyright:
  years: 2020, 2024
lastupdated: "2024-01-03"

keywords: satellite, hybrid, multicloud, default link enpoints, default link

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Default Link endpoints for {{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satelliteshort}} location
{: #default-link-endpoints}

Find information about default Link endpoints for {{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satellitelong}} location.

## Default link endpoints for CoreOS enabled locations
{: #rhcos-default-endpoints}

Default endpoints are created for your location's control plane cluster and for any other {{site.data.keyword.satelliteshort}}-enabled services that you run in your location. These endpoints are accessible only from within the {{site.data.keyword.cloud_notm}} private network.
{: shortdesc}

The following table describes the Link endpoints that are automatically created in your Red Hat CoreOS enabled location.

| Name | Description | Type | Instances |
| ---- | ----------- | ---- | --------- |
| `local-appLd-<location-ID>` | Allows your location to communicate with LaunchDarkly. | Cloud | One per location |
| `local-containerapi-<location-ID>` | Allows your location to communicate with the {{site.data.keyword.cloud_notm}} containers API. | Cloud | One per location |
| `local-cosRegional-<location-ID>` | Allows the control plane data of your location to be backed up to your {{site.data.keyword.cos_full}} instance. management plane data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own. | Cloud | One per location |
| `local-cosUS-<location-ID>` | Allows your location to communicate with {{site.data.keyword.cos_full}}. | Cloud | One per location |
| `local-csLd-<location-ID>` | Allows your location to communicate with LaunchDarkly. | Cloud | One per location |
| `local-iam-<location-ID>` | Allows requests to your location in {{site.data.keyword.cloud_notm}} to be authenticated and user actions to be authorized by Identity and Access Management (IAM). | Cloud | One per location |
| `local-icr-<location-ID>` | Allows the control plane data of your location to be backed up to your {{site.data.keyword.cos_full}} instance. management plane data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own. | Cloud | One per location |
| `local-icrau-<location-ID>` | Allows your location to communicate with {{site.data.keyword.registryshort}}. | Cloud | One per location
| `local-icrus-<location-ID>` | Allows your location to communicate with {{site.data.keyword.registryshort}}. | Cloud | One per location
| `local-ignition-<location-ID>`| Allows your location to communicate with the ignition service. |Cloud | One per location |
| `local-konnectivity-<location-ID>` | Allows your location to communicate with the Konnectivity service | Cloud | One per location |
| `local-master-<location-ID>` | Allows your location to communicate with the management plane. | Cloud | One per location |
| `local-oauth-<location-ID>` | Allows your location to communicate with the oAuth service. | Cloud | One per location |
| `satellite-containersApi` | Allows your location to communicate with the {{site.data.keyword.cloud_notm}} containers API. |  Cloud | One per location |
| `satellite-cosCrossRegion-<location_ID>` | Allows the control plane data of your location to be backed up to your {{site.data.keyword.cos_full}} instance. management plane data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own. | Cloud | One per location |
| `satellite-cosRegional-<location_ID>` | Allows the control plane data of your location to be backed up to your {{site.data.keyword.cos_full}} instance. management plane data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own. | Cloud | One per location |
| `satellite-cosResConf-<location_ID>` | Allows the control plane data of your location to be backed up to your {{site.data.keyword.cos_full}} instance. management plane data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own. | Cloud | One per location |
| `satellite-healthcheck-<location_ID>` | Allows the {{site.data.keyword.satelliteshort}} management plane to check the health of your location's control plane cluster. | Location | One per location |
| `satellite-iam-<location_ID>` | Allows requests to your location in {{site.data.keyword.cloud_notm}} to be authenticated and user actions to be authorized by Identity and Access Management (IAM). | Cloud | One per location |
| `satellite-kpRegional-<location_ID>` | Allows apps and services in the location to communicate with the {{site.data.keyword.keymanagementservicelong_notm}} service API | Cloud | One per location |
| `satellite-logdna-<location_ID>` | Allows logs for your location to be sent to your {{site.data.keyword.la_full}} instance. | Cloud | One per location |
| `satellite-logdnaapi-<location_ID>` | Allows your location to communicate with the {{site.data.keyword.la_full}} API. | Cloud | One per location |
| `satellite-sysdig-<location_ID>` | Allows metrics for your location to be sent to your {{site.data.keyword.mon_full}} instance. | Cloud | One per location |
| `satellite-sysdigapi-<location_ID>` | Allows your location to communicate with the {{site.data.keyword.mon_full_notm}} API. | Cloud | One per location |
| `satellite-sysdigPrometheus-<location-ID>` | Allows your location to communicate with the {{site.data.keyword.mon_full_notm}} API. | Cloud | One per location |
| `openshift-api-<cluster_ID>` | Allows the {{site.data.keyword.openshiftlong_notm}} API to communicate with the master for the service cluster. By default, your {{site.data.keyword.openshiftlong_notm}} API {{site.data.keyword.satelliteshort}} link endpoints are protected to accept traffic from only the {{site.data.keyword.cloud_notm}} control plane. To access them, you must [create a source list](/docs/satellite?topic=satellite-link-endpoint-secure) for your endpoint to be accessible from other sources. | Location | One per {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service in your location |
{: caption="Default Link endpoints." caption-side="bottom"}

These endpoints are used to manage and update your location and are enabled by default. If you disable any of these endpoints, your client services that are running on your location can be negatively impacted. To avoid issues, do not disable these endpoints.
{: important}


## Default link endpoints for locations without CoreOS enabled
{: #non-rhcos-default-endpoints}

Default {{site.data.keyword.satelliteshort}} Link endpoints are created for your location's control plane cluster and for any other {{site.data.keyword.satelliteshort}}-enabled services that you run in your location. These default {{site.data.keyword.satelliteshort}} Link endpoints are accessible only from within the {{site.data.keyword.cloud_notm}} private network.
{: shortdesc}

The following table describes the Link endpoints that are automatically created in your {{site.data.keyword.satelliteshort}} location.

| Name | Description | Type | Instances |
| ---- | ----------- | ---- | --------- |
| `satellite-healthcheck-<location_ID>` | Allows the {{site.data.keyword.satelliteshort}} management plane to check the health of your location's control plane cluster. | Location | One per location |
| `satellite-containersApi` | Allows your {{site.data.keyword.satelliteshort}} location to communicate with the {{site.data.keyword.cloud_notm}} containers API. | Cloud | One per location |
| `satellite-cosCrossRegion-<location_ID>` | Allows the control plane data of your {{site.data.keyword.satelliteshort}} location to be backed up to your {{site.data.keyword.cos_full}} instance. management plane data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own. | Cloud | One per location |
| `satellite-cosRegional-<location_ID>` | Allows the control plane data of your {{site.data.keyword.satelliteshort}} location to be backed up to your {{site.data.keyword.cos_full}} instance. management plane data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own. | Cloud | One per location |
| `satellite-cosResConf-<location_ID>` | Allows the control plane data of your {{site.data.keyword.satelliteshort}} location to be backed up to your {{site.data.keyword.cos_full}} instance. management plane data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own. | Cloud | One per location |
| `satellite-iam-<location_ID>` | Allows requests to your {{site.data.keyword.satelliteshort}} location in {{site.data.keyword.cloud_notm}} to be authenticated and user actions to be authorized by Identity and Access Management (IAM). | Cloud | One per {{site.data.keyword.satelliteshort}} location |
| `satellite-kpRegional-<location_ID>` | Allows apps and services in the location to communicate with the {{site.data.keyword.keymanagementservicelong_notm}} service API | Cloud | One per location |
| `satellite-logdna-<location_ID>` | Allows logs for your {{site.data.keyword.satelliteshort}} location to be sent to your {{site.data.keyword.la_full}} instance. | Cloud | One per location |
| `satellite-logdnaapi-<location_ID>` | Allows your {{site.data.keyword.satelliteshort}} location to communicate with the {{site.data.keyword.la_full}} API. | Cloud | One per {{site.data.keyword.satelliteshort}} location |
| `satellite-sysdig-<location_ID>` | Allows metrics for your {{site.data.keyword.satelliteshort}} location to be sent to your {{site.data.keyword.mon_full}} instance. | Cloud | One per location |
| `satellite-sysdigapi-<location_ID>` | Allows your {{site.data.keyword.satelliteshort}} location to communicate with the {{site.data.keyword.mon_full_notm}} API. | Cloud | One per {{site.data.keyword.satelliteshort}} location |
| `openshift-api-<cluster_ID>` | Allows the {{site.data.keyword.openshiftlong_notm}} API to communicate with the master for the service cluster. By default, your {{site.data.keyword.openshiftlong_notm}} API {{site.data.keyword.satelliteshort}} link endpoints are protected to accept traffic from only the {{site.data.keyword.cloud_notm}} control plane. To access them, you must [create a source list](/docs/satellite?topic=satellite-link-endpoint-secure) for your endpoint to be accessible from other sources. | Location | One per {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service in your location |
{: caption="Default Link endpoints." caption-side="bottom"}

These endpoints are used to manage and update your location and are enabled by default. If you disable any of these endpoints, your client services that are running on your {{site.data.keyword.satelliteshort}} location can be negatively impacted. To avoid issues, do not disable these endpoints.
{: important}
