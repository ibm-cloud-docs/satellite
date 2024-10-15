---


copyright:
  years: 2022, 2024
lastupdated: "2024-10-10"

keywords: satellite storage, satellite config, satellite configurations, cos, object storage, storage configuration, cloud object storage

subcollection: satellite
---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.cos_full_notm}} Driver
{: #storage-ibm-object-storage-plugin}

You can use the {{site.data.keyword.cos_full_notm}} driver template to create and access data in multiple s3 storage providers such as IBM, AWS, Wasabi, Azure, and more. Review the following steps to deploy the {{site.data.keyword.cos_full_notm}} driver to your {{site.data.keyword.satelliteshort}} clusters.
{: shortdesc}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config by selecting the **Enable cluster admin access for Satellite Config** option in the console or including the `--enable-config-admin` option when you create your cluster.
{: important}

You cannot scope {{site.data.keyword.satelliteshort}} storage service to resource groups. However, if you are scoping other resources such as location and cluster to resource groups, you need to add {{site.data.keyword.satelliteshort}} reader and link administrator role for all resources in the account.
{: note}

## Prerequisites
{: #storage-ibm-object-storage-plugin-prereqs}

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).

1. Create a set of service credentials in your object storage provider.
    * [{{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-service-credentials).
    * [AWS service credential](https://docs.aws.amazon.com/cli/latest/reference/iam/create-service-specific-credential.html){: external}.
    * [Wasabi access key](https://docs.wasabi.com/docs/creating-a-user-account-and-access-key#assigning-an-access-key){: external}.
1. [Create a secret that contains your s3 credentials](#config-storage-cos-secret).

## Creating a secret in your cluster that contains your object storage credentials
{: #config-storage-cos-secret}

Create the Kubernetes secret in your cluster that contains your service credentials.

1. Follow the steps based on your object storage provider to create a secret in your cluster. When you create your secret, all values are automatically encoded to base64. In the following example, the secret name is `cos-write-access`.

    - {{site.data.keyword.cos_full_notm}}
    
        1. Find your service instance ID.
            ```sh
            ibmcloud resource service-instance <service_name> | grep GUID
            ```
            {: pre}

        1. Create the secret in your cluster.

            ```sh
            oc create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=api-key=API-KEY --from-literal=service-instance-id=SERVICE-INSTANCE-ID
            ```
            {: pre}

    - AWS or Wasabi

        ```sh
        oc create secret generic cos-write-access --type=ibm/ibmc-s3fs --from-literal=access-key=ACCESS-KEY-ID --from-literal=secret-key=SECRET-ACCESS-KEY
        ```
        {: pre}





## Creating and assigning a configuration in the console
{: #ibm-object-storage-plugin-config-create-console}
{: ui}


1. Review the [parameter reference](#ibm-object-storage-plugin-parameter-reference).


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


1. Review the [parameter reference](#ibm-object-storage-plugin-parameter-reference) for the template version that you want to use.


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
    
1. Copy one of the following example command for the template version that you want to use. For more information about the command, see `ibmcloud sat storage config create` in the [command reference](/docs/satellite?topic=satellite-satellite-cli-reference#storage-config-create-cli).


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

1. Review the [parameter reference](#ibm-object-storage-plugin-parameter-reference) for the template version that you want to use.


1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 2.2 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"ibm-object-storage-plugin\", \"storage-template-version\": \"2.2\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"HELM-RELEASE-NAME\", { \"entry.name\": \"PARAMETERS\", { \"entry.name\": \"LICENSE\", { \"entry.name\": \"S3PROVIDER\", { \"entry.name\": \"COS-STORAGECLASS\", { \"entry.name\": \"COS-ENDPOINT\",\"user-secret-parameters\": }
    ```
    {: pre}










{{site.data.content.assignment-create-cli}}
{{site.data.content.assignment-create-api}}
{{site.data.content.configuration-upgrade-console}}
{{site.data.content.assignment-upgrade-cli}}
{{site.data.content.assignment-autopatch-cli}}
{{site.data.content.assignment-upgrade-api}}
{{site.data.content.assignment-autopatch-api}}

## Deploying an app that uses {{site.data.keyword.cos_full_notm}}
{: #config-storage-cos-app}


You can use the `ibm-object-s3fs` driver to create PVCs that you can use in your cluster workloads.
{: shortdesc}

1. Create a PVC that references your object storage configuration.

    ```yaml
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata: 
      name: demo #Enter a name for your PVC.
      namespace: default
      annotations: 
      ibm.io/auto-create-bucket: "false"
      ibm.io/auto-delete-bucket: "false" 
      ibm.io/bucket: BUCKET-NAME #Enter the name of your object storage bucket.
      ibm.io/secret-name: SECRET-NAME #Enter the name of the secret you created earlier.
      ibm.io/secret-namespace: NAMESPACE #Enter the namespace where you want to create the PVC.
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
    kind: Pod
    metadata:
      name: demo-pod
      namespace: default
    spec:
      securityContext:
        runAsUser: 2000
        fsGroup: 2000
      volumes:
      - name: demo-vol
        persistentVolumeClaim:
            claimName: demo
      containers:
      - name: test
        image: nginxinc/nginx-unprivileged
        imagePullPolicy: Always
        volumeMounts:
        - name: demo-vol
          mountPath: /mnt/cosvol
    ```
    {: codeblock}
   

1. Create the pod in your cluster.

    ```sh
    oc apply -f demo-pod.yaml
    ```
    {: pre}

1. Verify that the pod is deployed. Note that it might take a few minutes for your app to get into a `Running` state.

    ```sh
    oc get pods
    ```
    {: pre}

    ```sh
    NAME                                READY   STATUS    RESTARTS   AGE
    demo-pod                            1/1     Running   0          2m58s
    ```
    {: screen}

1. Verify that the app can write to your block storage volume by logging in to your pod.

   ```sh
   oc exec demo-pod -- bash -c "touch /mnt/cosvol/test.txt && ls /mnt/cosvol" test.txt
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
{: #ibm-object-storage-plugin-2.2-parameters}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| Release name | `helm-release-name` | Config | Helm chart release name. | true | `ibm-object-storage-plugin` |
| Helm Chart additional parameters | `parameters` | Config | Helm Chart additional parameters. | false | N/A |
| Object Storage plug-in license | `license` | Config | Object storage plug-in license: Apache license Version 2.0. Set to `true` to accept the license and install the plug-in. | true | N/A |
| Object Storage provider | `s3provider` | Config | Available providers are `IBM`, `AWS` and `Wasabi`. For providers other than these, you must provide the `Object Storage service endpoint` parameter. | false | N/A |
| Object Storage region | `cos-storageclass` | Config | Enter the region where your object storage is located. For IBM COS regions, see https://ibm.biz/cos-endpoints-list. For Wasabi, see https://ibm.biz/wasabi-endpoints. For AWS, see https://ibm.biz/aws-endpoints. | true | N/A |
| Object Storage service endpoint | `cos-endpoint` | Config | Object Storage service endpoint. Required when using Object Storage providers other than IBM, AWS or Wasabi. Preference is given to `Object Storage provider` when both are set. | false | N/A |
{: caption="2.2 parameter reference" caption-side="bottom"}


## Storage class reference for {{site.data.keyword.cos_full_notm}}
{: #config-storage-cos-sc-ref}

| Storage class name | Volume binding mode | Retain | 
| --- | --- | --- |
| `ibm-s3fs-cos` | Immediate | False |
| `ibm-s3fs-cos-perf` | Immediate | False |
{: caption="Cloud Object storage class reference" caption-side="bottom"}

## Getting help and support for {{site.data.keyword.cos_full_notm}}
{: #sat-storage-cos-support}

When you use {{site.data.keyword.cos_full_notm}}, try the following resources before you open a support case. 
{: shortdesc}

1. Review the FAQs in the [{{site.data.keyword.block_storage_is_short}} docs](/docs/cloud-object-storage?topic=cloud-object-storage-faq).
1. Review the [troubleshooting documentation](/docs/satellite?topic=satellite-storage-must-gather) to troubleshoot and resolve common issues.
1. Check the status of the {{site.data.keyword.Bluemix_notm}} platform and resources by going to the [Status page](https://cloud.ibm.com/status){: external}.
1. Review [Stack Overflow](https://stackoverflow.com/questions/tagged/ibm-cloud){: external} to see whether other users experienced the same problem. Tag any questions with `ibm-cloud` and `COS`.
1. If you run into an issue with {{site.data.keyword.block_storage_is_short}}, submit a support request with [{{site.data.keyword.cloud}} Support](https://www.ibm.com/cloud/support){: external}.
