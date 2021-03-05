---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-05"

keywords: ocs, satellite storage, satellite config, satellite configurations, container storage, local storage

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# OpenShift Container Storage using local disks
{: #config-storage-ocs-local}

Set up [OpenShift Container Storage](https://docs.openshift.com/container-platform/4.6/storage/persistent_storage/persistent-storage-ocs.html){: external} for {{site.data.keyword.satelliteshort}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. After you create a storage configuration, you can assign it to your clusters. When you assign a storage configuration to your clusters, the storage drivers for the provider that you used to create your configuration are installed on your cluster.
{: shortdesc}

The {{site.data.keyword.satelliteshort}} storage templates are currently available in beta and should not be used for production workloads.
{: beta}

<br />

## Creating an OCS local storage configuration
{: #sat-storage-ocs-local}

When you create an OCS local storage configuration, you must provide your local storage device details. When you assign your configuration to your clusters, the storage drivers then install the object storage daemon (OSD) pods and MON pods on the devices that you specify. The OSD pods replicate your local storage data across your worker nodes. The MON pods monitor your devices.

<br />

### Prerequisites
{: #sat-storage-ocs-local-prereq}

1. Before you can create a storage configuration, follow the steps to set up a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. If you do not have any clusters in your location, [create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) that meets the minimum requirements for {{site.data.keyword.satelliteshort}} and OCS. Review the following requirements when you create your cluster.
  1. Your cluster must have a minimum of 3 worker nodes with at least 16CPUs and 64GB RAM per worker node.
  1. Your hosts must meet the [{{site.data.keyword.satelliteshort}} host requirements](/docs/satellite?topic=satellite-host-reqs) in addition to having one of the following local storage configurations.
    * Raw devices that have no partitions or formatted file systems. If your devices have no partitions, each node must have 2 free disks. One disk for the OSD and one disk for the MON.
    * Raw partitions that have no formatted file system. If your raw devices are partitioned, they must have at least 2 partitions per disk, per worker node.
1. The OCP version should be compatible with the OCS version that you want to install.
1. Your cluster must have an `openshift-storage` [namespace](#sat-storage-ocs-local-ns).
1. [Create an IBM COS service instance](#sat-storage-ocs-local-cos).
  1. Create HMAC credentials for your COS instance.
  1. Create a Kubernetes secret that uses your COS HMAC credentials.
1. [Get the details of the raw, unformatted devices that you want to use for your configuration](#sat-storage-ocs-local-devices).


<br />

### Creating the `openshift-storage` namespace
{: #sat-storage-ocs-local-ns}

Create an `openshift-storage` namespace in your cluster. The OCS driver pods are deployed to this namespace.

1. Copy the following YAML and save it as `os-namespace.yaml` on your local machine.
    ```yaml
    apiVersion: v1
    kind: Namespace
    metadata:
      labels:
        openshift.io/cluster-monitoring: "true"
      name: openshift-storage
    ```
    {: codeblock}

2. Create the `openshift-storage` namespace by using the YAML file that you saved.
    ```sh
    oc create -f os-namespace.yaml
    ```
    {: pre}

3. Verify that the namespace is created.
    ```sh
    oc get namespaces | grep storage
    ```
    {: pre}

<br />
### Creating the IBM COS service instance
{: #sat-storage-ocs-local-cos}

Create an instance of IBM COS for the backing storage of your OCS local configuration. Then, create a set of HMAC credentials and a Kubernetes secret that uses your COS HMAC credentials.

1. Create an IBM COS Service Instance.
    ```sh
    ibmcloud resource service-instance-create noobaa-store cloud-object-storage standard global
    ```
    {: pre}

2. Create HMAC credentials. Make a note of your credentials.
    ```sh
    ibmcloud resource service-key-create cos-cred-rw Writer --instance-name noobaa-store --parameters '{"HMAC": true}'
    ```
    {: pre}

3. Encode your access key ID to base64.
  ```sh
  echo -n "<access-key-id>" | base64
  ```
  {: pre}

4. Encode you secret access key to base64.
  ```sh
  echo -n "<secret-access-key>" | base64
  ```
  {: pre}

5. Create the Kubernetes secret named `ibm-cloud-cos-creds` in the `openshift-storage` namespace that uses your COS HMAC credentials. Your secret must be named `ibm-cloud-cos-creds`.
  ```yaml
  apiVersion: v1
  kind: Secret
  metadata:
    name: ibm-cloud-cos-creds # Important: Your secret must be named ibm-cloud-cos-creds
    namespace: openshift-storage
  type: Opaque
  stringData:
    IBM_COS_Endpoint: <cos-endpoint> # The public regional endpoint. Example: https://s3.us-east.cloud-object-storage.appdomain.cloud
    IBM_COS_Location: <cos-location> # The COS region. Example: us-east-standard
  data:
    IBM_COS_ACCESS_KEY_ID: <base-64-encoded-access-key-ID>  
    IBM_COS_SECRET_ACCESS_KEY: <base-64-encoded-secret-access-key>
  ```
  {: pre}

6. Create the secret in your cluster.
  ```sh
  oc create -f secret.yaml
  ```
  {: pre}

7. Verify that your secret is created.
  ```sh
  oc get secrets -n openshift-storage | grep ibm-cloud-cos-creds
  ```
  {: pre}

<br />

### Getting the device details for your OCS configuration
{: #sat-storage-ocs-local-devices}

When you create your OCS configuration, you must specify which devices that you want to use for the object storage daemons (OSDs) and the MON. The OSD deploys a series of pods, in multiples of 3, that replicate your local storage across the worker nodes and disks in your OCS deployment. The device paths that you retrieve in the following steps are specified as parameters when you create your OCS configuration.

The following steps show how you can manually retrieve the local device information from each worker node in your clusters. You can also use the [OCS disk gatherer](https://access.redhat.com/solutions/4928841) to retrieve a list of all the local devices in your cluster.
{: tip}

1. Log in to your cluster and get a list of available worker nodes. Make a note of the worker nodes that you want to use in your OCS configuration.
    ```sh
    oc get nodes
    ```
    {: pre}

2. Log in to each worker node that you want to use for your OCS configuration.
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

  1. Get the details of your devices. Verify that the devices that you want to use are unmounted and unformatted.
    ```sh
    fdisk -l
    ```
    {: pre}

4. Review the command output for available disks. Disks that can be used for your OCS configuration must be unmounted. In the following example output from the `lsblk` command, the `sdc` disk has two available, unformatted partitions that you can use for the OSD and MON device paths for this worker node. If your worker node has raw disks with no partitions you need one disk for the OSD and one disk for the MON. As a best practice, and to maximize storage capacity on this disk, you can specify the smaller partition or disk for the MON, and the larger partition or disk for the OSD. Note that the initial storage capacity of your OCS configuration is equal to the size of the disk that you specify as the `osd-device-path` when you create your configuration.
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

5. Find the `by-id` for each disk that you want to use in your configuration. In this case, the `sdc1` and `sdc2` partitions are unformatted and unmounted. Find `by-id` for each disk is specified as a command parameter when you create your configuration.

    ```sh
    ls -l /dev/disk/by-id/
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

7. Repeat the previous steps for each worker node that you want to use for your OCS configuration.   


<br />




<br />

## Creating an OpenShift Container Storage configuration in the command line
{: #sat-storage-ocs-local-cli}

1. Before you can create a storage configuration, [review the prerequisites](#sat-storage-ocs-local-prereq).
1. Review the [Red Hat OpenShift container storage configuration parameters](#sat-storage-ocs-local-params-cli).
1. Copy the following the command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `--param "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create).
  ```sh
  ibmcloud sat storage config create --name <name> --template-name ocs-local --template-version 4.6 -p "ocs-cluster-name=<ocs-cluster-name" -p "osd-device-path=<by-id1>,<by-id2>,<by-id3>" -p "mon-device-path=<by-id1>,<by-id2>,<by-id3>" -p "num-of-osd=1" -p "worker-nodes=<worker-node-IP>,<worker-node-IP>,<worker-node-IP>"
  ```
  {: pre}
1. Verify that your storage configuration is created.
  ```sh
  ibmcloud sat storage config get --config <config>
  ```
  {: pre}
1. [Assign your storage configuration to clusters](#assign-storage-ocs-local).

<br />
### OpenShift Container Storage configuration parameter reference
{: #sat-storage-ocs-local-params-cli}

| Parameter | Required? | Description | Default value if not provided |
| --- | --- | --- | --- |
| `--name` | Required | Enter a name for your storage configuration. | N/A |
| `--template-name` | Required | Enter `ocs-local`. | N/A |
| `--template-version` | Required | Enter `4.6`. | N/A |
| `ocs-cluster-name` | Required | Enter a name for your `OcsCluster` custom resource. | N/A |
| `mon-device-path` | Required | Enter a comma separated list of the `disk-by-id` paths for the devices that you want to use for the MON pods. Example partition `by-id`: `/dev/scsi-3600605b00d87b43027b3bc310a64c6c9-part1`. | N/A |
| `osd-device-path` | Required | Enter a comma separated list of the `disk-by-id` paths for the devices that you want to use for the OSD pods. Example partition `by-id`: `/dev/scsi-3600605b00d87b43027b3bc310a64c6c9-part2`. | N/A |
| `num-of-osd` | Optional | Enter the number of OSDs. OCS creates 3 times the value specified. | 1 |
|`worker-nodes` | Optional | Enter the IP addresses of the worker nodes that you want to use in your OCS configuration. Your configuration must have at least 3 worker nodes. If this value is not specified, all of the worker nodes in the cluster are included in your OCS configuration. Example: `169.48.170.90` | N/A |
| `billing-type` | Optional | Enter the billing option that you want to use. You can enter either `hourly` or `monthly`. | `hourly` |
| `ocs-upgrade` | Optional | Set to `true` if you want to upgrade the major version of OCS while creating a configuration of the newer version. | false |
{: caption="Table 1. OpenShift Container storage parameter reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column is a brief description of the parameter. The third column is the default value of the parameter."}


<br />
## Assigning your OCS storage configuration to a cluster
{: #assign-storage-ocs-local}

After you [create a {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-ocs-local), you can assign you configuration to your {{site.data.keyword.satelliteshort}} clusters.

<br />



### Assigning a storage configuraton in the command line
{: #assign-storage-ocs-local-cli}

1. List your {{site.data.keyword.satelliteshort}} storage configurations and make a note of the storage configuration that you want to assign to your clusters.
  ```sh
  ibmcloud sat storage config ls
  ```
  {: pre}

2. List your {{site.data.keyword.satelliteshort}} cluster groups and make a note of the group that you want to assign storage.
  ```sh
  ibmcloud sat group ls
  ```
  {: pre}

3. Assign storage to the clusters that you retrieved in step 2. Replace `<group>` with the name of your cluster group, `<config>` with the name of your storage config, and `<name>` with a name for your storage assignment. For more information, see the `ibmcloud sat storage assignment create` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create).
  ```sh
  ibmcloud sat storage assignment create --group <group> --config <config> --name <name>
  ```
  {: pre}

4. Verify that your assignment is created.
  ```sh
  ibmcloud sat storage assignment ls | grep <storage-assignment-name>
  ```
  {: pre}
5. Verify that the storage configuration resources are deployed. For more information about the resources that are deployed with the configuration that you assigned to your clusters. Note that this process might take up to 10 minutes to complete.

  1. Get the `storagecluster` that you deployed and verify that the phase is `Ready`.
    ```sh
    oc get storagecluster -n openshift-storage
    ```
    {: pre}

    Example output:
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

    Example output:
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


<br />



## Storage class reference
{: #sat-storage-ocs-local-sc-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for OpenShift Container Storage. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command. You can also view the YAML spec for the OCS storage classes in [GitHub](https://github.com/IBM/ibm-satellite-storage/blob/master/config-templates/redhat/ocs-local/4.6/storage-class.yaml).
{: shortdesc}

| Storage class name | Type | File system | Provisioner | Volume binding mode | Allow volume expansion | Reclaim policy |
| --- | --- | --- | --- | --- | --- | --- |
| `sat-ocs-cephrbd-gold` | Block | ext4 | `openshift-storage.rbd.csi.ceph.com` | Immediate | True | Delete |
| `sat-ocs-cephfs-gold` | File | N/A | `openshift-storage.cephfs.csi.ceph.com` | Immediate | True |Delete |
| `sat-ocs-cephrgw-gold` | Object | N/A | `openshift-storage.ceph.rook.io/bucket` | Immediate | N/A | Delete |
| `sat-ocs-noobaa-gold` | OBC | N/A | `openshift-storage.noobaa.io/obc` | Immediate | N/A | Delete |
{: caption="Table 2. Storage class reference for OpenShift Container storage" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system type. The fourth column is the provisioner. The fifth column is the volume binding mode. The sixth column is volume expansion support. The seventh column is the reclaim policy."}
