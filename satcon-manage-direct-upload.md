---

copyright:
  years: 2020, 2023
lastupdated: "2023-02-20"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Managing Direct Upload configurations
{: #satcon-manage-direct-upload}

Create a configuration to deploy your apps to clusters by uploading resources directly.
{: shortdesc}

## Creating {{site.data.keyword.satelliteshort}} configurations 
{: #satcon-create}

With {{site.data.keyword.satelliteshort}} Config, you create a configuration to specify what Kubernetes resources you want to deploy to a group of {{site.data.keyword.openshiftlong_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}. 
{: shortdesc}

Before you begin

- Make sure that you have the [required permissions](/docs/satellite?topic=satellite-iam#iam-resource-config) and that [{{site.data.keyword.satelliteshort}} Config can access your clusters](/docs/satellite?topic=satellite-setup-clusters-satconfig-access). For more information, see [Checking user permissions](/docs/satellite?topic=satellite-iam-assign-access#checking-perms).
- Review the [key concepts](/docs/satellite?topic=satellite-cluster-config#satcon-terminology) for {{site.data.keyword.satelliteshort}} Config.

### Creating {{site.data.keyword.satelliteshort}} configurations from the console
{: #create-satconfig-ui}
{: ui}

To deploy an example app, see [Deploying Kubernetes resources to clusters with Satellite Config](/docs/satellite?topic=satellite-begin-sat-config-tutorial).

To create your custom configuration, use the {{site.data.keyword.satelliteshort}} UI and follow these steps.

1. Log in to the [{{site.data.keyword.satelliteshort}} Config UI](https://cloud.ibm.com/satellite/configuration){: external} with your {{site.data.keyword.cloud_notm}} credentials. 

1. Click **Create config**.

1. On the **Browse templates** page, select the **Direct Upload** template. 

1. On the **Configuration** page, specify your configuration name and data location.

1. On the **Version** page, specify your version name and description. Then upload an existing file or use the YAML editor to paste in the file content.

1. On the **Subscription** page, specify your subscription name, subscription version, and cluster group to deploy to.

1. On the **Summary** page, confirm that the displayed information is correct and then click **Complete**.



### Creating {{site.data.keyword.satelliteshort}} configurations from the CLI
{: #create-satconfig-cli}
{: cli}

Use the CLI plug-in for {{site.data.keyword.satelliteshort}} commands to create a configuration and upload the Kubernetes resource definition that you want to deploy to your clusters.
{: shortdesc}

To create the configuration:

1. [Set up your clusters to use with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig). This setup includes creating a cluster group and granting {{site.data.keyword.satelliteshort}} Config access to your clusters.
2. Add clusters to your cluster group. The clusters can run in your location or in {{site.data.keyword.cloud_notm}}.
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

3. Create a **{{site.data.keyword.satelliteshort}} configuration**.
    ```sh
    ibmcloud sat config create --name <config_name> [--data-location <location>] [-q]
    ```
    {: pre}
    
    | Component | Description | 
    |--------------------|------------------|
    | `--name <config_name>` | Enter the name of the Satellite configuration. | 
    | `--data-location <location>` | Enter the location to store your {{site.data.keyword.satelliteshort}} configurations, for example `us-east`. {{site.data.keyword.satelliteshort}} configurations are Kubernetes resource definitions, such as ConfigMaps, storage classes, or secrets that are deployed to the clusters in your location through subscriptions. If `--data-location` is not specified, your configurations are stored in `us-east` by default. These locations are {{site.data.keyword.cos_full_notm}} buckets that are owned by {{site.data.keyword.IBM_notm}} and are pre-provisioned for each region. For more information about how your data is stored, see [How is my information stored, backed up, and encrypted?](/docs/satellite?topic=satellite-data-security). For a list of locations, see [Supported locations](/docs/satellite?topic=satellite-sat-regions).  |
    | `-q` | Do not show the message of the day or update reminders. | 
    {: caption="Understanding this command's components" caption-side="bottom"}

    Example output
    ```sh
    Creating configuration...
    OK
    Configuration <config_name> was successfully created with ID 116fffde-0835-467c-8987-67dd42e4e393.
    ```
    {: screen}


5. Create a **Subscription** for your cluster group to the {{site.data.keyword.satelliteshort}} configuration. After you create the subscription, {{site.data.keyword.satelliteshort}} Config automatically downloads the Kubernetes resource file for the version that you specified and starts applying this file across all clusters that belong to the cluster group. This process takes a few minutes to complete. In addition, information about all Kubernetes resources that you create are sent back from your clusters to {{site.data.keyword.satelliteshort}} Config and can be reviewed in the {{site.data.keyword.satelliteshort}} [**Cluster resources**](https://cloud.ibm.com/satellite/resources){: external} dashboard. Review the command options by running `ibmcloud sat subscription create`.

    ```sh
    ibmcloud sat subscription create --group <cluster_group_name> --config <config_name_or_ID> --name <subscription_name> --version <version_name_or_ID>
    ```
    {: pre}

    | Component | Description | 
    |--------------------|------------------|
    | `--group <cluster_group_name>` | Enter the name of the cluster group where you want to deploy your Kubernetes resources. | 
    | `--config <config_name_or_ID>` | Enter the name or ID of the {{site.data.keyword.satelliteshort}} configuration that you created earlier. |
    | `--name <subscription_name>` | Enter a name for your {{site.data.keyword.satelliteshort}} subscription. |
    | `--version <version_name_or_ID>` | Enter the name or ID of the Kubernetes resource definition that you added as a version to your configuration. To list available versions, run `ibmcloud sat config get --config <config_name_or_ID>` | 
    {: caption="Understanding this command's components" caption-side="bottom"}

    Example output
    ```sh
    Creating subscription...
    OK
    Subscription <subscription_name> was successfully created with ID f6114bd5-f71e-4335-b034-ca45fa3cab81.
    ```
    {: screen}

6. Follow step 5 in [Creating {{site.data.keyword.satelliteshort}} configurations from the console](/docs/satellite?topic=satellite-setup-clusters-satconfig) to review the rollout status of your Kubernetes resources.

  
  
## Updating your {{site.data.keyword.satelliteshort}} Config configuration
{: #satcon-manage-update}

To update your {{site.data.keyword.satelliteshort}} Config configuration, upload or create a new version, then create a subscription for the new version.

### Updating your {{site.data.keyword.satelliteshort}} Config from the console
{: #satcon-manage-update-console}
{: ui}

Use the {{site.data.keyword.satelliteshort}} console to upload a new version file and change your subscription to use it.
{: shortdesc}

1. From the actions menu of a configuration, click **Add version**.
    1. Enter a name and an optional description for your version.
    2. Upload a Kubernetes resource YAML file or use the editor to enter your Kubernetes resource definition directly. Make sure to specify the Kubernetes namespace where you want your resource to be deployed. If you do not specify a namespace, the resource is deployed to the `razeedeploy` namespace by default. 
    3. **Optional**: To view the resources after they are created in the cluster through the {{site.data.keyword.satelliteshort}} Config dashboard, add the `razee/watch-resource=lite` label to the `metadata.labels` section of your YAML file or [choose another option to view your deployed resources](/docs/satellite?topic=satellite-satcon-resources), such as adding a ConfigMap to your cluster. 
    4. Click **Add** to add the Kubernetes resource definition as a version to your configuration.
2. Create a  **Subscription** for your cluster group. The subscription defines which {{site.data.keyword.satelliteshort}} configuration to deploy the Kubernetes resources to your clusters.
    1. Select the configuration that you created to see the configuration details.
    2. Click **Create subscription**.
    3. Enter a name for your subscription and select the version name and the cluster group that you created earlier.
    4. Click **Create** to create the subscription. 
3. Select your subscription to see the subscription details and the rollout status of your Kubernetes resource deployment. If errors occur during the deployment, such as YAML files with formatting errors or unsupported API version values, you can view the error message in the **Message** column of your subscription details.

### (TO UPDATE WITH NEW COMMANDS:) Updating your {{site.data.keyword.satelliteshort}} Config with the CLI
{: #satcon-manage-update-cli}
{: cli}

Use the CLI plug-in for {{site.data.keyword.satelliteshort}} commands to upload a new version file and change your subscription to use it.
{: shortdesc}

1. Create a **Version** by uploading a Kubernetes resource file to your configuration. Make sure to specify the Kubernetes namespace where you want your resource to be deployed. If you do not specify a namespace, the resource is deployed to the `razeedeploy` namespace by default. Review the command options by running `ibmcloud sat config version create`.

    To view the resources after they are created in the cluster through the {{site.data.keyword.satelliteshort}} Config dashboard, add the `razee/watch-resource=lite` label to the `metadata.labels` section of your YAML file or [choose another option to view your deployed resources](/docs/satellite?topic=satellite-satcon-resources), such as adding a ConfigMap to your cluster. 
    {: tip}

    ```sh
    ibmcloud sat config version create --name <version_name> --config <config_name_or_ID> --file-format yaml --read-config <file_path>
    ```
    {: pre}

    | Component | Description | 
    |--------------------|------------------|
    | `--name <version_name>` | Enter a name for your configuration version. | 
    | `--config <config_name_or_ID>` | Enter the name or ID of the {{site.data.keyword.satelliteshort}} configuration that you created earlier. |
    | `--read-config <file_path>` | Enter the relative file path to the Kubernetes resource file on your local machine. | 
    {: caption="Understanding this command's components" caption-side="bottom"}

    Example output
    
    ```sh
    Creating configuration version...
    OK
    Configuration Version <version_name> was successfully created with ID ad5ae7a9-4f74-486c-816a-32de98de00df.
    ```
    {: screen}

2. Create a **Subscription** for your cluster group to the {{site.data.keyword.satelliteshort}} configuration. After you create the subscription, {{site.data.keyword.satelliteshort}} Config automatically downloads the Kubernetes resource file for the version that you specified and starts applying this file across all clusters that belong to the cluster group. This process takes a few minutes to complete. In addition, information about all Kubernetes resources that you create are sent back from your clusters to {{site.data.keyword.satelliteshort}} Config and can be reviewed in the {{site.data.keyword.satelliteshort}} [**Cluster resources**](https://cloud.ibm.com/satellite/resources) dashboard. Review the command options by running `ibmcloud sat subscription create`.

    ```sh
    ibmcloud sat subscription create --group <cluster_group_name> --config <config_name_or_ID> --name <subscription_name> --version <version_name_or_ID>
    ```
    {: pre}

    | Component | Description | 
    |--------------------|------------------|
    | `--group <cluster_group_name>` | Enter the name of the cluster group where you want to deploy your Kubernetes resources. | 
    | `--config <config_name_or_ID>` | Enter the name or ID of the {{site.data.keyword.satelliteshort}} configuration that you created earlier. |
    | `--name <subscription_name>` | Enter a name for your {{site.data.keyword.satelliteshort}} subscription. |
    | `--version <version_name_or_ID>` | Enter the name or ID of the Kubernetes resource definition that you added as a version to your configuration. To list available versions, run `ibmcloud sat config get --config <config_name_or_ID>` | 
    {: caption="Understanding this command's components" caption-side="bottom"}

    Example output
    ```sh
    Creating subscription...
    OK
    Subscription <subscription_name> was successfully created with ID f6114bd5-f71e-4335-b034-ca45fa3cab81.
    ```
    {: screen}

3. Follow step 5 in [Creating {{site.data.keyword.satelliteshort}} configurations from the console](/docs/satellite?topic=satellite-setup-clusters-satconfig) to review the rollout status of your Kubernetes resources.



