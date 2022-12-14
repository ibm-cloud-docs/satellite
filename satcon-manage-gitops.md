---

copyright:
  years: 2022, 2022
lastupdated: "2022-12-14"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite config gitops, satellite configuration gitops, satellite gitops

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}



# Managing GitOps configurations
{: #satcon-manage-gitops}

Create a configuration to deploy apps from your GitHub or GitLab repository to your clusters.
{: shortdesc}

{{site.data.keyword.satelliteshort}} Config reinforces the current configuration every 5 minutes, whether successful or not. If any update is made to the configuration in the cluster, for example by using `kubectl edit`, {{site.data.keyword.satelliteshort}} Config will reinforce the configuration to match what is defined in the GitHub or GitLab repository. 
{: note}

## Creating {{site.data.keyword.satelliteshort}} configurations 
{: #satcon-create-gitops}

With {{site.data.keyword.satelliteshort}} Config, you create a configuration to specify what Kubernetes resources you want to deploy to a group of {{site.data.keyword.openshiftlong_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}. 
{: shortdesc}

Before you begin

- Make sure that you have the [required permissions](/docs/satellite?topic=satellite-iam#iam-resource-config) and that [{{site.data.keyword.satelliteshort}} Config can access your clusters](/docs/satellite?topic=satellite-setup-clusters-satconfig#setup-clusters-satconfig-access). For more information, see [Checking user permissions](/docs/satellite?topic=satellite-iam-assign-access#checking-perms).
- Review the [key concepts](/docs/satellite?topic=satellite-cluster-config#satcon-terminology) for {{site.data.keyword.satelliteshort}} Config.
- If you want to use a private repository, you must [create a secret](#create-satconfig-private-secret) on the target cluster that contains a personal access token with at least read access to your private repository. 


### Creating a secret for using a private repository
{: #create-satconfig-private-secret}

To connect to private repositories, you must create and apply a git personal access token to your clusters.

1. Create a Github or Gitlab token.
    - For Github, [create a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token){: external}. When you create the token, grant it `repo` scope so that it can read from private repositories. You can also create a [machine user](https://docs.github.com/en/developers/overview/managing-deploy-keys#machine-users){: external} and bind your personal access token to the machine user.
    - For Gitlab, [create a personal token](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html){: external} or [create a deploy token](https://docs.gitlab.com/ee/user/project/deploy_tokens/){: external}.

1. Encode the token to base64.
    ```sh
    printf "TOKEN_FROM_GITHUB_OR_GITLAB" | base64
    ```
    {: pre}   

1. For each cluster that you want to use, log in to the cluster and create a secret named `razee-git` in the `razeedeploy` namespace by applying the following YAML file:
    ```yaml 
    apiVersion: v1
    data:
      token: [BASE64_ENCODED_TOKEN_FROM_GITHUB_OR_GITLAB]
    kind: Secret
    metadata:
      name: razee-git
      namespace: razeedeploy
    type: Opaque
    ```
    {: pre}  
  
    You can have only one secret between GitHub and GitLab repositories.
    {: note} 

### Creating {{site.data.keyword.satelliteshort}} configurations from the console
{: #create-satconfig-ui-gitops}
{: ui}
  
To deploy an example app, see [Deploying Kubernetes resources to clusters with Satellite Config](/docs/satellite?topic=satellite-begin-sat-config-tutorial).

To create your custom configuration, use the {{site.data.keyword.satelliteshort}} UI and follow these steps. 

1. Log in to the [{{site.data.keyword.satelliteshort}} Config UI](https://cloud.ibm.com/satellite/configuration){: external} with your {{site.data.keyword.cloud_notm}} credentials. 

1. Click **Create configuration**.

1. On the **Browse templates** page, select the **GitOps** template. 

1. On the **Configuration** page, specify your configuration name and provider.
    | Field | Description | 
    |--------------------|------------------|
    | Configuration name | Enter the name of the Satellite configuration. | 
    | Provider | Select the remote provider for the {{site.data.keyword.satelliteshort}} configurations, which stores the definitions of Kubernetes resources to be deployed to your clusters. Options are `GitHub` and `GitLab`.  | 
    {: caption="Configuration screen fields" caption-side="bottom"}

1. On the **Subscription** page, specify your repository URL, repository visibility, Git ref details, path to your file or directory, and cluster group to deploy to.
    | Field | Description | 
    |--------------------|------------------|
    | Repository URL | Enter the remote repository to use for this {{site.data.keyword.satelliteshort}} subscription. |
    | Repository visibility | Select whether this repository is public or private. |
    | Git ref type | Select the type of Git ref to use for this {{site.data.keyword.satelliteshort}} subscription. Valid values are `Branch`, `Tag`, `Commit`, and `Release`. |
    | Branch/Tag/Commit/Release name | Enter the Git ref to use for this {{site.data.keyword.satelliteshort}} subscription. |
    | Path | Enter the path to the files in the remote repository to use for this {{site.data.keyword.satelliteshort}} subscription. |
    | Subscription name | Enter a name for your {{site.data.keyword.satelliteshort}} subscription. The branch/tag/commit/release name is prefilled as the subcription name by default. |
    | Cluster group | Enter the name of the cluster group where you want to deploy your Kubernetes resources. | 
    {: caption="Subscription screen fields" caption-side="bottom"}

1. On the **Summary** page, confirm that the displayed information is corect and then click **Complete**.


### Creating {{site.data.keyword.satelliteshort}} configurations from the CLI 
{: #create-satconfig-cli-gitops}
{: cli}

Use the CLI plug-in for {{site.data.keyword.satelliteshort}} commands to create a configuration.
{: shortdesc}

To create the configuration:

1. [Set up your clusters to use with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig). This setup includes creating a cluster group and granting {{site.data.keyword.satelliteshort}} Config access to your clusters.

1. Create a **{{site.data.keyword.satelliteshort}} configuration**.
    ```sh
    ibmcloud sat config create --name <config_name> [--provider <provider>] [-q]
    ```
    {: pre}
    
    | Component | Description | 
    |--------------------|------------------|
    | `--name <config_name>` | Enter the name of the Satellite configuration. | 
    | `--provider <provider>` | Enter remote provider for the {{site.data.keyword.satelliteshort}} configurations, which stores the definitions of Kubernetes resources to be deployed to your clusters. Valid values are `github` and `gitlab`.  |
    | `-q` | Do not show the message of the day or update reminders. | 
    {: caption="Understanding this command's components" caption-side="bottom"}

    Example output
    ```sh
    Creating configuration...
    OK
    Configuration <config_name> was successfully created with ID 116fffde-0835-467c-8987-67dd42e4e393.
    ```
    {: screen}

1. Create a **Subscription** for your cluster group to the {{site.data.keyword.satelliteshort}} configuration.
    ```sh
    ibmcloud sat subscription create --name <subscription_name> --group <cluster_group_name> --config <config_name_or_ID> --repository <repository_value> --gitref-type <type_value> --gitref <gitref_value> --path <path_value> [-q] 
    ```
    {: pre}

    | Component | Description | 
    |--------------------|------------------|
    | `--name <subscription_name>` | Enter a name for your {{site.data.keyword.satelliteshort}} subscription. |
    | `--group <cluster_group_name>` | Enter the name of the cluster group where you want to deploy your Kubernetes resources. | 
    | `--config <config_name_or_ID>` | Enter the name or ID of the {{site.data.keyword.satelliteshort}} configuration that you created earlier. |
    | `--repository <repository_value>` | Enter the remote repository to use for this {{site.data.keyword.satelliteshort}} subscription. |
    | `--gitref-type <type_value>` | Enter the type of Git ref to use for this {{site.data.keyword.satelliteshort}} subscription. Valid values are `branch`, `commit`, `tag`, and `release`. |
    | `--gitref <gitref_value>` | Enter the Git ref to use for this {{site.data.keyword.satelliteshort}} subscription. |
    | `--path <path_value>` | Enter the path to the files in the remote repository to use for this {{site.data.keyword.satelliteshort}} subscription. |
    | `-q` | Do not show the message of the day or update reminders. | 
    {: caption="Understanding this command's components" caption-side="bottom"}

    Example output
    ```sh
    Creating subscription...
    OK
    Subscription <subscription_name> was successfully created with ID f6114bd5-f71e-4335-b034-ca45fa3cab81.
    ```
    {: screen}

1. Follow step 5 in [Creating {{site.data.keyword.satelliteshort}} configurations from the console](/docs/satellite?topic=satellite-setup-clusters-satconfig) to review the rollout status of your Kubernetes resources.

  
  
## Updating your Satellite configuration
{: #satcon-manage-update-gitops}

To update your configuration, you can make updates on subsription level. The actions you need to take depends on what kind of updates you want to make.

- To deploy a new version of your application to the same cluster group, update the files in the GitHub or GitLab repository defined in your configuration and commit the changes. 
- To change any properties of your source repository, you can edit the subscription and update the fields with new details.
- To associate multiple repositories with the same configuration, add a new source repository, or add new Git ref details for the same source repository, you can create new subscriptions. Click **Create subscription** and fill in the details of your new subscription. If you want to create a new subscription that only has a few details changed, you can use the **Duplicate** option to prefill all the fields with properties of the existing subscription and modify the fields that you want to chagne.
- To delete a subcription, use the **Remove** option.

### Updating your subscription from the console
{: #satcon-manage-update-console-gitops}
{: ui}

Use the {{site.data.keyword.satelliteshort}} console to update your configuration.
{: shortdesc}

1. Log in to the [{{site.data.keyword.satelliteshort}} Config UI](https://cloud.ibm.com/satellite/configuration){: external} with your {{site.data.keyword.cloud_notm}} credentials. 

2. Select the configuration you want to update.

3. Click the overflow menu in the line of the subscription to update and select **Edit**.

4. On the **Edit subscription** page, make the changes and click **Save**.



### Updating your {{site.data.keyword.satelliteshort}} Config with the CLI
{: #satcon-manage-update-cli-gitops}
{: cli}

Use the CLI plug-in for {{site.data.keyword.satelliteshort}} commands to update your configuration. If you want to update your configuration to use a new file, you can update your subscription to point to the new file.
{: shortdesc}

1. Update the **Subscription** for your cluster group to use the new {{site.data.keyword.satelliteshort}} configuration in the remote repository. You only need specify the parameters you want to change.  
    ```sh
    ibmcloud sat subscription update --subscription <subscription_name> [--name <new_subscription_name>] [--group <cluster_group_name>] [--repository <repository_value>] [--gitref-type <type_value>] [--gitref <gitref_value>] [--path <path_value>] [-f] [-q]
    ```
    {: pre}

    | Component | Description | 
    |--------------------|------------------|
    | `--subscription <subscription_value>` | Enter the name or ID of the subscription to update. |
    | `--name <new_subscription_name>` | Enter the new name of the subscription. |
    | `--group <cluster_group_name>` | Enter the name of the cluster group where you want to deploy your Kubernetes resources. | 
    | `--repository <repository_value>` | Enter the remote repository to use for this {{site.data.keyword.satelliteshort}} subscription. |
    | `--gitref-type <type_value>` | Enter the type of Git ref to use for this {{site.data.keyword.satelliteshort}} subscription. Valid values are `branch`, `commit`, `tag`, and `release`. |
    | `--gitref <gitref_value>` | Enter the Git ref to use for this {{site.data.keyword.satelliteshort}} subscription. |
    | `--path <path_value>` | Enter the path to the files in the remote repository to use for this {{site.data.keyword.satelliteshort}} subscription. |
    | `-f` | Force the command to run without user prompts. |
    | `-q` | Do not show the message of the day or update reminders. |  
    {: caption="Understanding this command's components" caption-side="bottom"}

    Example output
    ```sh
    Are you sure you want to update the subscription <subscription_name>? [y/N]y
    OK
    Subscription f6114bd5-f71e-4335-b034-ca45fa3cab81 was successfully updated.
    ```
    {: screen}

1. Follow step 5 in [Creating {{site.data.keyword.satelliteshort}} configurations from the console](/docs/satellite?topic=satellite-setup-clusters-satconfig) to review the rollout status of your Kubernetes resources.



