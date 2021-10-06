---
copyright:
  years: 2020, 2021
lastupdated: "2021-10-06"

keywords: satellite storage, netapp, trident, ontap, satellite config, satellite configurations,

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# NetApp Trident Operator
{: #config-storage-netapp-trident}

Set up [NetApp Trident storage](https://netapp-trident.readthedocs.io/en/stable-v20.07/){: external} for {{site.data.keyword.satelliteshort}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

You must deploy the NetApp Trident template to your clusters before you can create configurations with the NetApp ONTAP-NAS or NetApp ONTAP-SAN templates.
{: important}




## Creating a NetApp Trident storage configuration in the command line
{: #sat-storage-netapp-cli}

1. Log in to the {{site.data.keyword.cloud_notm}} CLI.

    ```sh
    ibmcloud login
    ```
    {: pre}

1. List your {{site.data.keyword.satelliteshort}} locations and note the `Managed from` column.

    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

1. Target the `Managed from` region of your {{site.data.keyword.satelliteshort}} location. For example, for `wdc` target `us-east`. For more information, see [{{site.data.keyword.satelliteshort}} regions](/docs/satellite?topic=satellite-sat-regions).

    ```sh
    ibmcloud target -r us-east
    ```
    {: pre}

1. If you use a resource group other than `default`, target it.

    ```sh
    ibmcloud target -g <resource-group>
    ```
    {: pre}
    
1. List the available templates and versions and review the output. Make a note of the template and version that you want to use.

    ```sh
    ibmcloud sat storage template ls
    ```
    {: pre}
    
1. Run the following command to create a NetApp Trident configuration. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
    ```sh
    ibmcloud sat storage config create --name <config_name> --location <location> --template-name netapp-trident --template-version <template_version>
    ```
    {: pre}

1. Verify that your storage configuration is created.
    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}

## Assigning your NetApp storage configuration to a cluster
{: #assign-storage-netapp}

After you [create a {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-netapp-trident), you can assign you configuration to your {{site.data.keyword.satelliteshort}} clusters.




### Assigning a storage configuration in the command line
{: #assign-storage-netapp-cli}

1. List your {{site.data.keyword.satelliteshort}} storage configurations and make a note of the storage configuration that you want to assign to your clusters.
    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

1. Get the ID of the cluster or cluster group that you want to assign storage to. To make sure that your cluster is registered with {{site.data.keyword.satelliteshort}} Config or to create groups, see [Setting up clusters to use with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
    - Group
      ```sh
      ibmcloud sat group ls
      ```
      {: pre}

    - Cluster
      ```sh
      ibmcloud oc cluster ls --provider satellite
      ```
      {: pre}

    - {{site.data.keyword.satelliteshort}}-enabled service cluster
      ```sh
      ibmcloud sat service ls --location <location>
      ```
      {: pre}

1. Assign storage to the cluster or group that you retrieved in step 2. Replace `<group>` with the ID of your cluster group or `<cluster>` with the ID of your cluster. Replace `<config>` with the name of your storage config, and `<name>` with a name for your storage assignment. For more information, see the `ibmcloud sat storage assignment create` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create).

    - Group
      ```sh
      ibmcloud sat storage assignment create --group <group> --config <config> --name <name>
      ```
      {: pre}

    - Cluster
      ```sh
      ibmcloud sat storage assignment create --cluster <cluster> --config <config> --name <name>
      ```
      {: pre}

    - {{site.data.keyword.satelliteshort}}-enabled service cluster
      ```sh
      ibmcloud sat storage assignment create --service-cluster-id <cluster> --config <config> --name <name>
      ```
      {: pre}

1. Verify that your assignment is created.
    ```sh
    ibmcloud sat storage assignment ls (--cluster <cluster_id> | --service-cluster-id <cluster_id>) | grep <storage-assignment-name>
    ```
    {: pre}

5. Verify that the storage configuration resources are deployed.
    ```sh
    oc get pods -n trident
    ```
    {: pre}

    Example output
    ```sh
    NAME                                READY   STATUS    RESTARTS   AGE
    trident-csi-2nrt4                   2/2     Running   0          87s
    trident-csi-7f999bfb96-z4dr5        6/6     Running   0          87s
    trident-csi-cd5mx                   2/2     Running   0          87s
    trident-csi-zlwwn                   2/2     Running   0          87s
    trident-operator-794f74cd4b-zpnt4   1/1     Running   0          94s
    ```
    {: screen}


### Removing the NetApp Trident storage assignment and configuration from the CLI
{: #netapp-trident-template-rm-cli}

Use the CLI to remove a storage assignment and storage configuration.
{: shortdesc}

1. List your storage assignments and find the one that you used for your cluster.
    ```sh
    ibmcloud sat storage assignment ls (--cluster <cluster_id> | --service-cluster-id <cluster_id>)
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
