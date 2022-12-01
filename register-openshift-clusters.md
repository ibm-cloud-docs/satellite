copyright:
  years: 2020, 2022
lastupdated: "2022-11-30"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Registering clusters with {{site.data.keyword.satelliteshort}} Config
{: #register-openshift-clusters}

You can register any clusters you created or your existing {{site.data.keyword.openshiftlong_notm}} clusters with {{site.data.keyword.satelliteshort}} Config. Follow the steps to run the registration script in your cluster to set up the {{site.data.keyword.satelliteshort}} Config components and make the cluster visible in {{site.data.keyword.satelliteshort}}. 
{: shortdesc}

After you complete these steps, the cluster can be added to a cluster group in your location and [subscribed to {{site.data.keyword.satelliteshort}} configurations](/docs/satellite?topic=satellite-satcon-manage). However, you must still use {{site.data.keyword.openshiftlong_notm}} to manage the worker nodes for these clusters.
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

7. Verify your cluster's status on the {{site.data.keyword.satelliteshort}} [**Clusters**](https://cloud.ibm.com/satellite/clusters){: external} dashboard.
    
8. Optional: Click on your cluster to view the Kubernetes resources that are deployed to the cluster.
