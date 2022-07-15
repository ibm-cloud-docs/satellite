---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-15"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Using {{site.data.keyword.satelliteshort}} Config with existing clusters in {{site.data.keyword.cloud_notm}} 
{: #satcon-existing}

You can make existing {{site.data.keyword.openshiftlong_notm}} clusters available to the {{site.data.keyword.satelliteshort}} Config component so that you can include them when you roll out Kubernetes resource versions across your clusters.
{: shortdesc}

## Registering existing {{site.data.keyword.redhat_openshift_notm}} clusters with {{site.data.keyword.satelliteshort}} Config
{: #register-openshift-clusters}

Run a script in your {{site.data.keyword.openshiftlong_notm}} cluster to set up the {{site.data.keyword.satelliteshort}} Config components and make the cluster visible in {{site.data.keyword.satelliteshort}}. 
{: shortdesc}

After you complete these steps, the cluster can be added to a cluster group in your location and [subscribed to {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-satcon-create). However, you must still use {{site.data.keyword.openshiftlong_notm}} to manage the worker nodes for these clusters.
{: note}

1. Find the cluster that you want to attach to your location. To list available clusters, run `ibmcloud oc cluster ls` or go to the [{{site.data.keyword.redhat_openshift_notm}} cluster dashboard](https://cloud.ibm.com/kubernetes/clusters?platformType=openshift){: external}.
2. From the {{site.data.keyword.satelliteshort}} [**Clusters**](https://cloud.ibm.com/satellite/clusters){: external} dashboard, click **Register cluster**.
3. Enter the name of your cluster and click **Register cluster**. Registering a cluster creates an entry in the {{site.data.keyword.satelliteshort}} Config ConfigMap. However, your cluster cannot be subscribed to a {{site.data.keyword.satelliteshort}} configuration until you install the {{site.data.keyword.satelliteshort}} Config agent in your cluster.
4. Copy the command that is displayed to you.
5. [Log in to your {{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-access_cluster) and run the command in your cluster. The command creates the `razeedeploy` project, custom resource definitions and RBAC policies on your cluster that are required to make your cluster visible to {{site.data.keyword.satelliteshort}} Config.

    Example output
    ```sh
    namespace/razeedeploy created
    serviceaccount/razeedeploy-sa created
    clusterrole.rbac.authorization.k8s.io/razeedeploy-admin-cr created
    clusterrolebinding.rbac.authorization.k8s.io/razeedeploy-rb created
    job.batch/razeedeploy-job created
    ```
    {: screen}

6. Verify that all pods in the `razeedeploy` project are in a **Running** state.

    ```sh
    oc get pods -n razeedeploy
    ```
    {: pre}

    Example output 
    ```sh
    NAME                                                  READY     STATUS      RESTARTS   AGE
    clustersubscription-c9cfb6f8b-7p5sw            1/1     Running     0          41m
    encryptedresource-controller-5c68f9746-vhdsk   1/1     Running     0          41m
    mustachetemplate-controller-5f9b554f69-f22v5   1/1     Running     0          41m
    razeedeploy-job-2wbd7                          0/1     Completed   0          47m
    remoteresource-controller-56bbfd6db6-mpngf     1/1     Running     0          41m
    watch-keeper-5d4dd9f56b-bt6jz                  1/1     Running     0          3m41s
    ```
    {: screen}

7. Verify that your cluster shows on the {{site.data.keyword.satelliteshort}} [**Clusters**](https://cloud.ibm.com/satellite/clusters){: external} dashboard.

8. Optional: Click on your cluster to view the Kubernetes resources that are deployed to the cluster.


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

