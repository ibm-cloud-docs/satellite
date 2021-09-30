---
copyright:
  years: 2020, 2021
lastupdated: "2021-09-30"

keywords: satellite storage, netapp, trident, ontap, satellite config, satellite configurations,

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# NetApp ONTAP-SAN 21.04
{: #config-storage-netapp-2104}

Set up [NetApp ONTAP-SAN storage](https://netapp-trident.readthedocs.io/en/stable-v21.04/){: external} for {{site.data.keyword.satelliteshort}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

Before you can create storage configurations by using the NetApp NAS template, you must deploy the [NetApp Trident template](/docs/satellite?topic=satellite-config-storage-netapp-trident), which installs the required operator.
{: important}

## Prerequisites
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
1. [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/satellite?topic=openshift-satellite-clusters). 
    - Your cluster must meet the requirements for ONTAP-SAN. For more information, see the [NetApp documentation](https://netapp-trident.readthedocs.io/en/stable-v21.04/support/requirements.html).
    - Your hosts must meet the [{{site.data.keyword.satelliteshort}} host requirements](/docs/satellite?topic=satellite-host-reqs) in addition to the requirements for ONTAP-SAN.
1. [Add your {{site.data.keyword.satelliteshort}} to a cluster group](/docs/satellite?topic=satellite-setup-clusters-satconfig#setup-clusters-satconfig-groups).
1. [Set up {{site.data.keyword.satelliteshort}} Config on your clusters](/docs/satellite?topic=satellite-setup-clusters-satconfig).




## Creating a NetApp Trident SAN storage configuration in the command line
{: #sat-storage-netapp-cli-san-2104}

1. Log in to the {{site.data.keyword.cloud_notm}} CLI.
    ```sh
    ibmcloud login
    ```
    {: pre}

1. Before you can create a storage configuration, follow the steps to set up a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. If you do not have any clusters in your location, [create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) or [attach existing {{site.data.keyword.openshiftlong_notm}} clusters to your location](/docs/satellite?topic=satellite-satcon-existing).

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
1. Review the [NetApp Trident storage configuration parameters](#sat-storage-netapp-params-cli-san-2104).
1. Copy the following the command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `--param "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
    ```sh
    ibmcloud sat storage config create --name 'ontapsan-config' --location <location id> --template-name 'netapp-ontap-san' --template-version '21.04' -p 'managementLIF=10.0.0.1' -p 'dataLIF=10.0.0.2' -p 'svm=svm-san' -p 'username=admin' -p 'password=<admin password>'
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




### Assigning a storage configuration in the command line
{: #assign-storage-netapp-cli-san-2104}

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

1. Verify that the storage configuration resources are deployed.
    ```sh
    oc get pods -A | grep <template-name>
    ```
    {: pre}


## NetApp Trident storage configuration parameter reference
{: #sat-storage-netapp-params-cli-san-2104}

For more information about the NetApp Trident configuration parameters, see the [NetApp documentation](https://netapp-trident.readthedocs.io/en/stable-v21.04/docker/install/ndvp_ontap_config.html#configuration-file-options){: external}.

| Parameter name | Required? | Description | Default value if not provided |
| --- | --- | --- | --- |
| `managementLIF` | Required | The IP address of the management LIF. Example: `10.0.0.1`. | N/A |
| `dataLIF` | Required | The IP address of data LIF. Example: `10.0.0.2`. | N/A | 
| `svm` | Required | The name of the storage virtual machine. Example: `svm-iscsi`. | N/A | 
| `username` | Required | The username to connect to the storage device. | N/A |
| `password` | Required | The password to connect to the storage device. | N/A |
| `limitVolumeSize` | Optional | Maximum volume size that can be requested and qtree parent volume size. Example: `50Gi`| `(not enforced by default)` |
| `limitAggregateUsage` | Optional | Limit provisioning of volumes if parent volume usage exceeds this value. For example, if a volume is requested that causes parent volume usage to exceed this value, the volume provisioning fails. Example: `80%`  | `(not enforced by default)` |
{: caption="Table 1. NetApp Trident storage parameter reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column is a brief description of the parameter. The third column is the default value of the parameter. The fourth column is the default value, if one is provided."}

## Storage class reference
{: #netapp-sc-reference-san-2104}

Before deployings apps that use the `sat-netapp` storage classes, review the following notes.

By default, the `sat-netapp-file-gold` storage class doesn't include any QoS limits (unlimited IOPS).
{: note}

To use the `sat-netapp-file-silver` and `sat-netapp-file-bronze` storage classes, you must create corresponding `silver` and `bronze` QoS policy groups on the storage controller and define the QoS limits. To create a policy group on the storage system, log in to the system CLI and run the `netapp1::> qos policy-group create -policy-group <policy_group_name> -vserver <svm_name> [-min-throughput <min_IOPS>] -max-throughput <max_IOPS>` command.
{: note}

**min-throughput** is only supported on all-flash systems. For more information about creating and managing QoS Policy groups, see  the [ONTAP 9 Storage Management documentation](https://docs.netapp.com/ontap-9/index.jsp).
{: note}

To use an **encrypted** storage class, NetApp Volume Encryption (NVE) must be enabled on your storage system by using either the NetApp ONTAP onboard key manager or a supported (off-box) third-party key manager, such as {{site.data.keyword.IBM_notm}} 's TKLM key manager. To enable the onboard key manager, run the `netapp1::> security key-manager onboard enable` command. For more information about configuring encryption, see the [ONTAP 9 Security and Data Encryption documentation](https://docs.netapp.com/ontap-9/topic/com.netapp.nav.aac/home.html?cp=14).
{: note}

Review the {{site.data.keyword.satelliteshort}} storage classes for NetApp ONTAP-SAN. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.

| Storage class name | Type | File system | IOPs | Encryption |Reclaim policy |
| --- | --- | --- | --- | --- | --- |
| `sat-netapp-block-gold` | ONTAP-SAN | ext4 | no QoS limits. | Encryption disabled. | Delete |
| `sat-netapp-block-gold-encrypted` | ONTAP-SAN | ext4 | no QoS limits. | Encryption enabled. | Delete |
| `sat-netapp-block-silver` | ONTAP-SAN | ext4 | User-defined QoS limit. | Encryption disabled. | Delete |
| `sat-netapp-block-silver-encrypted` | ONTAP-SAN | ext4 | User-defined QoS limit. | Encryption enabled. | Delete |
| `sat-netapp-block-bronze` | ONTAP-SAN | ext4 | User defined QoS limit. | Encryption disabled. | Delete |
| `sat-netapp-block-bronze-encrypted` | ONTAP-SAN | ext4 | User-defined QoS limit. | Encryption enabled. | Delete |
{: caption="Table 2. NetApp ONTAP-SAN storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system. The fourth column is the reclaim policy."}




