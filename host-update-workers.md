---


copyright:
  years: 2020, 2024
lastupdated: "2024-01-03"

keywords: satellite, hybrid, multicloud, os upgrade, operating system, security patch, host, update, host update

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Updating hosts that are assigned as worker nodes 
{: #host-update-workers}

{{site.data.keyword.IBM_notm}} provides version updates for your hosts that are assigned as worker nodes to {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services such as clusters. The version updates include OpenShift Container Platform, the operating system, and security patches. You choose when to apply the host version updates. Note that service clusters, which are the underlying platform for all {{site.data.keyword.cloud_notm}} services are created by services such as {{site.data.keyword.codeengineshort}} or {{site.data.keyword.cos_full_notm}} and are maintained by {{site.data.keyword.IBM_notm}}.
{: shortdesc}

What happens to my apps during an update?
:    If you run apps as part of a deployment on worker nodes that you update, the apps are rescheduled onto other worker nodes in the cluster. These worker nodes might be in a different worker pool, or if you have stand-alone worker nodes, apps might be scheduled onto stand-alone worker nodes. To avoid downtime for your app, you must ensure that you have enough capacity in the cluster to carry the workload.

How can I control how many worker nodes go down at a time during an update or reload?
:    If you need all your worker nodes to be up and running, consider [attaching](/docs/satellite?topic=satellite-attach-hosts) and [assigning](/docs/satellite?topic=satellite-assigning-hosts) additional hosts
to your service. You can add additional hosts to your location temporarily and then remove them when the update is complete.
:    In addition, you can create a Kubernetes ConfigMap that specifies the maximum number of worker nodes that can be unavailable at a time, such as during an update. Worker nodes are identified by the worker node labels. You can use IBM-provided labels or custom labels that you added to the worker node.

## Checking if a version update is available for worker node hosts
{: #host-update-workers-check}

You can check if a version update is available for a host that is assigned as a worker node to a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services) by using the {{site.data.keyword.cloud_notm}} CLI or the {{site.data.keyword.cloud_notm}} console.
{: shortdesc}

To review the changes that are included in each version update, see the [Version change log for {{site.data.keyword.openshiftlong_notm}}](/docs/openshift?topic=openshift-openshift_versions).

### Checking if a version update is available with the {{site.data.keyword.cloud_notm}} CLI
{: #host-update-workers-check-cli}

1. Log in to {{site.data.keyword.cloud_notm}}. Include the `--sso` option if you have a federated account.
    ```sh
    ibmcloud login [--sso]
    ```
    {: pre}

2. List the {{site.data.keyword.satelliteshort}} clusters in your account. 
    ```sh
    ibmcloud ks cluster ls --provider satellite
    ```
    {: pre}

3. List the worker nodes in the cluster that you want to update the version for. In the output, check for an asterisk `*` with a message that indicates a version update is available.
    ```sh
    ibmcloud ks worker ls -c <cluster_name_or_ID>
    ```
    {: pre}

    Example output
    ```sh
    ID                Primary IP       Flavor   State    Status   Zone     Version   
    sat-worker-<ID>   <IP_address>     upi      normal   Ready    zone-1   4.5.35_1534_openshift*   

    * To update to 4.5.37_1537_openshift version, run 'ibmcloud ks worker replace'. Review and make any required version changes before you update: 'https://ibm.biz/upworker'
    ```
    {: screen}

### Checking if a version update is available from the {{site.data.keyword.cloud_notm}} console
{: #host-update-workers-check-console}

1. Log in to the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}.
2. Click the location with the hosts that you want to update.
3. Click the **Hosts** tab.
4. From the host list, click the link to the **Cluster** for the host that you want to update. A new tab opens for the {{site.data.keyword.openshiftlong_notm}} cluster details.
5. Click the **Worker nodes** tab.
6. In the **Version** column, check for an information icon that says `Update available` when you click on the icon. If no update is available, no icon is present.
7. [Determine if the version update is a major, minor, or patch update](#host-update-workers-type). 

## Identifying worker node hosts
{: #host-identify}

Determine whether your hosts are part of the control plane, assigned to a managed service, or attached to the location.
{: shortdesc}

1. List your location hosts and make note of their IDs. The worker node hosts do not have `infrastructure` listed in the `Cluster` column of the output.

   ```sh
   ibmcloud sat host ls --location <location>
   ```
   {: pre}
   
   Review the example output.

   ```sh
   Name           ID                     State      Status   Zone     Cluster          Worker ID                                              Worker IP   

   satdemo-cp1    0bc3b92f55968a230985   assigned   Ready    zone-1   infrastructure   sat-satdemocp1-2bda578e901b4047c6e48d766cd99bc11a45fddd   169.62.42.178   
   satdemo-cp2    999cd38c39ddffe4b672   assigned   Ready    zone-2   infrastructure   sat-satdemocp2-940134e69c2609c5421b2426a7640fa80569668d   169.62.42.183   
   satdemo-cp5    6ca4fd8fcad1fa622aa4   assigned   Ready    zone-3   infrastructure   sat-satdemocp5-d46581b509357ea4b429fddc38a18b155463bf1c   169.62.42.181   
   satdemo-cp4    1ac2b92f55968a333335   assigned   Ready    zone-1   satdemo-cluster  sat-satdemocp4-2bda578e901b4047c6e48d766cd99bc11a45fddd   169.62.42.180   
   satdemo-cp6    234cd56c78ddffe4b672   assigned   Ready    zone-2   satdemo-cluster  sat-satdemocp6-940134e69c2609c5421b2426a7640fa80569668d   169.62.42.179   
   satdemo-cp3    8fg4ff8faaa1fa622bb5   assigned   Ready    zone-3   satdemo-cluster  sat-satdemocp3-d46581b509357ea4b429fddc38a18b155463bf1c   169.62.42.182  
   ```
   {: screen}

2. List your current hosts that are assigned as worker nodes to your {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service and make note of their IDs.

    ```sh
    ibmcloud ks worker ls -c <cluster_name_or_ID>
    ```
    {: pre}

    Review the example output.

    ```sh
    ID                                                        Primary IP     Flavor   State    Status   Zone        Version   
    sat-satellitei-5bd83963cdfedc7668c9040879c88113a134c4e7   10.241.0.4     upi      normal   Ready    us-east-2   4.7.55_1575_openshift   
    sat-satellitei-854beae4556401b5761e34ed849ba64c4b0a674c   10.241.128.4   upi      normal   Ready    us-east-1   4.7.55_1575_openshift   
    sat-satellitei-bf1fc9b135011d5c0d9d500855db6e489d15610b   10.241.64.4    upi      normal   Ready    us-east-3   4.7.55_1575_openshift    
    ```
    {: screen}

## Apply version updates to worker node hosts without detaching them
{: #host-update-workers-inplace}

You can update your worker node hosts without detaching them from the location. You can also perform a rolling update of your worker node hosts with a ConfigMap.
{: shortdesc}

Before you begin

- Verify that all your worker nodes are in a healthy state.
- If you are using persistent block storage volumes, you must detach these volumes from the node before you start your updates. Move the persistent volumes to a different worker node that does not require updates. Then, cordon and drain the workload from the worker node to update with the **`kubectl drain NODENAME`** command. If you cannot move the block storage volumes, use the [Applying version updates to worker nodes by replacing hosts](#host-update-workers-minor).

Applying updates to worker nodes can cause downtime for your apps and services. Do not perform any actions on the host while the update process is running. A maximum of 20% of all your worker nodes can be unavailable during the update process.
{: important}

### Apply version updates to worker node hosts one at a time
{: #host-update-workers-individually}

1. Optional: [Attach](/docs/satellite?topic=satellite-attach-hosts) and [assign](/docs/satellite?topic=satellite-assigning-hosts) extra hosts to the service cluster to handle the compute capacity while your existing hosts are updating.
2. [Identify your worker node hosts](#host-identify). The worker node hosts do not have `infrastructure` listed in the `Cluster` column of the output, but instead have the name of the cluster.
3. Update your worker nodes individually by running the **`ibmcloud ks worker update`** command.

    ```sh
    ibmcloud ks worker update -c CLUSTER_NAME_OR_ID --worker WORKER_ID
    ```
    {: pre}
    
4. Confirm that the update is complete by reviewing the Kubernetes version of your worker nodes.  
    ```sh
    kubectl get nodes
    ```
    {: pre}
    
    If the update failed, you must [apply version updates by replacing hosts](#host-update-workers-minor).

### Apply version updates to your worker node hosts with a ConfigMap
{: #host-update-workers-rolling}

You can roll out updates to all your worker node hosts with a ConfigMap. Specify which nodes to update by using labels. You can also specify 

1. Optional: [Attach](/docs/satellite?topic=satellite-attach-hosts) and [assign](/docs/satellite?topic=satellite-assigning-hosts) extra hosts to the service cluster to handle the compute capacity while your existing hosts are updating.
2. [Identify your worker node hosts](#host-identify). Your worker node hosts are not listed as `Infrastructure`.
3. View the labels of a worker node. You can find the worker node labels in the **Labels** section of your CLI output. Every label consists of a `NodeSelectorKey` and a `NodeSelectorValue`. You can use the labels to specify which worker nodes to update.
    ```sh
    kubectl get nodes -o yaml
    ```
    {: pre}

    Example output

    ```sh
    labels:
      arch: amd64
      beta.kubernetes.io/arch: amd64
      beta.kubernetes.io/instance-type: upi
      beta.kubernetes.io/os: linux
      failure-domain.beta.kubernetes.io/region: us-east
      failure-domain.beta.kubernetes.io/zone: us-east-2
      ibm-cloud.kubernetes.io/iaas-provider: upi
      ibm-cloud.kubernetes.io/internal-ip: 10.241.0.4
      ibm-cloud.kubernetes.io/machine-type: upi
      ibm-cloud.kubernetes.io/os: REDHAT_7_64
      ibm-cloud.kubernetes.io/region: us-east
      ibm-cloud.kubernetes.io/worker-id: sat-satellitei-5bd83963cdfedc7668c9040879c88113a134c4e7
      ibm-cloud.kubernetes.io/worker-pool-id: cbtljodw089nltg8k210-9a3f763
      ibm-cloud.kubernetes.io/worker-pool-name: default
      ibm-cloud.kubernetes.io/worker-version: 4.7.59_1583_openshift
      ibm-cloud.kubernetes.io/zone: us-east-2
      kubernetes.io/arch: amd64
      kubernetes.io/hostname: satellite-ibm-host-3
      kubernetes.io/os: linux
      node-role.kubernetes.io/master: ""
      node-role.kubernetes.io/worker: ""
      node.kubernetes.io/instance-type: upi
      node.openshift.io/os_id: rhel
      privateVLAN: "1"
      topology.kubernetes.io/region: us-east
      topology.kubernetes.io/zone: us-east-2
    ```
    {: screen}

4. Create a ConfigMap and define the unavailability rules for your worker nodes. The following example shows two checks, the `defaultcheck.json` and a check template. You can use this example check to define rules for all worker nodes that don't match any of the checks that you defined in the ConfigMap (`defaultcheck.json`). Use the check template to create your own check. For every check, to identify a worker node, you must choose one of the worker node labels that you retrieved in the previous step.  

    For every check, you can set only one value for `NodeSelectorKey` and `NodeSelectorValue`. Define up to 10 checks in a ConfigMap. If you add more checks, they are ignored.
    {: note}

    Example
    ```yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: ibm-cluster-update-configuration
      namespace: kube-system
    data:
      drain_timeout_seconds: "120"
      defaultcheck.json: |
        {
          "MaxUnavailablePercentage": 20
        }
      <check_name>: |
        {
          "MaxUnavailablePercentage": <value_in_percentage>,
          "NodeSelectorKey": "<node_selector_key>",
          "NodeSelectorValue": "<node_selector_value>"
        }
    ```
    {: codeblock}

    `drain_timeout_seconds`
    :    Optional: The timeout in seconds to wait for the [drain](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/){: external} to complete. Draining a worker node safely removes all existing pods from the worker node and reschedules the pods onto other worker nodes in the cluster. Accepted values are integers in the range 1 - 180. The default value is 30.

    `defaultcheck.json`
    :   When you update worker nodes in Satellite hosts, only 20% of the worker nodes in the cluster can be unavailable at a time.

    `MaxUnavailablePercentage`
    :   The maximum number of nodes that are allowed to be unavailable for a specified label key and value, which is specified as a percentage. A worker node is unavailable during the deploying, reloading, or provisioning process. The queued worker nodes are blocked from updating if it exceeds any defined maximum unavailable percentages. 

    `NodeSelectorKey`
    :   The label key of the worker node for which you want to set a rule. You can set rules for the default labels that are provided by IBM, as well as on worker node labels that you created.

    `NodeSelectorValue`
    :   The label value that the worker node must have to be considered for the rule that you define.
        
5. Create the ConfigMap in your cluster.
    ```sh
    kubectl apply -f <filepath/configmap.yaml>
    ```
    {: pre}

6. Verify that the ConfigMap is created.
    ```sh
    kubectl get configmap --namespace kube-system
    ```
    {: pre}

7. Update the worker nodes by listing them by ID.

    ```sh
    ibmcloud ks worker update --cluster <cluster_name_or_ID> --worker <worker_node1_ID> --worker <worker_node2_ID>
    ```
    {: pre}

8. Optional: Verify the events that are triggered by the ConfigMap and any validation errors that occur. The events can be reviewed in the  **Events** section of your CLI output.
    ```sh
    kubectl describe -n kube-system cm ibm-cluster-update-configuration
    ```
    {: pre}

9. Confirm that the update is complete by reviewing the Kubernetes version of your worker nodes.  
    ```sh
    kubectl get nodes
    ```
    {: pre}
    
    If the update failed, you must [apply version updates by replacing hosts](#host-update-workers-minor).

10. Verify that you don't have duplicate worker nodes. Sometimes, older clusters might list duplicate worker nodes with a **`NotReady`** status after an update. To remove duplicates, see [troubleshooting](/docs/containers?topic=containers-cs_duplicate_nodes).

## Applying version updates to worker nodes by replacing hosts
{: #host-update-workers-minor}

Hosts that are attached to a location do not update automatically. To apply a version update, you can first attach and assign new hosts to your [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services) and then remove the old hosts. You can also apply [minor and patch version updates in place](#host-update-workers-inplace).
{: shortdesc}


1. List your current hosts and make note of their IDs. These are the hosts to remove after you attach updated hosts.

    ```sh
    ibmcloud ks worker ls -c <cluster_name_or_ID>
    ```
    {: pre}

    Review the example output.

    ```sh
    ID                                                        Primary IP      Flavor   State    Status   Zone     Version   
    sat-satliberty-5b4c7f3a7bfc14cf58cbb14ad5c08429475274fe   208.43.36.202   upi      normal   Ready    zone-1   4.7.19_1525_openshift*   
    ```
    {: screen}


1. [Attach new hosts to your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-attach-hosts). The number of hosts you attach must match the number of hosts that you want to update.   
2. [Assign the newly attached hosts to your {{site.data.keyword.satelliteshort}} resource](/docs/satellite?topic=satellite-assigning-hosts). These hosts automatically receive the update when you assign them.
3. After the new hosts are successfully assigned to your {{site.data.keyword.satelliteshort}} resource, [remove and delete the old hosts that you previously noted](/docs/satellite?topic=satellite-host-remove).


## Updating worker node hosts in the {{site.data.keyword.openshiftlong_notm}} console
{: #host-update-roks-console}

You can update worker node hosts by using the {{site.data.keyword.openshiftlong_notm}} console.
{: shortdesc}

1. Log in to the {{site.data.keyword.cloud_notm}} console and click **OpenShift** > **Clusters**.
2. Click the cluster where the hosts that you want to update are assigned and navigate to the **Worker nodes** page.
3. Select each host that you want to update. After you select the hosts, an **Update** option appears. 
4. Click **Update**. In the dialog box that appears, click **Update** again. A message appears that the update started successfully. 
5. Wait while the hosts update. The update process for each host is complete when the **Status** of the host returns to **Normal** and the new version is listed in the **Version** column. 

## Determining if the worker node version update is a major, minor, or patch update
{: #host-update-workers-type}

The process to update a worker node is the same for all types of updates. However, you can find information on whether the update is a major, minor, or patch update.
{: shortdesc}

To determine the type of update that is available, compare your current worker node versions to the latest `worker node fix pack` version in the [{{site.data.keyword.redhat_openshift_notm}} version change log](/docs/openshift?topic=openshift-openshift_versions). Major updates are indicated by the first digit in the version label (4.x.x), minor updates are indicated by the second digit (x.7.x) and patch updates are indicated by the trailing digits (x.x.23_1528_openshift). For more information on version updates, see [Version information and update actions](/docs/openshift?topic=openshift-openshift_versions).
