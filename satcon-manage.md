---

copyright:
  years: 2020, 2022
lastupdated: "2022-11-03"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Managing your {{site.data.keyword.satelliteshort}} Config resources
{: #satcon-manage}

To create and roll out new versions of your applications, update your {{site.data.keyword.satelliteshort}} Config configurations and subscriptions. You can also use {{site.data.keyword.satelliteshort}} Config to review an inventory of all the resources that are managed by your configurations across clusters.
{: shortdesc}
  
  
## Updating your {{site.data.keyword.satelliteshort}} Config configuration
{: #satcon-manage-update}

To update your {{site.data.keyword.satelliteshort}} Config configuration, upload or create a new version, then create a subscription for the new version.

### Updating your {{site.data.keyword.satelliteshort}} Config from the console
{: #satcon-manage-update-console}

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
    4. Click **Create** to create the subscription. After you create the subscription, {{site.data.keyword.satelliteshort}} Config automatically downloads the Kubernetes resource YAML file for the version that you specified and starts applying this YAML file across all clusters that belong to the cluster group. This process takes a few minutes to complete. In addition, information about all Kubernetes resources that you create are sent back from your clusters to {{site.data.keyword.satelliteshort}} Config and can be reviewed in the {{site.data.keyword.satelliteshort}} [**Cluster resources**](https://cloud.ibm.com/satellite/resources) dashboard.
3. Select your subscription to see the subscription details and the rollout status of your Kubernetes resource deployment. If errors occur during the deployment, such as YAML files with formatting errors or unsupported API version values, you can view the error message in the **Message** column of your subscription details.

### Updating your {{site.data.keyword.satelliteshort}} Config with the CLI
{: #satcon-manage-update-cli}

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



## Removing {{site.data.keyword.satelliteshort}} Config from your cluster
{: #remove-satconfig}

If you do not want your cluster to be available to {{site.data.keyword.satelliteshort}} Config, you can remove the {{site.data.keyword.satelliteshort}} Config components from your cluster. 
{: shortdesc}

You can remove {{site.data.keyword.satelliteshort}} Config components only from {{site.data.keyword.openshiftlong_notm}} clusters that you manually registered. If you created a cluster on {{site.data.keyword.satelliteshort}}-provided infrastructure, {{site.data.keyword.satelliteshort}} Config components are automatically set up in the location control plane and cannot be removed. 
{: note}

Removing {{site.data.keyword.satelliteshort}} Config components automatically removes all cluster resources that you created by using {{site.data.keyword.satelliteshort}} configurations. Make sure to back up any data or redeploy any apps that you want to keep. 
{: important}

1. [Log in to your {{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-access_cluster).
2. Remove all the {{site.data.keyword.satelliteshort}} Config components from your cluster by running a [{{site.data.keyword.satelliteshort}} Config removal job](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/satellite/satconfig/satconfig_remove.yaml){: external}. 
    ```sh
    oc apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/satellite/satconfig/satconfig_remove.yaml
    ```
    {: pre}

3. Wait a few minutes for the {{site.data.keyword.satelliteshort}} Config components to be removed. 
4. Verify that your {{site.data.keyword.satelliteshort}} Config components are removed. 
    ```sh
    oc get pods -n razeedeploy
    ```
    {: pre}

    Example output
    ```sh
    No resources found.
    ```
    {: screen}

5. From the [{{site.data.keyword.satelliteshort}} cluster dashboard](https://cloud.ibm.com/satellite/clusters){: external}, find the cluster that you want to remove from {{site.data.keyword.satelliteshort}} Config. 
6. From the actions menu, click **Unregister** and verify that your cluster is removed from the {{site.data.keyword.satelliteshort}} cluster dashboard. 
