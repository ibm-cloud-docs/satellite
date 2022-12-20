---
copyright:
  years: 2020, 2022
lastupdated: "2022-12-20"

keywords: satellite storage, netapp, trident, ontap, satellite config, satellite configurations,

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# NetApp Trident Operator
{: #storage-netapp-trident}

Set up [NetApp Trident storage](https://netapp-trident.readthedocs.io/en/stable-v20.07/){: external} for {{site.data.keyword.satellitelong}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

You must deploy the NetApp Trident template to your clusters before you can create configurations with the NetApp ONTAP-NAS or NetApp ONTAP-SAN templates.
{: important}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}

## Prerequisites for NetApp Trident
{: #sat-storage-netapp-trident-prereq}

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).





### Removing the NetApp Trident storage assignment and configuration from the console
{: #netapp-trident-template-rm-ui}
{: ui}

Use the console to remove a storage assignment and storage configuration.

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**
1. Enter the name of your storage configuration.
1. Select **Delete**.

### Removing the NetApp Trident storage assignment and configuration from the CLI
{: #netapp-trident-template-rm-cli}
{: cli}

Use the CLI to remove a storage assignment and storage configuration.
{: shortdesc}

1. List your storage assignments and find the one that you used for your cluster.
    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
    ```
    {: pre}

2. Remove the assignment. After the assignment is removed, the NetApp Trident driver pods and storage class are removed from all clusters that were part of the storage assignment.
    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment_ID>
    ```
    {: pre}

3. List the pods in the `trident` namespace and verify that the NetApp Trident storage driver pods are removed.
    ```sh
    oc get pods -n trident
    ```
    {: pre}

4. Verify that the `trident` namespace is removed.
    ```sh
    oc get ns
    ```
    {: pre}

    If the `trident` namespace is stuck in `Terminating` status, follow the steps to [remove the namespace](/docs/satellite?topic=satellite-storage-namespace-terminating). Alternatively, you can follow the NetApp documentation for [uninstalling the Trident operator](https://netapp-trident.readthedocs.io/en/stable-v20.07/kubernetes/operations/tasks/managing.html#uninstalling-with-the-trident-operator){: external}
    {: note}

5. **Optional**: List your storage configurations and remove your Trident configuration.
    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

    ```sh
    ibmcloud sat storage config rm --config <config_name>
    ```
    {: pre}

## Getting help and support for NetApp Trident
{: #sat-trident-support}

If you run into an issue with using NetApp Trident, you can visit the [NetApp support page](https://netapp-trident.readthedocs.io/en/stable-v20.04/support/support.html){: external}. 


