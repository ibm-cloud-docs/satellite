---
copyright:
  years: 2020, 2022
lastupdated: "2022-12-14"

keywords: satellite storage, netapp, trident, ontap, satellite config, satellite configurations,

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# NetApp ONTAP-SAN
{: #config-storage-netapp-2104}

Set up [NetApp ONTAP-SAN storage](https://netapp-trident.readthedocs.io/en/stable-v21.04/){: external} for {{site.data.keyword.satellitelong}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

Before you can create storage configurations by using the NetApp NAS template, you must deploy the [NetApp Trident template](/docs/satellite?topic=satellite-config-storage-netapp-trident), which installs the required operator.
{: important}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}

## Prerequisites for NetApp ONTAP-SAN storage
{: #netapp-san-2104-pre}

Review the following prerequisites before you deploy the NetApp ONTAP-SAN drivers to your {{site.data.keyword.satelliteshort}} clusters.
{: shortdesc}


1. You must configure your backend ONTAP cluster as a Trident backend.
1. You must have a dedicated Storage Virtual Machine (SVM) for Trident. Volumes and LUNs that are created by Trident are created in this SVM.
1. You must have one or more aggregates assigned to the SVM. You can add aggregates by running the `netapp1::> vserver modify -vs <svm_name> -aggr-list <aggregate(s)_to_be_added>` command.
1. You must have one or more `dataLIFs` for the SVM.
1. You must have iSCSI services enabled on the SVM.
1. You must set up a snapshot policy on the SVM.
1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
1. [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/satellite?topic=openshift-satellite-clusters). 
    - Your cluster must meet the requirements for ONTAP-SAN. For more information, see the [NetApp documentation](https://netapp-trident.readthedocs.io/en/stable-v21.04/support/requirements.html).
    - Your hosts must meet the [{{site.data.keyword.satelliteshort}} host requirements](/docs/satellite?topic=satellite-host-reqs) in addition to the requirements for ONTAP-SAN.
1. [Add your {{site.data.keyword.satelliteshort}} to a cluster group](/docs/satellite?topic=satellite-setup-clusters-satconfig-groups).
1. [Set up {{site.data.keyword.satelliteshort}} Config on your clusters](/docs/satellite?topic=satellite-setup-clusters-satconfig).



## Creating a NetApp Trident SAN storage configuration
{: #sat-storage-nettapp-san-2104}

You can use the [console](#sat-storage-netapp-ui-san-2104) or [CLI](#sat-storage-netapp-cli-san-2104) to create a NetApp Trident SAN storage configuration in your location and assign the configuration to your clusters.
{: shortdesc}

### Creating a NetApp Trident SAN storage configuration in the console
{: #sat-storage-netapp-ui-san-2104}
{: ui}

1. From the {{site.data.keyword.satelliteshort}} locations dashboard, select the location where you want to create a storage configuration.
1. Select **Storage** > **Create storage configuration**
1. Enter a name for your configuration.
1. Select the **Storage type** that you want to use to create your configuration and the **Version**.
1. On the **Parameters** tab, enter the parameters for your configuration.
1. On the **Secrets** tab, enter the secrets, if required, for your configuration.
1. On the **Storage classes** tab, review the storage classes that are deployed by the configuration or create a custom storage class.
1. On the **Assign to service** tab, select the service that you want to assign your configuration to.
1. Click **Complete** to assign your storage configuration.


### Creating a NetApp Trident SAN storage configuration in the command line
{: #sat-storage-netapp-cli-san-2104}
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
    
1. Verify that the `netapp-trident` [driver](/docs/satellite?topic=satellite-config-storage-netapp-trident) is deployed on your clusters.
1. Review the template parameters and retrieve the values from your NetApp cluster.
1. Review the [NetApp Trident storage configuration parameters](#netapp-ontap-san-parameter-reference).
1. Copy the following the command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `-p "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).



    Example command to create a version 21.04 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name netapp-ontap-san --template-version 21.04  --param "managementLIF=MANAGEMENTLIF"   --param "dataLIF=DATALIF"   --param "svm=SVM"   --param "username=USERNAME"   --param "password=PASSWORD"   [--param "limitVolumeSize=LIMITVOLUMESIZE"]   [--param "limitAggregateUsage=LIMITAGGREGATEUSAGE"] 
    ```
    {: pre}


    Example command to create a version 22.04 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name netapp-ontap-san --template-version 22.04  --param "managementLIF=MANAGEMENTLIF"   --param "dataLIF=DATALIF"   --param "svm=SVM"   --param "username=USERNAME"   --param "password=PASSWORD"   [--param "limitVolumeSize=LIMITVOLUMESIZE"]   [--param "limitAggregateUsage=LIMITAGGREGATEUSAGE"] 
    ```
    {: pre}


1. Verify that your storage configuration is created.
    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}
    



## Assigning your NetApp storage configuration to a cluster
{: #assign-storage-netapp-san-2104}

After you [create a {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-netapp-2104), you can assign your configuration to your {{site.data.keyword.satelliteshort}} clusters.


### Assigning a NetApp storage configuration in the console
{: #assign-storage-netapp-ui-san-2104}
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


### Assigning a NetApp storage configuration in the command line
{: #assign-storage-netapp-cli-san-2104}
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
    oc get pods -A | grep <template-name>
    ```
    {: pre}


## Upgrading a NetApp storage configuration
{: #sat-storage-netapp-san-2104-upgrade-config}
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

## Upgrading a NetApp storage assignment
{: #sat-storage-netapp-san-2104-upgrade-assignment}
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

## Updating a NetApp storage assignment
{: #sat-storage-netapp-san-2104-update-assignment}
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


## Parameter reference
{: #netapp-ontap-san-parameter-reference}

### 21.04 parameter reference
{: #21.04-parameter-reference}

| Display name | CLI option | Description | Required? |
| --- | --- | --- | --- |
| Management LIF | `managementLIF` | The IP address of the Management LIF. | true | 
| Data LIF | `dataLIF` | The IP address of the Data LIF. | true | 
| SVM | `svm` | The name of the SVM. | true | 
| User Name | `username` | The username to connect to the storage device. | true | 
| User Password | `password` | The password to connect to the storage device. | true | 
| Limit Volume Size | `limitVolumeSize` | The maximum volume size (in Gibibytes) that can be requested and the qtree parent volume size. | false | 
| Limit AggregateUsage | `limitAggregateUsage` | Provisioning fails if usage is above this percentage. | false | 
{: caption="Table 1. 21.04 parameter reference" caption-side="bottom"}


### 22.04 parameter reference
{: #22.04-parameter-reference}

| Display name | CLI option | Description | Required? |
| --- | --- | --- | --- |
| Management LIF | `managementLIF` | The IP address of the Management LIF. | true | 
| Data LIF | `dataLIF` | The IP address of the Data LIF. | true | 
| SVM | `svm` | The name of the SVM. | true | 
| User Name | `username` | The username to connect to the storage device. | true | 
| User Password | `password` | The password to connect to the storage device. | true | 
| Limit Volume Size | `limitVolumeSize` | The maximum volume size (in Gibibytes) that can be requested and the qtree parent volume size. | false | 
| Limit AggregateUsage | `limitAggregateUsage` | Provisioning fails if usage is above this percentage. | false | 
{: caption="Table 2. 22.04 parameter reference" caption-side="bottom"}


## Storage class reference for NetApp Trident
{: #netapp-sc-reference-san-2104}

Before you deploy apps that use the `sat-netapp` storage classes, review the following notes.

By default, the `sat-netapp-file-gold` storage class doesn't include any QoS limits (unlimited IOPS).
{: note}

To use the `sat-netapp-file-silver` and `sat-netapp-file-bronze` storage classes, you must create corresponding `silver` and `bronze` QoS policy groups on the storage controller and define the QoS limits. To create a policy group on the storage system, log in to the system CLI and run the `netapp1::> qos policy-group create -policy-group <policy_group_name> -vserver <svm_name> [-min-throughput <min_IOPS>] -max-throughput <max_IOPS>` command.
{: note}

The **min-throughput** options is supported only on all-flash systems. For more information about creating and managing QoS Policy groups, see the [ONTAP 9 Storage Management documentation](https://docs.netapp.com/us-en/ontap/index.html){: external}.
{: note}

To use an **encrypted** storage class, NetApp Volume Encryption (NVE) must be enabled on your storage system by using either the NetApp ONTAP onboard key manager or a supported (off-box) third-party key manager, such as {{site.data.keyword.IBM_notm}} 's TKLM key manager. To enable the onboard key manager, run the `netapp1::> security key-manager onboard enable` command. For more information about configuring encryption, see the [ONTAP 9 Security and Data Encryption documentation](https://docs.netapp.com/us-en/ontap/security-encryption/index.html){: external}.
{: note}

Review the {{site.data.keyword.satelliteshort}} storage classes for NetApp ONTAP-SAN. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.

| Storage class name | Type | File system | IOPs | Encryption |Reclaim policy |
| --- | --- | --- | --- | --- | --- |
| `sat-netapp-block-gold` **Default** | ONTAP-SAN | ext4 | no QoS limits. | Encryption disabled. | Delete |
| `sat-netapp-block-gold-encrypted` | ONTAP-SAN | ext4 | no QoS limits. | Encryption enabled. | Delete |
| `sat-netapp-block-silver` | ONTAP-SAN | ext4 | User-defined QoS limit. | Encryption disabled. | Delete |
| `sat-netapp-block-silver-encrypted` | ONTAP-SAN | ext4 | User-defined QoS limit. | Encryption enabled. | Delete |
| `sat-netapp-block-bronze` | ONTAP-SAN | ext4 | User defined QoS limit. | Encryption disabled. | Delete |
| `sat-netapp-block-bronze-encrypted` | ONTAP-SAN | ext4 | User-defined QoS limit. | Encryption enabled. | Delete |
{: caption="netapp-ontap-san version 21.04 parameter reference"}


## Getting help and support for NetApp Trident
{: #sat-san-2104-support}

If you run into an issue with using NetApp Trident, you can visit the [NetApp support page](https://netapp-trident.readthedocs.io/en/stable-v20.04/support/support.html){: external}. 



