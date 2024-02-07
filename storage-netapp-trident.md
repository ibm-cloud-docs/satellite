---
copyright:
  years: 2020, 2024
lastupdated: "2024-02-07"

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

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config by selecting the **Enable cluster admin access for Satellite Config** option in the console or including the `--enable-config-admin` option when you create your cluster.
{: important}

You cannot scope {{site.data.keyword.satelliteshort}} storage service to resource groups. However, if you are scoping other resources such as location and cluster to resource groups, you need to add {{site.data.keyword.satelliteshort}} reader and link administrator role for all resources in the account.
{: note}

## Prerequisites for NetApp Trident
{: #sat-storage-netapp-trident-prereq}

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).






## Creating and assigning a configuration in the console
{: #netapp-trident-config-create-console}
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

## Creating a configuration in the CLI
{: #netapp-trident-config-create-cli}
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
    
1. Copy one of the following example command for the template version that you want to use. For more information about the command, see `ibmcloud sat storage config create` in the [command reference](/docs/satellite?topic=satellite-satellite-cli-reference#storage-config-create-cli).


    Example command to create a version 22.04 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name netapp-trident --template-version 22.04
    ```
    {: pre}

    Example command to create a version 22.10 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name netapp-trident --template-version 22.10
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
{: #netapp-trident-config-create-api}
{: api}

1. Generate an API key, then request a refresh token. For more information, see [Generating an IBM Cloud IAM token by using an API key](/docs/account?topic=account-iamtoken_from_apikey).

1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 22.04 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"netapp-trident\", \"storage-template-version\": \"22.04\", \"update-assignments\": true}
    ```
    {: pre}

    Example request to create a version 22.10 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"netapp-trident\", \"storage-template-version\": \"22.10\", \"update-assignments\": true}
    ```
    {: pre}







{{site.data.content.assignment-create-console}}
{{site.data.content.assignment-create-cli}}
{{site.data.content.assignment-create-api}}
{{site.data.content.configuration-upgrade-console}}
{{site.data.content.assignment-upgrade-cli}}
{{site.data.content.assignment-upgrade-api}}



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

{{site.data.content.configuration-upgrade-manual-cli}}
{{site.data.content.configuration-upgrade-console2}}

## Getting help and support for NetApp Trident
{: #sat-trident-support}

If you run into an issue with NetApp Trident, you can visit the [NetApp support page](https://netapp-trident.readthedocs.io/en/stable-v20.04/support/support.html){: external}. 


