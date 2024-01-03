---


copyright:
  years: 2020, 2024
lastupdated: "2024-01-03"

keywords: satellite config, satellite configurations, deploy kubernetes resources with satellite, satellite deploy apps, satellite subscription, satellite version

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Removing {{site.data.keyword.satelliteshort}} Config from your cluster
{: #remove-satconfig}

If you do not want your cluster to be available to {{site.data.keyword.satelliteshort}} Config, you can remove the {{site.data.keyword.satelliteshort}} Config components from your cluster. 
{: shortdesc}

You can remove {{site.data.keyword.satelliteshort}} Config components from only {{site.data.keyword.openshiftlong_notm}} clusters that you manually registered. If you created a cluster on {{site.data.keyword.satelliteshort}}-provided infrastructure, {{site.data.keyword.satelliteshort}} Config components are automatically set up in the location control plane and cannot be removed. 
{: note}

Removing {{site.data.keyword.satelliteshort}} Config components automatically removes all cluster resources that you created by using {{site.data.keyword.satelliteshort}} configurations. Make sure to back up any data or redeploy any apps that you want to keep. 
{: important}

1. [Log in to your {{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-access_cluster).
2. Remove all the {{site.data.keyword.satelliteshort}} Config components from your cluster by running a [{{site.data.keyword.satelliteshort}} Config removal job](https://config.satellite.cloud.ibm.com/api/cleanup/razeedeploy-job){: external}. 
    ```sh
    oc apply -f https://config.satellite.cloud.ibm.com/api/cleanup/razeedeploy-job
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
