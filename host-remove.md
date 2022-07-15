---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-15"

keywords: satellite, hybrid, multicloud, os upgrade, operating system, security patch

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Removing hosts and locations
{: #host-remove}

When you remove a host from your location, the host is unassigned from a {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service cluster or the {{site.data.keyword.satelliteshort}} location control plane, detached from the location, and no longer available to run workloads from {{site.data.keyword.satelliteshort}}. If you delete a {{site.data.keyword.redhat_openshift_notm}} cluster or resize a worker pool, the hosts are still attached to your location, but you must detach and reattach the hosts to use them with another {{site.data.keyword.satelliteshort}} resource.
{: shortdesc}

After removal, the host machine still exists in your underlying infrastructure provider. Reload the operating system before using the host machine for another purpose.
{: tip}

## Removing hosts from the console
{: #host-remove-console}

Use the {{site.data.keyword.satelliteshort}} console to remove your hosts as compute capacity from the {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

Removing a host cannot be undone. Before you remove a host, make sure that your cluster or location control plane has enough compute resources to continue running even after you remove the host, or back up any data that you want to keep.
{: important}

1. Make sure that your cluster or location control plane has enough compute resources to continue running even after you remove the host, or back up any data that you want to keep.
2. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Locations** and then click your location.
3. From the **Hosts** table, find the host that you want to remove.
4. Depending on the type of host, remove the host from a cluster before you remove the host.
    1. If the host **Cluster** is `Control plane`, continue to the next step.
    2. If the host **Cluster** is a hyperlink to the name of a {{site.data.keyword.openshiftlong_notm}} cluster, note the host **IP address** and click the cluster name hyperlink.
    3. From the cluster **Worker Nodes** tab, find the worker node with an **IP address** that matches the **IP address** of the host that you want to remove.
    4. Select the worker node.
    5. From the table action menu, click **Delete**.
    6. In the confirmation message, clear the option to replace the worker node and click **Delete**.
    7. Return to the {{site.data.keyword.satelliteshort}} **Locations > Hosts** table.
5. From the **Hosts** table, hover over the host that you want to remove and click the **Action menu** icon ![Action menu icon](../icons/action-menu-icon.svg).
6. Click **Remove host**, enter the host name to confirm deletion, and click **Remove**.
7. Follow the instructions from your underlying infrastructure provider to complete one of the following actions:
    - To reuse the host for other purposes, reload the operating system of the host. For example, you might reattach the host to a {{site.data.keyword.satelliteshort}} location later. When you reattach a host, the host name can remain the same as the previous name, but a new host ID is generated.
    - To no longer use the host, delete the host from your infrastructure provider.
    
 After the host is removed, shut it down and do not activate it again without reloading the OS.
 {: note}

## Removing hosts with the CLI
{: #host-remove-cli}

Use the CLI plug-in for {{site.data.keyword.satelliteshort}} commands to remove your hosts as compute capacity from the {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

Removing a host cannot be undone. Before you remove a host, make sure that your cluster or location control plane has enough compute resources to continue running even after you remove the host, or back up any data that you want to keep. Note that the underlying host infrastructure is not deleted.
{: important}

1. Make sure that your cluster or location control plane has enough compute resources to continue running even after you remove the host, or back up any data that you want to keep.
2. Log in your {{site.data.keyword.cloud_notm}} account. If you have a federated account, include the `--sso` option, or create an API key to log in.
    ```sh
    ibmcloud login [--sso]
    ```
    {: pre}

3. List your locations and note the name of the location for the host that you want to remove.
    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

4. List your hosts. If the host is assigned to a cluster (and not to **infrastructure**) note the worker **ID** of the host that you want to remove.
    ```sh
    ibmcloud sat host ls --location <location_name_or_ID>
    ```
    {: pre}

    Example output
    ```sh
    Retrieving hosts...
    OK
    Name              ID                     State      Status   Cluster          Worker ID                                                 Worker IP   
    machine-name-1    aaaaa1a11aaaaaa111aa   assigned   Ready    infrastructure   sat-virtualser-4d7fa07cd3446b1f9d8131420f7011e60d372ca2   169.xx.xxx.xxx   
    machine-name-2    bbbbbbb22bb2bbb222b2   assigned   Ready    infrastructure   sat-virtualser-9826f0927254b12b4018a95327bd0b45d0513f59   169.xx.xxx.xxx   
    machine-name-3    ccccc3c33ccccc3333cc   assigned   Ready    mycluster12345   sat-virtualser-948b454ea091bd9aeb8f0542c2e8c19b82c5bf7a   169.xx.xxx.xxx   
    ```
    {: screen}

5. If your host is assigned to a cluster, remove the worker node of the host by using the cluster name and worker ID that you previously retrieved.
    ```sh
    ibmcloud ks worker rm --cluster <cluster_name> --worker <worker_ID>
    ```
    {: pre}

6. Remove the host from your {{site.data.keyword.satelliteshort}} location.
    ```sh
    ibmcloud sat host rm --location <location_name_or_ID> --host <host_ID>
    ```
    {: pre}

7. Follow the instructions from your underlying infrastructure provider to complete one of the following actions:
    - To reuse the host for other purposes, reload the operating system of the host. For example, you might reattach the host to a {{site.data.keyword.satelliteshort}} location later. When you reattach a host, the host name can remain the same as the previous name, but a new host ID is generated.
    - To no longer use the host, delete the host from your infrastructure provider.

## Removing locations from the console
{: #location-remove-console}

Use the {{site.data.keyword.satelliteshort}} console to remove your locations.
{: shortdesc}

Removing a location cannot be undone. Before you remove a location, back up any information that you want to keep and remove any hosts and clusters that run in the location. Note that the underlying host infrastructure is not automatically deleted when you delete a location because you manage the infrastructure yourself.
{: important}

1. [Remove all {{site.data.keyword.openshiftlong_notm}} clusters](/docs/openshift?topic=openshift-remove) from your location.
2. [Remove all hosts](/docs/satellite?topic=satellite-host-remove) from your location.
3. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} **Locations** table, hover over the location that you want to remove and click the **Action menu** icon ![Action menu icon](../icons/action-menu-icon.svg).
4. Click **Remove location**, enter the location name to confirm the deletion, and click **Remove**.

Now that the location is removed, check the hosts in your underlying infrastructure provider. To reuse the hosts for other purposes, you must reload the operating system. If you no longer need the hosts, delete them from your infrastructure provider.

## Removing locations with the CLI
{: #location-remove-cli}

Use the CLI plug-in for {{site.data.keyword.satelliteshort}} commands to remove your locations.
{: shortdesc}

Removing a location cannot be undone. Before you remove a location, back up any information that you want to keep and remove any hosts and clusters that run in the location. Note that the underlying host infrastructure is not automatically deleted when you delete a location because you manage the infrastructure yourself.
{: important}

1. [Remove all {{site.data.keyword.openshiftlong_notm}} clusters](/docs/openshift?topic=openshift-remove) from your location.

2. [Remove all hosts](#host-remove) from your location.

3. Remove the location.

    ```sh
    ibmcloud sat location rm --location <location_name_or_ID>
    ```
    {: pre}

4. Confirm that your location is removed. The location no longer is displayed in the output of the following command.

    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

Now that the location is removed, check the hosts in your underlying infrastructure provider. To reuse the hosts for other purposes, you must reload the operating system. If you no longer need the hosts, delete them from your infrastructure provider.

If you provisioned storage devices for your location, cluster, or services; or used dynamic provisioning to provision storage in your PVCs, make sure to remove the storage devices that you no longer need after removing your location. If you created a location using cloud provider infrastructure, make sure to remove the storage from your account to avoid incurring charges.
{: important}
