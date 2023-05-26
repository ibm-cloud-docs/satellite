---

copyright:
  years: 2022, 2023
lastupdated: "2023-05-26"

keywords: satellite, hybrid, multicloud, odf, openshift data foundation

subcollection: satellite

content-type: tutorial
services: satellite, containers, vpc
account-plan: paid
completion-time: 90m

---

{{site.data.keyword.attribute-definition-list}}


# Deploying OpenShift Data Foundation on {{site.data.keyword.satelliteshort}} clusters with Azure worker nodes
{: #odf-tutorial}
{: toc-content-type="tutorial"}
{: toc-services="satellite, containers, vpc, azure"}
{: toc-completion-time="90m"}

## Objectives
{: #odf-tutorial-objectives}

In this tutorial, you complete the following tasks to set up OpenShift Data Foundation.

- A {{site.data.keyword.satelliteshort}} Location that uses Azure hosts.
- A {{site.data.keyword.satelliteshort}} cluster that uses your Azure hosts as worker nodes.
- A {{site.data.keyword.satelliteshort}} storage configration that deploys the Azure Disk CSI driver to your cluster.
- A {{site.data.keyword.satelliteshort}} storage configration that deploys OpenShift Data Foundation to your cluster.



## Audience
{: #odf-tutorial-audience}

This tutorial is for location administrators who are using OpenShift Data Foundation and {{site.data.keyword.satelliteshort}} storage config for the first time.

## Deploy the Azure location template
{: #odf-tutorial-deploying-azure-location}
{: step}


1. [Verify you have the required permissions in your Azure account](/docs/satellite?topic=satellite-azure&interface=ui#infra-creds-azure).
1. [Follow the steps to deploy an Azure location by using the {{site.data.keyword.bpshort}} template](/docs/satellite?topic=satellite-azure&interface=ui#azure-template). Make sure to deploy hosts across 3 zones and select a VM size that has at least 16CPUs and 64GB RAM.


## Create a {{site.data.keyword.satelliteshort}} cluster that uses your Azure hosts
{: #odf-tutorial-deploying-azure-cluster}
{: step}

After setting up your location, you have unassigned hosts that can be used to create a cluster.

1. [Follow the steps to deploy a cluster to your location](/docs/openshift?topic=openshift-satellite-clusters#satcluster-create-console). Make sure to select the **Enable admin access for {{site.data.keyword.satelliteshort}} config** option.
1. Wait until your cluster finishes deploying then deploy the Azure Disk CSI driver.


## Deploy the Azure Disk CSI driver
{: #odf-tutorial-deploying-azure}
{: step}


1. [Follow the steps to create an assign an Azure Disk storage configuration](/docs/satellite?topic=satellite-storage-azuredisk-csi-driver&interface=ui#azuredisk-csi-driver-config-create-console) to your cluster.
1. Wait until the configuration is successfully assigned, then list the storage classes in your cluster.
    ```sh
    oc get sc
    ```
    {: pre}

1. Make sure the [Azure Disk storage classes](/docs/satellite?topic=satellite-storage-azuredisk-csi-driver&interface=ui#azure-disk-sc-ref) are deployed, then [deploy ODF](#odf-tutorial-deploying-odf).


## Deploy OpenShift Data Foundation
{: #odf-tutorial-deploying-odf}
{: step}

Follow the steps to create a {{site.data.keyword.satelliteshort}} storage configuration that uses the OpenShift Data Foundation for remote storage template. When you deploy ODF for remote storage you must provide a storage driver and a storage class that are used for provisioning app storage. In this example, you use the Azure Disk CSI driver and the `sat-azure-block-gold-metro` storage class that you deployed to your cluster in the previous step.


1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} click **Locations**, then click the location where you want to deploy OpenShift Data Foundation.
1. Click **Storage > Create storage configuration**.
1. On the **Basics** tab, enter a name for your configuration and select the **OpenShift Data Foundation for remote storage**, select the version that matches your cluster version, and click **Next**.
1. On the **Parameters** tab, enter `sat-azure-block-gold-metro` as the **OSD pod storage class**, and leave the remaining fields as their default values.
1. On the **Secrets** tab, enter your IAM API Key and click **Next**.
1. On the **Storage Classes** tab, review the storage classes that are deployed to your cluster. These storage classes are available for your apps.
1. On the **Assign to service** tab, select your cluster and click **Complete**.

## Verify your deployment
{: #odf-tutorial-verify}
{: step}

1. [Access your {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat).

1. Verify that the storage configuration resources are deployed. Run the following commands or review the `openshift-storage` namespace in the {{site.data.keyword.redhat_openshift_notm}} console.

    1. Get the `storagecluster` that you deployed and verify that the phase is `Ready`.
        ```sh
        oc get ocscluster -n openshift-storage
        ```
        {: pre}

        Example output
        ```sh
        NAME                 AGE   PHASE   EXTERNAL   CREATED AT             VERSION
        ocs-storagecluster   72m   Ready              2021-02-10T06:00:20Z   4.6.0
        ```
        {: screen}

    1. Get a list of pods in the `openshift-storage` namespace and verify that the status is `Running`.
        ```sh
        oc get pods -n openshift-storage
        ```
        {: pre}

        Example output
        ```sh
        NAME                                                              READY   STATUS      RESTARTS   AGE
        csi-cephfsplugin-9g2d5                                            3/3     Running     0          8m11s
        csi-cephfsplugin-g42wv                                            3/3     Running     0          8m11s
        csi-cephfsplugin-provisioner-7b89766c86-l68sr                     5/5     Running     0          8m10s
        csi-cephfsplugin-provisioner-7b89766c86-nkmkf                     5/5     Running     0          8m10s
        csi-cephfsplugin-rlhzv                                            3/3     Running     0          8m11s
        csi-rbdplugin-8dmxc                                               3/3     Running     0          8m12s
        csi-rbdplugin-f8c4c                                               3/3     Running     0          8m12s
        csi-rbdplugin-nkzcd                                               3/3     Running     0          8m12s
        csi-rbdplugin-provisioner-75596f49bd-7mk5g                        5/5     Running     0          8m12s
        csi-rbdplugin-provisioner-75596f49bd-r2p6g                        5/5     Running     0          8m12s
        noobaa-core-0                                                     1/1     Running     0          4m37s
        noobaa-db-0                                                       1/1     Running     0          4m37s
        noobaa-endpoint-7d959fd6fb-dr5x4                                  1/1     Running     0          2m27s
        noobaa-operator-6cbf8c484c-fpwtt                                  1/1     Running     0          9m41s
        ocs-operator-9d6457dff-c4xhh                                      1/1     Running     0          9m42s
        rook-ceph-crashcollector-169.48.170.83-89f6d7dfb-gsglz            1/1     Running     0          5m38s
        rook-ceph-crashcollector-169.48.170.88-6f58d6489-b9j49            1/1     Running     0          5m29s
        rook-ceph-crashcollector-169.48.170.90-866b9d444d-zk6ft           1/1     Running     0          5m15s
        rook-ceph-drain-canary-169.48.170.83-6b885b94db-wvptz             1/1     Running     0          4m41s
        rook-ceph-drain-canary-169.48.170.88-769f8b6b7-mtm47              1/1     Running     0          4m39s
        rook-ceph-drain-canary-169.48.170.90-84845c98d4-pxpqs             1/1     Running     0          4m40s
        rook-ceph-mds-ocs-storagecluster-cephfilesystem-a-6dfbb4fcnqv9g   1/1     Running     0          4m16s
        rook-ceph-mds-ocs-storagecluster-cephfilesystem-b-cbc56b8btjhrt   1/1     Running     0          4m15s
        rook-ceph-mgr-a-55cc8d96cc-vm5dr                                  1/1     Running     0          4m55s
        rook-ceph-mon-a-5dcc4d9446-4ff5x                                  1/1     Running     0          5m38s
        rook-ceph-mon-b-64dc44f954-w24gs                                  1/1     Running     0          5m30s
        rook-ceph-mon-c-86d4fb86-s8gdz                                    1/1     Running     0          5m15s
        rook-ceph-operator-69c46db9d4-tqdpt                               1/1     Running     0          9m42s
        rook-ceph-osd-0-6c6cc87d58-79m5z                                  1/1     Running     0          4m42s
        rook-ceph-osd-1-f4cc9c864-fmwgd                                   1/1     Running     0          4m41s
        rook-ceph-osd-2-dd4968b75-lzc6x                                   1/1     Running     0          4m40s
        rook-ceph-osd-prepare-ocs-deviceset-0-data-0-29jgc-kzpgr          0/1     Completed   0          4m51s
        rook-ceph-osd-prepare-ocs-deviceset-1-data-0-ckvv2-4jdx5          0/1     Completed   0          4m50s
        rook-ceph-osd-prepare-ocs-deviceset-2-data-0-szmjd-49dd4          0/1     Completed   0          4m50s
        rook-ceph-rgw-ocs-storagecluster-cephobjectstore-a-7f7f6df9rv6h   1/1     Running     0          3m44s
        rook-ceph-rgw-ocs-storagecluster-cephobjectstore-b-554fd9dz6dm8   1/1     Running     0          3m41s
        ```
        {: screen}

## Next Steps
{: #odf-tutorial-next-steps}
{: step}

To deploy an example application, see [Deploying an app that uses OpenShift Data Foundation](/docs/satellite?topic=satellite-storage-odf-remote#sat-storage-odf-remote-deploy).


