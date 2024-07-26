---


copyright:
  years: 2022, 2024
lastupdated: "2024-07-26"

keywords: satellite, subscription, identity, satellite config, satellite configuration

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.satelliteshort}} Config subscription identity and authorization
{: #sat-config-subid}

When you use {{site.data.keyword.satelliteshort}} Config to apply Kubernetes resources to a managed cluster or cluster group, the resources are applied according to the permissions available to the subscription identity. 
{: shortdesc}

## Subscription identity
{: #sat-config-subid-exp}

The subscription identity is the user ID or service ID that creates the subscription. When a user creates a subscription to apply Kubernetes resources, the **Location clusters** resources are applied on each cluster as that user and according to that user's permissions.

When Kubernetes resources are applied by {{site.data.keyword.satelliteshort}} Config to managed clusters, they are applied by a different identity depending on how the cluster was attached.

- For clusters that are manually registered with {{site.data.keyword.satelliteshort}} Config, Kubernetes resources are always applied to the cluster by the `razeedeploy-sa` service account.

- Clusters in a {{site.data.keyword.satelliteshort}} Location are automatically configured for use by {{site.data.keyword.satelliteshort}} Config. Kubernetes resources are applied to the cluster by the identity (user or service ID) that created the subscription.

If the subscription identity uses a user ID that does not have permissions to create certain resources in certain namespaces, the rollout might fail. Additionally, if the subscription owner loses access to a cluster, such as by leaving the organization, the Satellite Config operators still attempt to apply or delete child resources as that user. These actions fail with a `403 not authorized` error. For steps to resolve this issue, see [Why does my {{site.data.keyword.satelliteshort}} Config rollout fail and result in a `not authorized` error?](/docs/satellite?topic=satellite-ts-satconfig-subid-perms).
{: important}

### Viewing the cluster's subscription identity
{: #sat-config-subid-view}

You can view a cluster's subscription identity by navigating to the **Subscription details** page in the UI or by running **`ibmcloud sat subscription get`** in the CLI. 

## Syncing the subscription identity with the cluster
{: #sat-config-subid-sync}

The subscription identity syncs to a cluster so that the cluster recognizes the associated user ID or service ID and the permissions assigned to the ID. When an ID is synced to every cluster in a cluster group, the {{site.data.keyword.satelliteshort}} Config rollout can complete successfully across all clusters because the user has the same permissions for each cluster. 

The subscription identity can sync automatically with each cluster when a user completes certain actions. However, other actions can prevent identity syncing from occuring automatically in certain clusters. Generally, actions that result in automatic syncing are those that involve a subscription, and actions that prevent automatic syncing are those that affect the cluster group membership *and* are made by a user whose ID is not used as the subscription ID. For example, creating a new subscription or editing an existing subscription updates the subscription identity and syncs it to all clusters. However if a user whose ID is not used as the subscription ID makes a change to the cluster group, such as adding or removing a cluster from the group, then the subscription identity cannot automatically sync to any new clusters that are later added to the group. This can result in some clusters not syncing with the correct subscription identity.  

When clusters are not correctly synced, the {{site.data.keyword.satelliteshort}} Config rollout cannot complete successfully because not all clusters can read the permissions available to the subscription identity (the ID of the user who created or edited the subscription). You can check for this scenario by running **`ibmcloud sat subscription get`** to check the subscription identity for each cluster. For steps to resolve this issue, see [Why does my {{site.data.keyword.satelliteshort}} Config rollout fail and result in a `not authorized` error?](/docs/satellite?topic=satellite-ts-satconfig-subid-perms).

