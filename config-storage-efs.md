---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: satellite storage, satellite config, satellite configurations, aws, efs, file storage

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# AWS EFS
{: #config-storage-efs}

Set up [Amazon Elastic File System (EFS)](https://docs.aws.amazon.com/efs/?id=docs_gateway){: external} for {{site.data.keyword.satelliteshort}} clusters by creating a storage configuration in your location. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

To use AWS EFS storage for your apps, your {{site.data.keyword.satelliteshort}} hosts must reside in AWS. Only static provisioning is supported with this storage template. You must manually provision an [AWS EFS file system](https://docs.aws.amazon.com/efs/latest/ug/gs-step-two-create-efs-resources.html){: external} on AWS before you create your {{site.data.keyword.satelliteshort}} storage configuration. Make sure that the EFS device is in the same VPC and subnet that you used for your AWS hosts, and that your hosts and EFS device use the same security group.
{: important}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}

## Prerequisites for using AWS EFS
{: #sat-storage-efs-prereqs}

To use the AWS EFS storage template, complete the following tasks:

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
1. [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/satellite?topic=openshift-satellite-clusters) that runs on compute hosts in Amazon Web Services (AWS). For more information about how to add hosts from AWS to your {{site.data.keyword.satelliteshort}} location so that you can assign them to a cluster, see [Adding AWS hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-aws#aws-host-attach).
1. Manually provision an [AWS EFS file system](https://docs.aws.amazon.com/efs/latest/ug/gs-step-two-create-efs-resources.html){: external} in your AWS account. Make sure that the EFS device is in the same VPC and subnet that you used for your AWS hosts, and that your hosts and EFS device use the same security group.

## Creating an AWS EFS storage configuration
{: #sat-storage-aws-efs}


You can use the [console](#sat-storage-aws-efs-ui) or [CLI](#sat-storage-aws-efs-cli) to create an AWS EFS storage configuration in your location and assign the configuration to your clusters to dynamically provision AWS EFS storage for your apps. 
{: shortdesc}

### Creating an AWS EFS storage configuration from the console
{: #sat-storage-aws-efs-ui}
{: ui}

Use the console to create an AWS EFS storage configuration for your location.
{: shortdesc}

Before you begin, review and complete the [prerequisites](#sat-storage-efs-prereqs) and review the [parameter reference](#sat-storage-aws-efs-params-cli).

1. From the {{site.data.keyword.satelliteshort}} locations dashboard, select the location where you want to create a storage configuration.
1. Select **Storage** > **Create storage configuration**
1. Enter a name for your configuration.
1. Select the **Storage type** that you want to use to create your configuration and the **Version**.
1. On the **Parameters** tab, enter the parameters for your configuration.
1. On the **Secrets** tab, enter the secrets, if required, for your configuration.
1. On the **Storage classes** tab, review the storage classes that are deployed by the configuration or create a custom storage class.
1. On the **Assign to service** tab, select the service that you want to assign your configuration to.
1. Click **Complete** to assign your storage configuration.

### Creating an AWS EFS storage configuration from the CLI
{: #sat-storage-aws-efs-cli}
{: cli}


Use the CLI to create an AWS EFS storage configuration for your location.
{: shortdesc}

Before you begin, review and complete the [prerequisites](#sat-storage-efs-prereqs).

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
    
1. Review the [AWS EFS storage configuration parameters](#sat-storage-aws-efs-params-cli).
1. Create an AWS EFS storage configuration. Replace the variables with the parameters that you retrieved in the previous step.
    ```sh
    ibmcloud sat storage config create --name <config_name> --location <location> --template-name aws-efs-csi-driver --template-version <template_version>
    ```
    {: pre}

1. Verify that your storage configuration is created.
    ```sh
    ibmcloud sat storage config get --config <config_name>
    ```
    {: pre}

1. List your {{site.data.keyword.satelliteshort}} cluster groups and note the group that you want to use. The cluster group determines the {{site.data.keyword.satelliteshort}} clusters where you want to install the AWS EFS driver. If you do not have any cluster groups yet, or your cluster is not yet part of a cluster group, follow these [steps](/docs/satellite?topic=satellite-setup-clusters-satconfig#setup-clusters-satconfig-groups) to create a cluster group and add your clusters. Note that all clusters in a cluster group must belong to the same {{site.data.keyword.satelliteshort}} location.
    ```sh
    ibmcloud sat group ls
    ```
    {: pre}



### Assigning your AWS EFS storage configuration to clusters or cluster groups
{: #efs-config-assign}
{: cli}

1. Create a storage assignment for your cluster group. After you create the assignment, the AWS EFS driver is installed in all clusters that belong to the cluster group. Replace `<group_name>` with the name of your cluster group, `<config_name>` with the name of your storage configuration, and `<assignment_name>` with a name for your storage assignment. For more information, see the [`ibmcloud sat storage assignment create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create) command.
    ```sh
    ibmcloud sat storage assignment create --group <group_name> --config <config_name> --name <assignment_name>
    ```
    {: pre}

2. Verify that your assignment is created.
    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
    ```
    {: pre}

3. Verify that the AWS EFS storage configuration resources are successfully deployed in your cluster.
    1. [Access your cluster](/docs/openshift?topic=openshift-access_cluster).
    2. List the AWS EFS driver pods in the `kube-system` namespace and verify that the status is `Running`.
        ```sh
        oc get pods -n kube-system | grep efs
        ```
        {: pre}

        Example output
        ```sh
        efs-csi-node-gfm9x                      3/3     Running   0          7m48s
        efs-csi-node-hz45b                      3/3     Running   0          7m48s
        efs-csi-node-pv8m7                      3/3     Running   0          7m48s
        ```
        {: screen}

    3. List the AWS EFS storage classes.
        ```sh
        oc get sc | grep aws-file
        ```
        {: pre}

        Example output
        ```sh         
        sat-aws-file-gold     efs.csi.aws.com      Delete          Immediate              false                  8m27s
        ```
        {: screen}

## Deploying an app that uses AWS EFS storage
{: #sat-storage-efs-deploy}
{: cli}

You can use the `efs-csi-driver` to statically provision AWS EFS storage for the apps in your clusters.
{: shortdesc}

Before you begin, make sure that you [created an AWS EFS instance](https://docs.aws.amazon.com/efs/latest/ug/gs-step-two-create-efs-resources.html){: external} in your AWS account.The EFS device must be in the same VPC and subnet that you used for your AWS hosts, and your hosts and EFS device must use the same security group.

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
          storageClassName: sat-aws-file-gold
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
          storageClassName: sat-aws-file-gold
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

## Upgrading an AWS EFS storage configuration
{: #aws-efs-upgrade-config}
{: cli}

You can upgrade your {{site.data.keyword.satelliteshort}} storage configurations to use the latest storage template revision within the same major version. 

1. List your {{site.data.keyword.satelliteshort}} storage configurations, make a note of the {{site.data.keyword.satelliteshort}} configuration you want to upgrade.
    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

1. Upgrade the {{site.data.keyword.satelliteshort}} configuration. Note, only the configuration is updated. If you want to upgrade the assignments that use this configuration, you can specify the `--include-assignments` option or you can manually update each assignment using the `assignment update` command.
    ```sh
    ibmcloud sat storage config upgrade --config CONFIG [--include-assignments]
    ```
    {: pre}

## Upgrading an AWS EFS storage assignment
{: #aws-efs-upgrade-assignment}
{: cli}

You can use the `storage assignment upgrade` command to upgrade an assignment to the latest version of the storage configuration it uses. 

1. List your {{site.data.keyword.satelliteshort}} storage assignments, make a note of the {{site.data.keyword.satelliteshort}} assignment you want to upgrade.
    ```sh
    ibmcloud sat storage assignment ls
    ```
    {: pre}

1. List the {{site.data.keyword.satelliteshort}} storage templates to see the latest available versions.
    ```sh
    ibmcloud sat storage template ls
    ```
    {: pre}

1. Upgrade the {{site.data.keyword.satelliteshort}} assignment.
    ```sh
   ibmcloud sat storage assignment upgrade --assignment ASSIGNMENT
    ```
    {: pre}

## Updating an AWS EFS storage assignment
{: #aws-efs-update-assignment}
{: cli}

## Removing AWS EFS storage from your apps
{: #aws-efs-rm}
{: cli}

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

1. From the {{site.data.keyword.satelliteshort}} storage dashboard, select the storage configuration you want to delete.
1. Select **Actions** > **Delete**
1. Enter the name of your storage configuration.
1. Select **Delete**.


### Removing the AWS EFS storage configuration from the CLI
{: #aws-efs-template-rm-cli}
{: cli}

Use the CLI to remove a storage configuration.
{: shortdesc}

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


## AWS EFS storage configuration parameter reference
{: #sat-storage-aws-efs-params-cli}

| Parameter | Required? | Description | Default if not provided |
| --- | --- | --- | --- |
| `--name` | Required | Enter a name for your storage configuration. | N/A |
| `--template-name` | Required | Enter `aws-efs-csi-driver`. | N/A |
| `--template-version` | Required | Enter the version of the `aws-efs-csi-driver` template that you want to use. To get a list of storage templates and versions, run `ibmcloud sat storage template ls`. | N/A |
{: caption="Table 1. AWS EFS parameter reference." caption-side="top"}


## Storage class reference for AWS EFS
{: #efs-sc-reference}

Review the {{site.data.keyword.satelliteshort}} storage classes for AWS EFS. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | File system | Reclaim policy |
| --- | --- | --- |
| `sat-aws-file-gold` **Default** | NFS | Delete |
{: caption="Table 2. AWS EFS storage class reference." caption-side="top"}


## Getting help and support for AWS EFS
{: #sat-efs-support}

If you run into an issue with using AWS EFS, you can refer to the [AWS Knowledge Center](https://aws.amazon.com/premiumsupport/knowledge-center/){: external} and review some documentation for the most frequently asked questions for various AWS services. The [AWS Support Center](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fsupport%2Fhome%3Fstate%3DhashArgs%2523%252F%26isauthcode%3Dtrue&client_id=arn%3Aaws%3Aiam%3A%3A015428540659%3Auser%2Fsupportcenter&forceMobileApp=0&code_challenge=u3nT-WHT9gSG_PS93w4dwD6R_PWLj1eOU9GLUMEOkzo&code_challenge_method=SHA-256){: external} is another resource available to AWS customers looking for more in-depth support options. 


