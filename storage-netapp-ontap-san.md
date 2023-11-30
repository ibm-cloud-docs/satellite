---
copyright:
  years: 2020, 2023
lastupdated: "2023-11-30"

keywords: satellite storage, netapp, trident, ontap, satellite config, satellite configurations,

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# NetApp ONTAP-SAN
{: #storage-netapp-ontap-san}

Set up [NetApp ONTAP-SAN storage](https://netapp-trident.readthedocs.io/en/stable-v21.04/){: external} for {{site.data.keyword.satellitelong}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

Before you can create storage configurations by using the NetApp NAS template, you must deploy the [NetApp Trident template](/docs/satellite?topic=satellite-storage-netapp-trident), which installs the required operator.
{: important}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config by selecting the **Enable cluster admin access for Satellite Config** option in the console or including the `--enable-config-admin` option when you create your cluster.
{: important}

You cannot scope {{site.data.keyword.satelliteshort}} storage service to resource groups. However, if you are scoping other resources such as location and cluster to resource groups, you need to add {{site.data.keyword.satelliteshort}} reader and link administrator role for all resources in the account.
{: note}

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

1. [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-satellite-clusters).
    - Make sure that you select the **Enable cluster admin access for {{site.data.keyword.satelliteshort}} Config** option when you create the cluster. If you don't enable Administrator (admin) access for {{site.data.keyword.satelliteshort}} Config when creating your cluster, you must re-create your cluster and enable admin access before you can deploy storage.
    - Your cluster must meet the requirements for ONTAP-SAN. For more information, see the [NetApp documentation](https://netapp-trident.readthedocs.io/en/stable-v21.04/support/requirements.html).
    - Your hosts must meet the [{{site.data.keyword.satelliteshort}} host requirements](/docs/satellite?topic=satellite-host-reqs) in addition to the requirements for ONTAP-SAN.
1. [Add your {{site.data.keyword.satelliteshort}} to a cluster group](/docs/satellite?topic=satellite-setup-clusters-satconfig-groups).
1. [Set up {{site.data.keyword.satelliteshort}} Config on your clusters](/docs/satellite?topic=satellite-setup-clusters-satconfig).







## Creating and assigning a configuration in the console
{: #netapp-ontap-san-config-create-console}
{: ui}


1. Review the [parameter reference](#netapp-ontap-san-parameter-reference).


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

## Creating a configuration in the CLI
{: #netapp-ontap-san-config-create-cli}
{: cli}


1. Review the [parameter reference](#netapp-ontap-san-parameter-reference) for the template version that you want to use.


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
    
1. Copy one of the following example command for the template version that you want to use. For more information about the command, see `ibmcloud sat storage config create` in the [command reference](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).


    Example command to create a version 22.04 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name netapp-ontap-san --template-version 22.04 --param "managementLIF=MANAGEMENTLIF"  --param "dataLIF=DATALIF"  --param "svm=SVM"  --param "username=USERNAME"  --param "password=PASSWORD"  --param "limitVolumeSize=LIMITVOLUMESIZE"  --param "limitAggregateUsage=LIMITAGGREGATEUSAGE" 
    ```
    {: pre}


    Example command to create a version 22.10 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name netapp-ontap-san --template-version 22.10 --param "managementLIF=MANAGEMENTLIF"  --param "dataLIF=DATALIF"  --param "svm=SVM"  --param "username=USERNAME"  --param "password=PASSWORD"  --param "limitVolumeSize=LIMITVOLUMESIZE"  --param "limitAggregateUsage=LIMITAGGREGATEUSAGE" 
    ```
    {: pre}



1. Customize the command based on the settings that you want to use.

1. Run the command to create a configuration.

1. Verify your configuration was created.
    ```sh
    ibmcloud sat storage config get --config CONFIG
    ```
    {: pre}

## Creating a configuration in the API
{: #netapp-ontap-san-config-create-api}
{: api}

1. Generate an API key, then request a refresh token. For more information, see [Generating an IBM Cloud IAM token by using an API key](/docs/account?topic=account-iamtoken_from_apikey).

1. Review the [parameter reference](#netapp-ontap-san-parameter-reference) for the template version that you want to use.


1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 22.04 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"netapp-ontap-san\", \"storage-template-version\": \"22.04\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"MANAGEMENTLIF\", { \"entry.name\": \"DATALIF\", { \"entry.name\": \"SVM\", { \"entry.name\": \"LIMITVOLUMESIZE\", { \"entry.name\": \"LIMITAGGREGATEUSAGE\",\"user-secret-parameters\": { \"entry.name\": \"USERNAME\",{ \"entry.name\": \"PASSWORD\",}
    ```
    {: pre}


    Example request to create a version 22.10 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"netapp-ontap-san\", \"storage-template-version\": \"22.10\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"MANAGEMENTLIF\", { \"entry.name\": \"DATALIF\", { \"entry.name\": \"SVM\", { \"entry.name\": \"LIMITVOLUMESIZE\", { \"entry.name\": \"LIMITAGGREGATEUSAGE\",\"user-secret-parameters\": { \"entry.name\": \"USERNAME\",{ \"entry.name\": \"PASSWORD\",}
    ```
    {: pre}








{{site.data.content.assignment-create-console}}
{{site.data.content.assignment-create-cli}}
{{site.data.content.assignment-create-api}}
{{site.data.content.configuration-upgrade-console}}
{{site.data.content.assignment-upgrade-cli}}
{{site.data.content.assignment-upgrade-api}}








## Parameter reference
{: #netapp-ontap-san-parameter-reference}

### 22.04 parameter reference
{: #netapp-ontap-san-22.04-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| Management LIF | `managementLIF` | Config | The IP address of the Management LIF. | true | N/A |
| Data LIF | `dataLIF` | Config | The IP address of the Data LIF. | true | N/A |
| SVM | `svm` | Config | The name of the SVM. | true | N/A |
| User Name | `username` | Secret | The username to connect to the storage device. | true | N/A |
| User Password | `password` | Secret | The password to connect to the storage device. | true | N/A |
| Limit Volume Size | `limitVolumeSize` | Config | The maximum volume size (in Gibibytes) that can be requested and the qtree parent volume size. | true | `50Gi` |
| Limit AggregateUsage | `limitAggregateUsage` | Config | Provisioning fails if usage is above this percentage. | true | `80%` |
{: caption="Table 1. 22.04 parameter reference" caption-side="bottom"}


### 22.10 parameter reference
{: #netapp-ontap-san-22.10-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| Management LIF | `managementLIF` | Config | The IP address of the Management LIF. | true | N/A |
| Data LIF | `dataLIF` | Config | The IP address of the Data LIF. | true | N/A |
| SVM | `svm` | Config | The name of the SVM. | true | N/A |
| User Name | `username` | Secret | The username to connect to the storage device. | true | N/A |
| User Password | `password` | Secret | The password to connect to the storage device. | true | N/A |
| Limit Volume Size | `limitVolumeSize` | Config | The maximum volume size (in Gibibytes) that can be requested and the qtree parent volume size. | true | `50Gi` |
| Limit AggregateUsage | `limitAggregateUsage` | Config | Provisioning fails if usage is above this percentage. | true | `80%` |
{: caption="Table 2. 22.10 parameter reference" caption-side="bottom"}


## Storage class reference for NetApp ONTAP-SAN
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


## Getting help and support for NetApp ONTAP-SAN
{: #sat-san-2104-support}

If you run into an issue with NetApp Trident, you can visit the [NetApp support page](https://netapp-trident.readthedocs.io/en/stable-v20.04/support/support.html){: external}. 



