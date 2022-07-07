---
copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: satellite storage, netapp, trident, ontap, satellite config, satellite configurations,

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# NetApp Trident Operator
{: #config-storage-netapp-trident}

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

## Creating a NetApp Trident storage configuration from the console
{: #sat-storage-netapp-ui}

1. From the {{site.data.keyword.satelliteshort}} locations dashboard, select the location where you want to create a storage configuration.
1. Select **Storage** > **Create storage configuration**
1. Enter a name for your configuration.
1. Select the **Storage type** that you want to use to create your configuration and the **Version**.
1. On the **Parameters** tab, enter the parameters for your configuration.
1. On the **Secrets** tab, enter the secrets, if required, for your configuration.
1. On the **Storage classes** tab, review the storage classes that are deployed by the configuration or create a custom storage class.
1. On the **Assign to service** tab, select the service that you want to assign your configuration to.
1. Click **Complete** to assign your storage configuration.

## Creating a NetApp Trident storage configuration in the command line
{: #sat-storage-netapp-cli}
{: cli}

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



### Assigning a NetApp Trident storage configuration in the console
{: #assign-storage-netapp-ui}
{: ui}


1. Open the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} in your browser.
1. Select the location where you want to create a storage configuration.
1. Click the **Configurations** tab and click the storage configuration that you want to assign to a cluster group.
1. On the **Configuration details** page, click **Create subscription**.
1. In the **Create a subscription** pane, enter a name for your subscription. When you create a subscription you assign your storage configuration to your clusters.
1. From the **Version** drop-down list, select the storage configuration version that you want to assign.
1. From the **Cluster group** drop-down list, select the cluster group that you want to assign to the storage configuration. Note that the clusters in your cluster group where you want to assign storage must all be in the same {{site.data.keyword.satelliteshort}} location.
1. Click **Create** to create the subscription.
1. Verify that your storage configuration is deployed to your cluster. 
    1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, navigate to the **Configurations** tab.
    1. Click the storage configuration that you created and review the **Subscriptions** tab.
    1. Click the **Subscription** that you created and review the **Rollout status** for your configuration.



### Assigning a NetApp Trident storage configuration in the command line
{: #assign-storage-netapp-cli}
{: cli}

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

    - {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster
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

    - {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster
      ```sh
      ibmcloud sat storage assignment create --service-cluster-id <cluster> --config <config> --name <name>
      ```
      {: pre}

1. Verify that your assignment is created.
    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER) | grep <storage-assignment-name>
    ```
    {: pre}

1. Verify that the storage configuration resources are deployed.
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

## Upgrading a NetApp Trident storage configuration
{: #netapp-trident-upgrade-config}
{: cli}

You can upgrade your {{site.data.keyword.satelliteshort}} storage configurations to use the latest storage template revision within the same major version. 

1. List your {{site.data.keyword.satelliteshort}} storage configurations, make a note of the {{site.data.keyword.satelliteshort}} configuration you want to upgrade.
    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

1. Upgrade the {{site.data.keyword.satelliteshort}} configuration. Note, only the configuration is updated. If you want to upgrade the assignments that use this configuration, you can specify the `--include-assignments` option or you can manually update each assignment using the `assignment update` command.
    ```sh
    ibmcloud sat storage config upgrade --config CONFIG [--include-assignments]
    ```
    {: pre}

## Upgrading a NetApp Trident storage assignment
{: #netapp-trident-upgrade-assignment}
{: cli}

You can use the `storage assignment upgrade` command to upgrade an assignment to the latest version of the storage configuration it uses. 

1. List your {{site.data.keyword.satelliteshort}} storage assignments, make a note of the {{site.data.keyword.satelliteshort}} assignment you want to upgrade.
    ```sh
    ibmcloud sat storage assignment ls
    ```
    {: pre}

1. List the {{site.data.keyword.satelliteshort}} storage templates to see the latest available versions.
    ```sh
    ibmcloud sat storage template ls
    ```
    {: pre}

1. Upgrade the {{site.data.keyword.satelliteshort}} assignment.
    ```sh
   ibmcloud sat storage assignment upgrade --assignment ASSIGNMENT
    ```
    {: pre}

## Updating a NetApp Trident storage assignment
{: #netapp-trident-update-assignment}
{: cli}

You can use the `storage assignment update` command to rename your assignment or assign it to a new cluster or cluster group. 

1. List your {{site.data.keyword.satelliteshort}} storage assignments, make a note of the {{site.data.keyword.satelliteshort}} assignment you want to update and the clusters or cluster groups included in the assignment.
    ```sh
    ibmcloud sat storage assignment ls
    ```
    {: pre}

1. Update the {{site.data.keyword.satelliteshort}} assignment. 
    ```sh
    ibmcloud sat storage assignment update --assignment ASSIGNMENT [--group GROUP ...] [--name NAME]
    ```
    {: pre}

    Example command to update assignment name and assign different cluster groups.
    
    ```sh
    ibmcloud sat storage assignment update --assignment ASSIGNMENT --name new-name --group group-1 --group group-2 --group group-3
    ```
    {: pre}

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


