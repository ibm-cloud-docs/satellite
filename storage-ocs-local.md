---

copyright:
  years: 2020, 2022
lastupdated: "2022-01-12"

keywords: ocs, satellite storage, satellite config, satellite configurations, container storage, local storage

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# OpenShift Data Foundation using local disks
{: #config-storage-odf-local}

Set up OpenShift Data Foundation for {{site.data.keyword.satelliteshort}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

OpenShift Data Foundation is available in only internal mode, which means that your apps run in the same cluster as your storage. External mode, or storage heavy configurations, where your storage is located in a separate cluster from your apps is not supported.
{: note}


## Prerequisites
{: #sat-storage-odf-local-prereq}

To use the ODF storage with the local storage operator and local storage devices, complete the following tasks:

- [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
- [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/satellite?topic=openshift-satellite-clusters).
    - Your cluster must have a minimum of 3 worker nodes with at least 16CPUs and 64GB RAM per worker node.
    - Your hosts must meet the [{{site.data.keyword.satelliteshort}} host requirements](/docs/satellite?topic=satellite-host-reqs) in addition to having one of the following local storage configurations.
        * Two extra raw devices per worker node in addition to the minimum host requirements. These disks must not be partitioned or formatted file systems. If your devices are not partitioned, each node must have 2 free disks. One disk for the OSD and one disk for the MON.
        * Two extra raw partitions per worker node in addition to the minimum host requirements. These disks must not be  formatted file systems. If your raw devices are partitioned, they must have at least 2 partitions per disk, per worker node.
- [Set up {{site.data.keyword.satelliteshort}} Config on your clusters](/docs/satellite?topic=satellite-setup-clusters-satconfig#setup-clusters-satconfig-access ).
- **Optional**: If you want to use {{site.data.keyword.cos_full_notm}} as your object service, [Create an {{site.data.keyword.cos_short}} service instance](#sat-storage-odf-local-cos) and HMAC credentials. The {{site.data.keyword.cos_short}} instance that you create is used as the NooBaa backing store in your ODF configuration. The backing store is the underlying storage for the data in your NooBaa buckets. If you don't specify an {{site.data.keyword.cos_full_notm}} service instance when you create your storage configuration, the default NooBaa backing store is configured. You can create additional backing stores, including {{site.data.keyword.cos_full_notm}} backing stores after your storage configuration is assigned to your clusters and ODF is installed.
- [Get the details of the raw, unformatted devices that you want to use for your configuration](#sat-storage-odf-local-devices). The device IDs of your storage disks are used to create your {{site.data.keyword.satelliteshort}} storage configuration.


## Optional: Setting up an {{site.data.keyword.cos_full_notm}} backing store
{: #sat-storage-odf-local-cos}

If you want to use {{site.data.keyword.cos_full_notm}} as your object service, create an {{site.data.keyword.cos_short}} service instance and HMAC credentials. The {{site.data.keyword.cos_short}} instance that you create is the NooBaa backing store in your ODF configuration. The backing store is the underlying storage for the data in your NooBaa buckets. If you don't specify an {{site.data.keyword.cos_full_notm}} service instance when you create your storage configuration, the default NooBaa backing store is configured. You can create more backing stores, including {{site.data.keyword.cos_full_notm}} backing stores after assigning the configuration to to your clusters and installing ODF.
{: shortdesc}

1. Create an {{site.data.keyword.cos_full_notm}} service instance.

    ```sh
    ibmcloud resource service-instance-create noobaa-store cloud-object-storage standard global
    ```
    {: pre}

2. Create HMAC credentials and note the service access key and access key ID of your HMAC credentials.

    ```sh
    ibmcloud resource service-key-create cos-cred-rw Writer --instance-name noobaa-store --parameters '{"HMAC": true}'
    ```
    {: pre}


## Getting the device details for your ODF configuration
{: #sat-storage-odf-local-devices}

When you create your ODF configuration, you must specify the device paths of the disks that you want to use in your storage cluster. The storage cluster is comprised of the object storage daemon (OSD) pods and the monitoring (MON) pods. The devices that you specify as OSD devices are your storage devices where your app data is stored and the devices that you specify as MON devices are managed by the MON pod and used to store and maintain the storage cluster mapping and monitor storage events. For more information about the OSD and MON, see the [Ceph documentation](https://docs.ceph.com/en/latest/start/intro/){: external}.

The following steps show how you can manually retrieve the local device information from each worker node in your clusters. You can also use the [ODF disk gatherer](https://access.redhat.com/solutions/4928841) to retrieve a list of all the local devices in your cluster.
{: tip}

1. Log in to your cluster and get a list of available worker nodes. Make a note of the worker nodes that you want to use in your ODF configuration.

    ```sh
    oc get nodes
    ```
    {: pre}

2. Log in to each worker node that you want to use for your ODF configuration.

    ```sh
    oc debug node/<node-name>
    ```
    {: pre}

3. When the debug pod is deployed on the worker node, run the following commands to list the available disks on the worker node.

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

4. Review the command output for available disks. Disks that can be used for your ODF configuration must be unmounted. In the following example output from the `lsblk` command, the `sdc` disk has two available, unformatted partitions that you can use for the OSD and MON device paths for this worker node. If your worker node has raw disks without partitions, you need one disk for the OSD and one disk for the MON. As a best practice, and to maximize storage capacity on this disk, you can specify the smaller partition or disk for the MON, and the larger partition or disk for the OSD. Note that the initial storage capacity of your ODF configuration is equal to the size of the disk that you specify as the `osd-device-path` when you create your configuration.


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

5. Find the `by-id` for each disk that you want to use in your configuration. In this case, the `sdc1` and `sdc2` partitions are unformatted and unmounted. The `by-id` for each disk is specified as a command parameter when you create your configuration.

    ```sh
    ls -l /dev/disk/by-id/
    ```
    {: pre}
    
    If you have VMware hosts, run the following command.
    
    ```sh
    ls -l /dev/disk/by-path/
    ```
    {: pre}
    
    

6. Review the command output and make a note of the `by-id` values for the disks that you want to use in your configuration. In the following example output, the disk ids for the `sdc1` and `sdc2` partitions are: `scsi-3600605b00d87b43027b3bc310a64c6c9-part1` and `scsi-3600605b00d87b43027b3bc310a64c6c9-part2`.
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

7. Repeat the previous steps for each worker node that you want to use for your ODF configuration.   









## Creating an OpenShift Data Foundation configuration in the command line
{: #sat-storage-odf-local-cli}

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
    
1. Review the [Red Hat OpenShift container storage configuration parameters](#sat-storage-odf-local-params-cli).
1. Copy the following command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `--param "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create). Be sure to include the `/dev/disk/by-id/` prefix for your `mon-device-path` and `osd-device-path` values. If you are using a {{site.data.keyword.cos_short}} backing store, be sure to specify the regional public endpoint in the following format: `https://s3.us-east.cloud-object-storage.appdomain.cloud`. Don't specify the {{site.data.keyword.cos_short}} parameters if your existing configuration doesn't use {{site.data.keyword.cos_full_notm}}.

    Note that you must specify separate disks or separate partitions for each `mon-device-path` and `osd-device-path`. You must assign one disk or one partition for each OSD or MON storage device. For disks without partitions, specify separate disks. For partitioned disks, you can specify the same disk, but you must specify separate partitions.
    {: note}

    ```sh
    ibmcloud sat storage config create --name <config_name> --location <location> --template-name odf-local --template-version <template_version> -p "ocs-cluster-name=<ocs-cluster-name" -p "osd-device-path=/dev/disk/by-id/<device-1>,/dev/disk/by-id/<device-2>,/dev/disk/by-id/<device-3>" -p "mon-device-path=/dev/disk/by-id/<device-1>,/dev/disk/by-id/<device-2>,/dev/disk/by-id/<device-3>" -p "num-of-osd=1" -p "worker-nodes=<worker-node-name>,<worker-node-name>,<worker-node-name>" -p "ibm-cos-endpoint=<ibm-cos-endpoint>" -p "ibm-cos-location=<ibm-cos-location>" -p "ibm-cos-access-key=<ibm-cos-access-key>" -p "ibm-cos-secret-key=<ibm-cos-secret-key>" -p "iam-api-key=<iam-api-key>"
    ```
    {: pre}

1. Verify that your configuration was created.

    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}

1. [Assign your storage configuration to clusters](#assign-storage-odf-local).


## Assigning your ODF storage configuration to a cluster
{: #assign-storage-odf-local}

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

    - {{site.data.keyword.satelliteshort}}-enabled service cluster
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

    - {{site.data.keyword.satelliteshort}}-enabled service cluster
      ```sh
      ibmcloud sat storage assignment create --service-cluster-id <cluster> --config <config> --name <name>
      ```
      {: pre}

1. Verify that your assignment is created.
    ```sh
    ibmcloud sat storage assignment ls (--cluster <cluster_id> | --service-cluster-id <cluster_id>) | grep <storage-assignment-name>
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

    2. Get a list of pods in the `openshift-storage` namespace and verify that the status is `Running`.
    
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

2. List the ODF storage classes.

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

3. List the persistent volumes and verify that your MON and OSD volumes are created.

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

2. Create the PVC in your cluster.

    ```sh
    oc apply -f pvc.yaml
    ```
    {: pre}

3. Create a YAML configuration file for a pod that mounts the PVC that you created. The following example creates an `nginx` pod that writes the current date and time to a `test.txt` file.

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

4. Create the pod in your cluster.

    ```sh
    oc apply -f pod.yaml
    ```
    {: pre}

5. Verify that the pod is deployed. Note that it might take a few minutes for your app to get into a `Running` state.

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

6. Verify that the app can write data.

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





## Upgrading your ODF version
{: #odf-local-upgrade}

To upgrade the ODF version of your configuration, get the details of your configuration and create a new configuration with the same `ocs-cluster-name` and details, but with the `template-version` set to the target version that you want to upgrade to and the `ocs-upgrade` parameter to `true`.
{: shortdesc}

Deleting configurations and assignments might result in data loss.
{: important}

In the following example, the ODF configuration is updated to use template version 4.7,
- `--name` - Enter a name for your new configuration.
- `--template-name` - Use the same parameter value as in your existing configuration.
- `--template-version` - Enter the template version that you want to use to upgrade your configuration.
- `ocs-cluster-name` - Use the same parameter value as in your existing configuration.
- `osd-device-path` - Use the same parameter value as in your existing configuration.
- `mon-device-path` - Use the same parameter value as in your existing configuration.
- `num-of-osd` - Use the same parameter value as in your existing configuration.
- `iam-api-key` - Use the same parameter value as in your existing configuration.
- `worker-nodes` - Use the same parameter value as in your existing configuration.
- `ocs-upgrade` - Enter `true` to upgrade your `ocs-cluster` to the template version that you specified.
- `ibm-cos-access-key` - Optional: Use the same parameter value as in your existing configuration. Do not specify this parameter if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration.
- `ibm-cos-secret-access-key` - Optional: Use the same parameter value as in your existing configuration. Do not specify this parameter if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration.
- `ibm-cos-endpoint` - Optional: Use the same parameter value as in your existing configuration. Do not specify this parameter if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration.
- `ibm-cos-location` - Optional: Use the same parameter value as in your existing configuration. Do not specify this parameter if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration.


1. Get the details of your ODF configuration.
    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}

2. Get the configuration details of your `ocscluster` custom resource.

    ```sh
    oc get ocscluster <ocs-cluster-name>
    ```
    {: pre}

3. Save the configuration details. When you upgrade your ODF version, you must enter the same configuration details as in your existing ODF configuration. In addition, you must set the `template-version` to the version you want to upgrade to and change the `ocs-upgrade` parameter to `true`. Do not specify the {{site.data.keyword.cos_short}} parameters when you create your configuration if you don't use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration. Note that Kubernetes resources can't contain capital letters or special characters. Enter an `ocs-cluster-name` that uses only lowercase letters, numbers, `-`, or `.`.
    ```sh
    ibmcloud sat storage config create --name <config_name> --location <location> --template-name odf-local --template-version <template_version> -p "ocs-cluster-name=testocscluster" -p "osd-device-path=/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part2,/dev/disk/by-id/scsi-3600605b00d87b43027b3bbf306bc28a7-part2,/dev/disk/by-id/scsi-3600062b206ba6f00276eb58065b5da94-part2" -p "mon-device-path=/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part1,/dev/disk/by-id/scsi-3600605b00d87b43027b3bbf306bc28a7-part1,/dev/disk/by-id/scsi-3600062b206ba6f00276eb58065b5da94-part1" -p "num-of-osd=1" -p "worker-nodes=<worker-node-name>,<worker-node-name>,<worker-node-name>" -p "ocs-upgrade=true" -p "ibm-cos-endpoint=<ibm-cos-endpoint>" -p "ibm-cos-location=<ibm-cos-location>" -p "ibm-cos-access-key=<ibm-cos-access-key>" -p "ibm-cos-secret-key=<ibm-cos-secret-key>"
    ```
    {: pre}

4. Assign your configuration to your clusters.
    ```sh
    ibmcloud sat storage assignment create --name <name> --group <group> --config <config>
    ```
    {: pre}

5. Verify that your configuration is updated
    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}



## Removing OpenShift Data Foundation from your apps
{: #odf-local-rm}

If you no longer need your OpenShift Data Foundation, you can remove your PVC, PV, and the ODF operator from your clusters.
{: shortdesc}

1. List your PVCs and note the name of the PVC and the corresponding PV that you want to remove.
    ```sh
    oc get pvc
    ```
    {: pre}

2. Remove any pods that mount the PVC.
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

3. Delete the PVC.
    ```sh
    oc delete pvc <pvc_name>
    ```
    {: pre}

4. Delete the corresponding PV.
    ```sh
    oc delete pv <pv_name>
    ```
    {: pre}


## Removing the ODF local storage configuration from your cluster
{: #odf-local-template-rm}

If you no longer plan to use OpenShift Data Foundation in your cluster, you can remove the assignment from your cluster from the storage configuration.
{: shortdesc}

Removing the storage configuration uninstalls the ODF operators from all assigned clusters. Your PVCs, PVs, and data are not removed. However, you might not be able to access your data until you re-install the driver in your cluster again.
{: important}


1. List your storage assignments and find the one that you used for your cluster.
    ```sh
    ibmcloud sat storage assignment ls (--cluster <cluster_id> | --service-cluster-id <cluster_id>)
    ```
    {: pre}

2. Remove the assignment. After the assignment is removed, the ODF driver pods and storage classes are removed from all clusters that were part of the storage assignment.
    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment_ID>
    ```
    {: pre}

3. List your storage assignments and find the one that you used for your cluster.
    ```sh
    ibmcloud sat storage assignment ls (--cluster <cluster_id> | --service-cluster-id <cluster_id>)
    ```
    {: pre}

4. Remove the assignment. After the assignment is removed, the local file driver pods and storage classes are removed from all clusters that were part of the storage assignment.
    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment_ID>
    ```
    {: pre}

5. Remove your storage assignment.
    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment>
    ```
    {: pre}

6. Clean up the remaining Kubernetes resources from your cluster. Save the following script in a file called `cleanup.sh` to your local machine.
    ```sh
    #!/bin/bash
    ocscluster_name=`oc get ocscluster | awk 'NR==2 {print $1}'`
    oc delete ocscluster --all --wait=false
    kubectl patch ocscluster/$ocscluster_name -p '{"metadata":{"finalizers":[]}}' --type=merge
    oc delete ns openshift-storage --wait=false
    sleep 20
    kubectl -n openshift-storage patch persistentvolumeclaim/db-noobaa-db-0 -p '{"metadata":{"finalizers":[]}}' --type=merge
    kubectl -n openshift-storage patch cephblockpool.ceph.rook.io/ocs-storagecluster-cephblockpool -p '{"metadata":{"finalizers":[]}}' --type=merge
    kubectl -n openshift-storage patch cephcluster.ceph.rook.io/ocs-storagecluster-cephcluster -p '{"metadata":{"finalizers":[]}}' --type=merge
    kubectl -n openshift-storage patch cephfilesystem.ceph.rook.io/ocs-storagecluster-cephfilesystem -p '{"metadata":{"finalizers":[]}}' --type=merge
    kubectl -n openshift-storage patch cephobjectstore.ceph.rook.io/ocs-storagecluster-cephobjectstore -p '{"metadata":{"finalizers":[]}}' --type=merge
    kubectl -n openshift-storage patch cephobjectstoreuser.ceph.rook.io/noobaa-ceph-objectstore-user -p '{"metadata":{"finalizers":[]}}' --type=merge
    kubectl -n openshift-storage patch cephobjectstoreuser.ceph.rook.io/ocs-storagecluster-cephobjectstoreuser -p '{"metadata":{"finalizers":[]}}' --type=merge
    kubectl -n openshift-storage patch NooBaa/noobaa -p '{"metadata":{"finalizers":[]}}' --type=merge
    kubectl -n openshift-storage patch backingstores.noobaa.io/noobaa-default-backing-store -p '{"metadata":{"finalizers":[]}}' --type=merge
    kubectl -n openshift-storage patch bucketclasses.noobaa.io/noobaa-default-bucket-class -p '{"metadata":{"finalizers":[]}}' --type=merge
    kubectl -n openshift-storage patch storagecluster.ocs.openshift.io/ocs-storagecluster -p '{"metadata":{"finalizers":[]}}' --type=merge
    sleep 20
    oc delete pods -n openshift-storage --all --force --grace-period=0
    oc delete ns local-storage --wait=false
    sleep 20
    kubectl -n local-storage patch localvolume.local.storage.openshift.io/local-block -p '{"metadata":{"finalizers":[]}}' --type=merge
    kubectl -n local-storage patch localvolume.local.storage.openshift.io/local-file -p '{"metadata":{"finalizers":[]}}' --type=merge
    sleep 20
    oc delete pods -n local-storage --all --force --grace-period=0
    ```
    {: codeblock}

7. Run the `cleanup.sh` script.

    ```sh
    sh ./cleanup.sh
    ```
    {: pre}

8. After you run the cleanup script, log in to each worker node and run the following commands.

    1. Deploy a debug pod and run `chroot /host`.
    
        ```bash
        oc debug node/<node_name> -- chroot /host
        ```
        {: pre}

    2. Run the following command to remove any files or directories on the specified paths. Repeat this step for each worker node that you used in your ODF configuration.
        ```bash
        rm -rvf /var/lib/rook /mnt/local-storage
        ```
        {: codeblock}

        Example output
        
        ```sh
        removed '/var/lib/rook/openshift-storage/log/ocs-deviceset-0-data-0-6fgp6/ceph-volume.log'
        removed directory: '/var/lib/rook/openshift-storage/log/ocs-deviceset-0-data-0-6fgp6'
        removed directory: '/var/lib/rook/openshift-storage/log'
        removed directory: '/var/lib/rook/openshift-storage/crash/posted'
        removed directory: '/var/lib/rook/openshift-storage/crash'
        removed '/var/lib/rook/openshift-storage/client.admin.keyring'
        removed '/var/lib/rook/openshift-storage/openshift-storage.config'
        removed directory: '/var/lib/rook/openshift-storage'
        removed directory: '/var/lib/rook'
        removed '/mnt/local-storage/localblock/nvme3n1'
        removed directory: '/mnt/local-storage/localblock'
        removed '/mnt/local-storage/localfile/nvme2n1'
        removed directory: '/mnt/local-storage/localfile'
        removed directory: '/mnt/local-storage'
        ```
        {: screen}


9. **Optional**: If you no longer want to use the local volumes that you used in your ODF configuration, you can delete them from the cluster. List the local PVs.

    ```sh
    oc get pv
    ```
    {: pre}

    Example output

    ```sh
    local-pv-180cfc58   139Gi      RWO            Delete           Available           localfile               11m
    local-pv-67f21982   139Gi      RWO            Delete           Available           localfile               12m
    local-pv-80c5166    100Gi      RWO            Delete           Available           localblock              12m
    local-pv-9b049705   139Gi      RWO            Delete           Available           localfile               12m
    local-pv-b09e0279   100Gi      RWO            Delete           Available           localblock              12m
    local-pv-f798e570   100Gi      RWO            Delete           Available           localblock              12m
    ```
    {: screen}

10. Delete the local PVs.

    ```sh
    oc delete pv <pv_name> <pv_name> <pv_name>
    ```
    {: pre}

11. List the ODF and local storage classes.

    ```sh
    oc get sc
    ```
    {: pre}

    Example output

    ```sh
    localblock                    kubernetes.io/no-provisioner            Delete          WaitForFirstConsumer   false                  42m
    localfile                     kubernetes.io/no-provisioner            Delete          WaitForFirstConsumer   false                  42m
    ocs-storagecluster-ceph-rbd   openshift-storage.rbd.csi.ceph.com      Delete          Immediate              true                   41m
    ocs-storagecluster-ceph-rgw   openshift-storage.ceph.rook.io/bucket   Delete          Immediate              false                  41m
    ocs-storagecluster-cephfs
    ```
    {: screen}

12. Delete the storage classes.

    ```sh
    oc delete sc localblock localfile ocs-storagecluster-ceph-rbd ocs-storagecluster-ceph-rgw ocs-storagecluster-cephfs
    ```
    {: pre}

    Example output

    ```sh
    storageclass.storage.k8s.io "localblock" deleted
    storageclass.storage.k8s.io "localfile" deleted
    storageclass.storage.k8s.io "ocs-storagecluster-ceph-rgw" deleted
    storageclass.storage.k8s.io "ocs-storagecluster-cephfs" deleted
    storageclass.storage.k8s.io "ocs-storagecluster-cephrbd" deleted
    ```
    {: screen}

## OpenShift Data Foundation configuration parameter reference
{: #sat-storage-odf-local-params-cli}

| Parameter | Required? | Description | Default value if not provided | Data type | 
| --- | --- | --- | --- | --- | 
| `--name` | Required | Enter a name for your storage configuration. Note that Kubernetes resources can't contain capital letters or special characters. Enter a name that uses only lowercase letters, numbers, `-`, or `.`. | N/A | `string` |
| `--template-name` | Required | Enter `odf-local`. | N/A | `string` |
| `--template-version` | Required | Enter `4.7`. | N/A | `string` |
| `iam-api-key` | Required | Enter your IAM API key. | N/A | `string` | 
| `ocs-cluster-name` | Required | Enter a name for your `OcsCluster` custom resource. Note that Kubernetes resources can't contain capital letters or special characters. Enter a name that uses only lowercase letters, numbers, `-`, or `.`. | N/A | `string` |
| `mon-device-path` | Required | Enter a comma-separated list of the `disk-by-id` paths   the storage devices that you want to use for the ODF monitoring (MON) pods. The devices that you specify must have at least 20GiB of space and must be unformatted and unmounted. The parameter format is `/dev/disk/by-id/<device-id>` or `/dev/disk/by-path/` for VMware. Example `mon-device-path` value for a partitioned device: `/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part1`. If you specify more than one device path, be sure there are no spaces between each path. For example: `/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part1`,`/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part2`. | N/A | `string` |
| `osd-device-path` | Required | Enter a comma-separated list of the `disk-by-id` paths for the devices that you want to use for the OSD pods. The devices that you specify are used as your storage devices in your ODF configuration. Your OSD devices must have at least 100GiB of space and must be unformatted and unmounted. The parameter format is `/dev/disk/by-id/<device-id>` or `/dev/disk/by-path/` for VMware and `osd-device-path` or `/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part2` for a partitioned device. If you specify more than one device path, be sure there are no spaces between each path. For example: `/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part1`,`/dev/disk/by-id/scsi-3600605b00d87b43027b3bc310a64c6c9-part2`. | N/A | `string` |
| `ibm-cos-access-key` | Optional | Enter the {{site.data.keyword.cos_full_notm}} access key ID. Do not encode this value to base64. Your {{site.data.keyword.cos_short}} access key ID is used to create a Kubernetes secret in your cluster. | N/A | `string` |
| `ibm-cos-secret-access-key` | Optional | Enter the {{site.data.keyword.cos_full_notm}} secret access key. Do not encode this value to base64. Your {{site.data.keyword.cos_short}} secret access key is used to create a Kubernetes secret in your cluster. |N/A | `string` |
| `ibm-cos-endpoint` | Optional | Enter the {{site.data.keyword.cos_full_notm}} regional public endpoint. Be sure that you enter the regional public endpoint. Example: `https://s3.us-east.cloud-object-storage.appdomain.cloud`. | N/A | `string` |
| `ibm-cos-location` | Optional | Enter the {{site.data.keyword.cos_full_notm}} region. Example: `us-east-standard`. | N/A |
| `num-of-osd` | Optional | Enter the number of OSDs. ODF creates 3 times the value specified. | 1 | `integer` |
|`worker-nodes` | Optional | The name of the worker nodes where you want to deploy ODF. To find the worker node names, run `oc get nodes`. The minimum number of worker nodes is 3. If this value is not specified, all of the worker nodes in the cluster are included in your ODF configuration. You can retrieve this parameter by running `oc get nodes`. | N/A | `csv` |
| `billing-type` | Optional | Enter the billing option that you want to use. You can enter either `hourly` or `monthly`. | `hourly` | `string` |
| `ocs-upgrade` | Optional | Set to `true` if you want to upgrade the major version of ODF while creating a configuration of the newer version. | false | `boolean`|
{: caption="Table 1. OpenShift Container storage parameter reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name.  The second column indicates if the parameter is optional. The third column is a brief description of the parameter. The fourth column is the default value of the parameter."}




## Storage class reference
{: #sat-storage-odf-local-sc-ref}

{[storage-class-odf-local.md]}
{: caption="Table 2. Storage class reference for OpenShift Container storage" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system type. The fourth column is the provisioner. The fifth column is the volume binding mode. The sixth column is volume expansion support. The seventh column is the reclaim policy."}


