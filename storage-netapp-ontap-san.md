---
copyright:
  years: 2020, 2022
lastupdated: "2022-12-20"

keywords: satellite storage, netapp, trident, ontap, satellite config, satellite configurations,

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# NetApp ONTAP-SAN
{: #storage-netapp-ontap-san}

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





## Creating a configuration
{: #netapp-ontap-san-config-create}

Before you begin, review the [parameter reference](#netapp-ontap-san-parameter-reference) for the template version that you want to use.
{: important}

### Creating and assigning a configuration in the console
{: netapp-ontap-san-config-create-console}
{: ui}

1. [From the Locations console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to create a storage configuration.
1. Select **Storage** > **Create storage configuration**
1. Enter a name for your configuration.
1. Select the **Storage type**.
1. Select the **Version** and click **Next**
1. If the **Storage type** that you selected accepts custom parameters, enter them on the **Parameters** tab.
1. If the **Storage type** that you selected requires secrets, enter them on the **Secrets** tab.
1. On the **Storage classes** tab, review the storage classes that are deployed by the configuration or create a custom storage class.
1. On the **Assign to service** tab, select the service that you want to assign your configuration to.
1. Click **Complete** to assign your storage configuration.

### Creating a configuration in the CLI
{: #netapp-ontap-san-config-create-cli}
{: cli}

1. Copy one of the following example command for the template version that you want to use. For more information about the command, see `ibmcloud sat storage config create` in the [command reference](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).


    Example command to create a version 21.04 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name netapp-ontap-san --template-version 21.04 --param "managementLIF=MANAGEMENTLIF"  --param "dataLIF=DATALIF"  --param "svm=SVM"  --param "username=USERNAME"  --param "password=PASSWORD"  [--param "limitVolumeSize=LIMITVOLUMESIZE"]  [--param "limitAggregateUsage=LIMITAGGREGATEUSAGE"] 
    ```
    {: pre}

    Example command to create a version 22.04 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name netapp-ontap-san --template-version 22.04 --param "managementLIF=MANAGEMENTLIF"  --param "dataLIF=DATALIF"  --param "svm=SVM"  --param "username=USERNAME"  --param "password=PASSWORD"  [--param "limitVolumeSize=LIMITVOLUMESIZE"]  [--param "limitAggregateUsage=LIMITAGGREGATEUSAGE"] 
    ```
    {: pre}


1. Customize the command based on the settings that you want to use.

1. Run the command to create a configuration.

1. Verify your configuration was created.
    ```sh
    ibmcloud sat storage config get --config CONFIG
    ```
    {: pre}



## Assigning configurations to clusters
{: #netapp-ontap-san-assignment-create}

After you create a storage configuration, you can assign that configuration to clusters, cluster groups, or service clusters to automatically deploy storage resources across clusters in your Location.

If you haven't yet created a storage configuration, see [Creating a configuration](#netapp-ontap-san-config-create).

### Creating an assignment in the console
{: #netapp-ontap-san-assignment-create-console}
{: ui}

If you didn't assign your configuration to a cluster or service when you created it, you can create an assignment by completing the following steps.

1. Open the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} in your browser.
1. Select the location where you created your storage configuration.
1. Click the **Locations** tab, then click **Storage**.
1. Click the storage configuration that you want to assign to a cluster group.
1. On the **Configuration details** page, click **Create storage assignment**.
1. In the **Create an assignment** pane, enter a name for your assignment.
1. From the **Version** drop-down list, select the storage configuration version that you want to assign.
1. From the **Cluster group** drop-down list, select the cluster group that you want to assign to the storage configuration. Note that the clusters in your cluster group where you want to assign storage must all be in the same  location.
1. Click **Create** to create the assignment.
1. Verify that your storage configuration is deployed to your cluster. 
    1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, navigate to your Location and select **Storage**
    1. Click the storage configuration that you created and review the **Assignments** tab.
    1. Click the **Assignment** that you created and review the **Rollout status** for your configuration.


### Creating an assignment in the CLI
{: #netapp-ontap-san-assignment-create-cli}
{: ui}

1. List your storage configurations and make a note of the storage configuration that you want to assign to your clusters.
    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

1. Get the ID of the cluster, cluster group, or service that you want to assign storage to. 

    To make sure that your cluster is registered with {{site.data.keyword.satelliteshort}} Config or to create groups, see [Setting up clusters to use with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
    {: tip}
    
    List cluster groups.
    
    ```sh
    ibmcloud sat group ls
    ```
    {: pre}

    List clusters.
    
    ```sh
    ibmcloud oc cluster ls --provider satellite
    ```
    {: pre}
    
    List services.
    
    ```sh
    ibmcloud sat service ls --location <location>
    ```
    {: pre}

1. Assign storage to the cluster or group that you retrieved in step 2. For more information, see the `ibmcloud sat storage assignment create` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create).

    Assign a configuration to a cluster group.
    ```sh
    ibmcloud sat storage assignment create --group GROUP --config CONFIG --name NAME
    ```
    {: pre}

    Assign a configuration to a cluster.
    ```sh
    ibmcloud sat storage assignment create --cluster CLUSTER --config CONFIG --name NAME
    ```
    {: pre}

    Assign a configuration to a service cluster.
    ```sh
    ibmcloud sat storage assignment create --service-cluster-id CLUSTER --config CONFIG --name NAME
    ```
    {: pre}

1. Verify that your assignment is created.
    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER) | grep <storage-assignment-name>
    ```
    {: pre}


### Creating an assignment in the API
{: #netapp-ontap-san-assignment-create-console}
{: api}

1. Copy one of the following example requests. 

    Example request to assign a [configuration to a cluster](https://containers.cloud.ibm.com/global/swagger-global-api/#/satellite/createAssignmentByCluster){: external}.
    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createAssignmentByCluster" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -H "Content-Type: application/json" -d "{ \"channelName\": \"CONFIGURATION-NAME\", \"cluster\": \"CLUSTER-ID\", \"controller\": \"LOCATION-ID\", \"name\": \"ASSIGNMENT-NAME\"}"
    ```
    {: pre}
    
    Example request to [assign configuration to a cluster group](https://containers.cloud.ibm.com/global/swagger-global-api/#/satellite/createAssignment){: external}.
    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createAssignment" -H "accept: application/json" -H "Authorization: Bearer TOKEN" -H "Content-Type: application/json" -d "{ \"channelName\": \"CONFIGURATION-NAME\", \"cluster\": \"string\", \"groups\": [ \"CLUSTER-GROUP\" ], \"name\": \"ASSIGNMENT-NAME\"}"
    ```
    {: pre}
    
1. Replace the variables with your details and run the request.

1. Verify the assignment was created by listing your assignments.

    ```sh
    curl -X GET "https://containers.cloud.ibm.com/global/v2/storage/satellite/getAssignments" -H "accept: application/json" -H "Authorization: Bearer TOKEN"
    ```
    {: pre}
    
## Updating assignments
{: #netapp-ontap-san-assignment-update}

Update your assignments to rollout your configurations to new or different clusters, cluster groups, or service.


### Updating an assignment in the console
{: #netapp-ontap-san-assignment-update-console}
{: ui}

Updating assignments in the console is currently not available.
{: note}

However, you can use the [CLI](#netapp-ontap-san-assignment-update-cli) or [API](#netapp-ontap-san-assignment-update-api) to update your assignemnts.


### Updating an assignment in the CLI
{: #netapp-ontap-san-assignment-update-cli}
{: cli}

You can use the `storage assignment update` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-upgrade) to rename your assignment or assign it to a new cluster or cluster group.


1. List your assignments, make a note of the  assignment you want to update and the clusters or cluster groups included in the assignment.
    ```sh
    ibmcloud sat storage assignment ls
    ```
    {: pre}

1. Update the assignment.
    ```sh
    ibmcloud sat storage assignment update --assignment ASSIGNMENT [--group GROUP ...] [--name NAME]
    ```
    {: pre}

    Example command to update assignment name and assign different cluster groups.
    
    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name netapp-ontap-san --template-version 22.04 --param "managementLIF=MANAGEMENTLIF"  --param "dataLIF=DATALIF"  --param "svm=SVM"  --param "username=USERNAME"  --param "password=PASSWORD"  [--param "limitVolumeSize=LIMITVOLUMESIZE"]  [--param "limitAggregateUsage=LIMITAGGREGATEUSAGE"] 
    ```
    {: pre}


1. Customize the command based on the settings that you want to use.

1. Run the command to create a configuration.

1. Verify your configuration was created.
    ```sh
    ibmcloud sat storage config get --config CONFIG
    ```
    {: pre}

### Creating a configuration in the API
{: #netapp-ontap-san-config-create-api}

1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 21.04 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"netapp-ontap-san\", \"storage-template-version\": \"21.04\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"MANAGEMENTLIF\",\"user-secret-parameters\": { \"entry.name\": \"MANAGEMENTLIF\",  { \"entry.name\": \"DATALIF\",\"user-secret-parameters\": { \"entry.name\": \"DATALIF\",  { \"entry.name\": \"SVM\",\"user-secret-parameters\": { \"entry.name\": \"SVM\",  { \"entry.name\": \"USERNAME\",\"user-secret-parameters\": { \"entry.name\": \"USERNAME\",  { \"entry.name\": \"PASSWORD\",\"user-secret-parameters\": { \"entry.name\": \"PASSWORD\",  { \"entry.name\": \"LIMITVOLUMESIZE\",\"user-secret-parameters\": { \"entry.name\": \"LIMITVOLUMESIZE\",  { \"entry.name\": \"LIMITAGGREGATEUSAGE\",\"user-secret-parameters\": { \"entry.name\": \"LIMITAGGREGATEUSAGE\", 
    ```
    {: pre}

    Example request to create a version 22.04 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"netapp-ontap-san\", \"storage-template-version\": \"22.04\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"MANAGEMENTLIF\",\"user-secret-parameters\": { \"entry.name\": \"MANAGEMENTLIF\",  { \"entry.name\": \"DATALIF\",\"user-secret-parameters\": { \"entry.name\": \"DATALIF\",  { \"entry.name\": \"SVM\",\"user-secret-parameters\": { \"entry.name\": \"SVM\",  { \"entry.name\": \"USERNAME\",\"user-secret-parameters\": { \"entry.name\": \"USERNAME\",  { \"entry.name\": \"PASSWORD\",\"user-secret-parameters\": { \"entry.name\": \"PASSWORD\",  { \"entry.name\": \"LIMITVOLUMESIZE\",\"user-secret-parameters\": { \"entry.name\": \"LIMITVOLUMESIZE\",  { \"entry.name\": \"LIMITAGGREGATEUSAGE\",\"user-secret-parameters\": { \"entry.name\": \"LIMITAGGREGATEUSAGE\", 
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



