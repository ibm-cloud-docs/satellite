---

copyright:
  years: 2023, 2023
lastupdated: "2023-09-15"

keywords: odf, satellite storage, satellite config, satellite configurations, container storage, local storage, OpenShift Data Foundation

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Update OpenShift Data Foundation in your Satellite cluster
{: #sat-storage-odf-update}

* [Major]{: tag-red} Applies to major updates, for example if you are updating your worker nodes to a new major version, such as from `4.11` to `4.12` as well as OpenShift Data Foundation from `4.11` to `4.12`
* [Minor]{: tag-blue} Applies to minor patch updates, for example if you are updating from `4.12.15_1542_openshift` to `4.12.16_1544_openshift` while keeping OpenShift Data Foundation at version `4.12`.

## Prerequisites
{: #sat-storage-odf-update-prereq}

* You must have additional unassinged hosts available in your location. Make sure that you [attach](/docs/satellite?topic=satellite-attach-hosts) additional hosts before starting the update process.

## Update the cluster master
{: #sat-storage-update-cluster-master}

[Major]{: tag-red}

1. Update the [management plane](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_cluster_update)

    Example command: 
    ```sh
    ibmcloud oc cluster master update --cluster CLUSTER [--version MAJOR.MINOR.PATCH] [--force-update] [-f] [-q]
    ```
    {: pre}


## Determine which ODF worker nodes you want to update
{: #determine-worker-nodes-sat}
{: step}

[Major]{: tag-red} [Minor]{: tag-blue}

1. List your worker nodes by using the `oc get nodes` command and determining which worker nodes you want to update.

    ```sh
    oc get nodes
    ```
    {: pre}
    
    Example output
        
    ```sh
    NAME           STATUS   ROLES           AGE    VERSION
    10.241.0.4     Ready    master,worker   106s   v1.21.6+4b61f94
    10.241.128.4   Ready    master,worker   22d    v1.21.6+bb8d50a
    10.241.64.4    Ready    master,worker   22d    v1.21.6+bb8d50a
    ```
    {: screen}

## Scale down OpenShift Data Foundation
{: #scale-down-odf-sat}
{: step}

[Major]{: tag-red} [Minor]{: tag-blue}

1.  For each worker node that you found in the previous step, find the `rook-ceph-mon` and `rook-ceph-osd` deployments.
    ```sh
    oc get pods -n openshift-storage -o wide | grep -i <node_name>
    ```
    {: pre}
    
1. Scale down the deployments that you found in the previous step. 
    ```sh
    oc scale deployment rook-ceph-mon-c --replicas=0 -n openshift-storage
    oc scale deployment rook-ceph-osd-2 --replicas=0 -n openshift-storage
    oc scale deployment --selector=app=rook-ceph-crashcollector,node_name=NODE-NAME --replicas=0 -n openshift-storage
    ```
    {: pre}
    
## Cordon and drain the worker node
{: #cordon-drain-worker-node-sat}
{: step}

[Major]{: tag-red} [Minor]{: tag-blue}

1.  Cordon the node. Cordoning the node prevents any pods from being scheduled on this node.
    ```sh
    oc adm cordon NODE_NAME
    ```
    {: pre}
    
    Example output
    
    ```sh
    node/10.241.0.4 cordoned
    ```
    {: screen}
    
1. Drain the node to remove all the pods. When you drain the worker node, the pods move to the other worker nodes ensuring there is no downtime. Draining also ensures that there is no disruption of the pod disruption budget. 
    ```sh
    oc adm drain NODE_NAME --force --delete-emptydir-data --ignore-daemonsets
    ```
    {: pre}
    
    Example output
    ```sh
    evicting pod "managed-storage-validation-webhooks-7fd79bc9f7-pdpv6"
    evicting pod "calico-kube-controllers-647dbbd685-fmrp9"
    evicting pod "certified-operators-2v852"
    evicting pod "csi-snapshot-controller-77fbf474df-47ddt"
    evicting pod "calico-typha-8574d89b8c-7f2cc"
    evicting pod "dns-operator-6d48cbff67-vrrsw"
    evicting pod "router-default-6fc798b98b-9m6kh"
    evicting pod "prometheus-adapter-5b77ffdd5f-hzqrp"
    evicting pod "alertmanager-main-1"
    evicting pod "prometheus-k8s-0"
    evicting pod "network-check-source-66c7fbb86-2r78z"
    ```
    {: screen}
    
1. Wait until draining finishes, then complete the following steps to replace the worker node. 


## Assign a new host as a worker node
{: #assign-worker-node-sat}
{: step}

[Major]{: tag-red} [Minor]{: tag-blue}

1. [Assign](/docs/satellite?topic=satellite-assigning-hosts#host-assign-ui) one of the unassigned hosts in your location to your cluster.

1. Wait a few minutes until the bootstrapping process is complete and your host is successfully assigned to your cluster. When a new host is assigned to a cluster as a worker node it adopts the same version as the cluster master.

    
1. Verify that the nodes are in a `Ready` state.
    ```sh
    oc get nodes
    ```
    {: pre}
    
    Example output
    
    ```sh
    NAME           STATUS   ROLES           AGE   VERSION
    10.241.0.4     Ready    master,worker   22d   v1.21.6+bb8d50a
    10.241.128.4   Ready    master,worker   22d   v1.21.6+bb8d50a
    10.241.64.4    Ready    master,worker   22d   v1.21.6+bb8d50a
    ```
    {: screen}


## Optional: Update your configuration to add the new storage nodes
{: #add-storage-node-sat}
{: step}

[Major]{: tag-red} [Minor]{: tag-blue}

1. If you are deploying ODF on a subset of worker nodes, update your satellite configuration to add the new `worker node name(s)` from the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}.

1. Wait for the OpenShift Data Foundation pods to deploy to the new worker. Verify that he OSD persistent volumes are created and that all pods are in a `Running` state.
    ```sh
    oc get pv
    oc get ocscluster
    oc get pods -n openshift-storage
    ```
    {: pre}

1. Verify that all other required OpenShift Data Foundation pods are in Running state.
    ```sh
    oc get pod -n openshift-storage | grep mon
    ```
    {: pre}
    
    Example output:
    ```sh
    rook-ceph-mon-a-cd575c89b-b6k66         2/2     Running
    0          38m
    rook-ceph-mon-b-6776bc469b-tzzt8        2/2     Running
    0          38m
    rook-ceph-mon-d-5ff5d488b5-7v8xh        2/2     Running
    0          4m8s
    ```
    {: screen}

1. Verify that new OSD pods are running on the replacement node:
    ```sh
    oc get pods -o wide -n openshift-storage| egrep -i <new_node_name> | egrep osd
    ```
    {: pre}

1. Identify the `crashcollector` pod deployment.
    ```sh
    oc get deployment --selector=app=rook-ceph-crashcollector,node_name=NODE-NAME -n openshift-storage
    ```
    {: pre}

1. If there is an existing `crashcollector` deployment, delete it.
    ```sh
    oc delete deployment --selector=app=rook-ceph-crashcollector,node_name=NODE-NAME -n openshift-storage
    ```
    {: pre}

1. Delete the ocs-osd-removal-job. 
    ```sh
    oc delete -n openshift-storage job ocs-osd-removal-job
    ```
    {: pre}

    Example output:
    ```sh
    job.batch "ocs-osd-removal-job" deleted
    ```
    {: screen}
    

## Clean up the resources from the old node
{: #cleanup-os-storage-sat}
{: step}

[Major]{: tag-red} [Minor]{: tag-blue}

1. Navigate to the `openshift-storage` project.
    ```sh
    oc project openshift-storage
    ```
    {: pre}

1. Identify the Persistent Volume (PV) associated with the Persistent Volume Claim (PVC) from the old node from your local storage.
    ```sh
    oc get pv -L kubernetes.io/hostname | grep localblock | grep Released
    ```
    {: pre}

    If there is a PV in Released state, delete it:

    ```sh
    oc delete pv <persistent_volume>
    ```
    {: pre}

1. Remove the failed OSD from the cluster. You can specify multiple failed OSDs if required.
    ```sh
    oc process -n openshift-storage ocs-osd-removal -p FAILED_OSD_IDS=<failed_osd_id> [-p FORCE_OSD_REMOVAL=true] | oc create -f OSD-ID,OSD-ID, OSD-ID
    ```
    {: pre}

    The `FORCE_OSD_REMOVAL` value must be changed to `true` in clusters that have only three OSDs, or clusters with insufficient space to restore all three replicas of the data after the OSD is removed.
    {: note}

1. Verify that the OSD was removed successfully by checking the status of the ocs-osd-removal-job pod.
    ```sh
    oc get pod -l job-name=ocs-osd-removal-job -n openshift-storage
    ```
    {: pre}

1. Verify that the OSD removal is completed.
    ```sh
    oc logs -l job-name=ocs-osd-removal-job -n openshift-storage --tail=-1 | egrep -i 'completed removal'
    ```
    {: pre}

    Example output

    ```sh
    2023-03-10 06:50:04.501511 I | cephosd: completed removal of OSD 0
    ```
    {: screen}

1. Repeat the steps above for each worker node you are updating. 


## Edit your old storage assignment
{: #sat-storage-config-edit}
{: step}

[Major]{: tag-red}

1. Update your storage configuration to set the `perform-cleanup` parameter to `false` from the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}. If this parameter is not set to `false` ODf will be removed.

1. List your storage assignments and find the one that you used for your cluster.
    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
    ```
    {: pre}

1. Remove the assignment. After the assignment is removed, the ODF driver pods and storage classes remain on your cluster.
    ```sh
    ibmcloud sat storage assignment rm --assignment <assignment_ID>
    ```
    {: pre}

## Create a new storage configuration
{: #sat-odf-new-config}
{: step}

[Major]{: tag-red}

1. Create a new storage configuration that contains all the same information as the old configuration with the `upgrade` parameter set to `true`.

## Create a new assignment
{: #sat-odf-new-assignment}
{: step}

[Major]{: tag-red}

1. Create a new storage assignment.

1. Verify your storage assignment was created.
    ```sh
    ibmcloud sat storage assignment ls (--cluster CLUSTER | --config CONFIG | --location LOCATION | --service-cluster-id CLUSTER)
    ```
    {: pre}

1. Verify that ODF has been upgraded in your cluster. 
    ```sh
    oc get storagecluster -n openshift-storage
    NAME                 AGE   PHASE   EXTERNAL   CREATED AT             VERSION
    ocs-storagecluster   43h   Ready              2023-06-21T09:22:00Z   4.11.0

    oc get csv -n openshift-storage
    NAME                              DISPLAY                       VERSION   REPLACES                          PHASE
    mcg-operator.v4.11.8              NooBaa Operator               4.11.8    mcg-operator.v4.11.7              Succeeded
    ocs-operator.v4.11.8              OpenShift Container Storage   4.11.8    ocs-operator.v4.11.7              Succeeded
    odf-csi-addons-operator.v4.11.8   CSI Addons                    4.11.8    odf-csi-addons-operator.v4.11.7   Succeeded
    odf-operator.v4.11.8              OpenShift Data Foundation     4.11.8    odf-operator.v4.11.7              Succeeded
    ```
    {: screen}




