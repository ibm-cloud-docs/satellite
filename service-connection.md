---

copyright:
  years: 2020, 2022
lastupdated: "2022-06-10"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Securing your connection
{: #service-connection}

With {{site.data.keyword.satellitelong_notm}}, you bring {{site.data.keyword.cloud_notm}} to your own infrastructure environment by creating a {{site.data.keyword.satelliteshort}} location. This setup means that you do not need [{{site.data.keyword.cloud_notm}} service endpoints](/docs/account?topic=account-service-endpoints-overview) to access {{site.data.keyword.cloud_notm}}. Instead, {{site.data.keyword.cloud_notm}} needs a {{site.data.keyword.satelliteshort}} Link endpoint to access your infrastructure environment. You can access services in your {{site.data.keyword.satelliteshort}} location by creating {{site.data.keyword.satelliteshort}} Link endpoints, using the cluster URL, or creating a route or similar service for workloads in a cluster.
{: shortdesc}

## Access to resources that run in your {{site.data.keyword.satelliteshort}} location
{: #user-access}

You can access the resources that run in your {{site.data.keyword.satelliteshort}} location in several ways, depending on what users need to access: service-instance clusters in your {{site.data.keyword.satelliteshort}} location, a resource in your {{site.data.keyword.satelliteshort}} location from the {{site.data.keyword.IBM_notm}} private network, or an application workload in a cluster in your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

### Service-instance clusters
{: #user-access-service}

A cluster service URL is automatically created for any [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services) that you run in your location, such as a {{site.data.keyword.openshiftlong_notm}} cluster. These URLs allow you to access your {{site.data.keyword.cloud_notm}} service that runs in your location over the public network or from within your hosts' private network, depending on whether your location hosts have public and private or private only connectivity.
{: shortdesc}

For example, when you create an {{site.data.keyword.satellitelong_notm}} cluster, the cluster is accessible through a URL that consists one of the subdomains for your location and a port, such as `https://pacfd8bdae2d04696301d-6b64a6ccc9c596bf59a86625d8fa2202-ce00.us-east.satellite.appdomain.cloud:32200`. When you access your cluster, such as by using the `ibmcloud oc cluster config --cluster <cluster_name_or_ID> --admin` command or by getting a login token from the {{site.data.keyword.redhat_openshift_notm}} web console, this URL is automatically used for your connection to the cluster master. Note that if you use hosts that have private network connectivity only for your location, you must be connected to your hosts' private network, such as through VPN access, to connect to your cluster and access the {{site.data.keyword.redhat_openshift_notm}} web console.

For more information about connecting to services that run in your {{site.data.keyword.satelliteshort}} location by using the cluster service URL, see the documentation for that service, such as the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat).

### {{site.data.keyword.IBM_notm}} private network access with {{site.data.keyword.satelliteshort}} Link
{: #user-access-loc-ep}

If you have a resource on the {{site.data.keyword.IBM_notm}} private network that requires access to your {{site.data.keyword.satelliteshort}} location, you can [create a `location` endpoint in {{site.data.keyword.satelliteshort}} Link](/docs/satellite?topic=satellite-link-cloud-create#link-location).
{: shortdesc}

### Application workloads that run in clusters
{: #user-access-apps}

To make your apps available, see the options for [Exposing apps in {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-sat-expose-apps).
{: shortdesc}

## {{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satelliteshort}} location
{: #ibm-cloud-access}

Default {{site.data.keyword.satelliteshort}} Link endpoints are created for your location's control plane cluster and for any other {{site.data.keyword.satelliteshort}}-enabled services that you run in your location. These default {{site.data.keyword.satelliteshort}} Link endpoints are accessible only from within the {{site.data.keyword.cloud_notm}} private network.
{: shortdesc}

The following table describes the Link endpoints that are automatically created in your {{site.data.keyword.satelliteshort}} location.

| Name | Description | Type | Instances |
| ---- | ----------- | ---- | --------- |
| `satellite-healthcheck-<location_ID>` | Allows the {{site.data.keyword.satelliteshort}} control plane master to check the health of your location's control plane cluster. | Location | One per location |
| `satellite-containersApi` | Allows your {{site.data.keyword.satelliteshort}} location to communicate with the {{site.data.keyword.cloud_notm}} containers API. | Cloud | One per location |
| `satellite-cosCrossRegion-<location_ID>` | Allows the control plane data of your {{site.data.keyword.satelliteshort}} location to be backed up to your {{site.data.keyword.cos_full}} instance. Control plane master data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own. | Cloud | One per location |
| `satellite-cosRegional-<location_ID>` | Allows the control plane data of your {{site.data.keyword.satelliteshort}} location to be backed up to your {{site.data.keyword.cos_full}} instance. Control plane master data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own. | Cloud | One per location |
| `satellite-cosResConf-<location_ID>` | Allows the control plane data of your {{site.data.keyword.satelliteshort}} location to be backed up to your {{site.data.keyword.cos_full}} instance. Control plane master data is backed up by {{site.data.keyword.IBM_notm}} and stored in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cos_short}} instance. {{site.data.keyword.satelliteshort}} cluster master data is backed up to the {{site.data.keyword.cos_short}} instance that you own. | Cloud | One per location |
| `satellite-iam-<location_ID>` | Allows requests to your {{site.data.keyword.satelliteshort}} location in {{site.data.keyword.cloud_notm}} to be authenticated and user actions to be authorized by Identity and Access Management (IAM). | Cloud | One per {{site.data.keyword.satelliteshort}} location |
| `satellite-kpRegional-<location_ID>` | Allows apps and services in the location to communicate with the {{site.data.keyword.keymanagementservicelong_notm}} service API | Cloud | One per location |
| `satellite-logdna-<location_ID>` | Allows logs for your {{site.data.keyword.satelliteshort}} location to be sent to your {{site.data.keyword.la_full}} instance. | Cloud | One per location |
| `satellite-logdnaapi-<location_ID>` | Allows your {{site.data.keyword.satelliteshort}} location to communicate with the {{site.data.keyword.la_full}} API. | Cloud | One per {{site.data.keyword.satelliteshort}} location |
| `satellite-sysdig-<location_ID>` | Allows metrics for your {{site.data.keyword.satelliteshort}} location to be sent to your {{site.data.keyword.mon_full}} instance. | Cloud | One per location |
| `satellite-sysdigapi-<location_ID>` | Allows your {{site.data.keyword.satelliteshort}} location to communicate with the {{site.data.keyword.mon_full_notm}} API. | Cloud | One per {{site.data.keyword.satelliteshort}} location |
| `openshift-api-<cluster_ID>` | Allows the {{site.data.keyword.openshiftlong_notm}} API to communicate with the master for the service cluster. | Location | One per {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service in your location |
{: caption="Default Link endpoints." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the name of the default endpoint. The second column describes what the endpoint is for. The third column describes how many instances of the endpoint are created and for which component the endpoint is created."}

These endpoints are used to manage and update your location and are enabled by default. If you disable any of these endpoints, your client services that are running on your {{site.data.keyword.satelliteshort}} location can be negatively impacted. To avoid issues, do not disable these endpoints.
{: important}

For more information about {{site.data.keyword.satelliteshort}} Link endpoints and what kinds of access {{site.data.keyword.cloud_notm}} has to your {{site.data.keyword.satelliteshort}} location, see [Connecting {{site.data.keyword.satelliteshort}} locations with external services using Link endpoints](/docs/satellite?topic=satellite-link-location-cloud).


