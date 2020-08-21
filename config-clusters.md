---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-21"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:step: data-tutorial-type='step'}



# Deploying Kubernetes resources across clusters with {{site.data.keyword.satelliteshort}} configurations
{: #cluster-config}

Use a {{site.data.keyword.satelliteshort}} configuration to specify what Kubernetes resources you want to deploy to a group of {{site.data.keyword.openshiftlong_notm}} clusters that run in your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}.
{: shortdesc}

## Understanding {{site.data.keyword.satelliteshort}} configurations
{: #understanding-satcon}

To understand how {{site.data.keyword.satelliteshort}} configurations work, review the following process flow and key concepts.
{: shortdesc}

### How {{site.data.keyword.satelliteshort}} configurations work
{: #satcon-flow}

The following image shows how you can use a {{site.data.keyword.satelliteshort}} configuration to consistently deploy Kubernetes resources across {{site.data.keyword.openshiftlong_notm}} clusters.
{: shortdesc}

![How {{site.data.keyword.satelliteshort}} configurations work](/images/satcon.png)

1. A user creates a {{site.data.keyword.satelliteshort}} configuration and uploads Kubernetes resource files to it. Each Kubernetes resource file that you upload represents a version within the configuration.
2. The user creates a {{site.data.keyword.satelliteshort}} subscription and specifies which Kubernetes resource file version is deployed to a group of {{site.data.keyword.openshiftlong_notm}} clusters.
3. The version of the Kubernetes resource file that is specified in the subscription is automatically deployed to the {{site.data.keyword.openshiftlong_notm}} clusters that belong to the selected cluster group. The clusters in the cluster group can run in your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}.

### Key concepts
{: #satcon-terminology}

Review the following key concepts that are used when you create a {{site.data.keyword.satelliteshort}} configuration.
{: shortdesc}

|Term|Description|
|---------|-------------------|
|Configuration|A {{site.data.keyword.satelliteshort}} configuration lets you upload or create Kubernetes resource YAML file versions that you want to deploy to a group of {{site.data.keyword.openshiftlong_notm}} clusters. The version that you upload is not applied to your cluster until you add a subscription to your configuration.|
|Version|A version represents a Kubernetes resource YAML file that you uploaded or manually created for a {{site.data.keyword.satelliteshort}} configuration. You can include any Kubernetes resource in your version and upload as many versions to a configuration as you like.  |
|Subscription|A {{site.data.keyword.satelliteshort}} subscription is created for a {{site.data.keyword.satelliteshort}} configuration and specifies which version of the Kubernetes resource that you uploaded is deployed to one or more cluster groups. The {{site.data.keyword.openshiftlong_notm}} clusters in your cluster group can in your {{site.data.keyword.satelliteshort}} or in {{site.data.keyword.cloud_notm}}. To include {{site.data.keyword.openshiftlong_notm}} clusters that you run in {{site.data.keyword.cloud_notm}}, you must register the cluster with the {{site.data.keyword.satelliteshort}} Config component and install the {{site.data.keyword.satelliteshort}} Config agent on this cluster. For more information, see [Registering existing {{site.data.keyword.openshiftlong_notm}} clusters with {{site.data.keyword.satelliteshort}} Config](#existing-openshift-clusters).|
|Cluster groups|A cluster group specifies a set of {{site.data.keyword.openshiftlong_notm}} clusters that are registered with the {{site.data.keyword.satelliteshort}} Config component and that are included in a {{site.data.keyword.satelliteshort}} configuration. {{site.data.keyword.openshiftlong_notm}} clusters that run in your location are automatically registered and can be added to a cluster group. Clusters that run in {{site.data.keyword.cloud_notm}} must be [manually registered](#existing-openshift-clusters) with the {{site.data.keyword.satelliteshort}} Config component before you can add them to a cluster group. |

<br />


## Setting up clusters to use with {{site.data.keyword.satelliteshort}} Config
{: #setup-clusters-satconfig}

By default, clusters that you create in a {{site.data.keyword.satelliteshort}} location have {{site.data.keyword.satelliteshort}} Config components automatically installed. However, you must grant the service accounts that {{site.data.keyword.satelliteshort}} Config uses the appropriate access to view and manage Kubernetes resources in each cluster. You can also optionally create cluster groups to deploy resources to several clusters at once.
{: shortdesc}

If you do not grant {{site.data.keyword.satelliteshort}} Config access in each cluster, {{site.data.keyword.satelliteshort}} Config functionality does not work, such as not viewing or deploying Kubernetes resources for your clusters.
{: important}

* [Prerequisites](#setup-clusters-satconfig-prereq)
* [Setting up cluster groups](#setup-clusters-satconfig-groups)
* [Granting {{site.data.keyword.satelliteshort}} Config access to your clusters](#setup-clusters-satconfig-access)

### Prerequisites
{: #setup-clusters-satconfig-prereq}

*  If you have {{site.data.keyword.openshiftlong_notm}} clusters that run in {{site.data.keyword.cloud_notm}} (not your {{site.data.keyword.satelliteshort}} location), [register the clusters](#existing-openshift-clusters).
*  Make sure that you [have the following permissions](/docs/satellite?topic=satellite-iam#iam-assign) in {{site.data.keyword.cloud_notm}} IAM.
   - The **Administrator** {{site.data.keyword.cloud_notm}} IAM platform role for the **Cluster** resource in {{site.data.keyword.satellitelong_notm}}.
   - The **Administrator** {{site.data.keyword.cloud_notm}} IAM platform role for the **Clustergroup** resource in {{site.data.keyword.satellitelong_notm}}.
   - The **Manager** {{site.data.keyword.cloud_notm}} IAM service role for the cluster in {{site.data.keyword.openshiftlong_notm}}.

### Setting up cluster groups
{: #setup-clusters-satconfig-groups}

Create a cluster group. The cluster group specifies all {{site.data.keyword.openshiftlong_notm}} clusters that you want to include into the deployment of your Kubernetes resources. The clusters can run in your {{site.data.keyword.satelliteshort}} location or in {{site.data.keyword.cloud_notm}}.
{: shortdesc}

**From the console**:
1. From the [{{site.data.keyword.satelliteshort}} cluster dashboard](https://cloud.ibm.com/satellite/clusters){: external}, switch to the **Cluster group** tab and click **Create cluster group**.
2. Enter a name for your cluster group and click **Create**.
3. Go to the **Clusters** tab and find the {{site.data.keyword.openshiftlong_notm}} clusters that you want to add to your cluster group.
4. From the actions menu, click **Add to group** and choose the name of the cluster group where you want to add the cluster.

**From the CLI**:
```
ibmcloud sat cluster-group create --name <cluster_group_name>
```
{: pre}
Example output:
```
Creating cluster group...
OK
Created cluster group 'mygroup' with ID '6492111d-3211-4ed2-8f2e-4b99907476a9'.
```
{: screen}

### Granting {{site.data.keyword.satelliteshort}} Config access to your clusters
{: #setup-clusters-satconfig-access}

For each cluster in the cluster group, grant {{site.data.keyword.satelliteshort}} Config access to manage Kubernetes resources.
{: shortdesc}

1. Access each cluster in the cluster group. For more access options, see [Accessing {{site.data.keyword.openshiftshort}} clusters](/docs/openshift?topic=openshift-access_cluster).
   1. From the [{{site.data.keyword.openshiftlong_notm}} clusters page](https://cloud.ibm.com/kubernetes/clusters?platformType=openshift), click the cluster.
   2. Click **OpenShift web console**.
   3. Click **IAM#username@email.com > Copy Login Command**.
   4. Click **Display Token**.
   5. Copy the `oc login` token command.
   6. Run the `oc login` token command in your CLI to log in to your cluster.
2. For each cluster in the cluster group, grant {{site.data.keyword.satelliteshort}} Config access to manage Kubernetes resources. Choose from the following options: cluster admin access, custom access that is cluster-wide, or custom access that is scoped to a project. For more information, see the [Kubernetes documentation](https://kubernetes.io/docs/reference/access-authn-authz/authorization/){: external}.

   If you choose a custom access option, some {{site.data.keyword.satelliteshort}} Config components might not work. For example, if you grant access only to view certain resources, you cannot use subscriptions to create Kubernetes resources in your cluster group. To view an inventory of your Kubernetes resources in a cluster, {{site.data.keyword.satelliteshort}} Config must have an appropriate role that is bound to the `razee-viewer` service account. To deploy Kubernetes resources to a cluster via subscriptions, {{site.data.keyword.satelliteshort}} Config must have an appropriate role that is bound to the `razee-editor` service account.
   {: note}

   *  **Cluster admin access**: Grant the {{site.data.keyword.satelliteshort}} Config service accounts access to the cluster admin role. 
      ```
      kubectl create clusterrolebinding razee-cluster-admin --clusterrole=razee-cluster-admin --serviceaccount=razeedeploy:razee-viewer --serviceaccount=razeedeploy:razee-editor --serviceaccount=razeedeploy:razee-satcon
      ```
      {: pre}
   *  **Custom access, cluster-wide**: Create custom RBAC policies to grant {{site.data.keyword.satelliteshort}} Config access to the actions and Kubernetes resources that you want for the cluster.
      1. Create a cluster role with the actions and resources that you want to grant. For example, the following command creates a viewer role so that {{site.data.keyword.satelliteshort}} Config can list all the Kubernetes resources in a cluster, but cannot modify them.
         ```
         kubectl create clusterrole razee-viewer --verb=get,list,watch --resource="*.*"
         ```
         {: pre}

         <table summary="This table is read from left to right. The first column has the error message. The second column has the description of the how to resolve the error.">
         <caption>Understanding this command's components</caption>
         <thead>
         <th>Component</th>
         <th>Description</th>
         </thead>
         <tbody>
         <tr>
         <td><code>razee-viewer</code></td>
         <td>The name of the cluster role, such as <code>razee-viewer</code>.</td>
         </tr>
         <tr>
         <td><code>--verb=get,list,watch</code></td>
         <td>A comma-separated list of actions that the role authorizes. In this example, the action verbs are for roles typical for a viewer or auditor, <code>get,list,watch</code>. For other possible verbs, see the [Kubernetes documentation](https://kubernetes.io/docs/reference/access-authn-authz/authorization/#determine-the-request-verb){: external}.</td>
         </tr>
         <tr>
         <td><code>--resource="&#42;.&#42;"</code></td>
         <td>A comma-separated list of the Kubernetes resources that the role authorizes actions to. In this example, access is granted for all Kubernetes resources in all API groups, <code>&#42;.&#42;</code>. For other possible resources, run <code>kubectl api-resources -o wide</code>.</td>
         </tr>
         </tbody>
         </table>
      2. Create a cluster role binding that binds the {{site.data.keyword.satelliteshort}} Config service account to the cluster role that you previously created. Now, {{site.data.keyword.satelliteshort}} Config has the custom access to the cluster.
         ```
         kubectl create clusterrolebinding razee-viewer --clusterrole=razee-viewer --serviceaccount=razeedeploy:razee-viewer
         ```
         {: pre}

         <table summary="This table is read from left to right. The first column has the error message. The second column has the description of the how to resolve the error.">
         <caption>Understanding this command's components</caption>
         <thead>
         <th>Component</th>
         <th>Description</th>
         </thead>
         <tbody>
         <tr>
         <td><code>razee-viewer</code></td>
         <td>The name of the cluster role binding, such as <code>razee-viewer</code>.</td>
         </tr>
         <tr>
         <td><code>--clusterrole=razee-viewer</code></td>
         <td>The name of the cluster role that you previously created, such as <code>razee-viewer</code>.</td>
         </tr>
         <tr>
         <td><code>--serviceaccount=razeedeploy:razee-viewer</code></td>
         <td>The name of one of the service accounts that the {{site.data.keyword.satelliteshort}} Config components are set up by default to use, either <code>razeedeploy:razee-viewer</code> or <code>razeedeploy:razee-editor</code>.</td>
         </tr>
         </tbody>
         </table>

   *  **Custom access, scoped to a project**: Create custom RBAC policies to grant {{site.data.keyword.satelliteshort}} Config access to the actions, Kubernetes resources, and projects (namespaces) that you want.
      1. Create a role with the actions and resources that you want to grant in the project that you want to scope the role to. For example, the following command creates an editor role so that {{site.data.keyword.satelliteshort}} Config can deploy and update all the Kubernetes resources in the project.
         ```
         kubectl create role razee-editor --namespace=default --verb=get,list,watch,create,update,patch,delete --resource="*.*"
         ```
         {: pre}

         <table summary="This table is read from left to right. The first column has the error message. The second column has the description of the how to resolve the error.">
         <caption>Understanding this command's components</caption>
         <thead>
         <th>Component</th>
         <th>Description</th>
         </thead>
         <tbody>
         <tr>
         <td><code>razee-editor</code></td>
         <td>The name of the cluster role, such as <code>razee-editor</code>.</td>
         </tr>
         <tr>
         <td><code>--namespace default</code></td>
         <td>The project (namespace) to scope the role to, such as <code>default</code>.</td>
         </tr>
         <tr>
         <td><code>--verb=get,list,watch,create,update,patch,delete</code></td>
         <td>A comma-separated list of actions that the role authorizes. In this example, the action verbs are for roles typical for an editor, <code>get,list,watch,create,update,patch,delete</code>. For other possible verbs, see the [Kubernetes documentation](https://kubernetes.io/docs/reference/access-authn-authz/authorization/#determine-the-request-verb){: external}.</td>
         </tr>
         <tr>
         <td><code>--resource="&#42;.&#42;"</code></td>
         <td>A comma-separated list of the Kubernetes resources that the role authorizes actions to. In this example, access is granted for all Kubernetes resources in all API groups, <code>&#42;.&#42;</code>. For other possible resources, run <code>kubectl api-resources -o wide</code>.</td>
         </tr>
         </tbody>
         </table>
      2. Create a role binding that binds the {{site.data.keyword.satelliteshort}} Config service account to the cluster role that you previously created. Now, {{site.data.keyword.satelliteshort}} Config has the custom access to the cluster.
         ```
         kubectl create rolebinding razee-editor --namespace=default --role=razee-editor --serviceaccount=razeedeploy:razee-editor
         ```
         {: pre}

         <table summary="This table is read from left to right. The first column has the error message. The second column has the description of the how to resolve the error.">
         <caption>Understanding this command's components</caption>
         <thead>
         <th>Component</th>
         <th>Description</th>
         </thead>
         <tbody>
         <tr>
         <td><code>razee-editor</code></td>
         <td>The name of the role binding, such as <code>razee-editor</code>.</td>
         </tr>
         <tr>
         <td><code>--namespace default</code></td>
         <td>The project (namespace) to scope the role binding to, such as <code>default</code>. The namespace much match the namespace that the role is in.</td>
         </tr>
         <tr>
         <td><code>--role=razee-editor</code></td>
         <td>The name of the role that you previously created, such as <code>razee-editor</code>.</td>
         </tr>
         <tr>
         <td><code>--serviceaccount=razeedeploy:razee-editor</code></td>
         <td>The name of one of the service accounts that the {{site.data.keyword.satelliteshort}} Config components are set up by default to use, either <code>razeedeploy:razee-viewer</code> or <code>razeedeploy:razee-editor</code>.</td>
         </tr>
         </tbody>
         </table>

3. Create a configuration and subscribe your cluster group to deploy Kubernetes resources to your clusters[from the console](#create-satconfig-ui) or [the CLI](#create-satconfig-cli).

<br />


## Creating {{site.data.keyword.satelliteshort}} configurations from the console
{: #create-satconfig-ui}

Use the {{site.data.keyword.satelliteshort}} console to create a configuration and upload the Kubernetes resource definition that you want to deploy to your {{site.data.keyword.openshiftlong_notm}} clusters.
{: shortdesc}

Before you begin, make sure that you have the following permissions:
- The **Editor** {{site.data.keyword.cloud_notm}} IAM platform role for the **Configuration** resource in {{site.data.keyword.satellitelong_notm}}.
- The **Editor** {{site.data.keyword.cloud_notm}} IAM platform role for the **Subscription** resource in {{site.data.keyword.satellitelong_notm}}.
- The **Editor** {{site.data.keyword.cloud_notm}} IAM platform role for the **Clustergroup** resource in {{site.data.keyword.satellitelong_notm}}.
- The **Viewer** {{site.data.keyword.cloud_notm}} IAM platform role for the **Cluster** resource in {{site.data.keyword.satellitelong_notm}}.

To create the configuration:

1. [Set up your clusters to use with {{site.data.keyword.satelliteshort}} Config](#setup-clusters-satconfig). The setup includes creating a cluster group and granting {{site.data.keyword.satelliteshort}} Config access to your clusters.
2. Create a {{site.data.keyword.satelliteshort}} configuration.
   1. From the [{{site.data.keyword.satelliteshort}} configurations dashboard](https://cloud.ibm.com/satellite/configuration){: external}, click **Create configuration**.
   2. Enter a name for your configuration and click **Create**.
3. Upload or create a YAML file for the Kubernetes resources that you want to deploy to your clusters. This file is referred to as version.
   1. From the actions menu of a configuration, click **Add version**.
   2. Enter a name and an optional description for your version.
   3. Upload a Kubernetes resource YAML file or use the editor to enter your Kubernetes resource definition directly.
   4. Click **Add** to add the Kubernetes resource definition as a version to your configuration.
4. Subscribe your cluster group to the {{site.data.keyword.satelliteshort}} configuration to deploy the Kubernetes resources to your clusters.
   1. Select the configuration that you created to see the configuration details.
   2. Click **Create subscription**.
   3. Enter a name for your subscription and select the version name and the cluster group that you created earlier.
   4. Click **Create** to create the subscription. After you create the subscription, {{site.data.keyword.satelliteshort}} Config automatically downloads the Kubernetes resource YAML file for the version that you specified and starts applying this YAML file across all clusters that belong to the cluster group. This process takes a few minutes to complete. In addition, information about all Kubernetes resources that you create are sent back from your clusters to {{site.data.keyword.satelliteshort}} Config and can be reviewed in the {{site.data.keyword.satelliteshort}} [**Cluster resources**](https://cloud.ibm.com/satellite/resources) dashboard.
5. Select your subscription to see the subscription details and the rollout status of your Kubernetes resource deployment. If errors occur during the deployment, you can view the error message in the **Message** column of your subscription details.

<br />


## Creating {{site.data.keyword.satelliteshort}} configurations from the CLI
{: #create-satconfig-cli}

Use the {{site.data.keyword.satelliteshort}} CLI to create a configuration and upload the Kubernetes resource definition that you want to deploy to your {{site.data.keyword.openshiftlong_notm}} clusters.
{: shortdesc}

Before you begin, make sure that you have the following permissions:
- The **Editor** {{site.data.keyword.cloud_notm}} IAM platform role for the **Configuration** resource in {{site.data.keyword.satellitelong_notm}}.
- The **Editor** {{site.data.keyword.cloud_notm}} IAM platform role for the **Subscription** resource in {{site.data.keyword.satellitelong_notm}}.
- The **Editor** {{site.data.keyword.cloud_notm}} IAM platform role for the **Clustergroup** resource in {{site.data.keyword.satellitelong_notm}}.
- The **Viewer** {{site.data.keyword.cloud_notm}} IAM platform role for the **Cluster** resource in {{site.data.keyword.satellitelong_notm}}.

To create the configuration:

1. [Set up your clusters to use with {{site.data.keyword.satelliteshort}} Config](#setup-clusters-satconfig). The setup includes creating a cluster group and granting {{site.data.keyword.satelliteshort}} Config access to your clusters.
2. Add {{site.data.keyword.openshiftlong_notm}} clusters to your cluster group. The clusters can run in your location or in {{site.data.keyword.cloud_notm}}.
   1. List the {{site.data.keyword.openshiftlong_notm}} clusters that are registered with the {{site.data.keyword.satelliteshort}} Config component and note their ID.
      ```
      ibmcloud sat cluster ls
      ```
      {: pre}

   2. Add the cluster to your cluster group.    
      ```
      ibmcloud sat cluster-group attach --cluster <cluster_ID> --cluster-group <cluster_group_name>
      ```
      {: pre}

   3. Verify that your cluster is successfully added to your cluster group.
      ```
      ibmcloud sat cluster-group get --cluster-group <cluster_group_name>
      ```
      {: pre}

3. Create a {{site.data.keyword.satelliteshort}} configuration.
   ```
   ibmcloud sat config configuration create --name <configuration_name>
   ```
   {: pre}

   Example output:
   ```
   Creating configuration...
   OK
   Configuration <configuration_name> was successfully created with ID 116fffde-0835-467c-8987-67dd42e4e393.
   ```
   {: screen}

4. Upload a Kubernetes resource file to your configuration.
   ```
   ibmcloud sat config configuration version create --name <version_name> --configuration <configuration_name_or_ID> --type <type> --read-config <file_path>
   ```
   {: pre}

   Example output:
   ```
   Creating configuration version...
   OK
   Configuration Version <version_name> was successfully created with ID ad5ae7a9-4f74-486c-816a-32de98de00df.
   ```
   {: screen}

   <table>
   <caption>Understanding the command's components</caption>
   <col width="25%">
   <thead>
   <th>Parameter</th>
   <th>Description</th>
   </thead>
   <tbody>
   <tr>
   <td><code>--name <em>&lt;version_name&gt;</em></code></td>
   <td>Enter a name for your version.</td>
   </tr>
   <tr>
   <td><code>--configuration <em>&lt;configuration_name_or_ID&gt;</em></code></td>
   <td>Enter the name or ID of the {{site.data.keyword.satelliteshort}} configuration that you created earlier.</td>
   </tr>
   <tr>
   <td><code>--type <em>&lt;type&gt;</em></code></td>
   <td>Enter the file extension of your Kubernetes resource file. Supported extensions are <code>yaml</code>. </td>
   </tr>
   <tr>
   <td><code>--read-config <em>&lt;file_path&gt;</em></code></td>
   <td>Enter the relative file path to the Kubernetes resource file on your local machine. </td>
   </tr>
  </tbody>
  </table>

5. Subscribe your cluster group to the {{site.data.keyword.satelliteshort}} configuration. After you create the subscription, {{site.data.keyword.satelliteshort}} Config automatically downloads the Kubernetes resource file for the version that you specified and starts applying this file across all clusters that belong to the cluster group. This process takes a few minutes to complete. In addition, information about all Kubernetes resources that you create are sent back from your clusters to {{site.data.keyword.satelliteshort}} Config and can be reviewed in the {{site.data.keyword.satelliteshort}} [**Cluster resources**](https://cloud.ibm.com/satellite/resources) dashboard.
   ```
   ibmcloud sat config subscription create --cluster-group <cluster_group_name> --configuration <configuration_name_or_ID> --name <subscription_name> --version <version_name_or_ID>
   ```
   {: pre}

   Example output:
   ```
   Creating subscription...
   OK
   Subscription <subscription_name> was successfully created with ID f6114bd5-f71e-4335-b034-ca45fa3cab81.
   ```
   {: screen}

   <table>
   <caption>Understanding the command's components</caption>
   <col width="25%">
   <thead>
   <th>Parameter</th>
   <th>Description</th>
   </thead>
   <tbody>
   <tr>
   <td><code>--cluster-group <em>&lt;cluster_group_name&gt;</em></code></td>
   <td>Enter the name of the cluster group where you want to deploy your Kubernetes resources.</td>
   </tr>
   <tr>
   <td><code>--configuration <em>&lt;configuration_name_or_ID&gt;</em></code></td>
   <td>Enter the name or ID of the {{site.data.keyword.satelliteshort}} configuration that you created earlier.</td>
   </tr>
   <tr>
   <td><code>--name <em>&lt;subscription_name&gt;</em></code></td>
   <td>Enter a name for your {{site.data.keyword.satelliteshort}} subscription. </td>
   </tr>
   <tr>
   <td><code>--version <em>&lt;version_name_or_ID&gt;</em></code></td>
   <td>Enter the name or ID of the Kubernetes resource definition that you added as a version to your configuration. To list available versions, run <code>ibmcloud sat config configuration get --configuration &lt;configuration_name_or_ID&gt;</code>.</td>
   </tr>
  </tbody>
  </table>

6. Follow step 5 in [Creating {{site.data.keyword.satelliteshort}} configurations from the console](#create-satconfig-ui) to review the rollout status of your Kubernetes resources.

<br />


## Registering existing {{site.data.keyword.openshiftlong_notm}} clusters with {{site.data.keyword.satelliteshort}} Config
{: #existing-openshift-clusters}

You can make existing {{site.data.keyword.openshiftlong_notm}} clusters that you run in {{site.data.keyword.cloud_notm}} available to the {{site.data.keyword.satelliteshort}} Config component so that you can include them when you roll out Kubernetes resource versions across your clusters.
{: shortdesc}

After you complete these steps, the cluster can be added to a cluster group in your location and [subscribed to {{site.data.keyword.satelliteshort}} configurations](#create-satconfig-ui). However, you must still use {{site.data.keyword.openshiftlong_notm}} to manage the worker nodes for these clusters.
{: note}

1. Find the {{site.data.keyword.openshiftlong_notm}} cluster that you want to attach to your location. To list available clusters, run `ibmcloud oc cluster ls` or go to the [{{site.data.keyword.openshiftshort}} cluster dashboard](https://cloud.ibm.com/kubernetes/clusters?platformType=openshift){: external}.
2. From the {{site.data.keyword.satelliteshort}} [**Clusters**](https://cloud.ibm.com/satellite/clusters){: external} dashboard, click **Attach cluster**.
3. Enter the name of your cluster and click **Register cluster**. Registering a cluster creates an entry in the {{site.data.keyword.satelliteshort}} Config configmap. However, your cluster cannot be subscribed to a {{site.data.keyword.satelliteshort}} configuration until you install the {{site.data.keyword.satelliteshort}} Config agent in your cluster.
4. Copy the command that is displayed to you.
5. [Log in to your {{site.data.keyword.openshiftshort}} cluster](/docs/openshift?topic=openshift-access_cluster) and run the command in your cluster. The command creates the `razeedeploy` project, custom resource definitions and RBAC policies on your cluster that are required to make your cluster visible to {{site.data.keyword.satelliteshort}} Config.

   Example output:  
   ```
   namespace/razeedeploy created
   serviceaccount/razeedeploy-sa created
   clusterrole.rbac.authorization.k8s.io/razeedeploy-admin-cr created
   clusterrolebinding.rbac.authorization.k8s.io/razeedeploy-rb created
   job.batch/razeedeploy-job created
   ```
   {: screen}

6. Verify that all pods in the `razeedeploy` project are in a **Running** state.
   ```
   oc get pods -n razeedeploy
   ```
   {: pre}

   Example output:  
   ```
   NAME                                                  READY     STATUS      RESTARTS   AGE
   clustersubscription-687f644b7-lhc6q                   1/1       Running     0          1m
   featureflagsetld-controller-7d9cbc8b88-cjt75          1/1       Running     0          1m
   managedset-controller-5b9f46f68-xzrc2                 1/1       Running     0          1m
   mustachetemplate-controller-5cdb46d9dd-rdrwn          1/1       Running     0          1m
   razeedeploy-job-nhvsm                                 0/1       Completed   0          1m
   remoteresource-controller-6778d9684b-66qvx            1/1       Running     0          1m
   remoteresources3-controller-69bb556465-kqh2q          1/1       Running     0          1m
   remoteresources3decrypt-controller-7db5fdc864-xd27l   1/1       Running     0          1m
   watch-keeper-67f7d9d78-j442t                          1/1       Running     0          1m
   ```
   {: screen}

7. Verify that your cluster shows on the {{site.data.keyword.satelliteshort}} [**Clusters**](https://cloud.ibm.com/satellite/clusters){: external} dashboard.

8. Optional: Click on your cluster to view the Kubernetes resources that are deployed to the cluster.
