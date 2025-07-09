---


copyright:
  years: 2020, 2025
lastupdated: "2025-07-09"

keywords: satellite storage, satellite config, satellite configurations, aws, efs, file storage

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# AWS EFS 
{: #storage-aws-efs-csi-driver}

Set up [Amazon Elastic File System (EFS)](https://docs.aws.amazon.com/efs/?id=docs_gateway){: external} for {{site.data.keyword.satelliteshort}} clusters by creating a storage configuration in your location. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

To use AWS EFS storage for your apps, your {{site.data.keyword.satelliteshort}} hosts must reside in AWS. Only static provisioning is supported with this storage template. You must manually provision an [AWS EFS file system](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html){: external} on AWS before you create your {{site.data.keyword.satelliteshort}} storage configuration. Make sure that the EFS device is in the same VPC and subnet that you used for your AWS hosts, and that your hosts and EFS device use the same security group.
{: important}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config by selecting the **Enable cluster admin access for Satellite Config** option in the console or including the `--enable-config-admin` option when you create your cluster.
{: important}

You cannot scope {{site.data.keyword.satelliteshort}} storage service to resource groups. However, if you are scoping other resources such as location and cluster to resource groups, you need to add {{site.data.keyword.satelliteshort}} reader and link administrator role for all resources in the account.
{: note}

## Prerequisites for using AWS EFS
{: #sat-storage-efs-prereqs}

To use the AWS EFS storage template, complete the following tasks:

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).

1. [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-satellite-clusters) that runs on compute hosts in Amazon Web Services (AWS). Make sure that you select the **Enable cluster admin access for {{site.data.keyword.satelliteshort}} Config** option when you create the cluster. If you don't enable Administrator (admin) access for {{site.data.keyword.satelliteshort}} Config when creating your cluster, you must re-create your cluster and enable admin access before you can deploy storage. For more information about how to add hosts from AWS to your {{site.data.keyword.satelliteshort}} location so that you can assign them to a cluster, see [Adding AWS hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-aws#aws-host-attach).
1. Manually provision an [AWS EFS file system](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html){: external} in your AWS account. Make sure that the EFS device is in the same VPC and subnet that you used for your AWS hosts, and that your hosts and EFS device use the same security group.






## Creating and assigning a configuration in the console
{: #aws-efs-csi-driver-config-create-console}
{: ui}


1. Review the [parameter reference](#aws-efs-csi-driver-parameter-reference).


1. [From the Locations console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to create a storage configuration.
1. Select **Storage** > **Create storage configuration**
1. Enter a name for your configuration.
1. Select the **Storage type**.
1. Select the **Version** and click **Next**
1. If the **Storage type** that you selected accepts custom parameters, enter them on the **Parameters** tab.
1. If the **Storage type** that you selected requires secrets, enter them on the **Secrets** tab.
1. On the **Storage classes** tab, review the storage classes that are deployed by the configuration or create a custom storage class. For AWS EFS configurations, you must add a storage class before continuing.
1. On the **Assign to service** tab, select the service that you want to assign your configuration to.
1. Click **Complete** to assign your storage configuration.

## Creating a configuration in the CLI
{: #aws-efs-csi-driver-config-create-cli}
{: cli}


1. Review the [parameter reference](#aws-efs-csi-driver-parameter-reference) for the template version that you want to use.


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


    Example command to create a version 1.3.1 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name aws-efs-csi-driver --template-version 1.3.1 --param "aws-access-key=AWS-ACCESS-KEY"  --param "aws-secret-access-key=AWS-SECRET-ACCESS-KEY" 
    ```
    {: pre}


    Example command to create a version 1.3.7 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name aws-efs-csi-driver --template-version 1.3.7 --param "aws-access-key=AWS-ACCESS-KEY"  --param "aws-secret-access-key=AWS-SECRET-ACCESS-KEY" 
    ```
    {: pre}


    Example command to create a version 1.4.2 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name aws-efs-csi-driver --template-version 1.4.2 --param "aws-access-key=AWS-ACCESS-KEY"  --param "aws-secret-access-key=AWS-SECRET-ACCESS-KEY" 
    ```
    {: pre}


    Example command to create a version 2.0.3 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name aws-efs-csi-driver --template-version 2.0.3 --param "aws-access-key=AWS-ACCESS-KEY"  --param "aws-secret-access-key=AWS-SECRET-ACCESS-KEY" 
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
{: #aws-efs-csi-driver-config-create-api}
{: api}

1. Generate an API key, then request a refresh token. For more information, see [Generating an IBM Cloud IAM token by using an API key](/docs/account?topic=account-iamtoken_from_apikey).

1. Review the [parameter reference](#aws-efs-csi-driver-parameter-reference) for the template version that you want to use.


1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 1.3.1 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"aws-efs-csi-driver\", \"storage-template-version\": \"1.3.1\", \"update-assignments\": true, \"user-config-parameters\":\"user-secret-parameters\": { \"entry.name\": \"AWS-ACCESS-KEY\",{ \"entry.name\": \"AWS-SECRET-ACCESS-KEY\",}
    ```
    {: pre}


    Example request to create a version 1.3.7 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"aws-efs-csi-driver\", \"storage-template-version\": \"1.3.7\", \"update-assignments\": true, \"user-config-parameters\":\"user-secret-parameters\": { \"entry.name\": \"AWS-ACCESS-KEY\",{ \"entry.name\": \"AWS-SECRET-ACCESS-KEY\",}
    ```
    {: pre}


    Example request to create a version 1.4.2 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"aws-efs-csi-driver\", \"storage-template-version\": \"1.4.2\", \"update-assignments\": true, \"user-config-parameters\":\"user-secret-parameters\": { \"entry.name\": \"AWS-ACCESS-KEY\",{ \"entry.name\": \"AWS-SECRET-ACCESS-KEY\",}
    ```
    {: pre}


    Example request to create a version 2.0.3 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"aws-efs-csi-driver\", \"storage-template-version\": \"2.0.3\", \"update-assignments\": true, \"user-config-parameters\":\"user-secret-parameters\": { \"entry.name\": \"AWS-ACCESS-KEY\",{ \"entry.name\": \"AWS-SECRET-ACCESS-KEY\",}
    ```
    {: pre}







    


{{site.data.content.assignment-create-cli}}
{{site.data.content.assignment-create-api}}
{{site.data.content.configuration-upgrade-console}}
{{site.data.content.assignment-upgrade-cli}}
{{site.data.content.assignment-autopatch-cli}}
{{site.data.content.assignment-upgrade-api}}
{{site.data.content.assignment-autopatch-api}}
    
    
## Adding a custom AWS EFS storage class to your configuration
{: #aws-add-sc-efs}
{: cli}

After you create a {{site.data.keyword.satelliteshort}} storage configuration, you can add a custom storage class by using the `ibmcloud sat config sc add` command.

You can't add storage classes to {{site.data.keyword.satelliteshort}} storage configurations after the configurations are assigned to clusters or cluster groups. Make sure to add storage classes before assigning your configuration.
{: note}

1. List the storage class parameters for the template that you used to create your configuration and decide how you want to create your storage class.
    ```sh
    ibmcloud sat storage template get --name aws-efs-csi-driver --version <version>
    ```
    {: pre}

2. Create the storage class and pass in any custom parameters. Enter the name of the storage configuration you created earlier, the storage class name, and the custom parameters that you want to provide.
    ```sh
    ibmcloud sat storage config sc add --config-name <config-name> --name <storage-class-name> --param "key=value"
    ```
    {: pre}
    
    `basePath`
    :   Specify the BasePath. Base path is a path on the file system under which access point root directory is created.
    
    `directoryPerms`
    :   Specify directory permissions. Default: `700`.
    
    `fileSystemId`
    :   Required. Specify the EFS file system ID.
    
    `gidRangeEnd`
    :   Specify the GID range end. `gidRangeEnd` is the ending range of the POSIX Group ID. Default: `7000000`.
       
    `gidRangeStart`
    :   Specify the GID range start. `gidRangeStart` is the starting range of the POSIX Group ID to be applied onto the root directory of the access point. Default: `50000`.
    
    `is-default-class`
    :   Specify `true` or `false` to make the created storage class the default class.

    `name`
    :   Required. The name of the storage class.

    Example command to add a custom storage class to a configuration call `my-config`.
    
    ```sh
    ibmcloud sat storage config sc add --config-name my-config --name my-sc --param "fileSystemID=<filesystemID>" --param "is-default-class=true"
    ```
    {: pre}
    
3. Complete the following steps to assign your storage configuration to your clusters.


## Deploying an app that uses AWS EFS storage
{: #sat-storage-efs-deploy}


You can use the `efs-csi-driver` to statically provision AWS EFS storage for the apps in your clusters.
{: shortdesc}

Before you begin, make sure that you [created an AWS EFS instance](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html){: external} in your AWS account.The EFS device must be in the same VPC and subnet that you used for your AWS hosts, and your hosts and EFS device must use the same security group.

1. From the [AWS EFS console](https://console.aws.amazon.com/efs/home){: external}, find the file system that you want to use for your apps and note the file system ID.

2. Create a persistent volume (PV) that references the file system ID of your AWS EFS instance.
    1. Create a YAML configuration file for your PV and enter the file system ID in the `csi.volumeHandle` field.
        ```yaml
        apiVersion: v1
        kind: PersistentVolume
        metadata:
          name: efs
        spec:
          capacity:
          storage: 5Gi
          volumeMode: Filesystem
          accessModes:
          - ReadWriteOnce
          persistentVolumeReclaimPolicy: Retain
          storageClassName: # Enter the name of the custom storage class that you created earlier
          csi:
          driver: efs.csi.aws.com
          volumeHandle: <aws_efs_fileshare_ID>
        ```
        {: codeblock}

    2. Create the PV in your cluster.
        ```sh
        oc apply -f pv.yaml
        ```
        {: pre}

    3. Verify that the PV is created. Note that the PV remains in an `Available` status as no matching PVC is found yet.
        ```sh
        oc get pv
        ```
        {: pre}

        Example output
        ```sh
        NAME   CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS        CLAIM        STORAGECLASS           REASON   AGE
        efs    5Gi        RWO            Retain           Available                  sat-aws-file-gold               34m
        ```
        {: screen}

3. Create a persistent volume claim (PVC) that matches the PV that you created.
    1. Create a YAML configuration file for your PVC. In order for the PVC to match the PV, you must use the same values for the storage class and the size of the storage.
        ```yaml
        apiVersion: v1
        kind: PersistentVolumeClaim
        metadata:
          name: efs
        spec:
          accessModes:
          - ReadWriteOnce
          storageClassName: # Enter the custom storage class that you created earlier.
          resources:
          requests:
            storage: 5Gi
        ```
        {: codeblock}

    2. Create the PVC in your cluster.
        ```sh
        oc apply -f pvc.yaml
        ```
        {: pre}

    3. Verify that the PVC is created. Make sure that the PVC is in a `Bound` status and that the name of the PV that you created earlier is listed in the **VOLUME** column.
        ```sh
        oc get pvc
        ```
        {: pre}

        Example output
        ```sh
        NAME      STATUS        VOLUME     CAPACITY   ACCESS MODES   STORAGECLASS        AGE
        efs       Bound         efs        5Gi        RWO            sat-aws-file-gold   36m
        ```
        {: screen}

4. Create a YAML configuration file for a pod that mounts the PVC that you created. The following example creates an `nginx` pod that writes the current date and time to a `test.txt` file on your AWS EFS volume mount path.
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: app
    spec:
      containers:
       - name: app
      image: nginx
      command: ["/bin/sh"]
      args: ["-c", "while true; do echo $(date -u) >> /test/test.txt; sleep 5; done"]
      volumeMounts:
      - name: persistent-storage
        mountPath: /test
      volumes:
      - name: persistent-storage
      persistentVolumeClaim:
        claimName: efs
    ```
    {: codeblock}

5. Create the pod in your cluster.
    ```sh
    oc apply -f pod.yaml
    ```
    {: pre}

6. Verify that the pod is deployed. Note that it might take a few minutes for your app to get into a `Running` state.
    ```sh
    oc get pods
    ```
    {: pre}

    Example output
    ```sh
    NAME                                READY   STATUS    RESTARTS   AGE
    app                                 1/1     Running   0          2m58s
    ```
    {: screen}

7. Verify that the app can write to your AWS EFS instance.
    1. Log in to your pod.
        ```sh
        oc exec <app-pod-name> -it bash
        ```
        {: pre}

    2. Display the contents of the `test.txt` file to confirm that your app can write data to your persistent storage.
        ```sh
        cat /test/test.txt
        ```
        {: pre}

        Example output
        ```sh
        Tue Mar 2 20:09:19 UTC 2021
        Tue Mar 2 20:09:25 UTC 2021
        Tue Mar 2 20:09:31 UTC 2021
        Tue Mar 2 20:09:36 UTC 2021
        Tue Mar 2 20:09:42 UTC 2021
        Tue Mar 2 20:09:47 UTC 2021
        ```
        {: screen}

    3. Exit the pod.
        ```sh
        exit
        ```
        {: pre}

8. From the [AWS EFS console](https://console.aws.amazon.com/efs/home){: external}, find the file system that you used and verify that the file system grows in size.

## Removing AWS EFS storage from your apps
{: #aws-efs-rm}


If you no longer need your AWS EFS instance, you can remove your PVC, PV, and the AWS EFS instance in your AWS account.
{: shortdesc}

Removing your AWS EFS instance permanently removes all the data that is stored on this instance. This action cannot be undone. Make sure that you back up your data before you delete the AWS EFS instance.
{: important}

1. List your PVCs and note the name of the PVC and the corresponding PV that you want to remove.
    ```sh
    oc get pvc
    ```
    {: pre}

2. Remove any pods that mount the PVC.
    1. List all the pods that currently mount the PVC that you want to delete. If no pods are returned, you do not have any pods that currently use your PVC.
        ```sh
        oc get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
        ```
        {: pre}

        Example output
        ```sh
        app    sat-aws-block-bronze
        ```
        {: screen}

    2. Remove the pod that uses the PVC. If the pod is part of a deployment, remove the deployment.
        ```sh
        oc delete pod <pod_name>
        ```
        {: pre}

        ```sh
        oc delete deployment <deployment_name>
        ```
        {: pre}

    3. Verify that the pod or the deployment is removed.
        ```sh
        oc get pods
        ```
        {: pre}

        ```sh
        oc get deployments
        ```
        {: pre}

3. Delete the PVC. Because you statically provisioned the AWS EFS storage, deleting the PVC does not remove the PV or the AWS EFS instance in your AWS account.
    ```sh
    oc delete pvc <pvc_name>
    ```
    {: pre}

4. Delete the corresponding PV.
    ```sh
    oc delete pv <pv_name>
    ```
    {: pre}

5. From the [AWS EFS console](https://console.aws.amazon.com/efs/home){: external}, select the file system that you want to delete and click **Delete**.    


## Removing the AWS EFS storage configuration from your cluster
{: #aws-efs-template-rm}

If you no longer plan on using AWS EFS storage in your cluster, you can unassign your cluster from the storage configuration.
{: shortdesc}

Note that if you remove the storage configuration, the driver is then uninstalled from all assigned clusters. from all assigned clusters. Your PVCs, PVs and data are not removed. However, you might not be able to access your data until you re-install the driver in your cluster again.
{: important}


### Removing the AWS EFS storage configuration from the console
{: #aws-efs-template-rm-ui}
{: ui}

Use the console to remove a storage configuration.
{: shortdesc}

Note that you must delete your storage assignments before you can successfully delete your storage configuration. 
{: important}

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**
1. Enter the name of your storage configuration.
1. Select **Delete**.


### Removing the AWS EFS storage configuration from the CLI
{: #aws-efs-template-rm-cli}
{: cli}

Use the CLI to remove a storage configuration.
{: shortdesc}

Note that you must delete your storage assignments before you can successfully delete your storage configuration. 
{: important}

1. List your storage assignments and find the one that you used for your cluster.
    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
    ```
    {: pre}

2. Remove the assignment. After the assignment is removed, the AWS EFS driver pods and storage class are removed from all clusters that were part of the storage assignment.
    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment_ID>
    ```
    {: pre}

3. Verify that the AWS EFS driver is removed from your cluster.
    1. List the storage classes in your cluster and verify that the AWS EFS storage class is removed.
        ```sh
        oc get sc
        ```
        {: pre}

    2. List the pods in the `kube-system` namespace and verify that the AWS EFS storage driver pods are removed.
        ```sh
        oc get pods -n kube-system
        ```
        {: pre}

4. Optional: Remove the storage configuration.
    1. List the storage configurations.
        ```sh
        ibmcloud sat storage config ls
        ```
        {: pre}

    2. Remove the storage configuration.
        ```sh
        ibmcloud sat storage config rm --config <config_name>
        ```
        {: pre}



## Parameter reference
{: #aws-efs-csi-driver-parameter-reference}

### 1.3.1 parameter reference
{: #aws-efs-csi-driver-1.3.1-parameters}

| Display name | CLI option | Type | Description | Default value | 
| --- | --- | --- | --- | --- |
| AWS Access Key ID | `aws-access-key` | Secret | AWS Access Key ID. | N/A |
| AWS Secret Access Key | `aws-secret-access-key` | Secret | AWS Secret Access key. | N/A |
{: caption="Table 1. 1.3.1 parameter reference" caption-side="bottom"}


### 1.3.7 parameter reference
{: #aws-efs-csi-driver-1.3.7-parameters}

| Display name | CLI option | Type | Description | Default value | 
| --- | --- | --- | --- | --- |
| AWS Access Key ID | `aws-access-key` | Secret | AWS Access Key ID. | N/A |
| AWS Secret Access Key | `aws-secret-access-key` | Secret | AWS Secret Access key. | N/A |
{: caption="Table 2. 1.3.7 parameter reference" caption-side="bottom"}


### 1.4.2 parameter reference
{: #aws-efs-csi-driver-1.4.2-parameters}

| Display name | CLI option | Type | Description | Default value | 
| --- | --- | --- | --- | --- |
| AWS Access Key ID | `aws-access-key` | Secret | AWS Access Key ID. | N/A |
| AWS Secret Access Key | `aws-secret-access-key` | Secret | AWS Secret Access key. | N/A |
{: caption="Table 3. 1.4.2 parameter reference" caption-side="bottom"}


### 2.0.3 parameter reference
{: #aws-efs-csi-driver-2.0.3-parameters}

| Display name | CLI option | Type | Description | Default value | 
| --- | --- | --- | --- | --- |
| AWS Access Key ID | `aws-access-key` | Secret | AWS Access Key ID. | N/A |
| AWS Secret Access Key | `aws-secret-access-key` | Secret | AWS Secret Access key. | N/A |
{: caption="Table 4. 2.0.3 parameter reference" caption-side="bottom"}




## Getting help and support for AWS EFS
{: #sat-efs-support}

When you use AWS EFS Storage, try the following resources before you open a support case. 
{: shortdesc}

1. Review the FAQs in the [AWS Knowledge Center](https://repost.aws/knowledge-center){: external}.
1. Review the [troubleshooting documentation](/docs/satellite?topic=satellite-storage-must-gather) to troubleshoot and resolve common issues.
1. Check the status of the {{site.data.keyword.Bluemix_notm}} platform and resources by going to the [Status page](https://cloud.ibm.com/status){: external}.
1. Review [Stack Overflow](https://stackoverflow.com/questions/tagged/ibm-cloud){: external} to see whether other users experienced the same problem. Tag any questions with `ibm-cloud` and `AWS-EFS`.
1. Search the [AWS Support Center](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fsupport%2Fhome%3Fstate%3DhashArgs%2523%252F%26isauthcode%3Dtrue&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fsupportcenter&forceMobileApp=0&code_challenge=u3nT-WHT9gSG_PS93w4dwD6R_PWLj1eOU9GLUMEOkzo&code_challenge_method=SHA-256){: external} for more in-depth support options. 
