---

copyright:
  years: 2020, 2023
lastupdated: "2023-04-06"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Granting {{site.data.keyword.satelliteshort}} Config access to your clusters
{: #setup-clusters-satconfig}

By default, clusters that you create in a {{site.data.keyword.satelliteshort}} location have {{site.data.keyword.satelliteshort}} Config components automatically installed. However, you must grant the service accounts that {{site.data.keyword.satelliteshort}} Config uses the appropriate access to view and manage Kubernetes resources in each cluster. 
{: shortdesc}

{{site.data.keyword.satelliteshort}} Config requires admin access to your clusters to manage them. You can configure access in one of the following ways.
- **Automatically during cluster creation.** Choose this option if you want to use Satellite storage templates.
- **Manually after cluster creation.** Choose this option if you want more controlled access and do not plan on using Satellite storage templates.

If you do not grant {{site.data.keyword.satelliteshort}} Config access, you cannot later use the {{site.data.keyword.satelliteshort}} Config functionality to view or deploy Kubernetes resources for your clusters.

## Automatically granting {{site.data.keyword.satelliteshort}} Config access to your clusters
{: #auto-setup-clusters-satconfig}

You can give {{site.data.keyword.satelliteshort}} Config access to your cluster by specifying the relevant option when you create the cluster. 
{: shortdesc}

To give {{site.data.keyword.satelliteshort}} Config access to manage Kubernetes resources from the console, select **Enable cluster admin access for Satellite Config** when you create the cluster. To set up access with the CLI, specify the `--enable-config-admin` option when you create the cluster.

If you didn't give {{site.data.keyword.satelliteshort}} Config access at cluster creation time, follow the steps in [Manually granting {{site.data.keyword.satelliteshort}} Config access to your clusters](#manual-setup-clusters-satconfig).


## Manually granting {{site.data.keyword.satelliteshort}} Config access to your clusters
{: #manual-setup-clusters-satconfig}

If you did not grant {{site.data.keyword.satelliteshort}} Config access to your cluster during cluster creation time, you can still set up the access manually. 
{: shortdesc}

Choose from the following options.

- **Admin access when you create a {{site.data.keyword.satelliteshort}} cluster**: You can enable admin permissions when you create the cluster in the console or in the CLI by using the `--enable-config-admin` option in the `ibmcloud oc cluster create satellite` command. After creating the cluster, you must perform a one-time login by running `ibmcloud ks cluster config` in the command line.
- **Admin access for clusters in the public cloud**: See [Registering existing clusters with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-register-openshift-clusters).
- **Custom access, or access for {{site.data.keyword.satelliteshort}} clusters that you did not opt in for admin access**: Complete the following steps.

To customize access, or to add access for {{site.data.keyword.satelliteshort}} clusters that you did not opt in for admin access at cluster creation.

1. [Access your {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat).

2. For each cluster in the cluster group, grant {{site.data.keyword.satelliteshort}} Config access to manage Kubernetes resources. Choose from the following options: cluster admin access, custom access that is cluster-wide, or custom access that is scoped to a project. For more information, see the [Kubernetes documentation](https://kubernetes.io/docs/reference/access-authn-authz/authorization/){: external}.

    If you choose a custom access option, some {{site.data.keyword.satelliteshort}} Config components might not work. For example, if you grant access to view only certain resources, you cannot use subscriptions to create Kubernetes resources in your cluster group. To view an inventory of your Kubernetes resources in a cluster, {{site.data.keyword.satelliteshort}} Config must have an appropriate role that is bound to the `razee-viewer` service account. To deploy Kubernetes resources to a cluster by using subscriptions, {{site.data.keyword.satelliteshort}} Config must have an appropriate role that is bound to the `razee-editor` service account.
    {: note}


### Cluster admin access
{: #cluster-admin-access}

Grant the {{site.data.keyword.satelliteshort}} Config service accounts access to the cluster admin role.

```sh
kubectl create clusterrolebinding razee-cluster-admin --clusterrole=razee-cluster-admin --serviceaccount=razeedeploy:razee-viewer --serviceaccount=razeedeploy:razee-editor --serviceaccount=razeedeploy:razee-satcon
```
{: pre}

### Custom access, cluster-wide
{: #custom-access-cluster-wide}

Create custom RBAC policies to grant {{site.data.keyword.satelliteshort}} Config access to the actions and Kubernetes resources that you want for the cluster.

1. Create a cluster role with the actions and resources that you want to grant. For example, the following command creates a viewer role so that {{site.data.keyword.satelliteshort}} Config can list all the Kubernetes resources in a cluster, but cannot modify them.

    ```sh
    kubectl create clusterrole razee-viewer --verb=get,list,watch --resource="*.*"
    ```
    {: pre}

    | Component          | Description      | 
    |--------------------|------------------|
    | `razee-viewer` | The name of the cluster role, such as `razee-viewer`. | 
    | `--verb=get,list,watch` | A comma-separated list of actions that the role authorizes. In this example, the action verbs are for roles typical for a viewer or auditor, `get,list,watch`. For other possible verbs, see the [Kubernetes documentation](https://kubernetes.io/docs/reference/access-authn-authz/authorization/#determine-the-request-verb){: external}. |
    | `--resource="*.*"` | A comma-separated list of the Kubernetes resources that the role authorizes actions to. In this example, access is granted for all Kubernetes resources in all API groups, `"*.*"`. For other possible resources, run `kubectl api-resources -o wide`. |
    {: caption="Understanding this command's components" caption-side="bottom"}

2. Create a cluster role binding that binds the {{site.data.keyword.satelliteshort}} Config service account to the cluster role that you previously created. Now, {{site.data.keyword.satelliteshort}} Config has the custom access to the cluster.

    ```sh
    kubectl create clusterrolebinding razee-viewer --clusterrole=razee-viewer --serviceaccount=razeedeploy:razee-viewer
    ```
    {: pre}

    | Component          | Description      | 
    |--------------------|------------------|
    | `razee-viewer` | The name of the cluster role binding, such as `razee-viewer`. | 
    | `--clusterrole=razee-viewer` | The name of the cluster role that you previously created, such as `razee-viewer`. |
    | `--serviceaccount=razeedeploy:razee-viewer` | The name of one of the service accounts that the {{site.data.keyword.satelliteshort}} Config components are set up by default to use, either `razeedeploy:razee-viewer` or `razeedeploy:razee-editor`. |
    {: caption="Understanding this command's components" caption-side="bottom"}


### Custom access, scoped to a project
{: #custom-access-scoped-project}

Create custom RBAC policies to grant {{site.data.keyword.satelliteshort}} Config access to the actions, Kubernetes resources, and projects (namespaces) that you want.

1. Create a role with the actions and resources that you want to grant in the project that you want to scope the role to. For example, the following command creates an editor role so that {{site.data.keyword.satelliteshort}} Config can deploy and update all the Kubernetes resources in the project.

    ```sh
    kubectl create role razee-editor --namespace=default --verb=get,list,watch,create,update,patch,delete --resource="*.*"
    ```
    {: pre}

    | Component          | Description      | 
    |--------------------|------------------|
    | `razee-editor` | The name of the cluster role, such as `razee-editor`. | 
    | `--namespace default` | The project (namespace) to scope the role to, such as `default`. |
    | `--verb=get,list,watch,create,update,patch,delete` | A comma-separated list of actions that the role authorizes. In this example, the action verbs are for roles typical for an editor, `get,list,watch,create,update,patch,delete`. For other possible verbs, see the [Kubernetes documentation](https://kubernetes.io/docs/reference/access-authn-authz/authorization/#determine-the-request-verb){: external}. |
    | `--resource="*.*"` | A comma-separated list of the Kubernetes resources that the role authorizes actions to. In this example, access is granted for all Kubernetes resources in all API groups, `"*.*"`. For other possible resources, run `kubectl api-resources -o wide`. | 
    {: caption="Understanding this command's components" caption-side="bottom"}

2. Create a role binding that binds the {{site.data.keyword.satelliteshort}} Config service account to the cluster role that you previously created. Now, {{site.data.keyword.satelliteshort}} Config has the custom access to the cluster.

    ```sh
    kubectl create rolebinding razee-editor --namespace=default --role=razee-editor --serviceaccount=razeedeploy:razee-editor
    ```
    {: pre}

    | Component | Description | 
    |--------------------|------------------|
    | `razee-editor` | The name of the role binding, such as `razee-editor`. | 
    | `--namespace default` | The project (namespace) to scope the role binding to, such as `default`. The namespace must match the namespace that the role is in. |
    | `--role=razee-editor` | The name of the role that you previously created, such as `razee-editor`. |
    | `--serviceaccount=razeedeploy:razee-editor` | The name of one of the service accounts that the {{site.data.keyword.satelliteshort}} Config components are set up by default to use, either `razeedeploy:razee-viewer` or `razeedeploy:razee-editor`. | 
    {: caption="Understanding this command's components" caption-side="bottom"}
  

Continue to [set up cluster groups](/docs/satellite?topic=satellite-setup-clusters-satconfig-groups) and [register clusters with Satellite Config](/docs/satellite?topic=satellite-register-openshift-clusters).

