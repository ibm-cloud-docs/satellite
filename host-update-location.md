---


copyright:
  years: 2020, 2025
lastupdated: "2025-07-15"

keywords: satellite, hybrid, multicloud, os upgrade, operating system, security patch

subcollection: satellite

---


{{site.data.keyword.attribute-definition-list}}

# Updating {{site.data.keyword.satelliteshort}} location control plane hosts 
{: #host-update-location}

Get the latest OpenShift Container Platform, operating system, and security patches by detaching the hosts from your location, reloading the host machine in your infrastructure provider, then reattaching and reassigning the hosts to your {{site.data.keyword.satelliteshort}} location control plane. 
{: shortdesc}


## Considerations before you update control plane hosts
{: #host-update-considerations}

Review the following considerations before you update your {{site.data.keyword.satelliteshort}} location control plane hosts.
{: shortdesc}

How can I tell if a version update is available?
:    Version updates for hosts become available as the {{site.data.keyword.openshiftlong_notm}} team packages new versions for worker nodes. Typically, worker node version updates are released every two weeks. 
:    You might check for a version update to meet your required security cadence, such as updates on a monthly or bi-monthly basis. To review available version updates, see the [Version change log for {{site.data.keyword.openshiftlong_notm}}](/docs/openshift?topic=openshift-openshift_versions).

Does updating the hosts impact the cluster masters that run in the {{site.data.keyword.satelliteshort}} location control plane?
:    Yes. Because the cluster masters run in your {{site.data.keyword.satelliteshort}} location control plane, make sure that you have enough extra hosts in your control plane before you update any hosts. To attach extra hosts, see [Attaching capacity to your {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-attach-hosts).

Do the hosts in my {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services have to run the same version as my {{site.data.keyword.satelliteshort}} location control plane?
:    No, the hosts that are assigned to the {{site.data.keyword.satelliteshort}} location control plane do not have to run the same version as the hosts that are assigned to {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services that run in the location. However, all hosts in the location must run a supported version.
:    To review supported {{site.data.keyword.redhat_openshift_notm}} versions that hosts can run, see the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-openshift_versions) or run `ibmcloud ks versions` in the command line. 

Is my {{site.data.keyword.satelliteshort}} location control plane subdomain still reachable when I update the hosts?
:    If your location subdomain was created automatically for you, the host IP addresses that are registered for the subdomain are automatically managed for you, such as during an update. 
:    If you manually registered the host IP addresses for the location subdomain with the `ibmcloud sat location dns register` command when you created the {{site.data.keyword.satelliteshort}} location control plane, make sure that you attach three hosts to the control plane before you begin, and manually register these host IPs for the subdomain. Now, these new hosts process requests for the location. Then, you can update the hosts that were previously used for the subdomain.


How can I check the control plane version?
:   Run the `ibmcloud sat location get --location <locationID> --json` and review the `masterKubeVersion` field.

What version is applied when replacing control plane nodes?
:   The new nodes minor version matches the control plane's minor version. The patch version is the latest worker patch version which contains the most recent vulnerability fixes.

How can I avoid downtime when upating the control plane?
:   By following the control process, you ensure that all control plane components are able to run on a node and there won't be any distruptions. Ensure that you only update one worker at a time and leave enough time for a node to fully functional before starting the next one.

How often should I update the control plane hosts?
:   It's recommended to update control plane hosts when a new worker node fix pack is available to pick up all the recent vulnerability fixes.


## Updating control plane hosts
{: #host-update-cp-procedure}

To apply a version update, you must detach, reload, and reattach your host to the {{site.data.keyword.satelliteshort}} location. Then, you can assign the host back to the control plane or to another resource that runs in the location.
{: shortdesc}

When you update control plane hosts, **do not assign or remove multiple hosts at the same time** as doing so may break the control plane. You must wait for a host assignment or removal to complete before assigning or removing another host.
{: important}

1. Optional: [Attach](/docs/satellite?topic=satellite-attach-hosts) and [assign](/docs/satellite?topic=satellite-assigning-hosts) extra hosts to the {{site.data.keyword.satelliteshort}} location control plane to handle the compute capacity while your existing hosts are updating.
1. [Remove the host that you want to update from your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-host-remove).
1. Follow the guidelines from your infrastructure provider to reload the operating system of your host.
1. [Attach the host](/docs/satellite?topic=satellite-attach-hosts) back to your {{site.data.keyword.satelliteshort}} location.
1. [Assign the host](/docs/satellite?topic=satellite-assigning-hosts) back to your {{site.data.keyword.satelliteshort}} location control plane. 

As part of the bootstrapping process, the latest images and {{site.data.keyword.redhat_openshift_notm}} version that matches the cluster master is updated for your host and SSH access to the host is removed.


## Updating the Kubernetes API Server
{: #update-api-server}

Updating your Kubernetes API does not update your control plane hosts.
{: note}

To update your Kubernetes API server, run the [`cluster master update`](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_cluster_update) command. 


## Resetting the host key
{: #host-key-reset}

Reset the key that the control plane uses to communicate with all the hosts in the {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

When you create a location, an API key is generated that the {{site.data.keyword.satelliteshort}} API server uses to attach hosts to the location and assign hosts to the control plane or to [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-managed-services). The generated API key must be treated and protected as sensitive information. This key expires every 365 days. When the key expires, your [unassigned hosts enter an `unresponsive` status](/docs/satellite?topic=satellite-ts-host-unassigned-unknown). To attach additional hosts, you must [download a new host attach script](/docs/satellite?topic=satellite-host-attach-download) from the location and use it to attach hosts. **Your existing, assigned hosts are not affected**. 

You can reset the existing host key before the key expires. To reset your host key for your {{site.data.keyword.satelliteshort}} location, run the `ibmcloud sat host attach` command with the `--reset-key` option.

```sh
ibmcloud sat host attach --location <location_name> --reset-key
```
{: pre}

If you are using Terraform with your {{site.data.keyword.satelliteshort}} location and your host key is reset, either because it expired or you manually reset it, you are then prompted to replace all your hosts, including hosts that are assigned. You can avoid this issue by updating your Terraform script. For more information, see [Why is my host attach script triggering a state change in Terraform?](/docs/satellite?topic=satellite-ts-host-terraform).
{: note}

## Migrating your control plane from RHEL 7 to RHEL 8
{: #migrate-cp-rhel8}

Support for RHEL 7 for Satellite control plane hosts is deprecated and reaches end-of-life on March 2nd, 2023. After that date, you cannot assign any RHEL 7 hosts to the control plane of a location. Existing RHEL 7 hosts cannot be updated and must be replaced with RHEL 8 hosts. 

To replace your RHEL 7 control plane hosts, you must first add a RHEL 8 host to replace it. After you attach a RHEL 8 host to your location and assign it to the control plane, you can remove a RHEL 7 host in the same zone from the control plane. 

Before migrating to RHEL 8, note that Red Hat CoreOS (RHCOS) is now a supported OS in RHCOS enabled locations. RHCOS is a minimal, secure OS with tight integration into the {{site.data.keyword.redhat_openshift_notm}} ecosystem and is capable of doing in-place upgrades of major versions. Before you can use RHCOS hosts in your location, you must create a Red Hat CoreOS enabled location. If you already have a location that is RHCOS enabled, consider adding RHCOS hosts instead. To create a Red Hat CoreOS enabled Satellite location, see [Understanding locations](/docs/satellite?topic=satellite-location-host).
{: tip}

1. Identify which hosts you want to replace and which zones they are located in by running the following command. Look for the `"os": "RHEL7"` label or a host without an `os` label in the output file.
    ```sh
    ibmcloud sat hosts --location LOCATION_ID --output json 
    ```
    {: pre}

    The following example output displays the `"os": "RHEL7"` label.  This host is located in the `us-south-1` zone.

    ```sh
    [
        {
            "id": " LOCATION_ID",
            "name": "LOCATION_NAME",
            "labels": {
                ...
                "os": "RHEL7"
                ...
            "state": "assigned",
            "assignment": {
                "zone": "us-south-1",
            },

        },
    ...
    ]
    ```
    {: screen}
    
    
2. Download the host attach script for the location.

    ```sh
    ibmcloud sat host attach --location LOCATION_ID
    ```
    {: pre}
    
3. Provision a RHEL 8 host for your Satellite location in the identified zone and run the host attach script on that host. This process varies, depending on your infrastructure. For more information, see [Attaching hosts to your location](/docs/satellite?topic=satellite-attach-hosts).

4. After your RHEL 8 host is attached to the location, you can assign it to the location control plane by using the `host assign` command.
    ```sh
    ibmcloud sat host assign --host HOST_NAME --location LOCATION_ID --cluster infrastructure --zone ZONE
    ```
    {: pre}
    

5. After you assign the RHEL 8 host to the control plane, you can remove a RHEL 7 host from the same zone that the RHEL 8 host was added to.

    ```sh
    ibmcloud sat host rm --location LOCATION_ID --host HOST_NAME
    ```
    {: pre}

Repeat these steps until all your RHEL 7 control plane hosts are replaced by RHEL 8 hosts and removed from the location. 

If you have clusters running RHEL 7 hosts in your location, you might need to migrate your worker nodes. For more information, follow the [migration guide](/docs/openshift?topic=openshift-rhel_migrate) for your cluster version.
{: tip}
