---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-19"

keywords: ocs, satellite storage, satellite config, satellite configurations, container storage, remote devices, odf, openshift data foundation

subcollection: satellite

---


{{site.data.keyword.attribute-definition-list}}


# OpenShift Data Foundation for remote devices
{: #config-storage-odf-remote}

Set up OpenShift Data Foundation for {{site.data.keyword.satellitelong}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

OpenShift Data Foundation is available in only internal mode, which means that your apps run in the same cluster as your storage. External mode, or storage heavy configurations, where your storage is located in a separate cluster from your apps is not supported.
{: note}

Before you can deploy storage templates to clusters in your location, make sure you set up {{site.data.keyword.satelliteshort}} Config.
{: important}

## Prerequisites for using ODF for remote devices
{: #sat-storage-odf-remote-prereq}

1. Before you can create a storage configuration, follow the steps to set up a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. If you don't have any clusters in your location, [create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). Review the following requirements when you create your cluster.
1. [Set up {{site.data.keyword.satelliteshort}} Config on your clusters](/docs/satellite?topic=satellite-satcon-create).
1. Your cluster must have a minimum of 3 worker nodes with at least 16CPUs and 64GB RAM per worker node.
1. Your cluster must have a remote block storage provisioner available. For example, you can deploy the [AWS EBS](/docs/satellite?topic=satellite-config-storage-ebs) {{site.data.keyword.satelliteshort}} storage template to install the EBS block drivers that you can then use to provision AWS EBS volumes for ODF.
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


## Creating an OpenShift Data Foundation configuration in the command line
{: #sat-storage-odf-remote-cli}
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
    
    
1. Review the [Red Hat OpenShift container storage configuration parameters](#sat-storage-odf-remote-params-cli).

1. Copy the following command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `--param "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create). Note that Kubernetes resources can't contain capital letters or special characters. Enter a name for your config that uses only lowercase letters, numbers, hyphens or periods. Don't specify the {{site.data.keyword.cos_short}} parameters if your existing configuration doesn't use {{site.data.keyword.cos_full_notm}}.

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

1. Verify that your storage configuration is created.
    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}

1. [Assign your storage configuration to clusters](#assign-storage-odf-remote).

### Optional: Adding additional worker nodes to your ODF configuration
{: #add-worker-nodes-odf-remote}
{: cli}

1. Add more worker nodes to your ODF configuration.
    ```sh
    ibmcloud sat storage config param set --config <config-name> -p "worker-nodes=<comma-separated values of new worker-nodes followed by list of old worker-nodes>" --apply
    ```
    {: pre}


## Assigning your ODF storage configuration to a cluster
{: #assign-storage-odf-remote}

After you [create a {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-odf-remote), you can assign your configuration to your {{site.data.keyword.satelliteshort}}.


### Assigning a remote device storage configuration in the command line
{: #assign-storage-odf-remote-cli}
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

## Scaling up your ODF configuration
{: #sat-storage-odf-remote-scale-config}
{: cli}

To scale your ODF configuration by adding disks to your worker nodes, increase the `num-of-osd` parameter value. 
    ```sh
   ibmcloud sat storage config param set --config <config-name> -p num-of-osd=2 --apply
    ```

## Upgrading your ODF configuration
{: #sat-storage-odf-remote-upgrade-config}
{: cli}

Don't delete your storage configurations or assignments. Deleting configurations and assignments might result in data loss.
{: important}

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

1. [Assign your configuration to your clusters](#assign-storage-odf-remote-cli).


### Removing the ODF remote storage assignment from the command line
{: #odf-remote-template-rm-cli}
{: cli}

Use the command line to remove a storage assignment.
{: shortdesc}

1. Run the following command to delete your ODF storage assignment.
    ```sh
    oc delete ocscluster --all
    ```
    {: pre}



## OpenShift Data Foundation configuration parameter reference
{: #sat-storage-odf-remote-params-cli}

### OpenShift Data Foundation version 4.9 parameters
{: #odf-remote-49-params}

| Parameter | Required? | Description | Default value if not provided | Data type |
| --- | --- | --- | --- | --- |
| `--name` | Required | Enter a name for your storage configuration. Note that Kubernetes resources can't contain capital letters or special characters. Enter a name that uses only lowercase letters, numbers, `-`, or `.`. | N/A | `string` |
| `--template-name` | Required | Enter `odf-local`. | N/A | `string` |
| `--template-version` | Required | Note that your cluster version and template version must match. For example, Enter `4.7` for `4.7` clusters, `4.8` for `4.8`, or `4.9` for `4.9` clusters. To list available template version run `ibmcloud sat storage template ls`. | N/A | `string` |
| `iam-api-key` | Required | Enter your IAM API key. | N/A | `string` |
| `osd-storage-class` | Required | Enter the storage class that you want to use for the OSD pods. For multizone clusters, be sure to specify a storage class with the `waitForFirstConsumer` volume binding mode. | N/A | `csv` |
| `osd-size`| Required | Enter the size of the storage disks that you want to dynamically provision. You must specify at least `100Gi`. | 
| `num-of-osd` | Optional | Enter the number of OSDs. ODF creates 3 times the value specified. | 1 | `integer` |
|`worker-nodes` | Optional | The name of the worker nodes where you want to deploy ODF. To find the worker node names, run `oc get nodes`. The minimum number of worker nodes is 3. If this value is not specified, all the worker nodes in the cluster are included in your ODF configuration. You can retrieve this parameter by running `oc get nodes`. | N/A | `csv` |
| `billing-type` | Optional | Enter the billing option that you want to use. You can enter either `hourly` or `monthly`. | `hourly` | `string` |
| `cluster-encryption` | Optional | Set to `true` if you want to enable cluster-wide encryption. | false | `boolean` | 
| `odf-upgrade` | Optional | Set to `true` if you want to upgrade the major version of ODF while creating a configuration of the newer version. | false | `boolean` |
| `ibm-cos-access-key` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter your access key ID. For more information about how to retrieve your access key, see [Using HMAC credentials](/docs/cloud-object-storage?topic=cloud-object-storage-uhc-hmac-credentials-main){: external}. | N/A | `string` |
| `ibm-cos-secret-access-key` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter your secret access key. For more information about how to retrieve your secret access key, see [Using HMAC credentials](/docs/cloud-object-storage?topic=cloud-object-storage-uhc-hmac-credentials-main){: external}. | N/A | `string` |
| `ibm-cos-endpoint` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter the {{site.data.keyword.cos_full_notm}} regional public endpoint. Example: `https://s3.us-east.cloud-object-storage.appdomain.cloud`. | N/A | `string` |
| `ibm-cos-location` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter the {{site.data.keyword.cos_full_notm}} region. Example: `us-east-standard`. | N/A | `string` |
{: caption="Table 1. OpenShift Container storage parameter reference." caption-side="top"}

### OpenShift Data Foundation version 4.8 parameters
{: #odf-remote-48-params}

| Parameter | Required? | Description | Default value if not provided | Data type |
| --- | --- | --- | --- | --- |
| `--name` | Required | Enter a name for your storage configuration. Note that Kubernetes resources can't contain capital letters or special characters. Enter a name that uses only lowercase letters, numbers, `-`, or `.`. | N/A | `string` |
| `--template-name` | Required | Enter `odf-local`. | N/A | `string` |
| `--template-version` | Required | Note that your cluster version and template version must match. For example, Enter `4.7` for `4.7` clusters or `4.8` for 4.8 clusters. To list available template version run `ibmcloud sat storage template ls`. | N/A | `string` |
| `iam-api-key` | Required | Enter your IAM API key. | N/A | `string` |
| `osd-storage-class` | Required | Enter the storage class that you want to use for the OSD pods. For multizone clusters, be sure to specify a storage class with the `waitForFirstConsumer` volume binding mode. | N/A | `csv` |
| `osd-size`| Required | Enter the size of the storage disks that you want to dynamically provision. You must specify at least `100Gi`. | 
| `num-of-osd` | Optional | Enter the number of OSDs. ODF creates 3 times the value specified. | 1 | `integer` |
|`worker-nodes` | Optional | The name of the worker nodes where you want to deploy ODF. To find the worker node names, run `oc get nodes`. The minimum number of worker nodes is 3. If this value is not specified, all the worker nodes in the cluster are included in your ODF configuration. You can retrieve this parameter by running `oc get nodes`. | N/A | `csv` |
| `billing-type` | Optional | Enter the billing option that you want to use. You can enter either `hourly` or `monthly`. | `hourly` | `string` |
| `cluster-encryption` | Optional | Set to `true` if you want to enable cluster-wide encryption. | false | `boolean` | 
| `odf-upgrade` | Optional | Set to `true` if you want to upgrade the major version of ODF while creating a configuration of the newer version. | false | `boolean` |
| `ibm-cos-access-key` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter your access key ID. For more information about how to retrieve your access key, see [Using HMAC credentials](/docs/cloud-object-storage?topic=cloud-object-storage-uhc-hmac-credentials-main){: external}. | N/A | `string` |
| `ibm-cos-secret-access-key` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter your secret access key. For more information about how to retrieve your secret access key, see [Using HMAC credentials](/docs/cloud-object-storage?topic=cloud-object-storage-uhc-hmac-credentials-main){: external}. | N/A | `string` |
| `ibm-cos-endpoint` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter the {{site.data.keyword.cos_full_notm}} regional public endpoint. Example: `https://s3.us-east.cloud-object-storage.appdomain.cloud`. | N/A | `string` |
| `ibm-cos-location` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter the {{site.data.keyword.cos_full_notm}} region. Example: `us-east-standard`. | N/A | `string` |
{: caption="Table 2. OpenShift Container storage parameter reference." caption-side="top"}

### OpenShift Data Foundation version 4.7 parameters
{: #odf-remote-47-params}

| Parameter | Required? | Description | Default value if not provided | Data type |
| --- | --- | --- | --- | --- |
| `--name` | Required | Enter a name for your storage configuration. Note that Kubernetes resources can't contain capital letters or special characters. Enter a name that uses only lowercase letters, numbers, `-`, or `.`. | N/A | `string` |
| `--template-name` | Required | Enter `odf-local`. | N/A | `string` |
| `--template-version` | Required | Note that your cluster version and template version must match. For example, Enter `4.7` for `4.7` clusters or `4.8` for 4.8 clusters. To list available template version run `ibmcloud sat storage template ls`. | N/A | `string` |
| `iam-api-key` | Required | Enter your IAM API key. | N/A | `string` | 
| `mon-storage-class` | Required | Enter the storage class that you want to use for the MON pods. For multizone clusters, be sure to specify a storage class with the `waitForFirstConsumer` volume binding mode. | N/A | `csv` |
| `mon-size` | Required | Enter the size of the storage volumes that you want to provision for the MON pods. You must specify at least `20Gi`. |
| `osd-storage-class` | Required | Enter the storage class that you want to use for the OSD pods. For multizone clusters, be sure to specify a storage class with the `waitForFirstConsumer` volume binding mode. | N/A | `csv` |
| `osd-size`| Required | Enter the size of the storage disks that you want to dynamically provision. You must specify at least `100Gi`. | 
| `num-of-osd` | Optional | Enter the number of OSDs. ODF creates 3 times the value specified. | 1 | `integer` |
|`worker-nodes` | Optional | The name of the worker nodes where you want to deploy ODF. To find the worker node names, run `oc get nodes`. The minimum number of worker nodes is 3. If this value is not specified, all the worker nodes in the cluster are included in your ODF configuration. You can retrieve this parameter by running `oc get nodes`. | N/A | `csv` |
| `billing-type` | Optional | Enter the billing option that you want to use. You can enter either `hourly` or `monthly`. | `hourly` | `string` |
| `cluster-encryption` | Optional | Set to `true` if you want to enable cluster-wide encryption. | false | `boolean` | 
| `odf-upgrade` | Optional | Set to `true` if you want to upgrade the major version of ODF while creating a configuration of the newer version. | false | `boolean` |
| `ibm-cos-access-key` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter your access key ID. For more information about how to retrieve your access key, see [Using HMAC credentials](/docs/cloud-object-storage?topic=cloud-object-storage-uhc-hmac-credentials-main){: external}. | N/A | `string` |
| `ibm-cos-secret-access-key` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter your secret access key. For more information about how to retrieve your secret access key, see [Using HMAC credentials](/docs/cloud-object-storage?topic=cloud-object-storage-uhc-hmac-credentials-main){: external}. | N/A | `string` |
| `ibm-cos-endpoint` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter the {{site.data.keyword.cos_full_notm}} regional public endpoint. Example: `https://s3.us-east.cloud-object-storage.appdomain.cloud`. | N/A | `string` |
| `ibm-cos-location` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter the {{site.data.keyword.cos_full_notm}} region. Example: `us-east-standard`. | N/A | `string` |
{: caption="Table 3. OpenShift Container storage parameter reference." caption-side="top"}


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
{: caption="Table 4. Storage class reference for OpenShift Container storage" caption-side="top"}
