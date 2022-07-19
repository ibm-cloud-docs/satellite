---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-19"

keywords: odf, satellite storage, satellite config, satellite configurations, container storage, local storage, OpenShift Data Foundation

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}



# OpenShift Data Foundation using local disks
{: #config-storage-odf-local}

Set up OpenShift Data Foundation for {{site.data.keyword.satellitelong}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

OpenShift Data Foundation is available in only internal mode, which means that your apps run in the same cluster as your storage. External mode, or storage heavy configurations, where your storage is located in a separate cluster from your apps is not supported.
{: note}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}

## Prerequisites for ODF
{: #sat-storage-odf-local-prereq}

To use the ODF storage with the local storage operator and local storage devices, complete the following tasks:

1. [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. [Set up {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
1. [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/satellite?topic=openshift-satellite-clusters).
    - Your cluster must have a minimum of 3 worker nodes with at least 16CPUs and 64GB RAM per worker node.
    - **Version 4.8 and later**: Your hosts must meet the [{{site.data.keyword.satelliteshort}} host requirements](/docs/satellite?topic=satellite-host-reqs) in addition to having one of the following local storage configurations.
        * One extra raw device per worker node in addition to the minimum host requirements. This disk must not be partitioned or have formatted file systems.
        * One extra raw partition per worker node in addition to the minimum host requirements. If your host storage devices are partitioned, they must have at least one extra raw/unformatted partition per disk, per worker node.
    - **Version 4.7**: Your hosts must meet the [{{site.data.keyword.satelliteshort}} host requirements](/docs/satellite?topic=satellite-host-reqs) in addition to having one of the following local storage configurations.
        * Two extra raw devices per worker node in addition to the minimum host requirements. These disks must not be partitioned or formatted file systems. If your devices are not partitioned, each node must have 2 free disks. One disk for the OSD and one disk for the MON.
        * Two extra raw partitions per worker node in addition to the minimum host requirements. These disks must not be  formatted file systems. If your raw devices are partitioned, they must have at least 2 partitions per disk, per worker node.
1. **Optional**: If you want to use {{site.data.keyword.cos_full_notm}} as your object service, [Create an {{site.data.keyword.cos_short}} service instance](#sat-storage-odf-local-cos) and HMAC credentials. The {{site.data.keyword.cos_short}} instance that you create is used as the NooBaa backing store in your ODF configuration. The backing store is the underlying storage for the data in your NooBaa buckets. If you don't specify an {{site.data.keyword.cos_full_notm}} service instance when you create your storage configuration, the default NooBaa backing store is configured. You can create additional backing stores, including {{site.data.keyword.cos_full_notm}} backing stores after your storage configuration is assigned to your clusters and ODF is installed.
1. **Optional**: [Get the details of the raw, unformatted devices that you want to use for your configuration](#sat-storage-odf-local-devices). The device IDs of your storage disks are used to create your {{site.data.keyword.satelliteshort}} storage configuration.


## (Optional) Setting up an {{site.data.keyword.cos_full_notm}} backing store for ODF
{: #sat-storage-odf-local-cos}

If you want to use {{site.data.keyword.cos_full_notm}} as your object service, create an {{site.data.keyword.cos_short}} service instance and HMAC credentials. The {{site.data.keyword.cos_short}} instance that you create is the NooBaa backing store in your ODF configuration. The backing store is the underlying storage for the data in your NooBaa buckets. If you don't specify an {{site.data.keyword.cos_full_notm}} service instance when you create your storage configuration, the default NooBaa backing store is configured. You can create more backing stores, including {{site.data.keyword.cos_full_notm}} backing stores after assigning the configuration to to your clusters and installing ODF.
{: shortdesc}

1. Create an {{site.data.keyword.cos_full_notm}} service instance.

    ```sh
    ibmcloud resource service-instance-create noobaa-store cloud-object-storage standard global
    ```
    {: pre}

1. Create HMAC credentials and note the service access key and access key ID of your HMAC credentials.

    ```sh
    ibmcloud resource service-key-create cos-cred-rw Writer --instance-name noobaa-store --parameters '{"HMAC": true}'
    ```
    {: pre}


## (Optional) Getting the device details for your ODF configuration
{: #sat-storage-odf-local-devices}

The following steps show how you can manually retrieve the local device information from each worker node in your cluster. Note that for version 4.8 clusters and later, you can automatically find the available devices on your worker nodes by setting the `auto-discover-devices=true` parameter. However, if you have a 4.7 cluster, you must complete the following steps to retrieve the device paths for the disks on your worker nodes.
{: important}

When you create your ODF configuration, you must specify the device paths of the disks that you want to use in your storage cluster. The storage cluster is comprised of the object storage daemon (OSD) pods and the monitoring (MON) pods. The devices that you specify as OSD devices are your storage devices where your app data is stored and the devices that you specify as MON devices are managed by the MON pod and used to store and maintain the storage cluster mapping and monitor storage events. For more information about the OSD and MON, see the [Ceph documentation](https://docs.ceph.com/en/latest/start/intro/){: external}.

1. Log in to your cluster and get a list of available worker nodes. Make a note of the worker nodes that you want to use in your ODF configuration.

    ```sh
    oc get nodes
    ```
    {: pre}

1. Log in to each worker node that you want to use for your ODF configuration.

    ```sh
    oc debug node/<node-name>
    ```
    {: pre}

1. When the debug pod is deployed on the worker node, run the following commands to list the available disks on the worker node.

    1. Allow host binaries.
    
        ```sh
        chroot /host
        ```
        {: pre}

    1. List your devices.
    
        ```sh
        lsblk
        ```
        {: pre}

1. Review the command output for available disks. Disks that can be used for your ODF configuration must be unmounted. In the following example output from the `lsblk` command, the `sdc` disk has two available, unformatted partitions that you can use for the OSD and MON device paths for this worker node. If your worker node has raw disks without partitions, you need one disk for the OSD and one disk for the MON. As a best practice, and to maximize storage capacity on this disk, you can specify the smaller partition or disk for the MON, and the larger partition or disk for the OSD. Note that the initial storage capacity of your ODF configuration is equal to the size of the disk that you specify as the `osd-device-path` when you create your configuration.


    ```sh
    NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    sda      8:0    0   931G  0 disk
    |-sda1   8:1    0   256M  0 part /boot
    |-sda2   8:2    0     1G  0 part
    `-sda3   8:3    0 929.8G  0 part /
    sdb      8:16   0 744.7G  0 disk
    `-sdb1   8:17   0 744.7G  0 part /disk1
    sdc      8:32   0 744.7G  0 disk
    |-sdc1   8:33   0  18.6G  0 part
    `-sdc2   8:34   0 260.8G  0 part
    ```
    {: screen}

1. Find the `by-id` for each disk that you want to use in your configuration. In this case, the `sdc1` and `sdc2` partitions are unformatted and unmounted. The `by-id` for each disk is specified as a command parameter when you create your configuration.

    ```sh
    ls -l /dev/disk/by-id/
    ```
    {: pre}
    
    If you have VMware hosts, run the following command.
    
    ```sh
    ls -l /dev/disk/by-path/
    ```
    {: pre}
    
1. Review the command output and make a note of the `by-id` values for the disks that you want to use in your configuration. In the following example output, the disk ids for the `sdc1` and `sdc2` partitions are: `scsi-3600605b00d87b43027b3bc310a64c6c9-part1` and `scsi-3600605b00d87b43027b3bc310a64c6c9-part2`.
    ```sh
    lrwxrwxrwx. 1 root root  9 Feb  9 04:15 scsi-3600605b00d87b43027b3bbb603150cc6 -> ../../sda
    lrwxrwxrwx. 1 root root 10 Feb  9 04:15 scsi-3600605b00d87b43027b3bbb603150cc6-part1 -> ../../sda1
    lrwxrwxrwx. 1 root root 10 Feb  9 04:15 scsi-3600605b00d87b43027b3bbb603150cc6-part2 -> ../../sda2
    lrwxrwxrwx. 1 root root 10 Feb  9 04:15 scsi-3600605b00d87b43027b3bbb603150cc6-part3 -> ../../sda3
    lrwxrwxrwx. 1 root root  9 Feb  9 04:15 scsi-3600605b00d87b43027b3bbf306bc28a7 -> ../../sdb
    lrwxrwxrwx. 1 root root 10 Feb  9 04:15 scsi-3600605b00d87b43027b3bbf306bc28a7-part1 -> ../../sdb1
    lrwxrwxrwx. 1 root root  9 Feb  9 04:17 scsi-3600605b00d87b43027b3bc310a64c6c9 -> ../../sdc
    lrwxrwxrwx. 1 root root 10 Feb 11 03:14 scsi-3600605b00d87b43027b3bc310a64c6c9-part1 -> ../../sdc1
    lrwxrwxrwx. 1 root root 10 Feb 11 03:15 scsi-3600605b00d87b43027b3bc310a64c6c9-part2 -> ../../sdc2
    ```
    {: screen}

1. Repeat the previous steps for each worker node that you want to use for your ODF configuration.



## Creating an OpenShift Data Foundation configuration in the command line
{: #sat-storage-odf-local-cli}
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
    

1. List the available templates and versions and review the output. Make a note of the template and version that you want to use. Your storage template version and cluster version must match. 

    ```sh
    ibmcloud sat storage template ls
    ```
    {: pre}
    
1. Get the template parameters for your cluster version.
    ```sh
    ibmcloud sat storage template get --name odf-local --version <version>
    ```
    {: pre}
    

1. Review the [Red Hat OpenShift container storage configuration parameters](#sat-storage-odf-local-params-cli).

1. Copy the following command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `--param "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create). Be sure to include the `/dev/disk/by-id/` prefix for your `mon-device-path` and `osd-device-path` values. If you are using a {{site.data.keyword.cos_short}} backing store, be sure to specify the regional public endpoint in the following format: `https://s3.us-east.cloud-object-storage.appdomain.cloud`. Don't specify the {{site.data.keyword.cos_short}} parameters if your existing configuration doesn't use {{site.data.keyword.cos_full_notm}}.

    Note that you must specify separate disks or separate partitions for each `mon-device-path` and `osd-device-path`. You must assign one disk or one partition for each OSD or MON storage device. For disks without partitions, specify separate disks. For partitioned disks, you can specify the same disk, but you must specify separate partitions.
    {: note}
    
    Example storage config create command with `auto-discover-devices=true` on version 4.8 clusters.
    ```sh
    ibmcloud sat storage config create --name odf-local-auto --template-name odf-local --template-version 4.8 --location odf-sat-stage-location -p "ocs-cluster-name=ocscluster-auto" -p "auto-discover-devices=true" -p "iam-api-key=<api-key>"
    ```
    {: pre}
    

    Example `storage config create` command for version 4.8 clusters.
    ```sh
    ibmcloud sat storage config create --name odf-local-config --location <location> --template-name odf-local --template-version 4.8 -p "ocs-cluster-name=testocscluster" -p "osd-device-path=/dev/disk/by-id/<device-1>,/dev/disk/by-id/<device-2>,/dev/disk/by-id/<device-3>" -p "num-of-osd=1" -p "worker-nodes=<worker-node-name>,<worker-node-name>,<worker-node-name>" -p "ibm-cos-endpoint=<ibm-cos-endpoint>" -p "ibm-cos-location=<ibm-cos-location>" -p "ibm-cos-access-key=<ibm-cos-access-key>" -p "ibm-cos-secret-key=<ibm-cos-secret-key>" -p "iam-api-key=<iam-api-key>"
    ```
    {: pre}
    
    Example `storage config create` command for version 4.7 clusters.
    ```sh
    ibmcloud sat storage config create --name odf-local-config --location <location> --template-name odf-local --template-version 4.7 -p "ocs-cluster-name=<ocs-cluster-name" -p "osd-device-path=/dev/disk/by-id/<device-1>,/dev/disk/by-id/<device-2>,/dev/disk/by-id/<device-3>" -p "mon-device-path=/dev/disk/by-id/<device-1>,/dev/disk/by-id/<device-2>,/dev/disk/by-id/<device-3>" -p "num-of-osd=1" -p "worker-nodes=<worker-node-name>,<worker-node-name>,<worker-node-name>" -p "ibm-cos-endpoint=<ibm-cos-endpoint>" -p "ibm-cos-location=<ibm-cos-location>" -p "ibm-cos-access-key=<ibm-cos-access-key>" -p "ibm-cos-secret-key=<ibm-cos-secret-key>" -p "iam-api-key=<iam-api-key>"
    ```
    {: pre}

1. Verify that your configuration was created.

    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}

1. [Assign your storage configuration to clusters](#assign-storage-odf-local).

### Optional: Adding additional worker nodes to your ODF configuration
{: #add-worker-nodes-odf-local}
{: cli}

1. Add more worker nodes to your ODF configuration.
    ```sh
    ibmcloud sat storage config param set --config <config-name> -p "worker-nodes=<comma-seperated values of new worker-nodes followed by list of old worker-nodes>" --apply
    ```
    {: pre}

## Assigning your ODF storage configuration to a cluster
{: #assign-storage-odf-local}
{: cli}

After you [create a {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-odf-local), you can assign you configuration to your {{site.data.keyword.satelliteshort}} clusters.

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

1. Verify that the storage configuration resources are deployed. Note that this process might take up to 10 minutes to complete.

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

1. List the ODF storage classes.

    ```sh
    oc get sc
    ```
    {: pre}

    Example output
    ```sh
    NAME                          PROVISIONER                             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
    localblock                    kubernetes.io/no-provisioner            Delete          WaitForFirstConsumer   false                  107s
    localfile                     kubernetes.io/no-provisioner            Delete          WaitForFirstConsumer   false                  107s
    ocs-storagecluster-ceph-rbd   openshift-storage.rbd.csi.ceph.com      Delete          Immediate              true                   87s
    ocs-storagecluster-ceph-rgw   openshift-storage.ceph.rook.io/bucket   Delete          Immediate              false                  87s
    ocs-storagecluster-cephfs     openshift-storage.cephfs.csi.ceph.com   Delete          Immediate              true                   88s
    sat-ocs-cephfs-gold           openshift-storage.cephfs.csi.ceph.com   Delete          Immediate              true                   2m46s
    sat-ocs-cephrbd-gold          openshift-storage.rbd.csi.ceph.com      Delete          Immediate              true                   2m46s
    sat-ocs-cephrgw-gold          openshift-storage.ceph.rook.io/bucket   Delete          Immediate              false                  2m45s
    sat-ocs-noobaa-gold           openshift-storage.noobaa.io/obc         Delete          Immediate              false                  2m45s
    ```
    {: screen}

1. List the persistent volumes and verify that your MON and OSD volumes are created.

    ```sh
    oc get pv
    ```
    {: pre}

    Example output

    ```sh
    NAME                CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                                            STORAGECLASS   REASON   AGE
    local-pv-180cfc58   139Gi      RWO            Delete           Bound    openshift-storage/rook-ceph-mon-b                localfile               12m
    local-pv-67f21982   139Gi      RWO            Delete           Bound    openshift-storage/rook-ceph-mon-a                localfile               12m
    local-pv-80c5166    100Gi      RWO            Delete           Bound    openshift-storage/ocs-deviceset-2-data-0-5p6hd   localblock              12m
    local-pv-9b049705   139Gi      RWO            Delete           Bound    openshift-storage/rook-ceph-mon-c                localfile               12m
    local-pv-b09e0279   100Gi      RWO            Delete           Bound    openshift-storage/ocs-deviceset-1-data-0-gcq88   localblock              12m
    local-pv-f798e570   100Gi      RWO            Delete           Bound    openshift-storage/ocs-deviceset-0-data-0-6fgp6   localblock              12m
    ```
    {: screen}




## Deploying an app that uses OpenShift Data Foundation
{: #sat-storage-odf-local-deploy}
{: cli}

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



## Scaling your ODF configuration by attaching raw disks
{: #sat-storage-scale-odf-local-disk}
{: cli}

To scale your ODF configuration by adding disks to your worker nodes, increase the `num-of-osd` parameter value and specify the new worker node names with the `worker-nodes` parameter.

In the following example, 3 worker nodes are added to the configuration that was created previously in the steps above. You can scale your configuration by adding updating the command parameters as follows:
- `--name` - Create a configuration with a new name.
- `--template-name` - Use the same parameter value as in your existing configuration.
- `--template-version` - Use the same parameter value as in your existing configuration.
- `osd-device-path` - Specify all previous `osd-device-path` values from your existing configuration and the device paths from the worker nodes that you have added to your cluster. To retrieve the device ID values for your new worker nodes, see [Getting you device details](#sat-storage-odf-local-devices).
- `mon-device-path` - Specify all previous `mon-device-path` values from your existing configuration. ODF requires 3 MON devices. To retrieve the device ID values for your new worker nodes, see [Getting your device details](#sat-storage-odf-local-devices).
- `num-of-osd` - Increase the OSD number by 1 for each set of 3 disks or partitions that you add to your configuration.
- `worker-nodes` - Specify the worker nodes from your existing configuration.
- `ibm-cos-access-key` - Optional: Use the same parameter value as in your existing configuration. Do not specify this parameter if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration.
- `ibm-cos-secret-access-key` - Optional: Use the same parameter value as in your existing configuration. Do not specify this parameter if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration.
- `ibm-cos-endpoint` - Optional: Use the same parameter value as in your existing configuration. Do not specify this parameter if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration.
- `ibm-cos-location` - Optional: Use the same parameter value as in your existing configuration. Do not specify this parameter if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration.

1. Create the storage configuration and specify the updated values. In this example, the `osd-device-path` parameter is updated to include the device IDs of the disks that you want to use and the `num-of-osd` value is increased to 2. Do not specify the {{site.data.keyword.cos_short}} parameters when you create your configuration if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration. 
    ```sh
    ibmcloud sat storage config create --name ocs-config2 --template-name odf-local --template-version <template_version> -p "ocs-cluster-name=ocscluster" -p "osd-device-path=/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part2,/dev/disk/by-id/scsi-3600605b00d87b43027b3bbf306bc28a7-part2,/dev/disk/by-id/scsi-3600062b206ba6f00276eb58065b5da94-part2,/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part3,/dev/disk/by-id/scsi-3600605b00d87b43027b3bbf306bc28a7-part3,/dev/disk/by-id/scsi-3600062b206ba6f00276eb58065b5da94-part3" -p "mon-device-path=/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part1,/dev/disk/by-id/scsi-3600605b00d87b43027b3bbf306bc28a7-part1,/dev/disk/by-id/scsi-3600062b206ba6f00276eb58065b5da94-part1" -p "num-of-osd=2" -p "worker-nodes=169.48.170.83,169.48.170.88,169.48.170.90" -p "ibm-cos-endpoint=<ibm-cos-endpoint>" -p "ibm-cos-location=<ibm-cos-location>" -p "ibm-cos-access-key=<ibm-cos-access-key>" -p "ibm-cos-secret-key=<ibm-cos-secret-key>"
    ```
    {: pre}

1. Create a new assignment for this configuration :

    ```sh
    ibmcloud sat storage assignment create --name ocs-sub2 --group test-group2 --config ocs-config2
    ```
    {: pre}

### Scaling your ODF configuration with `auto-discover-devices`
{: #sat-storage-scale-odf-local-auto-discover}
{: cli}

If you set the `auto-discover-devices` parameter to `true` in your ODF configuration, you can scale your configuration by increasing the `num-of-osd` parameter value in the following command: 
    ```sh
    ibmcloud sat storage config param set --config <config-name> -p num-of-osd=2 --apply
    ```
    {: pre}


## Upgrading your ODF version
{: #odf-local-upgrade}
{: cli}

To upgrade the ODF version of your configuration, delete your existing assignment and create a new configuration with the newer version. When you create the new configuration, you can set the `odf-upgrade` parameter to `true` to upgrade the installed version of ODF when the new configuration is assigned.
{: shortdesc}

Deleting configurations and assignments might result in data loss.
{: important}

In the following example, the ODF configuration is updated to use template version 4.7,
- `--name` - Enter a name for your new configuration.
- `--template-name` - Use the same parameter value as in your existing configuration.
- `--template-version` - Enter the template version that you want to use to upgrade your configuration.
- `osd-device-path` - Use the same parameter value as in your existing configuration.
- `mon-device-path` - **Version 4.7 only**: Use the same parameter value as in your existing configuration.
- `num-of-osd` - Use the same parameter value as in your existing configuration.
- `iam-api-key` - Use the same parameter value as in your existing configuration.
- `worker-nodes` - Use the same parameter value as in your existing configuration.
- `odf-upgrade` - Enter `true` to upgrade your `ocs-cluster` to the template version that you specified.
- `ibm-cos-access-key` - Optional: Use the same parameter value as in your existing configuration. Do not specify this parameter if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration.
- `ibm-cos-secret-access-key` - Optional: Use the same parameter value as in your existing configuration. Do not specify this parameter if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration.
- `ibm-cos-endpoint` - Optional: Use the same parameter value as in your existing configuration. Do not specify this parameter if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration.
- `ibm-cos-location` - Optional: Use the same parameter value as in your existing configuration. Do not specify this parameter if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration.
- `auto-discover-devices` - **Optional: Version 4.8 only**: Set to `true` if you want to automatically discover available devices on your worker nodes.


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

1. When you upgrade your ODF version, you must enter the same configuration details as in your existing ODF configuration. In addition, you must set the `template-version` value to the version you want to upgrade to and change the `odf-upgrade` parameter to `true`. Do not specify the {{site.data.keyword.cos_short}} parameters when you create your configuration if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration. Note that Kubernetes resources can't contain capital letters or special characters.

    Example storage config create command with `auto-discover-devices=true` on version 4.8 clusters.
    ```sh
    ibmcloud sat storage config create --name odf-local-4.8 --template-name odf-local --template-version 4.8 --location odf-sat-stage-location  -p "auto-discover-devices=true" -p "iam-api-key=<api-key>"
    ```
    {: pre}
    

    Example `storage config create` command for version 4.8 clusters.
    ```sh
    ibmcloud sat storage config create --name <config_name> --location <location> --template-name odf-local --template-version <template_version> -p "osd-device-path=/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part2,/dev/disk/by-id/scsi-3600605b00d87b43027b3bbf306bc28a7-part2,/dev/disk/by-id/scsi-3600062b206ba6f00276eb58065b5da94-part2" -p "num-of-osd=1" -p "worker-nodes=<worker-node-name>,<worker-node-name>,<worker-node-name>" -p "odf-upgrade=true" -p "ibm-cos-endpoint=<ibm-cos-endpoint>" -p "ibm-cos-location=<ibm-cos-location>" -p "ibm-cos-access-key=<ibm-cos-access-key>" -p "ibm-cos-secret-key=<ibm-cos-secret-key>"
    ```
    {: pre}
    
    Example `storage config create` command for version 4.7 clusters.
    ```sh
    ibmcloud sat storage config create --name <config_name> --location <location> --template-name odf-local --template-version <template_version> -p "osd-device-path=/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part2,/dev/disk/by-id/scsi-3600605b00d87b43027b3bbf306bc28a7-part2,/dev/disk/by-id/scsi-3600062b206ba6f00276eb58065b5da94-part2" -p "mon-device-path=/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part1,/dev/disk/by-id/scsi-3600605b00d87b43027b3bbf306bc28a7-part1,/dev/disk/by-id/scsi-3600062b206ba6f00276eb58065b5da94-part1" -p "num-of-osd=1" -p "worker-nodes=<worker-node-name>,<worker-node-name>,<worker-node-name>" -p "odf-upgrade=true" -p "ibm-cos-endpoint=<ibm-cos-endpoint>" -p "ibm-cos-location=<ibm-cos-location>" -p "ibm-cos-access-key=<ibm-cos-access-key>" -p "ibm-cos-secret-key=<ibm-cos-secret-key>"
    ```
    {: pre}


1. Assign your configuration to your clusters.
    ```sh
    ibmcloud sat storage assignment create --name <name> --group <group> --config <config>
    ```
    {: pre}

1. Verify the configuration.
    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}



## Removing OpenShift Data Foundation from your apps
{: #odf-local-rm}
{: cli}

If you no longer need your OpenShift Data Foundation, you can remove your PVC, PV, and the ODF operator from your clusters.
{: shortdesc}

1. List your PVCs and note the name of the PVC and the corresponding PV that you want to remove.
    ```sh
    oc get pvc
    ```
    {: pre}

1. Remove any pods that mount the PVC.
    1. List all the pods that currently mount the PVC that you want to delete. If no pods are returned, you don't have any pods that currently use your PVC.
        ```sh
        oc get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
        ```
        {: pre}

        Example output
        ```sh
        app    sat-ocs-cephfs-gold
        ```
        {: screen}

    1. Remove the pod that uses the PVC. If the pod is part of a deployment, remove the deployment.
        ```sh
        oc delete pod <pod_name>
        ```
        {: pre}

        ```sh
        oc delete deployment <deployment_name>
        ```
        {: pre}

    1. Verify that the pod or the deployment is removed.
        ```sh
        oc get pods
        ```
        {: pre}

        ```sh
        oc get deployments
        ```
        {: pre}

1. Delete the PVC.
    ```sh
    oc delete pvc <pvc_name>
    ```
    {: pre}

1. Delete the corresponding PV.
    ```sh
    oc delete pv <pv_name>
    ```
    {: pre}


## Removing the ODF local storage configuration from your cluster
{: #odf-local-template-rm}
{: cli}

If you no longer plan to use OpenShift Data Foundation in your cluster, you can remove the assignment from your cluster from the storage configuration.
{: shortdesc}

Note that if you remove the storage configuration, the ODF operators is then uninstalled from all assigned clusters. Your PVCs, PVs, and data are not removed. However, you might not be able to access your data until you re-install the driver in your cluster again.
{: important}

1. Run the following command to delete your ODF storage assignment.
    ```sh
    oc delete ocscluster --all
    ```
    {: pre}



## OpenShift Data Foundation configuration parameter reference
{: #sat-storage-odf-local-params-cli}

### ODF version 4.9 parameters
{: #odf-params-49}

| Parameter | Required? | Description | Default value if not provided | Data type | 
| --- | --- | --- | --- | --- | 
| `--name` | Required | Enter a name for your storage configuration. Note that Kubernetes resources can't contain capital letters or special characters. Enter a name that uses only lowercase letters, numbers, `-`, or `.`. | N/A | `string` |
| `--template-name` | Required | Enter `odf-local`. | N/A | `string` |
| `--template-version` | Required | Enter `4.9`. | N/A | `string` |
| `iam-api-key` | Required | Enter your IAM API key. | N/A | `string` | 
| `osd-device-path` | Optional | If you specify `auto-discover-devices=true`, you don't need to specify the `osd-device-path`. However, if you don't specify `auto-discover-devices` or if you specify `auto-discover-devices=false`, you must specify the `osd-device-path` for each of the volumes that you want to use. Enter a comma-separated list of the `disk-by-id` paths for the devices that you want to use for the OSD pods. The devices that you specify are used as your storage devices in your ODF configuration. Your OSD devices must have at least 100GiB of space and must be unformatted and unmounted. The parameter format is `/dev/disk/by-id/<device-id>` or `/dev/disk/by-path/` for VMware and `osd-device-path` or `/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part2` for a partitioned device. If you specify more than one device path, be sure there are no spaces between each path. For example: `/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part1`,`/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part2`. | N/A | `string` |
| `ibm-cos-access-key` | Optional | Enter the {{site.data.keyword.cos_full_notm}} access key ID. Do not encode this value to base64. Your {{site.data.keyword.cos_short}} access key ID is used to create a Kubernetes secret in your cluster. | N/A | `string` |
| `ibm-cos-secret-access-key` | Optional | Enter the {{site.data.keyword.cos_full_notm}} secret access key. Do not encode this value to base64. Your {{site.data.keyword.cos_short}} secret access key is used to create a Kubernetes secret in your cluster. |N/A | `string` |
| `ibm-cos-endpoint` | Optional | Enter the {{site.data.keyword.cos_full_notm}} regional public endpoint. Be sure that you enter the regional public endpoint. Example: `https://s3.us-east.cloud-object-storage.appdomain.cloud`. | N/A | `string` |
| `ibm-cos-location` | Optional | Enter the {{site.data.keyword.cos_full_notm}} region. Example: `us-east-standard`. | N/A |
| `num-of-osd` | Optional | Enter the number of OSDs. ODF creates 3 times the value specified. | 1 | `integer` |
|`worker-nodes` | Optional | The name of the worker nodes where you want to deploy ODF. To find the worker node names, run `oc get nodes`. The minimum number of worker nodes is 3. If this value is not specified, all the worker nodes in the cluster are included in your ODF configuration. You can retrieve this parameter by running `oc get nodes`. | N/A | `csv` |
| `billing-type` | Optional | Enter the billing option that you want to use. You can enter either `essentials` or `advanced`. | `advanced` | `string` |
| `odf-upgrade` | Optional | Set to `true` if you want to upgrade the major version of ODF while creating a configuration of the newer version. | false | `boolean`|
| `auto-discover-devices` | Optional | Set to `true` if you want to automatically discover available devices on your worker nodes. You must have unformatted disks available on your worker nodes to use this feature. If you don't pass this parameter, or if you pass `auto-discover-devices=false`, you must specify the `osd-device-path` parameter. | false | boolean |
{: caption="Table 1. OpenShift Container storage parameter reference." caption-side="top"}

### ODF version 4.8 parameters
{: #odf-params-48}

| Parameter | Required? | Description | Default value if not provided | Data type | 
| --- | --- | --- | --- | --- | 
| `--name` | Required | Enter a name for your storage configuration. Note that Kubernetes resources can't contain capital letters or special characters. Enter a name that uses only lowercase letters, numbers, `-`, or `.`. | N/A | `string` |
| `--template-name` | Required | Enter `odf-local`. | N/A | `string` |
| `--template-version` | Required | Enter `4.8`. | N/A | `string` |
| `iam-api-key` | Required | Enter your IAM API key. | N/A | `string` | 
| `osd-device-path` | Optional | If you specify `auto-discover-devices=true`, you don't need to specify the `osd-device-path`. However, if you don't specify `auto-discover-devices` or if you specify `auto-discover-devices=false`, you must specify the `osd-device-path` for each of the volumes that you want to use. Enter a comma-separated list of the `disk-by-id` paths for the devices that you want to use for the OSD pods. The devices that you specify are used as your storage devices in your ODF configuration. Your OSD devices must have at least 100GiB of space and must be unformatted and unmounted. The parameter format is `/dev/disk/by-id/<device-id>` or `/dev/disk/by-path/` for VMware and `osd-device-path` or `/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part2` for a partitioned device. If you specify more than one device path, be sure there are no spaces between each path. For example: `/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part1`,`/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part2`. | N/A | `string` |
| `ibm-cos-access-key` | Optional | Enter the {{site.data.keyword.cos_full_notm}} access key ID. Do not encode this value to base64. Your {{site.data.keyword.cos_short}} access key ID is used to create a Kubernetes secret in your cluster. | N/A | `string` |
| `ibm-cos-secret-access-key` | Optional | Enter the {{site.data.keyword.cos_full_notm}} secret access key. Do not encode this value to base64. Your {{site.data.keyword.cos_short}} secret access key is used to create a Kubernetes secret in your cluster. |N/A | `string` |
| `ibm-cos-endpoint` | Optional | Enter the {{site.data.keyword.cos_full_notm}} regional public endpoint. Be sure that you enter the regional public endpoint. Example: `https://s3.us-east.cloud-object-storage.appdomain.cloud`. | N/A | `string` |
| `ibm-cos-location` | Optional | Enter the {{site.data.keyword.cos_full_notm}} region. Example: `us-east-standard`. | N/A |
| `num-of-osd` | Optional | Enter the number of OSDs. ODF creates 3 times the value specified. | 1 | `integer` |
|`worker-nodes` | Optional | The name of the worker nodes where you want to deploy ODF. To find the worker node names, run `oc get nodes`. The minimum number of worker nodes is 3. If this value is not specified, all the worker nodes in the cluster are included in your ODF configuration. You can retrieve this parameter by running `oc get nodes`. | N/A | `csv` |
| `billing-type` | Optional | Enter the billing option that you want to use. You can enter either `essentials` or `advanced`. | `advanced` | `string` |
| `odf-upgrade` | Optional | Set to `true` if you want to upgrade the major version of ODF while creating a configuration of the newer version. | false | `boolean`|
| `auto-discover-devices` | Optional | Set to `true` if you want to automatically discover available devices on your worker nodes. You must have unformatted disks available on your worker nodes to use this feature. If you don't pass this parameter, or if you pass `auto-discover-devices=false`, you must specify the `osd-device-path` parameter. | false | boolean |
{: caption="Table 2. OpenShift Container storage parameter reference." caption-side="top"}


### ODF version 4.7 parameters
{: #odf-params-47}

| Parameter | Required? | Description | Default value if not provided | Data type | 
| --- | --- | --- | --- | --- | 
| `--name` | Required | Enter a name for your storage configuration. Note that Kubernetes resources can't contain capital letters or special characters. Enter a name that uses only lowercase letters, numbers, `-`, or `.`. | N/A | `string` |
| `--template-name` | Required | Enter `odf-local`. | N/A | `string` |
| `--template-version` | Required | Note that your cluster version and template version must match. For example, Enter `4.7` for `4.7` clusters or `4.8` for 4.8 clusters. To list available template version run `ibmcloud sat storage template ls`. | N/A | `string` |
| `iam-api-key` | Required | Enter your IAM API key. | N/A | `string` | 
| `mon-device-path` | Required | Enter a comma-separated list of the `disk-by-id` paths   the storage devices that you want to use for the ODF monitoring (MON) pods. The devices that you specify must have at least 20GiB of space and must be unformatted and unmounted. The parameter format is `/dev/disk/by-id/<device-id>` or `/dev/disk/by-path/` for VMware. Example `mon-device-path` value for a partitioned device: `/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part1`. If you specify more than one device path, be sure there are no spaces between each path. For example: `/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part1`,`/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part2`. | N/A | `string` |
| `osd-device-path` | Required | Enter a comma-separated list of the `disk-by-id` paths for the devices that you want to use for the OSD pods. The devices that you specify are used as your storage devices in your ODF configuration. Your OSD devices must have at least 100GiB of space and must be unformatted and unmounted. The parameter format is `/dev/disk/by-id/<device-id>` or `/dev/disk/by-path/` for VMware and `osd-device-path` or `/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part2` for a partitioned device. If you specify more than one device path, be sure there are no spaces between each path. For example: `/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part1`,`/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part2`. | N/A | `string` |
| `ibm-cos-access-key` | Optional | Enter the {{site.data.keyword.cos_full_notm}} access key ID. Do not encode this value to base64. Your {{site.data.keyword.cos_short}} access key ID is used to create a Kubernetes secret in your cluster. | N/A | `string` |
| `ibm-cos-secret-access-key` | Optional | Enter the {{site.data.keyword.cos_full_notm}} secret access key. Do not encode this value to base64. Your {{site.data.keyword.cos_short}} secret access key is used to create a Kubernetes secret in your cluster. |N/A | `string` |
| `ibm-cos-endpoint` | Optional | Enter the {{site.data.keyword.cos_full_notm}} regional public endpoint. Be sure that you enter the regional public endpoint. Example: `https://s3.us-east.cloud-object-storage.appdomain.cloud`. | N/A | `string` |
| `ibm-cos-location` | Optional | Enter the {{site.data.keyword.cos_full_notm}} region. Example: `us-east-standard`. | N/A |
| `num-of-osd` | Optional | Enter the number of OSDs. ODF creates 3 times the value specified. | 1 | `integer` |
|`worker-nodes` | Optional | The name of the worker nodes where you want to deploy ODF. To find the worker node names, run `oc get nodes`. The minimum number of worker nodes is 3. If this value is not specified, all the worker nodes in the cluster are included in your ODF configuration. You can retrieve this parameter by running `oc get nodes`. | N/A | `csv` |
| `billing-type` | Optional | Enter the billing option that you want to use. You can enter either `essentials` or `advanced`. | `advanced` | `string` |
| `odf-upgrade` | Optional | Set to `true` if you want to upgrade the major version of ODF while creating a configuration of the newer version. | false | `boolean`|
{: caption="Table 3. OpenShift Container storage parameter reference." caption-side="top"}




## Storage class reference for ODF
{: #sat-storage-odf-local-sc-ref}

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
{: caption="Table 4. Storage class reference for OpenShift Container storage" caption-side="top"}


