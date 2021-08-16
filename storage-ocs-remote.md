---

copyright:
  years: 2020, 2021
lastupdated: "2021-08-13"

keywords: ocs, satellite storage, satellite config, satellite configurations, container storage, remote storage

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
{:audio: .audio}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: .ph data-hd-programlang='c#'}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: #curl .ph data-hd-programlang='curl'}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: .external target="_blank"}
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
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:middle: .ph data-hd-position='middle'}
{:navgroup: .navgroup}
{:new_window: target="_blank"}
{:node: .ph data-hd-programlang='node'}
{:note: .note}
{:objectc: .ph data-hd-programlang='Objective C'}
{:objectc: data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: .ph data-hd-programlang='PHP'}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:right: .ph data-hd-position='right'}
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
{:step: data-tutorial-type='step'} 
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:topicgroup: .topicgroup}
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


# OpenShift Data Foundation for remote devices
{: #config-storage-ocs-remote}

Set up [OpenShift Data Foundation](https://docs.openshift.com/container-platform/4.6/storage/persistent_storage/persistent-storage-ocs.html){: external} for {{site.data.keyword.satelliteshort}} clusters. You can use {{site.data.keyword.satelliteshort}} storage templates to create storage configurations. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

The OpenShift Data Foundation add-on is available as a technology preview and might change without prior notice. Do not use this add-on for production workloads.
{: preview}

## Prerequisites
{: #sat-storage-ocs-remote-prereq}

1. Before you can create a storage configuration, follow the steps to set up a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. If you do not have any clusters in your location, [create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). Review the following requirements when you create your cluster.
1. [Add your {{site.data.keyword.satelliteshort}} cluster to a cluster group](/docs/satellite?topic=satellite-setup-clusters-satconfig#setup-clusters-satconfig-groups).
1. [Set up {{site.data.keyword.satelliteshort}} Config on your clusters](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig).
1. Your cluster must have a minimum of 3 worker nodes with at least 16CPUs and 64GB RAM per worker node.
1. Your cluster must have a remote block storage provisioner available. For example, you can deploy the [AWS EBS](/docs/satellite?topic=satellite-config-storage-ebs) {{site.data.keyword.satelliteshort}} storage template to install the EBS block drivers that you can then use to provision AWS EBS volumes for ODF.
1. The OCP version must be compatible with the ODF version that you want to install. 
1. **Optional**: [Create an {{site.data.keyword.IBM_notm}} {{site.data.keyword.cos_full_notm}} service instance](#sat-storage-ocs-remote-cos).
    1. Create HMAC credentials for your {{site.data.keyword.cos_full_notm}} instance.
    1. Create a Kubernetes secret that uses your {{site.data.keyword.cos_full_notm}} HMAC credentials.

<br />

### Optional: Creating the {{site.data.keyword.cos_full_notm}} service instance
{: #sat-storage-ocs-remote-cos}

If you want to use {{site.data.keyword.cos_full_notm}} as your object service, [Create an {{site.data.keyword.cos_short}} service instance](#sat-storage-ocs-remote-cos) and HMAC credentials. The {{site.data.keyword.cos_short}} instance that you create is used as the NooBaa backing store in your ODF configuration. The backing store is the underlying storage for the data in your NooBaa buckets. If you do not specify an {{site.data.keyword.cos_full_notm}} service instance when you create your storage configuration, the default NooBaa backing store is configured. You can create additional backing stores, including {{site.data.keyword.cos_full_notm}} backing stores after your storage configuration is assigned to your clusters and ODF is installed.
{: shortdesc}

Create an instance of {{site.data.keyword.cos_full_notm}} for the backing store of your ODF remote configuration. Then, create a set of HMAC credentials and a Kubernetes secret that uses your {{site.data.keyword.cos_full_notm}} HMAC credentials.

1. Create an {{site.data.keyword.cos_full_notm}} service instance.
    ```sh
    ibmcloud resource service-instance-create noobaa-store cloud-object-storage standard global
    ```
    {: pre}

2. Create HMAC credentials. Make a note of your credentials.
    ```sh
    ibmcloud resource service-key-create cos-cred-rw Writer --instance-name noobaa-store --parameters '{"HMAC": true}'
    ```
    {: pre}



## Creating an OpenShift Data Foundation configuration in the command line
{: #sat-storage-ocs-remote-cli}

1. Log in to the {{site.data.keyword.cloud_notm}} CLI.
    ```sh
    ibmcloud login
    ```
    {: pre}

1. Before you can create a storage configuration, follow the steps to set up a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
1. If you do not have any clusters in your location, [create a {{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters) or [attach existing {{site.data.keyword.openshiftlong_notm}} clusters to your location](/docs/satellite?topic=satellite-satcon-existing).

1. List your {{site.data.keyword.satelliteshort}} locations and note the `Managed from` column.
    ```
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
    
1. Review the [Red Hat OpenShift container storage configuration parameters](#sat-storage-ocs-remote-params-cli).
1. Copy the following the command and replace the variables with the parameters for your storage configuration. You can pass additional parameters by using the `--param "key=value"` format. For more information, see the `ibmcloud sat storage config create --name` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-config-create). Do not specify the {{site.data.keyword.cos_short}} parameters when you create your configuration if you do not use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration. 
    ```sh
    ibmcloud sat storage config create --name <config_name> --location <location> --template-name odf-remote --template-version <template_version> -p "ocs-cluster-name=<ocs-cluster-name>" -p "mon-storage-class=vpc-custom-10iops-tier" -p "mon-size=<mon-size>" -p "osd-storage-class=vpc-custom-10iops-tier" -p "osd-size=<osd-size>" -p "num-of-osd=1" -p "worker-nodes=<worker-IP>,<worker-IP>,<worker-IP>" -p "ibm-cos-endpoint=<cos-endpoint>" -p "ibm-cos-location=<ibm-cos-location>" -p "ibm-cos-access-key=<ibm-cos-access-key>" -p "ibm-cos-secret-key=<ibm-cos-secret-key>"
    ```
    {: pre}

1. Verify that your storage configuration is created.
    ```sh
    ibmcloud sat storage config get --config <config>
    ```
    {: pre}

1. [Assign your storage configuration to clusters](#assign-storage-ocs-remote).

<br />

## Assigning your ODF storage configuration to a cluster
{: #assign-storage-ocs-remote}

After you [create a {{site.data.keyword.satelliteshort}} storage configuration](#config-storage-ocs-remote), you can assign your configuration to your {{site.data.keyword.satelliteshort}}.




### Assigning a storage configuration in the command line
{: #assign-storage-ocs-remote-cli}

1. List your {{site.data.keyword.satelliteshort}} storage configurations and make a note of the storage configuration that you want to assign to your clusters.
    ```sh
    ibmcloud sat storage config ls
    ```
    {: pre}

1. Get the ID of the cluster or cluster group that you want to assign storage to. To make sure that your cluster is registered with {{site.data.keyword.satelliteshort}} Config or to create groups, see [Setting up clusters to use with {{site.data.keyword.satelliteshort}} Config](/docs/satellite?topic=satellite-setup-clusters-satconfig).
    * **Group**
      ```sh
      ibmcloud sat group ls
      ```
      {: pre}

    * **Cluster**
      ```sh
      ibmcloud oc cluster ls --provider satellite
      ```
      {: pre}

    * **{{site.data.keyword.satelliteshort}}-enabled service cluster**
      ```sh
      ibmcloud sat service ls --location <location>
      ```
      {: pre}

1. Assign storage to the cluster or group that you retrieved in step 2. Replace `<group>` with the ID of your cluster group or `<cluster>` with the ID of your cluster. Replace `<config>` with the name of your storage config, and `<name>` with a name for your storage assignment. For more information, see the `ibmcloud sat storage assignment create` [command](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create).

    * **Group**
      ```sh
      ibmcloud sat storage assignment create --group <group> --config <config> --name <name>
      ```
      {: pre}

    * **Cluster**
      ```sh
      ibmcloud sat storage assignment create --cluster <cluster> --config <config> --name <name>
      ```
      {: pre}

    * **{{site.data.keyword.satelliteshort}}-enabled service cluster**
      ```sh
      ibmcloud sat storage assignment create --service-cluster-id <cluster> --config <config> --name <name>
      ```
      {: pre}

1. Verify that your assignment is created.
    ```sh
    ibmcloud sat storage assignment ls (--cluster <cluster_id> | --service-cluster-id <cluster_id>) | grep <storage-assignment-name>
    ```
    {: pre}

5. Verify that the storage configuration resources are deployed.

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



## Upgrading your ODF configuration
{: #sat-storage-ocs-remote-upgrade-config}

Do not delete your storage configurations or assignments. Deleting configurations and assignments might result in data loss.
{: important}

To upgrade the ODF version of your configuration, complete the following steps:
1. Get the details of your ODF configuration.
    ```
    ibmcloud sat storage config get <config-name>
    ```
2. Create a new configuration with the same `ocs-cluster-name` and details. Make sure to change the `template-version` to the version that you want to upgrade to and set the `ocs-upgrade` parameter to `true`.

    In the following example, the ODF configuration is updated to use template version 4.7. When you create the configuration, the following parameter changes are required.
    - `name` - Enter a name for your new configuration.
    - `template-name` - Use the same parameter value as in your existing configuration.
    - `template-version` - Enter the template version that you want to use to upgrade your configuration.
    - `ocs-cluster-name` - Use the same parameter value as in your existing configuration.
    - `mon-storage-class` - Use the same parameter value as in your existing configuration.
    - `mon-size` - Use the same parameter value as in your existing configuration.
    - `osd-storage-class` - Use the same parameter value as in your existing configuration.
    - `osd-size` - Use the same parameter value as in your existing configuration.
    - `num-of-osd` - Use the same parameter value as in your existing configuration.
    - `worker-nodes` - Use the same parameter value as in your existing configuration.
    - `ibm-cos-endpoint` - Use the same parameter value as in your existing configuration.
    - `ibm-cos-location` - Use the same parameter value as in your existing configuration.
    - `ibm-cos-access-key` - Use the same parameter value as in your existing configuration.
    - `ibm-cos-secret-key` - Use the same parameter value as in your existing configuration.
    - `ocs-upgrade` - Enter `true` to upgrade your `ocs-cluster` to the template version that you specified.

2. Get the configuration details of your `ocscluster` custom resource.
    ```sh
    kubectl get ocscluster <ocs-cluster-name>
    ```
    {: pre}

3. Save the configuration details. When you upgrade your ODF version, you must enter the same configuration details and set the `template-version` to the version you want to upgrade to and set the `ocs-upgrade` parameter to `true`. Do not specify the {{site.data.keyword.cos_short}} parameters when you create your configuration if you do not use an {{site.data.keyword.cos_full_notm}} service instance as your backing store in your existing configuration. 
    ```sh
    ibmcloud sat storage config create --name ocs-config --template-name odf-remote --template-version <template_version> -p "ocs-cluster-name=testocscluster" -p "mon-storage-class=vpc-custom-10iops-tier" -p "mon-size=50Gi" -p "osd-storage-class=vpc-custom-10iops-tier" -p "osd-size=150Gi" -p "num-of-osd=1" -p "worker-nodes=169.48.170.83,169.48.170.88,169.48.170.90" -p "ocs-upgrade=true" -p "ibm-cos-endpoint=<ibm-cos-endpoint>" -p "ibm-cos-location=<ibm-cos-location>" -p "ibm-cos-access-key=<ibm-cos-access-key>" -p "ibm-cos-secret-key=<ibm-cos-secret-key>"
    ```
    {: pre}

4. [Assign your configuration to your clusters](#assign-storage-ocs-remote-cli).



### Removing the ODF remote storage assignment from the command line
{: #ocs-remote-template-rm-cli}

Use the command line to remove a storage assignment.
{: shortdesc}

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

3. Clean up the remaining Kubernetes resources from your cluster. Save the following script in a file called `cleanup.sh` to your local machine.
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
    sleep 20
    ```
    {: pre}

4. Run the `cleanup.sh` script.
    ```sh
    sh ./cleanup.sh
    ```
    {: pre}

## OpenShift Data Foundation configuration parameter reference
{: #sat-storage-ocs-remote-params-cli}

| Parameter | Required? | Description | Default value if not provided | Datatype |
| --- | --- | --- | --- | --- |
| `ocs-cluster-name` | Required | Enter a name for your `OcsCluster` custom resource. | N/A | string |
| `mon-storage-class` | Required | Enter the storage class that you want to use for the MON pods. For multizone clusters, be sure to specify a storage class with the `waitForFirstConsumer` volume binding mode. | N/A | csv |
| `mon-size` | Required | Enter the size of the storage volumes that you want to provision for the MON pods. You must specify at least `20Gi`. |
| `osd-storage-class` | Required | Enter the storage class that you want to use for the OSD pods. For multizone clusters, be sure to specify a storage class with the `waitForFirstConsumer` volume binding mode. | N/A | csv |
| `osd-size`| Required | Enter the size of the storage disks that you want to dynamically provision. You must specify at least `100Gi`. | 
| `num-of-osd` | Optional | Enter the number of OSDs. ODF creates 3 times the value specified. | 1 | integer |
|`worker-nodes` | Optional | Enter the IP addresses of the worker nodes that you want to use in your ODF configuration. Your configuration must have at least 3 worker nodes. If this value is not specified, all of the worker nodes in the cluster are included in your ODF configuration. Example: `169.48.170.90` | N/A |csv |
| `billing-type` | Optional | Enter the billing option that you want to use. You can enter either `hourly` or `monthly`. | `hourly` | string |
| `ocs-upgrade` | Optional | Set to `true` if you want to upgrade the major version of ODF while creating a configuration of the newer version. | false | boolean |
| `ibm-cos-access-key` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter your access key ID. For more information about how to retrieve your access key, see [Using HMAC credentials](/docs/cloud-object-storage?topic=cloud-object-storage-uhc-hmac-credentials-main){: external}. |
| `ibm-cos-secret-access-key` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter your secret access key. For more information about how to retrieve your secret access key, see [Using HMAC credentials](/docs/cloud-object-storage?topic=cloud-object-storage-uhc-hmac-credentials-main){: external}. |
| `ibm-cos-endpoint` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter the {{site.data.keyword.cos_full_notm}} regional public endpoint. Example: `https://s3.us-east.cloud-object-storage.appdomain.cloud`. | N/A | 
| `ibm-cos-location` | Optional | If you want to use an {{site.data.keyword.cos_full_notm}} service instance as your object store for your ODF cluster, enter the {{site.data.keyword.cos_full_notm}} region. Example: `us-east-standard`. | N/A |
{: caption="Table 1. OpenShift Container storage parameter reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column indicates if the parameter is optional. The third column is a brief description of the parameter. The second column is the default value of the parameter."}


<br />

## Storage class reference
{: #sat-storage-ocs-remote-sc-ref}

Review the {{site.data.keyword.satelliteshort}} storage classes for OpenShift Data Foundation. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command.
{: shortdesc}

| Storage class name | Type | File system | Provisioner | Volume binding mode | Allow volume expansion | Reclaim policy |
| --- | --- | --- | --- | --- | --- | --- |
| `sat-ocs-cephrbd-gold` | Block | ext4 | `openshift-storage.rbd.csi.ceph.com` | Immediate | True | Delete |
| `sat-ocs-cephfs-gold` | File | N/A | `openshift-storage.cephfs.csi.ceph.com` | Immediate | True |Delete |
| `sat-ocs-cephrgw-gold` | Object | N/A | `openshift-storage.ceph.rook.io/bucket` | Immediate | N/A | Delete |
| `sat-ocs-noobaa-gold` | OBC | N/A | `openshift-storage.noobaa.io/obc` | Immediate | N/A | Delete |
{: caption="Table 2. Storage class reference for OpenShift Container storage" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the storage type. The third column is the file system type. The fourth column is the provisioner. The fifth column is the volume binding mode. The sixth column is volume expansion support. The seventh column is the reclaim policy."}


