---

copyright:
  years: 2020, 2022
lastupdated: "2022-06-10"

keywords: satellite, hybrid, multicloud, os upgrade, operating system, security patch

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Updating hosts that are assigned as worker nodes to {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services such as clusters
{: #host-update-workers}

{{site.data.keyword.IBM_notm}} provides version updates for your hosts that are assigned to [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-managed-services). The version updates include OpenShift Container Platform, the operating system, and security patches. You choose when to apply the host version updates.
{: shortdesc}

Do not use the `ibmcloud ks worker update` command for hosts that are assigned as worker nodes to {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services.
{: important}

## Checking if a version update is available for worker node hosts
{: #host-update-workers-check}

You can check if a version update is available for a host that is assigned as a worker node to a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services) by using the {{site.data.keyword.cloud_notm}} CLI or the {{site.data.keyword.cloud_notm}} console.
{: shortdesc}

To review the changes that are included in each version update, see the [Version change log for {{site.data.keyword.openshiftlong_notm}}](/docs/openshift?topic=openshift-openshift_changelog).

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

## Manually updating worker node hosts in the CLI
{: #host-update-cli}

You can update your worker node hosts by using the CLI.
{: shortdesc}

### Determining if the worker node version update is a major, minor, or patch update
{: #host-update-workers-type}

The process to update a worker node depends on whether the update is a major, minor, or patch update.
{: shortdesc}

To determine the type of update that is available, compare your current worker node versions to the latest `worker node fix pack` version in the [{{site.data.keyword.redhat_openshift_notm}} version change log](/docs/openshift?topic=openshift-openshift_changelog). Major updates are indicated by the first digit in the version label (4.x.x), minor updates are indicated by the second digit (x.7.x) and patch updates are indicated by the trailing digits (x.x.23_1528_openshift). For more information on version updates, see [Version information and update actions](/docs/openshift?topic=openshift-openshift_versions).

### Applying minor and patch version updates to worker node hosts
{: #host-update-workers-minor}

Hosts that are attached to a location do not update automatically. To apply a minor version update, you must first attach and assign new hosts to your [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services) and then remove the old hosts.
{: shortdesc}

Before you begin

- List your current hosts and make note of their IDs. This helps determine which hosts to remove after applying the update.

    ```sh
    ibmcloud ks worker ls -c <cluster_name_or_ID>
    ```
    {: pre}

- Review the example output.

    ```sh
    ID                                                        Primary IP      Flavor   State    Status   Zone     Version   
    sat-satliberty-5b4c7f3a7bfc14cf58cbb14ad5c08429475274fe   208.43.36.202   upi      normal   Ready    zone-1   4.7.19_1525_openshift*   
    ```
    {: screen}

To apply a minor or patch update,

1. [Attach new hosts to your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-attach-hosts). The number of hosts you attach must match the number of hosts that you want to update.   
2. [Assign the newly attached hosts to your {{site.data.keyword.satelliteshort}} resource](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual). These hosts automatically receive the update when you assign them.
3. After the new hosts are successfully assigned to your {{site.data.keyword.satelliteshort}} resource, [remove and delete the old hosts that you previously noted](/docs/satellite?topic=satellite-host-remove).

### Applying major version updates to worker node host
{: #host-update-workers-major}

Hosts that are attached to a location do not update automatically. To apply a major version update to your worker node hosts, you must attach, assign, and remove hosts one at at time from your {{site.data.keyword.satelliteshort}} location's control plane. Then, you attach, assign, and remove hosts from your [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
{: shortdesc}

Before you begin

1. List your location's current control plane hosts and make note of their IDs. This helps determine which hosts to remove from the control plane after applying the update. The control plane hosts have `infrastructure` listed in the `Cluster` column of the output.

   ```sh
   ibmcloud sat hosts ls --location <location>
   ```
   {: pre}
   
   Review the example output.

   ```sh
   Name                ID                     State        Status   Zone     Cluster          Worker ID                                              Worker IP   

   satdemo-cp1         0bc3b92f55968a230985   assigned     Ready    zone-1   infrastructure   sat-satdemocp1-2bda578e901b4047c6e48d766cd99bc11a45fddd   169.62.42.178   
   satdemo-cp2         999cd38c39ddffe4b672   assigned     Ready    zone-2   infrastructure   sat-satdemocp2-940134e69c2609c5421b2426a7640fa80569668d   169.62.42.183   
   satdemo-cp5         6ca4fd8fcad1fa622bb4   assigned     Ready    zone-3   infrastructure   sat-satdemocp5-d46581b509357ea4b429fddc38a18b155463bf1c   169.62.42.181   
   ```
   {: screen}

2. List your current hosts that are assigned as worker nodes to your {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service and make note of their IDs. This helps determine which hosts to remove from your {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service after applying the update.

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
    
Choose from one of the following scenarios,

- Applying a major update to the control plane hosts.

    See [Procedure to update control plane hosts]/docs/satellite?topic=satellite-host-update-location.

- Applying a major update to your hosts assigned to a {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service.

    1. [Attach new hosts to your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-attach-hosts). The number of hosts that you attach must match the number of hosts that you want to update.
    2. [Assign the newly attached hosts to your {{site.data.keyword.satelliteshort}} resource](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual). These hosts automatically receive the new update when you assign them.
    3. After the new hosts are successfully assigned to your {{site.data.keyword.satelliteshort}} resource, [remove and delete the old worker node hosts that you previously noted](/docs/satellite?topic=satellite-host-remove).

## Updating worker node hosts in the {{site.data.keyword.openshiftlong_notm}} console
{: #host-update-roks-console}

You can update worker node hosts by using the {{site.data.keyword.openshiftlong_notm}} console. 
{: shortdesc}

The option to update worker node hosts in the {{site.data.keyword.openshiftlong_notm}} console is available in beta.
{: beta}

1. Log in to the {{site.data.keyword.cloud_notm}} console and click **OpenShift** > **Clusters**.
2. Click the cluster where the hosts that you want to update are assigned and navigate to the **Worker nodes** page.
3. Select each host that you want to update. After you select the hosts, an **Update** option appears. 
4. Click **Update**. In the dialog box that appears, click **Update** again. A message appears that the update started successfully. 
5. Wait while the hosts update. The update process for each host is complete when the **Status** of the host returns to **Normal** and the new version is listed in the **Version** column. 

## Updating host metadata
{: #host-update-metadata}

If you want to update metadata about a host, such as labels or zones, see the [`ibmcloud sat host update` command](/docs/satellite?topic=satellite-satellite-cli-reference#host-update). This metadata is used to help manage your hosts, such as for [auto assignment](/docs/satellite?topic=satellite-assigning-hosts#host-autoassign-ov). The metadata update does not apply security patches or operating system updates.
{: shortdesc}
