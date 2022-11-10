---

copyright:
  years: 2022, 2022
lastupdated: "2022-11-10"

keywords: satellite storage, satellite config, satellite configurations, cos, object storage, storage configuration, cloud object storage

subcollection: satellite
---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.cos_full_notm}} CSI Driver 
{: #config-storage-cos}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}

The template is currently in beta. Do not use it for production workloads.
{: beta}

## Prerequisites
{: #config-storage-cos-prereqs}

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
1. [Create a {{site.data.keyword.cos_full_notm}} Secret](/docs/cloud-object-storage?topic=cloud-object-storage-service-credentials). 

## Creating an {{site.data.keyword.cos_full_notm}} configuration from the console
{: #config-storage-cos-create-ui}
{: ui}

1. From the {{site.data.keyword.satelliteshort}} locations dashboard, select the location where you want to create a storage configuration.
1. Select **Storage** > **Create storage configuration**
1. Enter a name for your configuration.
1. Select the **Storage type** that you want to use to create your configuration and the **Version**.
1. On the **Parameters** tab, enter the parameters for your configuration.
1. On the **Secrets** tab, enter the secrets, if required, for your configuration.
1. On the **Storage classes** tab, review the storage classes that are deployed by the configuration or create a custom storage class.
1. On the **Assign to service** tab, select the service that you want to assign your configuration to.
1. Click **Complete** to assign your storage configuration.


## Creating a {{site.data.keyword.cos_short}} configuration from the CLI
{: #config-storage-cos-create-cli}
{: cli}

1. Log in to the {{site.data.keyword.cloud_notm}} CLI.

    ```sh
    ibmcloud login
    ```
    {: pre}

1. List your {{site.data.keyword.satelliteshort}} locations and note the `Managed from` column.

    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

1. Target the `Managed from` region of your {{site.data.keyword.satelliteshort}} location. For example, for `wdc` target `us-east`. For more information, see [{{site.data.keyword.satelliteshort}} regions](/docs/satellite?topic=satellite-sat-regions).

    ```sh
    ibmcloud target -r us-east
    ```
    {: pre}

1. If you use a resource group other than `default`, target it.

    ```sh
    ibmcloud target -g <resource-group>
    ```
    {: pre}
    
1. List the available templates and versions and review the output. Make a note of the template and version that you want to use.

    ```sh
    ibmcloud sat storage template ls
    ```
    {: pre}
    
    Example command to create a config by using `ibm-object-storage-plugin` version 2.2.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name ibm-object-storage-plugin --template-version 2.2  [--param "helm-release-name=HELM-RELEASE-NAME"] [--param "parameters=PARAMETERS"] --param "license=LICENSE" --param "cos-endpoint=COS-ENDPOINT" --param "cos-storageclass=COS-STORAGECLASS"
    ```
    {: pre}


## Assigning a {{site.data.keyword.cos_short}} configuration from the console
{: #config-storage-cos-assign-ui}
{: ui}

1. Open the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} in your browser.
1. Select the location where you want to create a storage configuration.
1. Click the **Configurations** tab and click the storage configuration that you want to assign to a cluster group.
1. On the **Configuration details** page, click **Create subscription**.
1. In the **Create a subscription** pane, enter a name for your subscription. When you create a subscription you assign your storage configuration to your clusters.
1. From the **Version** drop-down list, select the storage configuration version that you want to assign.
1. From the **Cluster group** drop-down list, select the cluster group that you want to assign to the storage configuration. Note that the clusters in your cluster group where you want to assign storage must all be in the same {{site.data.keyword.satelliteshort}} location.
1. Click **Create** to create the subscription.
1. Verify that your storage configuration is deployed to your cluster. 
    1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, navigate to the **Configurations** tab.
    1. Click the storage configuration that you created and review the **Subscriptions** tab.
    1. Click the **Subscription** that you created and review the **Rollout status** for your configuration.


## Assigning a {{site.data.keyword.cos_short}} configuration from the CLI
{: #config-storage-cos-assign-cli}
{: cli}

1. List your {{site.data.keyword.satelliteshort}} storage configurations and make a note of the storage configuration that you want to assign to your clusters.
    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

1. Get the ID of the cluster or cluster group that you want to assign storage to. To make sure that your cluster is registered with {{site.data.keyword.satelliteshort}} Config or to create groups, see [Setting up clusters to use with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
    - Group
      ```sh
      ibmcloud sat group ls
      ```
      {: pre}

    - Cluster
      ```sh
      ibmcloud oc cluster ls --provider satellite
      ```
      {: pre}

    - {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster
      ```sh
      ibmcloud sat service ls --location <location>
      ```
      {: pre}

1. Assign storage to the cluster or group that you retrieved in step 2. Replace `<group>` with the ID of your cluster group or `<cluster>` with the ID of your cluster. Replace `<config>` with the name of your storage config, and `<name>` with a name for your storage assignment. For more information, see the `ibmcloud sat storage assignment create` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create).

    - Group
      ```sh
      ibmcloud sat storage assignment create --group <group> --config <config> --name <name>
      ```
      {: pre}

    - Cluster
      ```sh
      ibmcloud sat storage assignment create --cluster <cluster> --config <config> --name <name>
      ```
      {: pre}

    - {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster
      ```sh
      ibmcloud sat storage assignment create --service-cluster-id <cluster> --config <config> --name <name>
      ```
      {: pre}

1. Verify that your assignment is created.
    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER) | grep <storage-assignment-name>
    ```
    {: pre}

1. Verify that the storage configuration resources are deplpoyed.

    ```sh
    $ oc get pods -n ibm-object-s3fs -o wide | grep object

    ibmcloud-object-storage-driver-njf57              1/1     Running   0          2m36s

    ibmcloud-object-storage-driver-xxhxw              1/1     Running   0          2m36s

    ibmcloud-object-storage-driver-zhqqs              1/1     Running   0          2m36s   

    ibmcloud-object-storage-plugin-6f85bfd684-9pj5l   1/1     Running   0          2m36s   
    ```
    {: screen}

1. List the storage classes.

    ```sh
    oc get storageclass | grep ibm-s3fs

    ibmc-s3fs-cos        ibm.io/ibmc-s3fs   Delete          Immediate           false                  2m23s

    ibmc-s3fs-cos-perf   ibm.io/ibmc-s3fs   Delete          Immediate           false                  2m23s
    ```
    {: codeblock}

## Deploying an app that uses your {{site.data.keyword.cos_full_notm}} configuration
{: #config-storage-cos-app}
{: cli} 

You can use the `ibm-object-s3fs` driver to create PVCs that you can use in your cluster workloads.
{: shortdesc}

1. Create a PVC that references your object storage configuration.

    ```yaml 
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata: 
        name: pvc-cos #Enter a name for your PVC.
        namespace: default
        annotations: 
        ibm.io/auto-create-bucket: "false"
        ibm.io/auto-delete-bucket: "false" 
        ibm.io/bucket: <BUCKET-NAME> #Enter the name of your object storage bucket.
        ibm.io/secret-name: <SECRET-NAME> #Enter the name of the secret you created earlier.
        ibm.io/secret-namespace: <default> #Enter the namespace where you want to create the PVC.
    spec: 
        accessModes:
        - ReadWriteOnce
        resources:
            requests:
                storage: 10gi
        storageClassName: ibmc-s3fs-cos #The storage class that you want to use.
    ```
    {: codeblock}

1. Create the PVC in your cluster.

    ```sh
    oc apply -f pvc-cos.yaml
    ```
    {: pre}

1. Create a YAML configuration file for a pod that mounts the PVC that you create. 

    ```yaml
    apiVersion: v1
    kind: List
    metadata:
      name: ibm-object-storage-plugin
      namespace: kube-system
      annotations:
        revision: 1  
    items:
      - apiVersion: satstorage.ibm.com/v1
        kind: SatStorageHelmChart
        metadata:
          name: ibm-object-storage-plugin
          namespace: ibm-satellite-storage
        spec:
          helm-repo-url: "raw.githubusercontent.com/IBM/charts/master/repo/ibm-helm"
          helm-chart-name: "ibm-object-storage-plugin"
          helm-chart-namespace: "ibm-object-s3fs"
          helm-chart-version: "2.2.1"  
          release-name: "{{{ helm-release-name }}}"
          storage-template-revision: "1" 
          params: 
            - license={{{ license }}}
            - cos.endpoint={{{ cos-endpoint }}}
            - cos.storageClass={{{ cos-storageclass }}}
            {{#parameters }}
            - "{{{ item }}}"
            {{/parameters }}
    ```
    {: codeblock}

1. Create the pod in your cluster. 

    ```sh
    oc apply -f ibm-object-storage-plugin
    ```
    {: pre}

1. Verify that the pod is deployed. Note that it might take a few minutes for your app to get into a `Running` state. 

    ```sh
    oc get pods
    ```
    {: pre}

    ```sh
        NAME                                READY   STATUS    RESTARTS   AGE
        ibm-object-storage-plugin           1/1     Running   0          2m58s
        ```
        {: screen}

1. Verify that the app can write to your block storage volume by logging in to your pod.

    ```sh
    oc exec ibm-object-storage-plugin
    ```
    {: pre}

1. View the contents of the `outfile` file to confirm that your app can write data to your persistent storage.

    ```sh
    cat /
    ```
    {: pre}

    Example output
    
    ```sh
    Fri Jul 16 07:49:39 EDT 2021
    Fri Jul 16 07:49:39 EDT 2021
    Fri Jul 16 07:49:39 EDT 2021
    ```
    {: screen}

1. Exit the pod.

    ```sh
    exit
    ```
    {: pre}


## Removing the {{site.data.keyword.cos_full_notm}} storage configuration using the console
{: #config-storage-cos-rm-ui}
{: ui}

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**
1. Enter the name of your storage configuration.
1. Select **Delete**.

## Removing the {{site.data.keyword.cos_full_notm}} storage configuration using the command line
{: #config-storage-cos-rm-cli}
{: cli}

If you no longer need your {{site.data.keyword.cos_full_notm}} configuration, you can remove your apps, PVCs, PVs, and assignment from your clusters. 
{: shortdesc}

1. List your storage assignments and find the one that you used for your cluster.

    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
    ```
    {: pre}

1. Remove the assignment. After the assignment is removed, the driver pods and storage classes are removed from all clusters that were part of the storage assignment.

    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment_ID>
    ```
    {: pre}

1. Verify that the driver is removed from your cluster.

    1. List of the storage classes in your cluster and verify that the storage classes are removed.
    
        ```sh
        oc get sc
        ```
        {: pre}

    1. List the pods in the `kube-system` namespace and verify that the storage driver pods are removed.
    
        ```sh
        oc get pods -n kube-system | grep cos
        ```
        {: pre}

1. Optional: Remove the storage configuration.

    1. List the storage configurations.
    
        ```sh
        ibmcloud sat storage config ls
        ```
        {: pre}

    1. Remove the storage configuration.
    
        ```sh
        ibmcloud sat storage config rm --config <config_name>
        ```
        {: pre}

## Parameter reference for {{site.data.keyword.cos_full_notm}}
{: #config-storage-cos-parameter-reference}

## Version 2.2 parameter reference
{: #ibm-object-storage-plugin-2.2}

| Display name | Name | Description | Required? | Default |
| --- | --- | --- | --- | --- |
| Helm Chart Release Name | `helm-release-name` | Release name of the chart | false |`ibm-object-storage-plugin` |
| Helm Chart Additional Parameters (Optional) | `parameters` | Helm Chart Additional Parameters (Optional) | false | N/A | 
| COS plug-in License: Apache License Version 2.0 | `license` | COS plug-in License: Apache License Version 2.0. Set to 'true' to accept the license and install the plugin | true | N/A | 
| COS Endpoint | `cos-endpoint` | Enter COS Endpoint. For more information, refer to https://ibm.biz/cos-endpoints | true | N/A | 
| COS storageclass | `cos-storageclass` | Enter COS storageclass. For more info, refer to https://ibm.biz/cos-storage-classes | true | N/A | 
{: caption="ibm-object-storage-plugin version 2.2 parameter reference"}

## Storage class reference for {{site.data.keyword.cos_full_notm}}
{: #config-storage-cos-sc-ref}

| Storage class name | Volume binding mode | Retain | 
| --- | --- | --- |
| `ibm-s3fs-cos` | Immediate | False |
| `ibm-s3fs-cos-perf` | Immediate | False |

## Getting help and support for {{site.data.keyword.cos_full_notm}}
{: #sat-storage-cos-support}

If you run into an issue with using {{site.data.keyword.block_storage_is_short}} you can submit a support request with [{{site.data.keyword.cloud}} Support](https://www.ibm.com/cloud/support){: external}.


