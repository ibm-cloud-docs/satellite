---

copyright:
  years: 2020, 2022
lastupdated: "2022-12-21"

keywords: satellite storage, satellite config, satellite configurations, aws, ebs, block storage, storage configuration

subcollection: satellite
---

{{site.data.keyword.attribute-definition-list}}

# AWS EBS
{: #storage-aws-ebs-csi-driver}

Set up [Amazon Elastic Block Storage (EBS)](https://docs.aws.amazon.com/ebs/?id=docs_gateway){: external} for {{site.data.keyword.satellitelong}} clusters by creating a storage configuration in your location. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

When you create your AWS EBS storage configuration, you provide your AWS credentials which are stored as a Kubernetes secret in the clusters that you assign your configuration to. The secret is mounted inside the CSI controller pod so that when you create a PVC by using one of the {{site.data.keyword.IBM_notm}}-provided storage classes, your AWS credentials are used to dynamically provision an EBS instance.

To use AWS EBS storage for your apps, the {{site.data.keyword.satelliteshort}} hosts that you use for your cluster's worker nodes must reside in AWS.
{: important}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}

## Prerequisites for using AWS EBS
{: #aws-ebs-prereq}

To use the AWS EBS storage template, complete the following tasks:

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
1. [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-satellite-clusters) that runs on compute hosts in Amazon Web Services (AWS). For more information about how to add hosts from AWS to your {{site.data.keyword.satelliteshort}} location so that you can assign them to a cluster, see [Adding AWS hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-aws#aws-host-attach).
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
    
1. [Create an AWS access key ID and secret access key](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html){: external} for your AWS login credentials. These credentials are needed to provision AWS EBS storage in your account. When you assign the storage configuration to your cluster, your AWS access key ID and secret access key are stored in a Kubernetes secret in your cluster.
1. Review the [AWS EBS storage configuration parameters](#aws-ebs-csi-driver-parameter-reference).
1. Review the [AWS EBS storage classes](#sat-ebs-sc-reference). The AWS EBS storage template does not support custom storage classes. 






## Creating a configuration
{: #aws-ebs-csi-driver-config-create}

Before you begin, review the [parameter reference](#aws-ebs-csi-driver-parameter-reference) for the template version that you want to use.
{: important}

### Creating and assigning a configuration in the console
{: aws-ebs-csi-driver-config-create-console}
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

### Creating a configuration in the CLI
{: #aws-ebs-csi-driver-config-create-cli}
{: cli}

1. Copy one of the following example command for the template version that you want to use. For more information about the command, see `ibmcloud sat storage config create` in the [command reference](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).


    Example command to create a version 1.1.0 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name aws-ebs-csi-driver --template-version 1.1.0 --param "aws-access-key=AWS-ACCESS-KEY"  --param "aws-secret-access-key=AWS-SECRET-ACCESS-KEY" 
    ```
    {: pre}

    Example command to create a version 1.5.1 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name aws-ebs-csi-driver --template-version 1.5.1 --param "aws-access-key=AWS-ACCESS-KEY"  --param "aws-secret-access-key=AWS-SECRET-ACCESS-KEY" 
    ```
    {: pre}

    Example command to create a version 1.12.0 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name aws-ebs-csi-driver --template-version 1.12.0 --param "aws-access-key=AWS-ACCESS-KEY"  --param "aws-secret-access-key=AWS-SECRET-ACCESS-KEY" 
    ```
    {: pre}


1. Customize the command based on the settings that you want to use.

1. Run the command to create a configuration.

1. Verify your configuration was created.
    ```sh
    ibmcloud sat storage config get --config CONFIG
    ```
    {: pre}

### Creating a configuration in the API
{: #aws-ebs-csi-driver-config-create-api}

1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 1.1.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"aws-ebs-csi-driver\", \"storage-template-version\": \"1.1.0\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"AWS-ACCESS-KEY\",\"user-secret-parameters\": { \"entry.name\": \"AWS-ACCESS-KEY\",  { \"entry.name\": \"AWS-SECRET-ACCESS-KEY\",\"user-secret-parameters\": { \"entry.name\": \"AWS-SECRET-ACCESS-KEY\", 
    ```
    {: pre}

    Example request to create a version 1.5.1 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"aws-ebs-csi-driver\", \"storage-template-version\": \"1.5.1\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"AWS-ACCESS-KEY\",\"user-secret-parameters\": { \"entry.name\": \"AWS-ACCESS-KEY\",  { \"entry.name\": \"AWS-SECRET-ACCESS-KEY\",\"user-secret-parameters\": { \"entry.name\": \"AWS-SECRET-ACCESS-KEY\", 
    ```
    {: pre}

    Example request to create a version 1.12.0 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"aws-ebs-csi-driver\", \"storage-template-version\": \"1.12.0\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"AWS-ACCESS-KEY\",\"user-secret-parameters\": { \"entry.name\": \"AWS-ACCESS-KEY\",  { \"entry.name\": \"AWS-SECRET-ACCESS-KEY\",\"user-secret-parameters\": { \"entry.name\": \"AWS-SECRET-ACCESS-KEY\", 
    ```
    {: pre}





{{site.data.content.managing-configurations-and-assignments}}

## Deploying an app that uses AWS EBS storage
{: #sat-storage-ebs-deploy}


You can use the `ebs-csi-driver` to dynamically provision AWS EBS storage for the apps in your clusters.
{: shortdesc}

1. List available storage classes and choose the storage class that you want to use.
    ```sh
    oc get sc | grep ebs
    ```
    {: pre}

    To see details for a storage class, use the `oc describe sc <sc-name>` command or review the [storage class reference](#sat-ebs-sc-reference).
    {: tip}

2. Create a PVC that provisions an AWS EBS storage instance with the characteristics that are described in the storage class that you selected. The following example uses the `sat-aws-block-bronze` storage class to create an `st1` HDD AWS EBS storage instance with a size of 125 GB. For more information about this volume type, see [Hard disk drives (HDD)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#hard-disk-drives){: external}.
    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: sat-aws-block-bronze
    spec:
      accessModes:
      - ReadWriteOnce
      storageClassName: sat-aws-block-bronze
      resources:
      requests:
        storage: 125Gi
    ```
    {: codeblock}

3. Create the PVC in your cluster.
    ```sh
    oc apply -f pvc.yaml
    ```
    {: pre}

4. Verify that your PVC is created. Because all {{site.data.keyword.IBM_notm}}-provided storage classes are configured as `WaitForFirstConsumer`, the status of your PVC remains `Pending` until you provision an app that mounts your PVC.
    ```sh
    oc get pvc
    ```
    {: pre}

    Example output
    ```sh
    NAME                   STATUS    VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS           AGE
    sat-aws-block-bronze   Pending                                      sat-aws-block-bronze   17s
    ```
    {: screen}

5. Create a YAML configuration file for your pod that mounts the PVC that you created. When you create this pod, the AWS EBS driver starts to fulfill your storage request by dynamically creating an AWS EBS instance in your AWS account. The following example creates an `nginx` pod that writes the current date and time to a `test.txt` file on your AWS EBS volume mount path.
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
        claimName: sat-aws-block-bronze
    ```
    {: codeblock}

6. Create the pod in your cluster.
    ```sh
    oc apply -f pod.yaml
    ```
    {: pre}

7. Verify that the pod is deployed. Note that it might take a few minutes for the storage request to be fulfilled and for your app to get into a `Running` state.
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

8. Verify that your PVC status changed to `Bound`.
    ```sh
    oc get pvc
    ```
    {: pre}

    Example output
    ```sh
    NAME                 STATUS  VOLUME                                   CAPACITY   ACCESS MODES   STORAGECLASS           AGE
    sat-aws-block-bronze Bound   pvc-86d2f9f4-78d4-4bb2-ab73-39726d144981 125Gi      RWO            sat-aws-block-bronze   33m
    ```
    {: screen}

    If your PVC remains in a `Pending` status, get the details for your PVC by running the `oc describe pvc <pvc_name>` command to see the error that occurred during the provisioning of your AWS EBS storage instance.
    {: tip}

9. Verify that the app can write to your AWS EBS instance.
    1. Log in to your pod.
        ```sh
        oc exec <app_pod_name> -it bash
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

10. Verify that your storage instance is created in AWS.
    1. List the PV that was created for your PVC.
        ```sh
        oc get pv
        ```
        {: pre}

    2. Get the details of your PV and note the ID of your AWS EBS instance that was created in the `source.volumeHandle` field.
        ```sh
        oc describe pv <pv_name>
        ```
        {: pre}

    3. From the [AWS EC2 dashboard](https://console.aws.amazon.com/ec2/v2/home){: external}, select **Elastic Block Store** > **Volumes**.
    4. Find your AWS EBS volume by using the ID that you retrieved earlier.



## Removing AWS EBS storage from your apps
{: #aws-ebs-rm}


If you no longer need your AWS EBS instance, you can remove your PVC, PV, and the AWS EBS instance in your AWS account.
{: shortdesc}

Removing your AWS EBS instance permanently removes all the data that is stored on this instance. This action cannot be undone. Make sure that you back up your data before you delete the AWS EBS instance.
{: important}

1. List your PVCs and note the name of the PVC that you want to remove.
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

3. Delete the PVC. Because all {{site.data.keyword.IBM_notm}}-provided AWS EBS storage classes are specified with a `Delete` reclaim policy, the PV and the AWS EBS instance in your AWS account are automatically deleted when you delete the PVC.
    ```sh
    oc delete pvc <pvc_name>
    ```
    {: pre}

4. Verify that your storage is removed.
    1. Verify that your PV is automatically removed.
        ```sh
        oc get pv
        ```
        {: pre}

    2. From the [AWS EC2 dashboard](https://console.aws.amazon.com/ec2/v2/home){: external}, select **Elastic Block Store** > **Volumes** and verify that your AWS EBS instance is removed.


## Removing the AWS EBS storage configuration from your cluster
{: #aws-ebs-template-rm}

If you no longer plan on using AWS EBS storage in your cluster, you can unassign your cluster from the storage configuration.
{: shortdesc}

Note that if you remove the storage configuration, the driver is then uninstalled from all assigned clusters. Your PVCs, PVs and data are not removed. However, you might not be able to access your data until you re-install the driver in your cluster again.
{: important}


### Removing the AWS EBS storage configuration from the console
{: #aws-ebs-template-rm-ui}
{: ui}

Use the console to remove the AWS EBS storage configuration. 
{: shortdesc}

Note that you must delete your storage assignments before you can successfully delete your storage configuration. 
{: important}

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**
1. Enter the name of your storage configuration.
1. Select **Delete**.

### Removing the AWS EBS storage configuration from the CLI
{: #aws-ebs-template-rm-cli}
{: cli}

Use the CLI to remove the AWS EBS storage configuration.
{: shortdesc}

Note that you must delete your storage assignments before you can successfully delete your storage configuration. 
{: important}

1. List your storage assignments and find the one that you used for your cluster.
    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
    ```
    {: pre}

2. Remove the assignment. After the assignment is removed, the AWS EBS driver pods and storage classes are removed from all clusters that were part of the storage assignment.
    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment_ID>
    ```
    {: pre}

3. Verify that the AWS EBS driver is removed from your cluster.
    1. List of the storage classes in your cluster and verify that the AWS EBS storage classes are removed.
        ```sh
        oc get sc
        ```
        {: pre}

    2. List the pods in the `kube-system` namespace and verify that the AWS EBS storage driver pods are removed.
        ```sh
        oc get pods -n kube-system | grep ebs
        ```
        {: pre}

    3. List the secrets in the `kube-system` namespace and verify that the AWS secret that stored your AWS credentials is removed.
        ```sh
        oc get secrets -n kube-system | grep aws
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
{: #aws-ebs-csi-driver-parameter-reference}

### 1.1.0 parameter reference
{: #1.1.0-parameter-reference}

| Display name | CLI option | Description | Required? |
| --- | --- | --- | --- |
| AWS Access Key ID | `aws-access-key` | AWS Access Key ID. | true | 
| AWS Secret Access Key | `aws-secret-access-key` | AWS Secret Access key. | true | 
{: caption="Table 1. 1.1.0 parameter reference" caption-side="bottom"}


### 1.5.1 parameter reference
{: #1.5.1-parameter-reference}

| Display name | CLI option | Description | Required? |
| --- | --- | --- | --- |
| AWS Access Key ID | `aws-access-key` | AWS Access Key ID. | true | 
| AWS Secret Access Key | `aws-secret-access-key` | AWS Secret Access key. | true | 
{: caption="Table 2. 1.5.1 parameter reference" caption-side="bottom"}


### 1.12.0 parameter reference
{: #1.12.0-parameter-reference}

| Display name | CLI option | Description | Required? |
| --- | --- | --- | --- |
| AWS Access Key ID | `aws-access-key` | AWS Access Key ID. | true | 
| AWS Secret Access Key | `aws-secret-access-key` | AWS Secret Access key. | true | 
{: caption="Table 3. 1.12.0 parameter reference" caption-side="bottom"}




## Storage class reference for AWS EBS
{: #sat-ebs-sc-reference}

Review the {{site.data.keyword.satelliteshort}} storage classes for AWS EBS. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command. Note that data volumes are automatically encrypted by an AWS managed default key. For more information, see [Default KMS key for EBS encryption](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html#EBSEncryption_key_mgmt){: external}. For more information on AWS EBS encryption, see [How AWS EBS encryption works](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html#how-ebs-encryption-works){: external}.
{: shortdesc}

| Storage class name | EBS volume type | File system type | Provisioner | Default IOPS per GB | Size range | Hard disk | Encrypted? | Volume binding mode | Reclaim policy | More info | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `sat-aws-block-gold` **Default** | io2 | ext4 | `ebs.csi.aws.com` | 10 | 10 GiB - 6.25 TiB | SSD | True | WaitforFirstConsumer | Delete | [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#solid-state-drives){: external}
| `sat-aws-block-silver` | gp3 | ext4 | `ebs.csi.aws.com` | N/A | 1 GiB - 16 TiB | SSD | True | WaitforFirstConsumer | Delete | [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#solid-state-drives){: external} |
| `sat-aws-block-bronze` | st1 | ext4 | `ebs.csi.aws.com` | N/A | 125 GiB - 16 TiB | HDD | True |  WaitforFirstConsumer | Delete | [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#hard-disk-drives){: external} |
| `sat-aws-block-bronze-metro` | st1 | ext4 | `ebs.csi.aws.com` | N/A | 125 GiB - 16 TiB | HDD | True |  WaitforFirstConsumer | Delete | [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#hard-disk-drives){: external} |
| `sat-aws-block-silver-metro` | gp3 | ext4 | `ebs.csi.aws.com` | 1 GiB - 16 TiB | SSD | True | WaitforFirstConsumer | Delete | [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#solid-state-drives){: external} |
| `sat-aws-block-gold-metro` | io2 | ext4 | `ebs.csi.aws.com` | 10 | 10 GiB - 6.25 TiB | SSD | True | WaitforFirstConsumer | Delete | [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#solid-state-drives){: external}
{: caption="Table 2. AWS EBS storage class reference." caption-side="bottom"}



## Getting help and support for AWS EBS
{: #sat-ebs-support}

If you run into an issue with using AWS EBS, you can refer to the [AWS Knowledge Center](https://aws.amazon.com/premiumsupport/knowledge-center/){: external} and review some documentation for the most frequently asked questions for various AWS services. The [AWS Support Center](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fsupport%2Fhome%3Fstate%3DhashArgs%2523%252F%26isauthcode%3Dtrue&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fsupportcenter&forceMobileApp=0&code_challenge=u3nT-WHT9gSG_PS93w4dwD6R_PWLj1eOU9GLUMEOkzo&code_challenge_method=SHA-256){: external} is another resource available to AWS customers looking for more in-depth support options. 


