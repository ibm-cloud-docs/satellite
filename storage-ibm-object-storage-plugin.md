---

copyright:
  years: 2022, 2023
lastupdated: "2023-03-29"

keywords: satellite storage, satellite config, satellite configurations, cos, object storage, storage configuration, cloud object storage

subcollection: satellite
---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.cos_full_notm}} Driver 
{: #storage-ibm-object-storage-plugin}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}


## Prerequisites
{: #storage-ibm-object-storage-plugin-prereqs}

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
1. Create a set of service credentials in your s3 provider. 
    * [{{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-service-credentials). 
    * [AWS service credential](https://docs.aws.amazon.com/cli/latest/reference/iam/create-service-specific-credential.html){: external} 
    * [Wasabi access key](https://docs.wasabi.com/docs/creating-a-user-account-and-access-key#assigning-an-access-key){: external}.
1. [Create a secret that contains your s3 credentials](#config-storage-cos-secret).

## Creating a secret in your cluster that contains your s3 provider credentials
{: #config-storage-cos-secret}

Create the Kubernetes secret in your cluster that contains your service credentials.


1. Run one of the following commands to create a secret in your cluster. When you create your secret, all values are automatically encoded to base64. In the following example, the secret name is `cos-write-access`.

        Example `create secret` command for {{site.data.keyword.cos_full_notm}} that uses an API key.
        ```sh
        oc create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=api-key=<api_key> --from-literal=service-instance-id=<service_instance_guid>
        ```
        {: pre}

    Example command to create a secret for your AWS or Wasabi credentials. 
        ```sh
        oc create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=access-key=<access_key_ID> --from-literal=secret-key=<secret_access_key>

        ```
        {: pre}




Before you begin, review the [parameter reference](#ibm-object-storage-plugin-parameter-reference) for the template version that you want to use.
{: important}

## Creating and assigning a configuration in the console
{: #ibm-object-storage-plugin-config-create-console}
{: ui}

1. [From the Locations console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to create a storage configuration.
1. Select **Storage** > **Create storage configuration**
1. Enter a name for your configuration.
1. Select the **Storage type**.
1. Select the **Version** and click **Next**
1. If the **Storage type** that you selected accepts custom parameters, enter them on the **Parameters** tab.
1. If the **Storage type** that you selected requires secrets, enter them on the **Secrets** tab.
1. On the **Storage classes** tab, review the storage classes that are deployed by the configuration or create a custom storage class.
1. On the **Assign to service** tab, select the service that you want to assign your configuration to.
1. Click **Complete** to assign your storage configuration.

## Creating a configuration in the CLI
{: #ibm-object-storage-plugin-config-create-cli}
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
    
1. Copy one of the following example command for the template version that you want to use. For more information about the command, see `ibmcloud sat storage config create` in the [command reference](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).


    Example command to create a version 2.2 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name ibm-object-storage-plugin --template-version 2.2 --param "helm-release-name=HELM-RELEASE-NAME"  [--param "parameters=PARAMETERS"]  --param "license=LICENSE"  [--param "s3provider=S3PROVIDER"]  --param "cos-storageclass=COS-STORAGECLASS"  [--param "cos-endpoint=COS-ENDPOINT"] 
    ```
    {: pre}



1. Customize the command based on the settings that you want to use.

1. Run the command to create a configuration.

1. Verify your configuration was created.
    ```sh
    ibmcloud sat storage config get --config CONFIG
    ```
    {: pre}

## Creating a configuration in the API
{: #ibm-object-storage-plugin-config-create-api}
{: api}

1. Generate an API key, then request a refresh token. For more information, see [Generating an IBM Cloud IAM token by using an API key](/docs/account?topic=account-iamtoken_from_apikey).

1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 2.2 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"ibm-object-storage-plugin\", \"storage-template-version\": \"2.2\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"HELM-RELEASE-NAME\", { \"entry.name\": \"PARAMETERS\", { \"entry.name\": \"LICENSE\", { \"entry.name\": \"S3PROVIDER\", { \"entry.name\": \"COS-STORAGECLASS\", { \"entry.name\": \"COS-ENDPOINT\",\"user-secret-parameters\": }
    ```
    {: pre}









{{site.data.content.assignment-create-console}}
{{site.data.content.assignment-create-cli}}
{{site.data.content.assignment-create-api}}

## Deploying an app that uses {{site.data.keyword.cos_full_notm}}
{: #config-storage-cos-app}


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
                storage: 10Gi
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


## Parameter reference
{: #ibm-object-storage-plugin-parameter-reference}

### 2.2 parameter reference
{: #2.2-parameter-reference}

| Display name | CLI option | Type | Description | Required? |
| --- | --- | --- | --- | --- |
| Helm Chart Release Name | `helm-release-name` | Config | Release name of the chart | true | 
| Helm Chart Additional Parameters (Optional) | `parameters` | Config | Helm Chart Additional Parameters (Optional) | false | 
| COS plug-in License: Apache License Version 2.0 | `license` | Config | COS plug-in License: Apache License Version 2.0. Set to `true` to accept the license and install the plugin | true | 
| Object storage service provider | `s3provider` | Config | Enter the object storage service provider. Supported providers are `IBM`, `AWS` and `Wasabi`. For other providers, you must explicitly provide the COS endpoint | false | 
| COS storage class | `cos-storageclass` | Config | Enter COS storage class. For more info, refer to https://ibm.biz/cos-storage-classes | true | 
| COS Endpoint | `cos-endpoint` | Config | COS Endpoint. Required when `s3provider` is not set. Precedence is given to `s3provider` when both are set. For more information, refer to https://ibm.biz/cos-endpoints | false | 
{: caption="Table 1. 2.2 parameter reference" caption-side="bottom"}


## Storage class reference for {{site.data.keyword.cos_full_notm}}
{: #config-storage-cos-sc-ref}

| Storage class name | Volume binding mode | Retain | 
| --- | --- | --- |
| `ibm-s3fs-cos` | Immediate | False |
| `ibm-s3fs-cos-perf` | Immediate | False |
{: caption="Table 2. Cloud Object storage class reference" caption-side="bottom"}

## Getting help and support for {{site.data.keyword.cos_full_notm}}
{: #sat-storage-cos-support}

If you run into an issue with using {{site.data.keyword.block_storage_is_short}} you can submit a support request with [{{site.data.keyword.cloud}} Support](https://www.ibm.com/cloud/support){: external}.


