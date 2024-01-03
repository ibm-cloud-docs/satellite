---


copyright:
  years: 2023, 2024
lastupdated: "2024-01-03"

keywords: satellite, hybrid, multicloud, storage errors

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}


# Debugging OpenShift Data Foundation
{: #sat-storage-odf-debug}

Complete the following steps to debug your OpenShift Data Foundation storage configurations. 
{: shortdesc}

{{../openshift/ts-storage-debug-ocs.md#debug_storage_ocs_deploy}}

{{../openshift/ts-storage-debug-ocs.md#debug_storage_ocs_restart}}

{{../openshift/ts-storage-debug-ocs.md#debug_storage_ocs_driver_plugin}}

{{../openshift/ts-storage-debug-ocs.md#debug_storage_ocs_cluster}}


1. Get OCS pod name.

    ```sh
    export POD_OCS_NAME=$(oc get pods -n kube-system | grep ocs | awk '{print $1}')
    ```
    {: pre}

1. Get the logs of OCS operator controller manager.
    ```sh
    oc logs -n kube-system $POD_OCS_NAME
    ```
    {: pre}

1. Get a list of your app pods.

    ```sh
    oc get pods -n NAMESPACE
    ```
    {: pre}

1. Get the logs for the pod you want to troubleshoot.
    ```sh
    oc logs POD -n NAMESPACE
    ```
    {: pre}


## Change worker node name assigned to ODF
{: #sat-storage-odf-node-name}

1. Get the worker names.
    ```sh
    oc get nodes
    ```

1. Change the names of the worker assigned to ODF.
    ```sh
    ibmcloud sat storage config param set --config <your_storage_config_name> --param "worker-nodes=<node-name-1>,<node-name-2>,<node-name-3>" --apply
    ```
    {: codeblock}



1. For {{site.data.keyword.satelliteshort}} clusters that use local volumes on the worker node, make sure that the `disk-by-id` for the volumes that you used for the `osd-device-path` and `mon-device-path` parameters exists on the worker nodes. 


1. [Review the troubleshooting documentation for steps to solve common errors](/docs/openshift?topic=openshift-sitemap#sitemap_openshift_data_foundation). 







