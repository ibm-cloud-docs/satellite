---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}


# Location error messages
{: #ts-locations-debug}

By default, {{site.data.keyword.satellitelong_notm}} monitors the health of your locations and tries to resolve issues automatically for you. For issues that cannot be resolved automatically, you can debug the location by reviewing the provided health information.
{: shortdesc}

## Reviewing error messages and logs
{: #review-messages-logs}

1. View your locations in the console or list your locations in the CLI, and review the **Status**. If the status is not healthy, continue to the next step. For more information, see [Viewing location health](/docs/satellite?topic=satellite-monitor#location-health).
    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

    Example output
    ```sh
    Name         ID                     Status            Ready   Created      Hosts (used/total)   Managed From   
    Port-North   aaaaa1a11aaaaaa111aa   action required   no      6 days ago   3 / 5                Washington DC  
    ```
    {: screen}

2. Get the details for your location, and review the **Message**. From the console, you can click the location and hover over the tooltip in the title with the location name and health.
    ```sh
    ibmcloud sat location get --location <location_name_or_ID>
    ```
    {: pre}

    Example output:
    ```sh
    Name:                           Port-NewYork   
    ID:                             aaaaa1a11aaaaaa111aa   
    Created:                        2020-06-05 13:50:58 -0400 (6 days ago)   
    Creator:                        name@email.com   
    Managed From:                   Washington DC   
    State:                          action required   
    Ready for deployments:          no   
    Message:                        R0015: Could not assign hosts because no hosts are available. Attach more hosts to the location and try again. For more information, see the docs: 'http://ibm.biz/sat-loc'
    ```
    {: screen}

3. Get more details about the error message and affected components by [setting up {{site.data.keyword.la_short}} to review {{site.data.keyword.satelliteshort}} location logs](/docs/satellite?topic=satellite-get-help#review-logs).

4. Review the following common location messages for steps to resolve the issue.

## R0001: Ready location
{: #R0001}

Location message
:    The {{site.data.keyword.satelliteshort}} location is ready for operations.

Steps to resolve
:    Your {{site.data.keyword.satelliteshort}} location has no critical alerts, and the {{site.data.keyword.IBM_notm}} monitoring component in the location control plane is monitoring the health of your location. You might still see some warning messages for actions that you can take to improve the state of resources in your location, such as hosts.

## R0002, R0018, R0020, R0023, R0029, R0037, R0039, R0042: Wait for location to be ready
{: #R0002}

Location message
:    R0002: The {{site.data.keyword.satelliteshort}} location has issues that {{site.data.keyword.cloud_notm}} Support is working to resolve. Check back later.
:    R0018: {{site.data.keyword.satelliteshort}} is attempting to recover. {: #R0018}
:    R0020: Wait while {{site.data.keyword.satelliteshort}} completes a recovery action. {: #R0020}
:    R0023: Wait while {{site.data.keyword.satelliteshort}} sets up the location control plane. {: #R0023}
:    R0029: Successfully initiated recovery action. {: #R0029}
:    R0037: The {{site.data.keyword.satelliteshort}} location has clusters that are in a failed state. {{site.data.keyword.cloud_notm}} Support is working to resolve. Check back later. {: #R0037}
:    R0039: The {{site.data.keyword.satelliteshort}} location control plane is currently unhealthy. {{site.data.keyword.cloud_notm}} Support is working to resolve. Check back later. {: #R0039}
:    R0042: {{site.data.keyword.cloud_notm}} Support is resolving link api failures. Check back later. If this issue persists, raise a support case. {: #R0042}

Steps to resolve
:    Check back later to see if the issue is resolved. If the issue persists for a while, you can [open a support case](/docs/satellite?topic=satellite-get-help).

To find more details about your issue, set up {{site.data.keyword.la_short}} for {{site.data.keyword.satelliteshort}} location.

1. [Set up {{site.data.keyword.la_short}} for {{site.data.keyword.satelliteshort}} location platform logs](/docs/satellite?topic=satellite-health#setup-la).
2. Search the platform logs for the error code for more details, such as failed API method due to a permissions error.
3. If the details indicate a permission error:
    1. As the account administrator, log in to the {{site.data.keyword.cloud_notm}} CLI and target the resource group and region that the location is in.
        ```sh
        ibmcloud login -g <resource_group> -r <region>
        ```
        {: pre}

    2. Reset the API key that is used for permissions.
        ```sh
        ibmcloud ks api-key reset
        ```
        {: pre}

## R0009: Unable to recover
{: #R0009}

Location message
:    R0009: {{site.data.keyword.satelliteshort}} is unable to recover from issues.

Steps to resolve
:    {{site.data.keyword.satelliteshort}} tried unsuccessfully to automatically resolve the issue. Review any further messages to troubleshoot further, such as adding more hosts to the location. If the issue persists for a while, you can [open a support case](/docs/satellite?topic=satellite-get-help).

## R0010, R0030, R0031, R0032: Control plane needs hosts
{: #R0010}

Location message
:    R0010: Assign more hosts to the location control plane, or replace unhealthy hosts.
:    R0030: A zone for the {{site.data.keyword.satelliteshort}} location control plane is reaching a critical capacity. If critical capacity is reached, you cannot add more clusters to location. Add more hosts to the control plane zone, or replace unhealthy hosts. {: #R0030}
:    R0031: A zone for the {{site.data.keyword.satelliteshort}} location control plane is reaching a warning capacity. Add more hosts to the control plane zone, or replace unhealthy hosts. {: #R0031}
:    R0032: Manually assign hosts to the control plane across all 3 zones. {: #R0032}

Steps to resolve
:    Your location has no available hosts for {{site.data.keyword.satelliteshort}} to automatically assign to the location control plane, and might be reaching capacity limits. You can choose from the following options.

- [Attach](/docs/satellite?topic=satellite-attach-hosts) and [assign](/docs/satellite?topic=satellite-locations#setup-control-plane) more hosts to the location control plane. Keep in mind that when you scale up the location control plane, scale evenly in multiples of 3, and assign the hosts evenly across zones.
- [Remove](/docs/satellite?topic=satellite-host-remove) and [reattach the host](/docs/satellite?topic=satellite-attach-hosts).

## R0011, R0040, R0041: Issues with the control plane hosts
{: #R0011}

Location message
:    R0011: Make sure that all hosts for your {{site.data.keyword.satelliteshort}} location are in a normal state. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your {{site.data.keyword.satelliteshort}} location ID.
:    R0040: The {{site.data.keyword.satelliteshort}} location data plane is currently unhealthy. To debug the host, see 'http://ibm.biz/sat-host-debug'. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your {{site.data.keyword.satelliteshort}} location ID. {: #R0040}
:    R0041: Unknown issues are detected with the {{site.data.keyword.satelliteshort}} location control plane hosts. Ensure that hosts meet the minimum requirements, http://ibm.biz/sat-host-reqs. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your {{site.data.keyword.satelliteshort}} location ID. {: #R0041}

Steps to resolve
:    1. Check the **Status** of your hosts. `ibmcloud sat host ls --location <location_name_or_ID>`
     2. If you have no hosts, [attach](/docs/satellite?topic=satellite-attach-hosts) hosts to your location.
     3. Make sure that you have at least 6 hosts (2 hosts per zone across 3 zones) that are assigned to the **infrastructure** cluster for the location, to run location control plane operations.
     4. If your hosts have no status and are unassigned, [log in to debug the host machines](/docs/satellite?topic=satellite-ts-hosts-login).
     5. Review the host status to [resolve the host issue](/docs/satellite?topic=satellite-ts-hosts-debug).

## R0012: Hosts are needed in all 3 zones
{: #R0012}

Location message
:    R0012: The location control plane does not have hosts in all 3 zones. Add available hosts to your location for the control plane.

Steps to resolve
:    If you just assigned hosts to the control plane, wait a while for the bootstrapping process to complete. Otherwise, [assign](/docs/satellite?topic=satellite-locations#setup-control-plane) at least one host to each of the three zones for the location itself, to run control plane operations.
     - If you did assign at least 2 hosts in each of the 3 zones, check the CPU and memory size of the hosts. The hosts must have at least 4 vCPU and 16 memory.
     - If you did assign at least 2 hosts per zone, make sure that the [hosts meet the minimum requirements](/docs/satellite?topic=satellite-host-reqs) to use in {{site.data.keyword.satelliteshort}}, such as operating system and networking configuration.
     - If you did assign at least 2 hosts in each of the 3 zones but the bootstrapping process failed, [log in to debug the host machines](/docs/satellite?topic=satellite-ts-hosts-login).


## R0013: Unavailable zone
{: #R0013}

Location message
:    R0013: A zone in the location control plane is unavailable. Attach more hosts to the location and assign the hosts to the zone, or replace unhealthy hosts.

Steps to resolve
:    [Assign](/docs/satellite?topic=satellite-locations#setup-control-plane) at least 2 hosts to each of the 3 zones for the location itself, to run control plane operations. If you did assign at least 2 hosts in each of the 3 zones, follow these steps.

:    1. Check the CPU and memory size of the hosts. The hosts must have at least 4 vCPU and 16 memory.
     2. Make sure that the [hosts meet the minimum requirements](/docs/satellite?topic=satellite-host-reqs) to use in {{site.data.keyword.satelliteshort}}, such as operating system and networking configuration.
     3. [Log in to debug the host machines](/docs/satellite?topic=satellite-ts-hosts-login).
     4. [Remove](/docs/satellite?topic=satellite-host-remove) and [reattach the host](/docs/satellite?topic=satellite-attach-hosts). When you remove a host, the host is unassigned from the location control plane, and you must assign another host to the zone.

## R0014: DNS record for control plane
{: #R0014}

Location message
:    R0014: Verify that the {{site.data.keyword.satelliteshort}} location has a DNS record for load balancing requests to the location control plane.

Steps to resolve
:    1. Verify that all hosts in your {{site.data.keyword.satelliteshort}} control plane show a **State** of `assigned` and a **Status** of `Ready` by running `ibmcloud sat host ls --location <location_ID_or_name>`.
     2. If all hosts show the correct state and status, the DNS record for your location is not yet created. This process can take up to 30 minutes to complete after all hosts are successfully assigned to your location.
     3. If one or more hosts do not show the correct state or status, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts-debug).

## R0015, R0016: Host issues
{: #R0015}

Location message
:    R0015: Could not assign hosts because no hosts are available. Attach more hosts to the location and try again. For more information, see the docs: 'http://ibm.biz/sat-loc'
:    R0016: Unexpected error occurred after assigning host. To debug the host, see 'http://ibm.biz/sat-host-debug'. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your {{site.data.keyword.satelliteshort}} location ID.
     {: #R0016}

Steps to resolve
:    [Attach more hosts](/docs/satellite?topic=satellite-attach-hosts) to the location. If you attached hosts that are not showing up as available, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts-debug).

## R0024, R0025, R0038: Cluster issues
{: #R0024}

Location message
:    R0024: The {{site.data.keyword.satelliteshort}} location has {{site.data.keyword.redhat_openshift_notm}} clusters in warning health.
:    R0025: The {{site.data.keyword.satelliteshort}} location has {{site.data.keyword.redhat_openshift_notm}} clusters in critical health. {: #R0025}
:    R0038: The {{site.data.keyword.satelliteshort}} location has clusters in the middle of an operation. Wait for them to finish and check back later. {: #R0038}

Steps to resolve
:    1. Wait to see if another message is returned, such as a message about host capacity.
     2. If a host message is returned, try [Debugging hosts](/docs/satellite?topic=satellite-ts-hosts-debug).
     3. If no further message is returned, try [Debugging your {{site.data.keyword.openshiftlong_notm}} clusters](/docs/openshift?topic=openshift-debug_clusters).

## R0026: Host disk space
{: #R0026}

Location message
:    R0026: Hosts in the location control plane are running out of disk space. Assign more hosts to the location control plane or reload the hosts with disk space issues.

Steps to resolve
:    1. List the hosts that are assigned to the control plane. by running `ibmcloud sat host ls --location <location_name_or_ID> | grep infrastructure`.
     2. Check the details of the hosts by running `ibmcloud sat host get --host <host_ID> --location <location_name_or_ID>`.
     3. In the infrastructure provider for the host, check the disk space of your host machine. Make sure that each host meets the [minimum requirements](/docs/satellite?topic=satellite-reqs-host-storage). [Remove](/docs/satellite?topic=satellite-host-remove) and [reattach the host](/docs/satellite?topic=satellite-attach-hosts).
     4. If debugging and reattaching the host do not resolve the issue, the location control plane needs more compute resources to continue running. [Assign more hosts to the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).

## R0033, R0034, R0035: Control plane capacity issues
{: #R0033}

Location message
:    R0033: Hosts in the location control plane have critical memory usage issues. Add more hosts to the location control plane and wait for the location to return to normal.
:    R0034: Hosts in the location control plane have critical CPU usage issues. Add more hosts to the location control plane and wait for the location to return to normal. {: #R0034}
:    R0035: The location control plane is running at max capacity and cannot support any more workloads. Add hosts to each zone and wait for the location to return to normal. {: #R0035}

Steps to resolve
:    1. In each zone, check the CPU and memory size of the hosts.
         - Across all hosts in a zone, at least 3 CPU total must be available.
         - Across all hosts in a zone, at least 4 GB memory total must be available.
     2. [Attach 3 more hosts to the location](/docs/satellite?topic=satellite-attach-hosts).
     3. [Assign](/docs/satellite?topic=satellite-locations#setup-control-plane) at least one host to each of the three zones to add capacity for control plane operations. Keep in mind that when you scale up the location control plane, scale evenly in multiples of 3, and assign the hosts evenly across zones.

## R0036: Location subdomain traffic routing
{: #R0036}

Location message
:    R0036: The location subdomains are not correctly routing traffic to your control plane hosts. Verify that the location subdomains are registered with the correct IP addresses for your control plane hosts with the `ibmcloud sat location dns` commands.

Steps to resolve
:    See [Why does the location subdomain not route traffic to control plane hosts?](/docs/satellite?topic=satellite-ts-location-subdomain).

## R0043: Layer 3 connectivity
{: #R0043}

Location message
:    R0043: The location does not meet the following requirement: Hosts must have TCP/UDP/ICMP Layer 3 connectivity for all ports across hosts. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your Satellite location ID.

Steps to resolve
:    Hosts must have TCP/UDP/ICMP Layer 3 connectivity for all ports across hosts. You cannot block access to certain ports that might block communication across hosts. Review [Host network requirements](/docs/satellite?topic=satellite-reqs-host-network) and unblock the ports on the host in your infrastructure provider.	This error can also mean that the host does not have the required RHEL packages installed, or the required CPU, memory, disk space and firewall ports opened up. After you verify that your hosts meet all the requirements, take a look at the platform logs. For more information, see [Setting up Log Analysis for Satellite location platform logs](/docs/satellite?topic=satellite-health#setup-la).

To test TCP/UDP/ICMP Layer 3 connectivity for all ports across hosts,
1. SSH into a host that is attached to your location but that is not assigned to any resources.

    You can only SSH into the machine if you did not assign the host to a cluster, or if the assignment failed. Otherwise, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host via SSH for security purposes. You can [remove the host](/docs/satellite?topic=satellite-host-remove) and reload the operating system to restore the ability to SSH into the machine.
    {: note}

2. To check TCP connectivity, verify that `netcat` receives a response from all other hosts on port `10250`. If the operation times out, review [Host network requirements](/docs/satellite?topic=satellite-reqs-host-network) to unblock the ports on the host in your infrastructure provider.
    ```sh
    nc -zv <host_IP> 10250
    ```
    {: pre}

3. To check ICMP connectivity, verify that a ping is successful to all other hosts. Repeat this step for each IP address of the hosts that are attached to your location. If the ping times out, review [Host network requirements](/docs/satellite?topic=satellite-reqs-host-network) to unblock the ports on the host in your infrastructure provider.
    ```sh
    ping <host_IP>
    ```
    {: pre}

4. If the TCP and ICMP connectivity checks do not reveal any issues, reboot all control plane hosts by rebooting one host at a time. Do not reboot control plane hosts concurrently, which can prevent etcd from running on the control plane hosts.

## R0044: DNS issues
{: #R0044}

Location message
:    R0044: DNS issues have been detected on one or more hosts. Verify that your DNS solution is working as expected. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your Satellite location ID.

Steps to resolve
:    One or more hosts in your locations is unable to resolve DNS queries or a search domain is causing unexpected issues. Verify that your DNS solution is working as expected and that all hosts meet the [network host requirements](/docs/satellite?topic=satellite-reqs-host-network).

To test DNS resolution,
1. SSH into a host that is attached to your location but that is not assigned to any resources.

    You can only SSH into the machine if you did not assign the host to a cluster, or if the assignment failed. Otherwise, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host via SSH for security purposes. You can [remove the host](/docs/satellite?topic=satellite-host-remove) and reload the operating system to restore the ability to SSH into the machine.
    {: note}

2. Ensure that DNS resolution is working properly.
    ```sh
    dig +short +timeout=5 +nocookie cloud.ibm.com
    ```
    {: pre}

3. Ensure that `localhost` with any appended search domains in your DNS configuration either do not resolve to anything, or resolve only to `127.0.0.1`. In the `/etc/resolv.conf` file that manages DNS resolution for each host, multiple search domains, such as `search ibm.com`, might be listed. Calico Typha pods on each host run a health check that uses `localhost` resolution. However, some search domains might be appended when the health check attempts to resolve `localhost`, which causes the health check to fail. To ensure that the health check can run properly, make sure that none of the listed search domains resolve to anything other than the `127.0.0.1` IP address when appended to `localhost`.

## R0045: Host read-only file system issues
{: #R0045}

Location message
:    R0045: A read-only file system has been detected on one or more hosts. Replace the affected host(s).

Steps to resolve
:    1. [Set up {{site.data.keyword.la_short}} for {{site.data.keyword.satelliteshort}} location platform logs](/docs/satellite?topic=satellite-health#setup-la) for more information about which hosts are affected.
     2. [Remove](/docs/satellite?topic=satellite-host-remove) the affected hosts and [reattach new hosts](/docs/satellite?topic=satellite-attach-hosts).
     3. If you still have issues, [open a support case](/docs/satellite?topic=satellite-get-help) and include your {{site.data.keyword.satelliteshort}} location ID.

## R0046: NTP issues
{: #R0046}

Location message
:    R0046: An NTP issue has been detected on one or more hosts. Verify that your NTP solution is working as expected.

Steps to resolve
:    One or more hosts in your locations has Network Time Protocol (NTP) issues that must be resolved.

To test NTP on your hosts,
1. SSH into a host that is attached to your location but that is not assigned to any resources.

    You can only SSH into the machine if you did not assign the host to a cluster, or if the assignment failed. Otherwise, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host via SSH for security purposes. You can [remove the host](/docs/satellite?topic=satellite-host-remove) and reload the operating system to restore the ability to SSH into the machine.
    {: note}

2. Ensure that the time that is reported by the host does not differ from the actual time by more than 3 minutes. If the time differs by more than 3 minutes, verify your NTP solution with your infrastructure provider.
    ```sh
    date +%s
    ```
    {: pre}

3. Repeat these steps to identify any hosts that have NTP issues.

## R0047: Location health checking
{: #R0047}

Location message
:    R0047: {{site.data.keyword.cloud_notm}} is unable to use the health check endpoint to check the location's health.

Steps to resolve
:    See [Why is {{site.data.keyword.cloud_notm}} unable to check my location's health?](/docs/satellite?topic=satellite-ts-location-healthcheck).

## R0048: etcd backup failure
{: #R0048}

Location message
:    R0048: The etcd backup for a cluster in your location failed to complete within the past day.

Steps to resolve

:    The etcd data is backed up every 8 hours from your {{site.data.keyword.satelliteshort}} location control plane to a bucket in your {{site.data.keyword.cos_full_notm}} instance. If this backup consecutively fails 3 times over 24 hours, problems might exist with the {{site.data.keyword.cos_short}} bucket or service instance, or with your {{site.data.keyword.satelliteshort}} location's connection to the {{site.data.keyword.cos_short}} instance.

To determine where your problem exists,
1. Ensure that the hosts that you assigned to the location control plane are able to access the {{site.data.keyword.cos_full_notm}} endpoint for the {{site.data.keyword.cloud_notm}} region that your location is managed from. For example, in your host firewall, you must allow outbound connectivity from your control plane hosts to the following endpoints.

    | Region | {{site.data.keyword.cos_short}} endpoint |
    | ------ | -------------- |
    | `wdc` | `s3.us.cloud-object-storage.appdomain.cloud` |
    | `lon` | `s3.eu.cloud-object-storage.appdomain.cloud` |
    {: caption="Table 1. Required outbound connectivity for hosts to {{site.data.keyword.cos_short}} endpoints" caption-side="top"}

2. Verify that the {{site.data.keyword.cos_short}} service instance and bucket that back up your etcd data are available and were not deleted.
    1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click the name of your location.
    2. In the details section of your location overview, copy the name of the **{{site.data.keyword.cos_short}} bucket**.
    3. In the {{site.data.keyword.cloud_notm}} console, navigate to your [{{site.data.keyword.cloud_notm}} resource list](https://cloud.ibm.com/resources){: external}.
    4. Expand the **Storage** row.
    5. Look for the {{site.data.keyword.cos_short}} instance where you created the bucket. If you did not specify a bucket name during location creation, check each {{site.data.keyword.cos_short}} instance until you find the autogenerated bucket for your location.
    6. Click the instance's name. The **Buckets** list page opens.
    7. Verify that the bucket for your control plane etcd backup exists.
    8. If either the service instance or bucket were deleted, [open a support case](/docs/satellite?topic=satellite-get-help) and include your {{site.data.keyword.satelliteshort}} location ID.

3. If the control plane hosts can reach the {{site.data.keyword.cos_short}} endpoint, and the {{site.data.keyword.cos_short}} service instance and bucket exist, [open a support case](/docs/satellite?topic=satellite-get-help) to investigate backup failures and include your {{site.data.keyword.satelliteshort}} location ID.

## R0049: {{site.data.keyword.satelliteshort}} Link IAM API key issue
{: #R0049}

Location message
:    The Link connector is experiencing authentication issues. Contact {{site.data.keyword.cloud_notm}} Support and include your {{site.data.keyword.satelliteshort}} location ID.

:    This error is reported because the IAM API key that is set for the region or resource group that the location is in does not have the required permissions in {{site.data.keyword.satelliteshort}} or {{site.data.keyword.containershort}}, usually because the permissions of the API key owner changed or the API owner left the account.

Steps to resolve
:    If you have a {{site.data.keyword.satelliteshort}} location and a Red Hat {{site.data.keyword.redhat_openshift_notm}} cluster in that location and you are the account owner or a user with Administrator permission for all {{site.data.keyword.satelliteshort}} components, you can resolve this issue by resetting the API key. Note that when you reset the API key, the old key is deleted. Be sure to check if other services are using this API key.
:    1. Log in to {{site.data.keyword.cloud_notm}}: `ibmcloud login`.
     2. Target the region that the location is managed from: `ibmcloud target -r <region>`.
     3. Target the resource group that the location is in: `ibmcloud target -g <resource-group>`.
     4. Reset the IAM API key for that region or resource group: `ibmcloud ks api-key reset --region <region>`.
     5. Review that the API key was set: `ibmcloud ks api-key info --cluster <roks_cluster_in_location>`.

If you only have a location without any clusters, then you cannot reset the API key. Instead, [open a support case](/docs/satellite?topic=satellite-get-help) and include your {{site.data.keyword.satelliteshort}} location ID. You can get your location ID by running the `ibmcloud sat location ls` command.


## R0050, R0051: {{site.data.keyword.satelliteshort}} Link connector issues
{: #R0050}

Location message
:    The Link connector is experiencing token authentication issues. Contact {{site.data.keyword.cloud_notm}} Support and include your Satellite location ID. 
:    The Link connector cannot retrieve the location ID. Contact {{site.data.keyword.cloud_notm}} Support and include your Satellite location ID. {: #R0051}

Steps to resolve
:    [Open a support case](/docs/satellite?topic=satellite-get-help) and include your {{site.data.keyword.satelliteshort}} location ID.

## R0052: Ingress certificate generation issues
{: #R0052}

Location message
:    Ingress certificates have not been generated for the location endpoints.

Steps to resolve
:    {{site.data.keyword.cloud_notm}} Support is notified and is working to resolve the issue. Try again later.


## R0056: Pod status stuck in `terminating`
{: #R0056}

Location message
:    Pods have been stuck in a terminating state on the location control plane node for over an hour.

Steps to resolve
:    Pods that remain in a terminating state for an hour or more indicate that the location control plane is not healthy. Follow the steps to replace the location control plane hosts.

When you update or replace control plane hosts, **do not assign or remove multiple hosts at the same time** as doing so may break the control plane. You must wait for a host assignment or removal to complete before assigning or removing another host.  \n To avoid possible service disruptions, make sure you attach and assign additional hosts to the control plane before removing any hosts.
{: important}

1. For each host you want to remove from the control plane, [attach an additional host](/docs/satellite?topic=satellite-attach-hosts).
2. [Assign the attached hosts](/docs/satellite?topic=satellite-attach-hosts) to your location. Make sure that you only assign hosts one at a time and that each assignment completes before you assign another host.  
3. [Remove the original hosts from your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-host-remove). Make sure that you only remove hosts one at a time and that each removal completes before you remove another host. 

For additional information about the affected components, [set up {{site.data.keyword.la_short}}](/docs/satellite?topic=satellite-get-help#review-logs) and review the [`R0056` error logs](/docs/satellite?topic=satellite-health#logs-error).

## R0057: Outbound traffic to IAM is failing
{: #R0057}

Location message
:    Outbound traffic to IBM Cloud IAM is failing. To ensure that all host requirements are met, see [Host system requirements](/docs/satellite?topic=satellite-host-reqs). More information is available in the IBM Cloud Platform Logs. If the issue persists, contact IBM Cloud Support and include your Satellite location ID.

Steps to resolve
:    Check the health status and ensure that the host system requirements are met.

1. Run the following command to check the health status. 

    ```sh
    curl https://iam.cloud.ibm.com/healthz 
    ```
    {: pre}

1. If the output from the previous step indicates a failure, check that your hosts meet all [system requirements](/docs/satellite?topic=satellite-host-reqs). 
1. If you have met all the system requirements and the issue persists, [open a support case](/docs/satellite?topic=satellite-get-help) and include your {{site.data.keyword.satelliteshort}} location ID. You can get your location ID by running the `ibmcloud sat location ls` command.

For additional information about the affected components, [set up {{site.data.keyword.la_short}}](/docs/satellite?topic=satellite-get-help#review-logs) and review the [`R0057` error logs](/docs/satellite?topic=satellite-health#logs-error).

## R0058: DNS registration is failing
{: #R0058}

Location message
:   A location with this name was recently deleted. If you want to reuse the location name of a recently deleted location, DNS registration might take a week to complete.

Steps to resolve
:   If you don't need to reuse this location name, delete this location and create a location with a unique name. If the issue persists, [open a support case](/docs/satellite?topic=satellite-get-help) and include your {{site.data.keyword.satelliteshort}} location ID.

