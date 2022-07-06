---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-05"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}


# Debugging host health
{: #ts-hosts-debug}

By default, {{site.data.keyword.satellitelong_notm}} monitors the health of your hosts and tries to resolve issues automatically for you. For issues that cannot be resolved automatically, you can debug the hosts by reviewing the provided health information.
{: shortdesc}


To check your host setup, you can use the `satellite-host-check` script. For more information, see [Checking your host setup](/docs/satellite?topic=satellite-host-network-check).
{: tip}


1. Review the health and status of your hosts. From the CLI, list your hosts in a location. From the console, click your location, and then click the **Hosts** tab.
    ```sh
    ibmcloud sat host ls --location <location_name_or_ID>
    ```
    {: pre}

    Example output
    ```sh
    Name               ID                     State        Status   Cluster          Worker ID   Worker IP   
    satellite-test-1   aaaaa1a11aaaaaa111aa   assigned     -        infrastructure   -           -   
    ```
    {: screen}

2. Review the states and the steps to resolve the issue in the following table.
3. Check that your hosts still meet the [minimum requirements](/docs/satellite?topic=satellite-host-reqs), such as for network connectivity. For example, if you change the firewall rules for the host machine's public or private network connectivity, you might make the host unsupported by blocking the necessary ports and IP addresses. For cloud provider-specific configurations, see [Cloud infrastructure providers](/docs/satellite?topic=satellite-infrastructure-plan).
4. If your host still has issues, try to [remove](/docs/satellite?topic=satellite-host-remove), update, and [reattach the host](/docs/satellite?topic=satellite-attach-hosts).

When you attach hosts to a {{site.data.keyword.satelliteshort}} location, {{site.data.keyword.cloud_notm}} monitors the host and reports back statuses that you can use to keep your hosts healthy. For more information, see [{{site.data.keyword.IBM_notm}} monitoring to resolve and report location alerts](/docs/satellite?topic=satellite-monitor#monitoring-default). For troubleshooting help, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts-debug).
{: shortdesc}

You can review the host health from the **Hosts** table in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, or by running `ibmcloud sat host ls --location <location_name_or_ID>`.

| Health state | Description |
| --- | --- |
| `assigned` | The host is assigned to a {{site.data.keyword.satelliteshort}} resource, such as a location control plane or cluster. View the status for more information. If the status is `-`, the hosts did not complete the bootstrapping process to the {{site.data.keyword.satelliteshort}} resource. For hosts that you just assigned, wait an hour or so for the process to complete. If you still see the status, [log in to the host to continue debugging](/docs/satellite?topic=satellite-ts-hosts-login).|
| `health-pending` | The host is assigned and bootstrapped into the cluster as worker nodes that are provisioned and deployed. However, the health components that {{site.data.keyword.IBM_notm}} sets up in the host cannot communicate status back to {{site.data.keyword.cloud_notm}}. Make sure that your hosts meet the [minimum host and network connectivity requirements](/docs/satellite?topic=satellite-reqs-host-network) and that the hosts are not blocked by a firewall in your infrastructure provider. |
| `provisioning` | The host is attached to the {{site.data.keyword.satelliteshort}} location and is in the process of bootstrapping to become part of a {{site.data.keyword.satelliteshort}} resource, such as the worker node of a {{site.data.keyword.openshiftlong_notm}} cluster. While the host reports a `provisioning` state, the worker node goes through the states of provisioning and deploying. |
| `ready` | The host is attached to the {{site.data.keyword.satelliteshort}} location and ready to be [assigned to a {{site.data.keyword.satelliteshort}} resource](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual).|
| `normal` | The host is assigned to a {{site.data.keyword.satelliteshort}} resource, such as a location control plane or cluster, and ready for usage. |
| `reload-required` | The host is attached to the {{site.data.keyword.satelliteshort}} location, but requires a reload before it can be assigned to a {{site.data.keyword.satelliteshort}} resource. For example, you might have deleted a {{site.data.keyword.satelliteshort}} cluster, and now all the hosts from the cluster require a reload. To reload a host, you must [remove the host from the location](/docs/satellite?topic=satellite-host-remove), reload the operating system in the underlying infrastructure provider, and [attach the host](/docs/satellite?topic=satellite-attach-hosts) back to the location. |
| `unassigned` | The host is attached to the {{site.data.keyword.satelliteshort}} location and ready to be [assigned to a {{site.data.keyword.satelliteshort}} resource](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual). If you tried to assign the host unsuccessfully, see [Cannot assign hosts to a cluster](/docs/satellite?topic=satellite-assign-fails).|
| `unknown` | The health of the host is unknown. If the host is unassigned, try [assigning the host](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual) to a {{site.data.keyword.satelliteshort}} resource, such as a cluster. If the host is assigned, try debugging the host by following the steps in [debugging the health of the host](/docs/satellite?topic=satellite-ts-hosts-debug). If the host still has issues, try removing, updating, and reattaching the host. |
| `unresponsive` | The host did not check in with the {{site.data.keyword.satelliteshort}} location control plane within the past 5 minutes. The host cannot be assigned when it is unresponsive. Try [debugging the health of the host](/docs/satellite?topic=satellite-ts-hosts-debug), particularly the network connectivity. |
{: caption="Host health states." caption-side="top"}
{: summary="The rows are read from left to right. The first column describes the health state of the host. The second column describes what the health state means."}


