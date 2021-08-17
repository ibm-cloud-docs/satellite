---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-17"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:audio: .audio}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: .ph data-hd-programlang='c#'}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: #curl .ph data-hd-programlang='curl'}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: .external target="_blank"}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:middle: .ph data-hd-position='middle'}
{:navgroup: .navgroup}
{:new_window: target="_blank"}
{:node: .ph data-hd-programlang='node'}
{:note: .note}
{:objectc: .ph data-hd-programlang='Objective C'}
{:objectc: data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: .ph data-hd-programlang='PHP'}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:right: .ph data-hd-position='right'}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:step: data-tutorial-type='step'} 
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:topicgroup: .topicgroup}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Creating {{site.data.keyword.satelliteshort}} configurations
{: #satcon-create}

With {{site.data.keyword.satelliteshort}} Config, you create a configuration to specify what Kubernetes resources you want to deploy to a group of {{site.data.keyword.openshiftlong_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}.
{: shortdesc}

Before you begin, make sure that you have the following permissions. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).
- The **Editor** {{site.data.keyword.cloud_notm}} IAM platform role for the **Configuration** resource in {{site.data.keyword.satellitelong_notm}}.
- The **Editor** {{site.data.keyword.cloud_notm}} IAM platform role for the **Subscription** resource in {{site.data.keyword.satellitelong_notm}}.
- The **Editor** {{site.data.keyword.cloud_notm}} IAM platform role for the **Clustergroup** resource in {{site.data.keyword.satellitelong_notm}}.
- The **Viewer** {{site.data.keyword.cloud_notm}} IAM platform role for the **Cluster** resource in {{site.data.keyword.satellitelong_notm}}.

## Creating {{site.data.keyword.satelliteshort}} configurations from the console
{: #create-satconfig-ui}

Use the {{site.data.keyword.satelliteshort}} console to create a configuration and upload the Kubernetes resource definition that you want to deploy to your {{site.data.keyword.openshiftlong_notm}} clusters.
{: shortdesc}

To create the configuration,

1. [Set up your clusters to use with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig). The setup includes creating a cluster group and granting {{site.data.keyword.satelliteshort}} Config access to your clusters.
2. Create a {{site.data.keyword.satelliteshort}} configuration.
    1. From the [{{site.data.keyword.satelliteshort}} configurations dashboard](https://cloud.ibm.com/satellite/configuration){: external}, click **Create configuration**.
    2. Enter a name for your configuration and click **Create**.
3. Upload or create a YAML file for the Kubernetes resources that you want to deploy to your clusters. This file is referred to as version.
    1. From the actions menu of a configuration, click **Add version**.
    2. Enter a name and an optional description for your version.
    3. Upload a Kubernetes resource YAML file or use the editor to enter your Kubernetes resource definition directly. Make sure to specify the Kubernetes namespace where you want your resource to be deployed. If you do not specify a namespace, the resource is deployed to the `razeedeploy` namespace by default. 
    4. **Optional**: To view the resources after they are created in the cluster through the {{site.data.keyword.satelliteshort}} Config dashboard, add the `razee/watch-resource=lite` label to the `metadata.labels` section of your YAML file or [choose another option to view your deployed resources](/docs/satellite?topic=satellite-satcon-manage#satconfig-resources), such as adding a configmap to your cluster. 
    5. Click **Add** to add the Kubernetes resource definition as a version to your configuration.
4. Subscribe your cluster group to the {{site.data.keyword.satelliteshort}} configuration to deploy the Kubernetes resources to your clusters.
    1. Select the configuration that you created to see the configuration details.
    2. Click **Create subscription**.
    3. Enter a name for your subscription and select the version name and the cluster group that you created earlier.
    4. Click **Create** to create the subscription. After you create the subscription, {{site.data.keyword.satelliteshort}} Config automatically downloads the Kubernetes resource YAML file for the version that you specified and starts applying this YAML file across all clusters that belong to the cluster group. This process takes a few minutes to complete. In addition, information about all Kubernetes resources that you create are sent back from your clusters to {{site.data.keyword.satelliteshort}} Config and can be reviewed in the {{site.data.keyword.satelliteshort}} [**Cluster resources**](https://cloud.ibm.com/satellite/resources) dashboard.
5. Select your subscription to see the subscription details and the rollout status of your Kubernetes resource deployment. If errors occur during the deployment, such as YAML files with formatting errors or unsupported API version values, you can view the error message in the **Message** column of your subscription details.

<br />

## Creating {{site.data.keyword.satelliteshort}} configurations from the CLI
{: #create-satconfig-cli}

Use the CLI plug-in for {{site.data.keyword.satelliteshort}} commands to create a configuration and upload the Kubernetes resource definition that you want to deploy to your {{site.data.keyword.openshiftlong_notm}} clusters.
{: shortdesc}

To create the configuration:

1. [Set up your clusters to use with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig). The setup includes creating a cluster group and granting {{site.data.keyword.satelliteshort}} Config access to your clusters.
2. Add {{site.data.keyword.openshiftlong_notm}} clusters to your cluster group. The clusters can run in your location or in {{site.data.keyword.cloud_notm}}.
    1. List the {{site.data.keyword.openshiftlong_notm}} clusters that are registered with the {{site.data.keyword.satelliteshort}} Config component and note their ID.
        ```
        ibmcloud sat cluster ls
        ```
        {: pre}

    2. Add the cluster to your cluster group.    
        ```
        ibmcloud sat group attach --cluster <cluster_ID> --group <cluster_group_name>
        ```
        {: pre}

    3. Verify that your cluster is successfully added to your cluster group.
        ```
        ibmcloud sat group get --group <cluster_group_name>
        ```
        {: pre}

3. Create a {{site.data.keyword.satelliteshort}} configuration.
    ```
    ibmcloud sat config create --name <configuration_name>
    ```
    {: pre}

    Example output:
    ```
    Creating configuration...
    OK
    Configuration <configuration_name> was successfully created with ID 116fffde-0835-467c-8987-67dd42e4e393.
    ```
    {: screen}

4. Upload a Kubernetes resource file to your configuration. Make sure to specify the Kubernetes namespace where you want your resource to be deployed. If you do not specify a namespace, the resource is deployed to the `razeedeploy` namespace by default. Review the command options by running `ibmcloud sat config version create`.

    To view the resources after they are created in the cluster through the {{site.data.keyword.satelliteshort}} Config dashboard, add the `razee/watch-resource=lite` label to the `metadata.labels` section of your YAML file or [choose another option to view your deployed resources](/docs/satellite?topic=satellite-satcon-manage#satconfig-resources), such as adding a configmap to your cluster. 
    {: tip}

    ```
    ibmcloud sat config version create --name <version_name> --config <configuration_name_or_ID> --file-format yaml --read-config <file_path>
    ```
    {: pre}

    | Component | Description | 
    |--------------------|------------------|
    | `--name <version_name>` | Enter a name for your configuration version. | 
    | `--config <configuration_name_or_ID>` | Enter the name or ID of the {{site.data.keyword.satelliteshort}} configuration that you created earlier. |
    | `--read-config <file_path>` | Enter the relative file path to the Kubernetes resource file on your local machine. | 
    {: caption="Understanding this command's components" caption-side="top"}

    **Example output**
    ```
    Creating configuration version...
    OK
    Configuration Version <version_name> was successfully created with ID ad5ae7a9-4f74-486c-816a-32de98de00df.
    ```
    {: screen}

5. Subscribe your cluster group to the {{site.data.keyword.satelliteshort}} configuration. After you create the subscription, {{site.data.keyword.satelliteshort}} Config automatically downloads the Kubernetes resource file for the version that you specified and starts applying this file across all clusters that belong to the cluster group. This process takes a few minutes to complete. In addition, information about all Kubernetes resources that you create are sent back from your clusters to {{site.data.keyword.satelliteshort}} Config and can be reviewed in the {{site.data.keyword.satelliteshort}} [**Cluster resources**](https://cloud.ibm.com/satellite/resources) dashboard. Review the command options by running `ibmcloud sat subscription create`.

    ```
    ibmcloud sat subscription create --group <cluster_group_name> --config <configuration_name_or_ID> --name <subscription_name> --version <version_name_or_ID>
    ```
    {: pre}

    | Component | Description | 
    |--------------------|------------------|
    | `--group <cluster_group_name>` | Enter the name of the cluster group where you want to deploy your Kubernetes resources. | 
    | `--config <configuration_name_or_ID>` | Enter the name or ID of the {{site.data.keyword.satelliteshort}} configuration that you created earlier. |
    | `--name <subscription_name>` | Enter a name for your {{site.data.keyword.satelliteshort}} subscription. |
    | `--version <version_name_or_ID>` | Enter the name or ID of the Kubernetes resource definition that you added as a version to your configuration. To list available versions, run `ibmcloud sat config get --config <configuration_name_or_ID>` | 
    {: caption="Understanding this command's components" caption-side="top"}

    **Example output**
    ```
    Creating subscription...
    OK
    Subscription <subscription_name> was successfully created with ID f6114bd5-f71e-4335-b034-ca45fa3cab81.
    ```
    {: screen}

6. Follow step 5 in [Creating {{site.data.keyword.satelliteshort}} configurations from the console](/docs/satellite?topic=satellite-setup-clusters-satconfig) to review the rollout status of your Kubernetes resources.


