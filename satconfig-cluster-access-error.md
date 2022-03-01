---

copyright:
  years: 2020, 2022
lastupdated: "2022-03-01"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does the list of Kubernetes resources not show up or update after registering my cluster with {{site.data.keyword.satelliteshort}} Config?
{: #satconfig-cluster-access-error}

When you register a cluster to use with {{site.data.keyword.satelliteshort}} Config, you do not see the cluster resources show up in the resources list. Even though you subscribe the cluster to a configuration, no Kubernetes resources are created or updated in the cluster.
{: tsSymptoms}

To use a cluster to use with {{site.data.keyword.satelliteshort}} Config, the proper components must be installed, and you must grant {{site.data.keyword.satelliteshort}} Config permissions in your cluster to manage Kubernetes resources.
{: tsCauses}

To resolve this issue, follow these steps.
{: tsResolve}

1. Re-attach the cluster to {{site.data.keyword.satelliteshort}} Config. For more information, see [Registering existing {{site.data.keyword.openshiftlong_notm}} clusters with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-satcon-existing).
    1. Get a `kubectl` command to register your cluster with {{site.data.keyword.satelliteshort}} Config.
        ```sh
        ibmcloud sat cluster register
        ```
        {: pre}

        Example output
        ```sh
        kubectl apply -f "https://config.satellite.cloud.ibm.com/api/install/razeedeploy-job?orgKey=<orgApiKey>&args=--clustersubscription=<number>&args=--featureflagsetld=<number>&args=--mustachetemplate=<number>&args=--managedset=<number>&args=--remoteresources<number>&args=--remoteresource=<number>&args=--watch-keeper=<number>"
        ```
        {: screen}

    2. Log in to your cluster. If you are not connected to your location host network, include the `--endpoint link` option. For more login options, see [Accessing {{site.data.keyword.redhat_openshift_notm}} clusters](/docs/openshift?topic=openshift-access_cluster).
        ```sh
        ibmcloud oc cluster config -c <cluster_name_or_ID> --admin [--endpoint link]
        ```
        {: pre}

    3. Run the `kubectl` command that you previously retrieved.
2. Grant {{site.data.keyword.satelliteshort}} Config permissions in your cluster to manage Kubernetes resources. The following command grants `cluster-admin` permissions for the entire cluster. For more options, see [Granting {{site.data.keyword.satelliteshort}} Config access to your clusters](/docs/satellite?topic=satellite-setup-clusters-satconfig#setup-clusters-satconfig-access).
    ```sh
    kubectl create clusterrolebinding razee-cluster-admin --clusterrole=razee-cluster-admin --serviceaccount=razeedeploy:razee-viewer --serviceaccount=razeedeploy:razee-editor --serviceaccount=razeedeploy:razee-satcon
    ```
    {: pre}


