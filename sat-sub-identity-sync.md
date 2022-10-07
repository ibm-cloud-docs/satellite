---

copyright:
  years: 2022, 2022
lastupdated: "2022-10-07"

keywords: satellite, subscription, identity, satellite config, satellite configuration

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Syncing subscription identity
{: #sat-sync-sub-identity}

After you create a {{site.data.keyword.satelliteshort}} configuration, you might need to sync the subscription identify for the rollout to be successful.  The subscription identity is the user or service ID that is used by the subscription.
{: shortdesc}

When Kubernetes resources are applied by {{site.data.keyword.satelliteshort}} Config to managed clusters, they are applied by a different identity depending on how the cluster was attached.

- For clusters that are manually registered with {{site.data.keyword.satelliteshort}} Config, Kubernetes resources are always applied to the cluster by the `razeedeploy-sa` service account.
- Clusters in a {{site.data.keyword.satelliteshort}} Location are automatically configured for use by {{site.data.keyword.satelliteshort}} Config. Kubernetes resources are applied to the cluster by the identity (user or service ID) that created the Subscription.

In short, if a user creates a subscription to apply Kubernetes resources, the **Location clusters** resources are applied as that user and following that user's permissions on each cluster. If that user does not have permissions to create certain resources in certain namespaces, the rollout might fail.

The subscription identity is shown in the **Subscription details** page in the UI and is also retrievable through the CLI by using the `**ibmcloud sat subscription get`** command.

Identity sync is triggered and happens automatically in most cases:

- When a user creates a subscription, all the clusters in the subscription's groups automatically sync.
- When a user modifies the membership of a cluster group and that user's identity is used by any subscriptions that are applied to the group, all the clusters in that group sync automatically.
- When a user logs into the cluster by running `ibmcloud ks cluster config` or by launching the cluster dashboard from the IBM Cloud UI, that cluster does not sync automatically.

Automatic identity sync can only be performed by the user or service ID being synced. If the user whose identity is used by the subscription makes a change, their identity syncs automatically to affected clusters. If a user different from the subscription identity makes a change, the subscription identity cannot sync automatically.
{: note}

As a result, in some cases the identities that are synced to each cluster might become incorrect:

- If a different user or service ID modifies the subscription's cluster groups, the subscription's identity cannot automatically sync to any new clusters.
- If a different user changes the membership of the cluster groups, the subscription's identity cannot sync automatically to any new clusters.

If the identity used is not available on the Location cluster, potentially causing rollout failure, you can resolve this problem in three ways:
1. The user associated with the subscription can explicitly request re-sync of their identity. This sync can be done from the {{site.data.keyword.cloud_notm}} console (a subscription action) or CLI (**`ibmcloud sat subscription identity set`**).
2. A different user can edit the Subscription to use their identity, also triggering the sync of their identity to appropriate clusters. This sync can be done from the {{site.data.keyword.cloud_notm}} console (a subscription action) or CLI (**`ic sat subscription identity set`**).
3. The user associated with the subscription can trigger sync of their identity to a single cluster by running **`ibmcloud ks cluster config`** or connecting to the cluster's dashboard from the {{site.data.keyword.cloud_notm}} console.
