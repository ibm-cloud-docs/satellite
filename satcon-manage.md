---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-15"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Managing your {{site.data.keyword.satelliteshort}} Config resources
{: #satcon-manage}

To create and roll out new versions of your applications, update your {{site.data.keyword.satelliteshort}} Config configurations and subscriptions. You can also use {{site.data.keyword.satelliteshort}} Config to review an inventory of all the resources that are managed by your configurations across clusters.
{: shortdesc}

## Reviewing resources that are managed by {{site.data.keyword.satelliteshort}} Config
{: #satconfig-resources}

You can use {{site.data.keyword.satelliteshort}} Config to review the Kubernetes resources that run in your registered clusters.
{: shortdesc}

Before you begin, make sure that you have the following permissions. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).
-  The **Administrator** platform role, **Reader** service role, or **Manager** service role in {{site.data.keyword.cloud_notm}} IAM for the **Resource** resource type in {{site.data.keyword.satellitelong_notm}}.
-  The appropriate permissions to enable the {{site.data.keyword.satelliteshort}} Config Watch-keeper, such as one of the following options.
    * The [permissions](/docs/satellite?topic=satellite-satcon-create) to create a configuration version and subscribe clusters to the version.
    * The **Writer** service role in {{site.data.keyword.cloud_notm}} IAM to the **Kubernetes Service** clusters that you want to watch resources for.

### Enabling Watch-keeper collection methods
{: #satconfig-enable-watchkeeper}

Review the [Watch-keeper collection methods](https://github.com/razee-io/WatchKeeper#collection-methods){: external} to decide how to set up Watch-keeper for your resources. Common use cases include,

#### Watch all the resources that my {{site.data.keyword.satelliteshort}} subscription creates
{: #satconfig-enable-watchkeeper-all}

1. [Add a ConfigMap](https://github.com/razee-io/WatchKeeper#watch-by-resource){: external} to the YAML file of your {{site.data.keyword.satelliteshort}} configuration version. 
2. In the `metadata.namespace` field of the ConfigMap, set the value to `razeedeploy`.
3. In the `data` section of the ConfigMap, add all the resources that you want {{site.data.keyword.satelliteshort}} Config to watch.
4. Subscribe your clusters to this version from the [console](/docs/satellite?topic=satellite-satcon-create#create-satconfig-ui) or [CLI](/docs/satellite?topic=satellite-satcon-create#create-satconfig-cli).

#### Watch a particular resource in my {{site.data.keyword.satelliteshort}} Config version
{: #satconfig-enable-watchkeeper-specific}

1. In the `metadata.labels` field of the Kubernetes resource in your {{site.data.keyword.satelliteshort}} Config version, set the value to `razee/watch-resource=lite`.
2. Subscribe your clusters to this version from the [console](/docs/satellite?topic=satellite-satcon-create#create-satconfig-ui) or [CLI](/docs/satellite?topic=satellite-satcon-create#create-satconfig-cli).

#### Watch a particular resource that I label in my cluster
{: #satconfig-enable-watchkeeper-label}

1. [Access your {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat).
2. Individually label the resource that you want {{site.data.keyword.satelliteshort}} Config to watch. For example, the following command watches a deployment that is called `nginx`.

    ```sh
    kubectl label deployment nginx razee/watch-resource=lite
    ```
    {: pre}
            
After you enable Watch-keeper for a resource, wait about an hour for the resources to display.

### Review the resources from {{site.data.keyword.satelliteshort}} Config
{: #satconfig-review-resources}

#### Reviewing resources from the console
{: #satconfig-review-resources-console}

You can review resources in several areas in the console as follows.

* From the [**Cluster resources** page](https://cloud.ibm.com/satellite/resources){: external}. 
* From the [**Configurations** page](https://cloud.ibm.com/satellite/configuration){: external}, click a configuration. Then, click a subscription and review the **Resources** tab.
* From the [**Clusters** page](https://cloud.ibm.com/satellite/clusters){: external}, click a cluster. Then, review the **Resources** tab.

#### Reviewing resources with the CLI
{: #satconfig-review-resources-cli}

Review resources with the CLI.

Use the **`ibmcloud sat resource ls`** [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-ls) and its options to list resources. To view the details of a particular resource, use the `ibmcloud sat resource get` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-resource-get).
