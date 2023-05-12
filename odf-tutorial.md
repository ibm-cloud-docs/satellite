---

copyright:
  years: 2022, 2023
lastupdated: "2023-05-12"

keywords: satellite, hybrid, multicloud, odf, openshift data foundation

subcollection: satellite

content-type: tutorial
services: satellite, containers, vpc
account-plan: paid
completion-time: 45m

---

{{site.data.keyword.attribute-definition-list}}


# Deploying OpenShift Data Foundation with {{site.data.keyword.block_storage_is_short}} on {{site.data.keyword.satelliteshort}} clusters 
{: #odf-tutorial}
{: toc-content-type="tutorial"}
{: toc-services="satellite, containers, vpc"}
{: toc-completion-time="45m"}

## Objectives
{: #odf-tutorial-objectives}

In this tutorial, you use the {{site.data.keyword.satelliteshort}} storage console to deploy the {{site.data.keyword.block_storage_is_short}} driver to a cluster in your location. After deploying the {{site.data.keyword.block_storage_is_short}} driver you then deploy OpenShift Data Foundation. With the {{site.data.keyword.satelliteshort}} console, you can manage [storage templates](/docs/satellite?topic=satellite-storage-template-ov) and deploy storage resources across multiple {{site.data.keyword.satelliteshort}} clusters in your location. For more information about OpenShift Data Foundation, see [Understanding OpenShift Data Foundation](/docs/openshift?topic=openshift-ocs-storage-prep). 


## Audience
{: #odf-tutorial-audience}

This tutorial is for location administrators who are using OpenShift Data Foundation and {{site.data.keyword.satelliteshort}} storage config for the first time.

## Prerequisites
{: #odf-tutorial-prereqs}

1. Before you can create a storage configuration, follow the steps to set up a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. You must have at least one cluster in your location. If you don't have any clusters in your location, [create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). Your cluster must meet the following requirements:
    * At least 3 worker nodes with at least 16CPUs and 64GB RAM per worker node
    * The hosts that you use as worker nodes must be IBM Cloud VPC Virtual Server Instances
1. [Set up {{site.data.keyword.satelliteshort}} Config on your clusters](/docs/satellite?topic=satellite-satcon-manage-direct-upload).


## Deploying the {{site.data.keyword.block_storage_is_short}} CSI driver
{: #odf-tutorial-deploying-vpc}
{: step}

Follow the steps to deploy the {{site.data.keyword.block_storage_is_short}} CSI driver by using the {{site.data.keyword.satelliteshort}} storage configuration console.
{: shortdesc}

The template is currently in beta. Do not use it for production workloads. 
{: beta}

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} click **Locations**, then click the location where you want to deploy OpenShift Data Foundation.
1. Click **Storage > Create storage configuration** 
1. On the **Basics** tab, enter a name for your configuration and select the **IBM VPC Block CSI driver**, leave the version as default, and click **Next**.
1. On the **Parameters** tab, enter the required parameters. 
    IAM Endpoint
    :   Enter `https://iam.cloud.ibm.com`

    VPC IaaS endpoint
    :   The VPC regional endpoint of your VPC cluster in the format `https://region.iaas.cloud.ibm.com`. Example: `https://eu-de.iaas.cloud.ibm.com`. For more information, see https://ibm.biz/vpc-endpoints

    Resource Group ID
    :   The ID of the resource group where your VPC is located. To retrieve this value, run the `ibmcloud is vpc VPC-ID` command and note the Resource group field.
1. On the **Secrets** tab, enter your IAM API Key and click **Next**. Note, there is currently an issue with auto fill in some browsers. If you don't see the IAM API Key field on the **Secrets** tab, try clearing the search field or by using a different web browser. 
1. On the **Storage Classes** tab, review the storage classes that are deployed to your cluster. These storage classes are available later when you deploy OpenShift Data Foundation. 
1. On the **Assign to service** tab, select your cluster and click **Complete**. 

## Deploying OpenShift Data Foundation
{: #odf-tutorial-deploying_odf}
{: step}

Follow the steps to create a {{site.data.keyword.satelliteshort}} storage configuration that uses the OpenShift Data Foundation for remote storage template. When you deploy ODF for remote storage you must provide a storage driver and a storage class that are used for provisioning app storage. In this example, you use the {{site.data.keyword.block_storage_is_short}} block storage driver and the `sat-vpc-block-gold-metro` storage class that you deployed to your cluster in the previous step.
{: shortdesc}

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} click **Locations**, then click the location where you want to deploy OpenShift Data Foundation.
1. Click **Storage > Create storage configuration** 
1. On the **Basics** tab, enter a name for your configuration and select the **OpenShift Data Foundation for remote storage**, select the version that matches your cluster version, and click **Next**.
1. On the **Parameters** tab, enter `sat-vpc-block-gold-metro` as the **OSD pod storage class**, and leave the remaining fields as their default values.
1. On the **Secrets** tab, enter your IAM API Key and click **Next**.
1. On the **Storage Classes** tab, review the storage classes that are deployed to your cluster. These storage classes are available for your apps.
1. On the **Assign to service** tab, select your cluster and click **Complete**. 

## Verifying your deployment
{: #odf-tutorial-verify}

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

To deploy an example application, see [Deploying an app that uses OpenShift Data Foundation](/docs/satellite?topic=satellite-storage-odf-remote#sat-storage-odf-remote-deploy). 

    




