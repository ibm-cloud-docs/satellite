---

copyright:
  years: 2020, 2023
lastupdated: "2023-05-09"

keywords: ocs, satellite storage, satellite config, satellite configurations, container storage, remote devices, odf, openshift data foundation

subcollection: satellite

---


{{site.data.keyword.attribute-definition-list}}


# OpenShift Data Foundation for remote devices
{: #storage-odf-remote}

Set up OpenShift Data Foundation for {{site.data.keyword.satellitelong}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster. Be aware that charges occur when you use the OpenShift Data Foundation service. Use the Cost Estimator to generate a cost estimate based on your projected usage.
{: shortdesc}

OpenShift Data Foundation is available in only internal mode, which means that your apps run in the same cluster as your storage. External mode, or storage heavy configurations, where your storage is located in a separate cluster from your apps is not supported.
{: note}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config by selecting the **Enable cluster admin access for Satellite Config** option in the console or including the `--enable-config-admin` option when you create your cluster.
{: important}

## Prerequisites for using ODF for remote devices
{: #sat-storage-odf-remote-prereq}

1. Make sure you have the following permissions.
    - **Editor** for the Billing service.
    - **Manager** and **Editor** for Kubernetes service.
    - **Satellite Link Administrator** and **Reader** for the {{site.data.keyword.satelliteshort}} service.
1. Before you can create a storage configuration, follow the steps to set up a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/satellite?topic=openshift-satellite-clusters).
    - Make sure that you select the **Enable cluster admin access for {{site.data.keyword.satelliteshort}} Config** option when you create the cluster. If you don't enable Administrator (admin) access for {{site.data.keyword.satelliteshort}} Config when creating your cluster, you must re-create your cluster and enable admin access before you can deploy storage.
1. Your cluster must have a minimum of 3 worker nodes with at least 16CPUs and 64GB RAM per worker node.
1. Your cluster must have a remote block storage provisioner available. For example, you can deploy the [AWS EBS](/docs/satellite?topic=satellite-storage-aws-ebs-csi-driver) {{site.data.keyword.satelliteshort}} storage template to install the EBS block drivers that you can then use to provision AWS EBS volumes for ODF.
1. The OCP version must be compatible with the ODF template version that you want to install. For example, for version 4.8 clusters, you must use template version 4.8.
1. **Optional**: [Create an {{site.data.keyword.IBM_notm}} {{site.data.keyword.cos_full_notm}} service instance](#sat-storage-odf-remote-cos).
    1. Create HMAC credentials for your {{site.data.keyword.cos_full_notm}} instance.
    1. Create a Kubernetes secret that uses your {{site.data.keyword.cos_full_notm}} HMAC credentials.



### Optional: Creating the {{site.data.keyword.cos_full_notm}} service instance
{: #sat-storage-odf-remote-cos}

If you want to use {{site.data.keyword.cos_full_notm}} as your object service, [Create an {{site.data.keyword.cos_short}} service instance](#sat-storage-odf-remote-cos) and HMAC credentials. The {{site.data.keyword.cos_short}} instance that you create is used as the NooBaa backing store in your ODF configuration. The backing store is the underlying storage for the data in your NooBaa buckets. If you don't specify an {{site.data.keyword.cos_full_notm}} service instance when you create your storage configuration, the default NooBaa backing store is configured. You can create more backing stores, including {{site.data.keyword.cos_full_notm}} backing stores after assigning the configuration to to your clusters and installing ODF.
{: shortdesc}

Create an instance of {{site.data.keyword.cos_full_notm}} for the backing store of your ODF remote configuration. Then, create a set of HMAC credentials and a Kubernetes secret that uses your {{site.data.keyword.cos_full_notm}} HMAC credentials.

1. Create an {{site.data.keyword.cos_full_notm}} service instance.
    ```sh
    ibmcloud resource service-instance-create noobaa-store cloud-object-storage standard global
    ```
    {: pre}

1. Create HMAC credentials. Make a note of your credentials.
    ```sh
    ibmcloud resource service-key-create cos-cred-rw Writer --instance-name noobaa-store --parameters '{"HMAC": true}'
    ```
    {: pre}





Before you begin, review the [parameter reference](#odf-remote-parameter-reference) for the template version that you want to use.
{: important}


## Creating and assigning a configuration in the console
{: #odf-remote-config-create-console}
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
{: #odf-remote-config-create-cli}
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


    Example command to create a version 4.9 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name odf-remote --template-version 4.9 --param "osd-size=OSD-SIZE"  --param "osd-storage-class=OSD-STORAGE-CLASS"  --param "num-of-osd=NUM-OF-OSD"  [--param "worker-nodes=WORKER-NODES"]  --param "odf-upgrade=ODF-UPGRADE"  --param "billing-type=BILLING-TYPE"  [--param "ibm-cos-endpoint=IBM-COS-ENDPOINT"]  [--param "ibm-cos-location=IBM-COS-LOCATION"]  [--param "ibm-cos-access-key=IBM-COS-ACCESS-KEY"]  [--param "ibm-cos-secret-key=IBM-COS-SECRET-KEY"]  --param "cluster-encryption=CLUSTER-ENCRYPTION"  --param "iam-api-key=IAM-API-KEY"  --param "perform-cleanup=PERFORM-CLEANUP" 
    ```
    {: pre}


    Example command to create a version 4.10 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name odf-remote --template-version 4.10 --param "osd-size=OSD-SIZE"  --param "osd-storage-class=OSD-STORAGE-CLASS"  --param "num-of-osd=NUM-OF-OSD"  [--param "worker-nodes=WORKER-NODES"]  --param "odf-upgrade=ODF-UPGRADE"  --param "billing-type=BILLING-TYPE"  [--param "ibm-cos-endpoint=IBM-COS-ENDPOINT"]  [--param "ibm-cos-location=IBM-COS-LOCATION"]  [--param "ibm-cos-access-key=IBM-COS-ACCESS-KEY"]  [--param "ibm-cos-secret-key=IBM-COS-SECRET-KEY"]  --param "cluster-encryption=CLUSTER-ENCRYPTION"  --param "iam-api-key=IAM-API-KEY"  --param "perform-cleanup=PERFORM-CLEANUP"  --param "kms-encryption=KMS-ENCRYPTION"  [--param "kms-instance-name=KMS-INSTANCE-NAME"]  [--param "kms-instance-id=KMS-INSTANCE-ID"]  [--param "kms-base-url=KMS-BASE-URL"]  [--param "kms-token-url=KMS-TOKEN-URL"]  [--param "kms-root-key=KMS-ROOT-KEY"]  [--param "kms-api-key=KMS-API-KEY"]  --param "ignore-noobaa=IGNORE-NOOBAA" 
    ```
    {: pre}


    Example command to create a version 4.11 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name odf-remote --template-version 4.11 --param "osd-size=OSD-SIZE"  --param "osd-storage-class=OSD-STORAGE-CLASS"  --param "num-of-osd=NUM-OF-OSD"  [--param "worker-nodes=WORKER-NODES"]  --param "odf-upgrade=ODF-UPGRADE"  --param "billing-type=BILLING-TYPE"  [--param "ibm-cos-endpoint=IBM-COS-ENDPOINT"]  [--param "ibm-cos-location=IBM-COS-LOCATION"]  [--param "ibm-cos-access-key=IBM-COS-ACCESS-KEY"]  [--param "ibm-cos-secret-key=IBM-COS-SECRET-KEY"]  --param "cluster-encryption=CLUSTER-ENCRYPTION"  --param "iam-api-key=IAM-API-KEY"  --param "perform-cleanup=PERFORM-CLEANUP"  --param "kms-encryption=KMS-ENCRYPTION"  [--param "kms-instance-name=KMS-INSTANCE-NAME"]  [--param "kms-instance-id=KMS-INSTANCE-ID"]  [--param "kms-base-url=KMS-BASE-URL"]  [--param "kms-token-url=KMS-TOKEN-URL"]  [--param "kms-root-key=KMS-ROOT-KEY"]  [--param "kms-api-key=KMS-API-KEY"]  --param "ignore-noobaa=IGNORE-NOOBAA" 
    ```
    {: pre}


    Example command to create a version 4.12 configuration.

    ```sh
    ibmcloud sat storage config create --location LOCATION --name NAME --template-name odf-remote --template-version 4.12 --param "osd-size=OSD-SIZE"  --param "osd-storage-class=OSD-STORAGE-CLASS"  --param "num-of-osd=NUM-OF-OSD"  [--param "worker-nodes=WORKER-NODES"]  --param "odf-upgrade=ODF-UPGRADE"  --param "billing-type=BILLING-TYPE"  [--param "ibm-cos-endpoint=IBM-COS-ENDPOINT"]  [--param "ibm-cos-location=IBM-COS-LOCATION"]  [--param "ibm-cos-access-key=IBM-COS-ACCESS-KEY"]  [--param "ibm-cos-secret-key=IBM-COS-SECRET-KEY"]  --param "cluster-encryption=CLUSTER-ENCRYPTION"  --param "iam-api-key=IAM-API-KEY"  --param "perform-cleanup=PERFORM-CLEANUP"  --param "kms-encryption=KMS-ENCRYPTION"  [--param "kms-instance-name=KMS-INSTANCE-NAME"]  [--param "kms-instance-id=KMS-INSTANCE-ID"]  [--param "kms-base-url=KMS-BASE-URL"]  [--param "kms-token-url=KMS-TOKEN-URL"]  [--param "kms-root-key=KMS-ROOT-KEY"]  [--param "kms-api-key=KMS-API-KEY"]  --param "ignore-noobaa=IGNORE-NOOBAA" 
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
{: #odf-remote-config-create-api}
{: api}

1. Generate an API key, then request a refresh token. For more information, see [Generating an IBM Cloud IAM token by using an API key](/docs/account?topic=account-iamtoken_from_apikey).

1. Copy one of the following example requests and replace the variables that you want to use.


    Example request to create a version 4.9 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"odf-remote\", \"storage-template-version\": \"4.9\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"OSD-SIZE\", { \"entry.name\": \"OSD-STORAGE-CLASS\", { \"entry.name\": \"NUM-OF-OSD\", { \"entry.name\": \"WORKER-NODES\", { \"entry.name\": \"ODF-UPGRADE\", { \"entry.name\": \"BILLING-TYPE\", { \"entry.name\": \"IBM-COS-ENDPOINT\", { \"entry.name\": \"IBM-COS-LOCATION\", { \"entry.name\": \"CLUSTER-ENCRYPTION\", { \"entry.name\": \"PERFORM-CLEANUP\",\"user-secret-parameters\": { \"entry.name\": \"IBM-COS-ACCESS-KEY\",{ \"entry.name\": \"IBM-COS-SECRET-KEY\",{ \"entry.name\": \"IAM-API-KEY\",}
    ```
    {: pre}


    Example request to create a version 4.10 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"odf-remote\", \"storage-template-version\": \"4.10\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"OSD-SIZE\", { \"entry.name\": \"OSD-STORAGE-CLASS\", { \"entry.name\": \"NUM-OF-OSD\", { \"entry.name\": \"WORKER-NODES\", { \"entry.name\": \"ODF-UPGRADE\", { \"entry.name\": \"BILLING-TYPE\", { \"entry.name\": \"IBM-COS-ENDPOINT\", { \"entry.name\": \"IBM-COS-LOCATION\", { \"entry.name\": \"CLUSTER-ENCRYPTION\", { \"entry.name\": \"PERFORM-CLEANUP\", { \"entry.name\": \"KMS-ENCRYPTION\", { \"entry.name\": \"KMS-INSTANCE-NAME\", { \"entry.name\": \"KMS-INSTANCE-ID\", { \"entry.name\": \"KMS-BASE-URL\", { \"entry.name\": \"KMS-TOKEN-URL\", { \"entry.name\": \"IGNORE-NOOBAA\",\"user-secret-parameters\": { \"entry.name\": \"IBM-COS-ACCESS-KEY\",{ \"entry.name\": \"IBM-COS-SECRET-KEY\",{ \"entry.name\": \"IAM-API-KEY\",{ \"entry.name\": \"KMS-ROOT-KEY\",{ \"entry.name\": \"KMS-API-KEY\",}
    ```
    {: pre}


    Example request to create a version 4.11 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"odf-remote\", \"storage-template-version\": \"4.11\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"OSD-SIZE\", { \"entry.name\": \"OSD-STORAGE-CLASS\", { \"entry.name\": \"NUM-OF-OSD\", { \"entry.name\": \"WORKER-NODES\", { \"entry.name\": \"ODF-UPGRADE\", { \"entry.name\": \"BILLING-TYPE\", { \"entry.name\": \"IBM-COS-ENDPOINT\", { \"entry.name\": \"IBM-COS-LOCATION\", { \"entry.name\": \"CLUSTER-ENCRYPTION\", { \"entry.name\": \"PERFORM-CLEANUP\", { \"entry.name\": \"KMS-ENCRYPTION\", { \"entry.name\": \"KMS-INSTANCE-NAME\", { \"entry.name\": \"KMS-INSTANCE-ID\", { \"entry.name\": \"KMS-BASE-URL\", { \"entry.name\": \"KMS-TOKEN-URL\", { \"entry.name\": \"IGNORE-NOOBAA\",\"user-secret-parameters\": { \"entry.name\": \"IBM-COS-ACCESS-KEY\",{ \"entry.name\": \"IBM-COS-SECRET-KEY\",{ \"entry.name\": \"IAM-API-KEY\",{ \"entry.name\": \"KMS-ROOT-KEY\",{ \"entry.name\": \"KMS-API-KEY\",}
    ```
    {: pre}


    Example request to create a version 4.12 configuration.

    ```sh
    curl -X POST "https://containers.cloud.ibm.com/global/v2/storage/satellite/createStorageConfigurationByController" -H "accept: application/json" -H "Authorization: TOKEN" -H "Content-Type: application/json" -d "{ \"config-name\": \"string\", \"controller\": \"string\", \"storage-class-parameters\": [ { \"additionalProp1\": \"string\", \"additionalProp2\": \"string\", \"additionalProp3\": \"string\" } ], \"storage-template-name\": \"odf-remote\", \"storage-template-version\": \"4.12\", \"update-assignments\": true, \"user-config-parameters\": { \"entry.name\": \"OSD-SIZE\", { \"entry.name\": \"OSD-STORAGE-CLASS\", { \"entry.name\": \"NUM-OF-OSD\", { \"entry.name\": \"WORKER-NODES\", { \"entry.name\": \"ODF-UPGRADE\", { \"entry.name\": \"BILLING-TYPE\", { \"entry.name\": \"IBM-COS-ENDPOINT\", { \"entry.name\": \"IBM-COS-LOCATION\", { \"entry.name\": \"CLUSTER-ENCRYPTION\", { \"entry.name\": \"PERFORM-CLEANUP\", { \"entry.name\": \"KMS-ENCRYPTION\", { \"entry.name\": \"KMS-INSTANCE-NAME\", { \"entry.name\": \"KMS-INSTANCE-ID\", { \"entry.name\": \"KMS-BASE-URL\", { \"entry.name\": \"KMS-TOKEN-URL\", { \"entry.name\": \"IGNORE-NOOBAA\",\"user-secret-parameters\": { \"entry.name\": \"IBM-COS-ACCESS-KEY\",{ \"entry.name\": \"IBM-COS-SECRET-KEY\",{ \"entry.name\": \"IAM-API-KEY\",{ \"entry.name\": \"KMS-ROOT-KEY\",{ \"entry.name\": \"KMS-API-KEY\",}
    ```
    {: pre}








{{site.data.content.assignment-create-console}}
{{site.data.content.assignment-create-cli}}
{{site.data.content.assignment-create-api}}

### Optional: Adding additional worker nodes to your ODF configuration
{: #add-worker-nodes-odf-remote}
{: cli}

1. Add more worker nodes to your ODF configuration.
    ```sh
    ibmcloud sat storage config param set --config <config-name> -p "worker-nodes=<comma-separated values of new worker-nodes followed by list of old worker-nodes>" --apply
    ```
    {: pre}




1. Verify that the storage configuration resources are deployed.

    1. Get the `storagecluster` that you deployed and verify that the phase is `Ready`.
        ```sh
        oc get storagecluster -n openshift-storage
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


## Deploying an app that uses OpenShift Data Foundation
{: #sat-storage-odf-remote-deploy}

You can use the ODF storage classes to create PVCs for the apps in your clusters.
{: shortdesc}

1. Create a YAML configuration file for your PVC. In order for the PVC to match the PV, you must use the same values for the storage class and the size of the storage.

    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: ocs-pvc
    spec:
      accessModes:
        - ReadWriteOnce
      storageClassName: sat-ocs-cephfs-gold
      resources:
        requests:
          storage: 5Gi
    ```
    {: codeblock}

1. Create the PVC in your cluster.

    ```sh
    oc apply -f pvc.yaml
    ```
    {: pre}

1. Create a YAML configuration file for a pod that mounts the PVC that you created. The following example creates an `nginx` pod that writes the current date and time to a `test.txt` file.

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
          claimName: ocs-pvc
    ```
    {: codeblock}

1. Create the pod in your cluster.

    ```sh
    oc apply -f pod.yaml
    ```
    {: pre}

1. Verify that the pod is deployed. Note that it might take a few minutes for your app to get into a `Running` state.

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

1. Verify that the app can write data.

    1. Log in to your pod.
    
        ```sh
        oc exec <app-pod-name> -it bash
        ```
        {: pre}

    1. Display the contents of the `test.txt` file to confirm that your app can write data to your persistent storage.
    
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

    1. Exit the pod.
    
        ```sh
        exit
        ```
        {: pre}

## Scaling up your ODF configuration
{: #sat-storage-odf-remote-scale-config}


To scale your ODF configuration by adding disks to your worker nodes, increase the `num-of-osd` parameter value. 

```sh
ibmcloud sat storage config param set --config <config-name> -p num-of-osd=2 --apply
```
{: pre}

## Upgrading your ODF configuration
{: #sat-storage-odf-remote-upgrade-config}



To upgrade the ODF version of your configuration, complete the following steps:
1. Get the details of your ODF configuration.
    ```sh
    ibmcloud sat storage config get <config-name>
    ```
1. Create a new configuration. Make sure to change the `template-version` value to the version that you want to upgrade to and set the `odf-upgrade` parameter to `true`.

    In the following example, the ODF configuration is updated to use template version 4.1. When you create the configuration, the following parameter changes are required.
    - `name` - Enter a name for your new configuration.
    - `template-name` - Use the same parameter value as in your existing configuration.
    - `template-version` - Enter the template version that you want to use to upgrade your configuration.
    - `mon-storage-class` - **Version 4.7 only**: Use the same parameter value as in your existing configuration.
    - `iam-api-key` - Use the same parameter as in your existing configuration.
    - `mon-size` - **Version 4.7 only**: Use the same parameter value as in your existing configuration.
    - `osd-storage-class` - Use the same parameter value as in your existing configuration.
    - `osd-size` - Use the same parameter value as in your existing configuration.
    - `num-of-osd` - Use the same parameter value as in your existing configuration.
    - `worker-nodes` - Use the same parameter value as in your existing configuration.
    - `ibm-cos-endpoint` - Use the same parameter value as in your existing configuration.
    - `ibm-cos-location` - Use the same parameter value as in your existing configuration.
    - `ibm-cos-access-key` - Use the same parameter value as in your existing configuration.
    - `ibm-cos-secret-key` - Use the same parameter value as in your existing configuration.
    - `odf-upgrade` - Enter `true` to upgrade your `ocs-cluster` to the template version that you specified.

1. Get the details of your ODF configuration and save the configuration details.
    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}

1. Delete the existing assignment. 
    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment>
    ```
    {: pre}

1.  When you upgrade your ODF version, you must enter the same configuration details and set the `template-version` value to the version you want to upgrade to and set the `odf-upgrade` parameter to `true`. Don't specify the {{site.data.keyword.cos_short}} parameters when you create your configuration if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration. Note that Kubernetes resources can't contain capital letters or special characters. 

    Example `storage config create` command for version 4.8 clusters.
    ```sh
    ibmcloud sat storage config create --name <config_name> --location <location> --template-name odf-remote --template-version <template_version>  -p "osd-storage-class=vpc-custom-10iops-tier" -p "osd-size=<osd-size>" -p "num-of-osd=1" -p "worker-nodes=<worker-node-name>,<worker-node-name>,<worker-node-name>" -p "ibm-cos-endpoint=<cos-endpoint>" -p "ibm-cos-location=<ibm-cos-location>" -p "ibm-cos-access-key=<ibm-cos-access-key>" -p "ibm-cos-secret-key=<ibm-cos-secret-key>" -p "iam-api-key=<iam-api-key>"
    ```
    {: pre}

    Example `storage config create` command for version 4.7 clusters.
    ```sh
    ibmcloud sat storage config create --name <config_name> --location <location> --template-name odf-remote --template-version <template_version>  -p "mon-storage-class=vpc-custom-10iops-tier" -p "mon-size=<mon-size>" -p "osd-storage-class=vpc-custom-10iops-tier" -p "osd-size=<osd-size>" -p "num-of-osd=1" -p "worker-nodes=<worker-node-name>,<worker-node-name>,<worker-node-name>" -p "ibm-cos-endpoint=<cos-endpoint>" -p "ibm-cos-location=<ibm-cos-location>" -p "ibm-cos-access-key=<ibm-cos-access-key>" -p "ibm-cos-secret-key=<ibm-cos-secret-key>" -p "iam-api-key=<iam-api-key>"
    ```
    {: pre}

1. [Assign your configuration to your clusters](#storage-odf-remote-include-assignment-create-cli).


### Removing the ODF remote storage assignment from the command line
{: #odf-remote-template-rm-cli}


Use the command line to remove a storage assignment.
{: shortdesc}

1. Run the following command to delete your ODF storage assignment.
    ```sh
    oc delete ocscluster --all
    ```
    {: pre}

1. List your storage assignments and find the one that you used for your cluster.
    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
    ```
    {: pre}

1. Remove the assignment. After the assignment is removed, the ODF driver pods and storage classes are removed from all clusters that were part of the storage assignment.
    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment_ID>
    ```
    {: pre}





## Parameter reference
{: #odf-remote-parameter-reference}

### 4.9 parameter reference
{: #4.9-parameter-reference}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| OSD pod volume size | `osd-size` | Config | The OSD storage size in Gi. The default value is `100Gi`. | true | `100Gi` |
| OSD pod storage class | `osd-storage-class` | Config | The storage class to use when dynamically provisioning volumes for the OSD pods. | true | N/A |
| Number of OSD volumes | `num-of-osd` | Config | The number of OSD volumes that you want to provision. The total storage available to your apps is equal to the volume size (osd-size) multiplied by the number of volumes (num-of-osd). The default value is `1`. | true | `1` |
| Worker node names | `worker-nodes` | Config | The node names where you want to deploy ODF. Leave this field blank to deploy ODF across all worker nodes in your cluster. The minimum number of worker nodes is 3. You can find your worker node names by running `oc get nodes`. | false | N/A |
| Upgrade | `odf-upgrade` | Config | Set to `true` if you want to upgrade the ODF version. | true | `false` |
| Billing type | `billing-type` | Config | The billing type you want to use. Choose from `essentials` or `advanced`. | true | `advanced` |
| IBM COS endpoint | `ibm-cos-endpoint` | Config | The IBM COS regional public endpoint. | false | N/A |
| IBM COS location constraint | `ibm-cos-location` | Config | The location constraint that you want to use when creating your bucket. For example `us-east-standard`. | false | N/A |
| Access key ID | `ibm-cos-access-key` | Secret | Your IBM COS HMAC access key ID . | false | N/A |
| Secret access key | `ibm-cos-secret-key` | Secret | Your IBM COS HMAC secret access key. | false | N/A |
| Encryption enabled | `cluster-encryption` | Config | Set to `true` if you want to enable cluster-wide encryption. | true | `false` |
| IAM API key | `iam-api-key` | Secret | Your IAM API key. | true | N/A |
| Perform Cleanup | `perform-cleanup` | Config | Set to `true` if you want to perform complete cleanup of ODF on assignment deletion | true | `false` |
{: caption="Table 1. 4.9 parameter reference" caption-side="bottom"}


### 4.10 parameter reference
{: #4.10-parameter-reference}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| OSD pod volume size | `osd-size` | Config | The OSD storage size in Gi. The default value is `100Gi`. | true | `100Gi` |
| OSD pod storage class | `osd-storage-class` | Config | The storage class to use when dynamically provisioning volumes for the OSD pods. | true | N/A |
| Number of OSD volumes | `num-of-osd` | Config | The number of OSD volumes that you want to provision. The total storage available to your apps is equal to the volume size (osd-size) multiplied by the number of volumes (num-of-osd). The default value is `1`. | true | `1` |
| Worker node names | `worker-nodes` | Config | The node names where you want to deploy ODF. Leave this field blank to deploy ODF across all worker nodes in your cluster. The minimum number of worker nodes is 3. You can find your worker node names by running `oc get nodes`. | false | N/A |
| Upgrade | `odf-upgrade` | Config | Set to `true` if you want to upgrade the ODF version. | true | `false` |
| Billing type | `billing-type` | Config | The billing type you want to use. Choose from `essentials` or `advanced`. | true | `advanced` |
| IBM COS endpoint | `ibm-cos-endpoint` | Config | The IBM COS regional public endpoint. | false | N/A |
| IBM COS location constraint | `ibm-cos-location` | Config | The location constraint that you want to use when creating your bucket. For example `us-east-standard`. | false | N/A |
| Access key ID | `ibm-cos-access-key` | Secret | Your IBM COS HMAC access key ID . | false | N/A |
| Secret access key | `ibm-cos-secret-key` | Secret | Your IBM COS HMAC secret access key. | false | N/A |
| Encryption enabled | `cluster-encryption` | Config | Set to `true` if you want to enable cluster-wide encryption. | true | `false` |
| IAM API key | `iam-api-key` | Secret | Your IAM API key. | true | N/A |
| Perform Cleanup | `perform-cleanup` | Config | Set to `true` if you want to perform complete cleanup of ODF on assignment deletion. | true | `false` |
| KMS encryption | `kms-encryption` | Config | Set to `true` if you want to enable storage class encryption. | true | `false` |
| KMS instance name | `kms-instance-name` | Config | Your KMS instance name. The instance name must only include alphanumeric characters, `-`, `_` or `.` and start and end with an alphanumeric character. | false | N/A |
| KMS instance id | `kms-instance-id` | Config | Your KMS instance id. | false | N/A |
| KMS instance Base URL | `kms-base-url` | Config | Your KMS instance public URL to connect to the instance. | false | N/A |
| KMS instance API key token URL | `kms-token-url` | Config | API key token URL to generate token for KMS instance. | false | N/A |
| KMS root key | `kms-root-key` | Secret | KMS root key of your instance. | false | N/A |
| KMS IAM API key | `kms-api-key` | Secret | IAM API key to access the KMS instance. The API key that you provide must have at least Viewer access to the KMS instance. | false | N/A |
| Ignore Noobaa | `ignore-noobaa` | Config | Set to `true` if you do not want to deploy MultiCloud Object Gateway (Noobaa) | true | `false` |
{: caption="Table 2. 4.10 parameter reference" caption-side="bottom"}


### 4.11 parameter reference
{: #4.11-parameter-reference}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| OSD pod volume size | `osd-size` | Config | The OSD storage size in Gi. The default value is `100Gi`. | true | `100Gi` |
| OSD pod storage class | `osd-storage-class` | Config | The storage class to use when dynamically provisioning volumes for the OSD pods. | true | N/A |
| Number of OSD volumes | `num-of-osd` | Config | The number of OSD volumes that you want to provision. The total storage available to your apps is equal to the volume size (osd-size) multiplied by the number of volumes (num-of-osd). The default value is `1`. | true | `1` |
| Worker node names | `worker-nodes` | Config | The node names where you want to deploy ODF. Leave this field blank to deploy ODF across all worker nodes in your cluster. The minimum number of worker nodes is 3. You can find your worker node names by running `oc get nodes`. | false | N/A |
| Upgrade | `odf-upgrade` | Config | Set to `true` if you want to upgrade the ODF version. | true | `false` |
| Billing type | `billing-type` | Config | The billing type you want to use. Choose from `essentials` or `advanced`. | true | `advanced` |
| IBM COS endpoint | `ibm-cos-endpoint` | Config | The IBM COS regional public endpoint. | false | N/A |
| IBM COS location constraint | `ibm-cos-location` | Config | The location constraint that you want to use when creating your bucket. For example `us-east-standard`. | false | N/A |
| Access key ID | `ibm-cos-access-key` | Secret | Your IBM COS HMAC access key ID . | false | N/A |
| Secret access key | `ibm-cos-secret-key` | Secret | Your IBM COS HMAC secret access key. | false | N/A |
| Encryption enabled | `cluster-encryption` | Config | Set to `true` if you want to enable cluster-wide encryption. | true | `false` |
| IAM API key | `iam-api-key` | Secret | Your IAM API key. | true | N/A |
| Perform Cleanup | `perform-cleanup` | Config | Set to `true` if you want to perform complete cleanup of ODF on assignment deletion. | true | `false` |
| KMS encryption | `kms-encryption` | Config | Set to `true` if you want to enable storage class encryption. | true | `false` |
| KMS instance name | `kms-instance-name` | Config | Your KMS instance name. The instance name must only include alphanumeric characters, `-`, `_` or `.` and start and end with an alphanumeric character. | false | N/A |
| KMS instance id | `kms-instance-id` | Config | Your KMS instance id. | false | N/A |
| KMS instance Base URL | `kms-base-url` | Config | Your KMS instance public URL to connect to the instance. | false | N/A |
| KMS instance API key token URL | `kms-token-url` | Config | API key token URL to generate token for KMS instance. | false | N/A |
| KMS root key | `kms-root-key` | Secret | KMS root key of your instance. | false | N/A |
| KMS IAM API key | `kms-api-key` | Secret | IAM API key to access the KMS instance. The API key that you provide must have at least Viewer access to the KMS instance. | false | N/A |
| Ignore Noobaa | `ignore-noobaa` | Config | Set to `true` if you do not want to deploy MultiCloud Object Gateway (Noobaa) | true | `false` |
{: caption="Table 3. 4.11 parameter reference" caption-side="bottom"}


### 4.12 parameter reference
{: #4.12-parameter-reference}

| Display name | CLI option | Type | Description | Required? | Default value | 
| --- | --- | --- | --- | --- | --- |
| OSD pod volume size | `osd-size` | Config | The OSD storage size in Gi. The default value is `100Gi`. | true | `100Gi` |
| OSD pod storage class | `osd-storage-class` | Config | The storage class to use when dynamically provisioning volumes for the OSD pods. | true | N/A |
| Number of OSD volumes | `num-of-osd` | Config | The number of OSD volumes that you want to provision. The total storage available to your apps is equal to the volume size (osd-size) multiplied by the number of volumes (num-of-osd). The default value is `1`. | true | `1` |
| Worker node names | `worker-nodes` | Config | The node names where you want to deploy ODF. Leave this field blank to deploy ODF across all worker nodes in your cluster. The minimum number of worker nodes is 3. You can find your worker node names by running `oc get nodes`. | false | N/A |
| Upgrade | `odf-upgrade` | Config | Set to `true` if you want to upgrade the ODF version. | true | `false` |
| Billing type | `billing-type` | Config | The billing type you want to use. Choose from `essentials` or `advanced`. | true | `advanced` |
| IBM COS endpoint | `ibm-cos-endpoint` | Config | The IBM COS regional public endpoint. | false | N/A |
| IBM COS location constraint | `ibm-cos-location` | Config | The location constraint that you want to use when creating your bucket. For example `us-east-standard`. | false | N/A |
| Access key ID | `ibm-cos-access-key` | Secret | Your IBM COS HMAC access key ID . | false | N/A |
| Secret access key | `ibm-cos-secret-key` | Secret | Your IBM COS HMAC secret access key. | false | N/A |
| Encryption enabled | `cluster-encryption` | Config | Set to `true` if you want to enable cluster-wide encryption. | true | `false` |
| IAM API key | `iam-api-key` | Secret | Your IAM API key. | true | N/A |
| Perform Cleanup | `perform-cleanup` | Config | Set to `true` if you want to perform complete cleanup of ODF on assignment deletion. | true | `false` |
| KMS encryption | `kms-encryption` | Config | Set to `true` if you want to enable storage class encryption. | true | `false` |
| KMS instance name | `kms-instance-name` | Config | Your KMS instance name. The instance name must only include alphanumeric characters, `-`, `_` or `.` and start and end with an alphanumeric character. | false | N/A |
| KMS instance id | `kms-instance-id` | Config | Your KMS instance id. | false | N/A |
| KMS instance Base URL | `kms-base-url` | Config | Your KMS instance public URL to connect to the instance. | false | N/A |
| KMS instance API key token URL | `kms-token-url` | Config | API key token URL to generate token for KMS instance. | false | N/A |
| KMS root key | `kms-root-key` | Secret | KMS root key of your instance. | false | N/A |
| KMS IAM API key | `kms-api-key` | Secret | IAM API key to access the KMS instance. The API key that you provide must have at least Viewer access to the KMS instance. | false | N/A |
| Ignore Noobaa | `ignore-noobaa` | Config | Set to `true` if you do not want to deploy MultiCloud Object Gateway (Noobaa) | true | `false` |
{: caption="Table 4. 4.12 parameter reference" caption-side="bottom"}





## Storage class reference for OpenShift Data Foundation for remote devices
{: #sat-storage-odf-remote-sc-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for OpenShift Data Foundation. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | Type | File system | Provisioner | Volume binding mode | Allow volume expansion | Reclaim policy |
| --- | --- | --- | --- | --- | --- | --- |
| `sat-ocs-cephrbd-gold` | Block | ext4 | `openshift-storage.rbd.csi.ceph.com` | Immediate | True | Delete |
| `sat-ocs-cephfs-gold` | File | N/A | `openshift-storage.cephfs.csi.ceph.com` | Immediate | True |Delete |
| `sat-ocs-cephrgw-gold` | Object | N/A | `openshift-storage.ceph.rook.io/bucket` | Immediate | N/A | Delete |
| `sat-ocs-noobaa-gold` **Default** | OBC | N/A | `openshift-storage.noobaa.io/obc` | Immediate | N/A | Delete |
| `sat-ocs-cephrbd-gold-metro` | Block | ext4 | `openshift-storage.rbd.csi.ceph.com` | WaitForFirstConsumer | True | Delete |
| `sat-ocs-cephfs-gold-metro` | File | N/A | `openshift-storage.cephfs.csi.ceph.com` | WaitForFirstConsumer | True | Delete |
{: caption="Table 4. Storage class reference for OpenShift Container storage" caption-side="bottom"}
