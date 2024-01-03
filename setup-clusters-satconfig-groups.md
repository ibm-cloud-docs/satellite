---


copyright:
  years: 2020, 2024
lastupdated: "2024-01-03"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Setting up cluster groups
{: #setup-clusters-satconfig-groups}

The cluster group specifies all clusters that you want to include in the deployment of your Kubernetes resources. The clusters can run in your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}.
{: shortdesc}

If you want to use the console to create {{site.data.keyword.satelliteshort}} configurations, you can create cluster groups as part of the configuration creation process. If you want to use the CLI to create {{site.data.keyword.satelliteshort}} configurations, you must create a cluster group first. Follow these steps to create a cluster group with the CLI:

1. List the clusters that are registered with the {{site.data.keyword.satelliteshort}} Config component and note their ID.
    ```sh
    ibmcloud sat cluster ls
    ```
    {: pre}

2. Add the cluster to your cluster group.    
    ```sh
    ibmcloud sat group attach --cluster <cluster_ID> --group <cluster_group_name>
    ```
    {: pre}

3. Verify that your cluster is successfully added to your cluster group.
    ```sh
    ibmcloud sat group get --group <cluster_group_name>
    ```
    {: pre}
