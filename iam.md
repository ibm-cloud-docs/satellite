---


copyright:
  years: 2020, 2024
lastupdated: "2024-01-03"

keywords: satellite, hybrid, multicloud, access, manage access

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Managing access overview
{: #iam}

Access to {{site.data.keyword.satellitelong}} service instances for users in your account is controlled by {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). Every user that accesses the {{site.data.keyword.satelliteshort}} service in your account must be assigned an access policy with an IAM role defined. The policy determines what actions that a user can perform within the context of the service or instance that you select. The allowable actions are customized and defined by the {{site.data.keyword.cloud_notm}} service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.
{: shortdesc}

The name for the {{site.data.keyword.satellitelong_notm}} service in IAM is
- **{{site.data.keyword.satellitelong_notm}}** in the UI
- **satellite** in the API and CLI

Keep in mind that you need permissions to {{site.data.keyword.cloud_notm}} services if you use the services with {{site.data.keyword.satelliteshort}}. For example, to create and manage clusters in your {{site.data.keyword.satelliteshort}} location, you must have the [appropriate permissions to {{site.data.keyword.openshiftlong_notm}}](/docs/openshift?topic=openshift-access_reference) in IAM (**Kubernetes Service** in the UI, **containers-kubernetes** in the API and CLI).
{: note}


## Locations and hosts
{: #iam-resource-loc}

Review details about the {{site.data.keyword.satelliteshort}} location IAM resource type, which includes actions for locations and hosts.
{: shortdesc}

If you scope an access policy to the `location` resource type, the users must target the regional endpoint to interact with the location. For more information, see the [troubleshooting topic](/docs/satellite?topic=satellite-ts-location-missing-location).
{: note}

Name of the resource type
:    UI: `Location`
:    API or CLI: `location`

Type of role that you can assign for the resource in IAM
:    Platform access **Viewer**, **Operator**, **Editor**, and **Administrator** roles
:    Custom service access role to create clusters, **{{site.data.keyword.satelliteshort}} Cluster Creator**

What you can scope an access policy for the resource to
:    Account
:    Resource group
:    Instances of the resource

Description
:    [Locations](/docs/satellite?topic=satellite-locations) are places that you use to extend {{site.data.keyword.cloud_notm}} by attaching your own host compute machines to the location. Access to the location resource lets users work with locations and hosts. However, location access does not grant access to other resources that run within the location, such as endpoints, configurations, or {{site.data.keyword.redhat_openshift_notm}} clusters.

## Configuration, subscription, cluster, cluster group, and resource
{: #iam-resource-config}

Review details about the {{site.data.keyword.satelliteshort}} Config IAM resource type, which includes actions for configurations, subscriptions, clusters, cluster groups, resources, and other components that use {{site.data.keyword.satelliteshort}} Config such as storage.
{: shortdesc}

Name of the resource type
:    Console: `Configuration`, `Subscription`, `Cluster`, `Clustergroup`, or `Resource`
:    API or CLI: `configuration`, `subscription`, `cluster`, `clustergroup`, or `resource`

Type of role that you can assign for the resource in IAM
:    Platform access **Viewer**, **Operator**, **Editor**, and **Administrator** roles
:    Service access **Reader**, **Writer**, and **Manager** roles, and a custom **Deployer** role

What you can scope an access policy for the resource to
:    Account
:    **Cluster** or **Clustergroup** only: Particular instance of the resource

Description
:    [{{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig) is a collection of configurations, versions, and subscriptions that you use to automatically deploy Kubernetes resources to groups of clusters that are registered with the {{site.data.keyword.satelliteshort}} Config component. However, access to {{site.data.keyword.satelliteshort}} Config does not give a user access to the clusters that run the Kubernetes resources of the configuration. You can scope access to the following {{site.data.keyword.satelliteshort}} Config resources.
     - **Configurations**, where you upload the version of the configuration file for the Kubernetes resources that you want to deploy. You cannot scope a policy to a particular configuration.
     - **Subscriptions**, which you use to specify the cluster group where you want to deploy the Kubernetes resource definition that you added as a version to your configuration. You cannot scope a policy to a particular configuration.
     - **Clusters** or **cluster groups**, which are {{site.data.keyword.openshiftlong_notm}} that are registered with {{site.data.keyword.satelliteshort}} Config and can be subscribed to configurations.
     - **Resources**, which are Kubernetes resources such as pods or services that are described in a {{site.data.keyword.satelliteshort}} Config and run in a subscribed cluster. Certain roles permit access to view and manage Kubernetes resources through {{site.data.keyword.satelliteshort}} Config, but you cannot scope an access policy to a particular resource.

## Link
{: #iam-resource-link}

Review details about the {{site.data.keyword.satelliteshort}} Link IAM resource type, which includes actions for endpoints and sources.
{: shortdesc}

Name of the resource type
:    UI: `Link`
:    API or CLI: `link`

Type of role that you can assign for the resource in IAM
:    Platform access **Viewer**, **Operator**, **Editor**, and **Administrator** roles
:    Custom **{{site.data.keyword.satelliteshort}} Link Administrator** and **{{site.data.keyword.satelliteshort}} Link Source Access Controller** service access roles

What you can scope an access policy for the resource to
:    Account
:    Resource group
:    Particular instances of the resource

Description
:    [Link endpoints](/docs/satellite?topic=satellite-link-location-cloud) connect services, servers, or apps that run in your {{site.data.keyword.satelliteshort}} location with an endpoint that runs in {{site.data.keyword.cloud_notm}}. Access to a {{site.data.keyword.satelliteshort}} Link does not give a user access to the resources that the endpoint connects, such as a location or service instance. Instead, the access is to manage the endpoint itself.

## Other services
{: #iam-resource-services}

Review details about other {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service IAM resource types, such as {{site.data.keyword.openshiftlong_notm}} clusters and other {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

Resource type, IAM role, and scope of access policies
:    Varies by service. For example, {{site.data.keyword.openshiftlong_notm}} is the **Kubernetes Service** in IAM and can scope access to cluster or namespace resources. For more information, consult the service documentation.

{{site.data.keyword.openshiftlong_notm}} clusters
:    You do not assign access policies for {{site.data.keyword.redhat_openshift_notm}} clusters in {{site.data.keyword.satelliteshort}}. Instead, access to clusters is assigned in {{site.data.keyword.cloud_notm}} IAM through {{site.data.keyword.openshiftlong_notm}} (**Kubernetes Service** in the console or `containers-kubernetes` in the API or CLI). For more information, see [Platform and service roles for {{site.data.keyword.redhat_openshift_notm}} clusters](#iam-roles-clusters).

:    If you have access to a {{site.data.keyword.satelliteshort}} location or configuration, you can view the clusters that are attached to the location or configuration. However, you might not be able to access the clusters if you do not have the appropriate roles to those clusters. For example, if you have the appropriate access to a {{site.data.keyword.satelliteshort}} configuration, you might be able to list all the Kubernetes resources that run in registered clusters through the {{site.data.keyword.satelliteshort}} Config API. However, without an access policy to the individual clusters, you cannot log in to the individual clusters and use {{site.data.keyword.redhat_openshift_notm}} APIs to list Kubernetes resources. For more information, see the following topics.
    - [Reference documentation](/docs/openshift?topic=openshift-access_reference) for user access permissions, including [platform](/docs/openshift?topic=openshift-access_reference) and [service](/docs/openshift?topic=openshift-access_reference) roles.
    - [Assigning access to clusters](/docs/openshift?topic=openshift-users), such as setting up the API key for underlying infrastructure permissions and granting users access with {{site.data.keyword.cloud_notm}} IAM.
    - [Accessing clusters](/docs/openshift?topic=openshift-access_cluster) on the public or private service endpoints, or by using an {{site.data.keyword.cloud_notm}} IAM API key such as for automation purposes.

Other managed services
:    To use {{site.data.keyword.satelliteshort}} with other [managed services](/docs/satellite?topic=satellite-managed-services), you must set up service to service access through IAM, with `Satellite` as your target service and the managed service as the source service.






### Platform and service roles for {{site.data.keyword.redhat_openshift_notm}} clusters
{: #iam-roles-clusters}

If you create {{site.data.keyword.openshiftlong_notm}} clusters to use in your {{site.data.keyword.redhat_openshift_notm}} locations, you manage access to these clusters in IAM for the {{site.data.keyword.redhat_openshift_notm}} service, not for {{site.data.keyword.redhat_openshift_notm}}. Review the following information to manage IAM access to {{site.data.keyword.redhat_openshift_notm}} clusters.
{: shortdesc}

- [Reference documentation](/docs/openshift?topic=openshift-access_reference) for user access permissions, including [platform](/docs/openshift?topic=openshift-access_reference) and [service](/docs/openshift?topic=openshift-access_reference) roles.
- [Assigning access to clusters](/docs/openshift?topic=openshift-users), such as setting up the API key for underlying infrastructure permissions and granting users access with {{site.data.keyword.cloud_notm}} IAM.
- [Accessing clusters](/docs/openshift?topic=openshift-access_cluster) on the public or private service endpoints, or by using an {{site.data.keyword.cloud_notm}} IAM API key such as for automation purposes.

## Common use cases and roles in {{site.data.keyword.cloud_notm}}
{: #iam-roles-usecases}

Wondering which access roles to assign to your {{site.data.keyword.satelliteshort}} access groups and users? Use the examples in the following table to determine which roles and scope to assign.
{: shortdesc}

| Use case | Example roles and scope |
| --- | --- |
| Creating a location | The user and the [API key that is set for the region and resource group](/docs/satellite?topic=satellite-iam-api-key) require the following permissions. **Administrator** platform role for all {{site.data.keyword.satelliteshort}} locations. The custom **{{site.data.keyword.satelliteshort}} Link Administrator** service role for {{site.data.keyword.satelliteshort}} Link. **Manager** service role to the {{site.data.keyword.cos_full_notm}} instance that backs up the location control plane data. To use automated templates such as to add hosts from AWS or Azure, the **Administrator** platform role for {{site.data.keyword.bplong_notm}} and **Administrator** platform role for Kubernetes Service. For other permissions to set up the location control plane, see [Permissions to create a cluster](/docs/openshift?topic=openshift-access_reference#cluster_create_permissions). |
| Creating a cluster in a location | See [Creating {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters). |
| Location auditor | **Viewer** platform role for the {{site.data.keyword.satelliteshort}} location and link endpoints. **Reader** service role for the configuration resources in the location. **Reader** service role to the {{site.data.keyword.cos_full_notm}} instance that backs up the location control plane data. |
| App developers | **Viewer** platform role for the {{site.data.keyword.satelliteshort}} location. **Writer** or **Deployer** service access role for the configuration resources. **Editor** platform role and **Writer** service role to {{site.data.keyword.redhat_openshift_notm}} clusters or particular projects in a cluster.|
| Billing | **Viewer** platform role for all the {{site.data.keyword.satelliteshort}} locations in the account. |
| Location administrator | **Administrator** platform role for the location and link resources. **Administrator** platform role to {{site.data.keyword.redhat_openshift_notm}} clusters. **Manager** service role to the {{site.data.keyword.cos_full_notm}} instance that backs up the location control plane data.|
| DevOps operator | **Editor** platform role for the location and link resources. **Deployer** service role for the configurations. **Operator** platform role to {{site.data.keyword.redhat_openshift_notm}} clusters.|
| Operator or site reliability engineer | **Administrator** platform role for the location and link resources. **Manager** service role for the configuration resources. **Administrator** platform role and **Manager** service role to {{site.data.keyword.redhat_openshift_notm}} clusters. |
{: caption="Types of roles you might assign to meet different use cases." caption-side="bottom"}







