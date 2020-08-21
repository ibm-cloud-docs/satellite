---

copyright:
  years: 2020, 2020
lastupdated: "2020-08-21"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:step: data-tutorial-type='step'}


# Locations
{: #ts-locations}

By default, {{site.data.keyword.satellitelong_notm}} monitors the health of your locations and tries to resolve issues automatically for you. For issues that cannot be resolved automatically, you can debug the location by reviewing the provided health information.
{: shortdesc}



## Debugging location health
{: #ts-locations-debug}

Review the health of your location, and use the error messages to resolve any issues that you might have.
{: shortdesc}


1.  View your locations in the console or list your locations in the CLI, and review the **Status**. If the status is not healthy, continue to the next step. For more information, see [Viewing location health](/docs/satellite?topic=satellite-health#location-health).
    ```
    ibmcloud sat location ls
    ```
    {: pre}

    Example output:
    ```
    Name         ID                     Status            Ready   Created      Hosts (used/total)   Managed From   
    Port-North   aaaaa1a11aaaaaa111aa   action required   no      6 days ago   3 / 5                Washington DC  
    ```
    {: screen}

2.  Get the details for your location, and review the **Message**. From the console, you can click the location and hover over the tooltip in the title with the location name and health.
    ```
    ibmcloud sat location get --location <location_name_or_ID>
    ```
    {: pre}

    Example output:
    ```
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

3.  Review the following messages and the steps to resolve the issue.

    <table summary="This table is read from left to right. The first column has the error message. The second column has the description of the how to resolve the error.">
    <caption>Steps to resolve error messages</caption>
    <thead>
    <th>Message</th>
    <th>Description</th>
    </thead>
    <tbody>
    <tr>
     <td>R0001 The {{site.data.keyword.satelliteshort}} location is ready for operations.</td>
     <td>Your {{site.data.keyword.satelliteshort}} location has no critical alerts, and the IBM monitoring component in the location control plane is monitoring the health of your location. You might still see some warning messages for actions that you can take to improve the state of resources in your location, such as hosts.</td>
    </tr>
    <tr>
    <td>R0002 The {{site.data.keyword.satelliteshort}} location has issues that {{site.data.keyword.cloud_notm}} Support is working to resolve. Check back later.<br><br>
    R0018 {{site.data.keyword.satelliteshort}} is attempting to recover.<br><br>
    R0020 Wait while {{site.data.keyword.satelliteshort}} completes a recovery action.<br><br>
    R0023 Wait while {{site.data.keyword.satelliteshort}} sets up the location control plane.<br><br>
    R0029 Successfully initiated recovery action.</td>
    <td>Check back later to see if the issue is resolved. If the issue persists for a while, you can [open a support case](/docs/satellite?topic=satellite-get-help).</td>
    </tr>
    <tr>
    <td>R0009 {{site.data.keyword.satelliteshort}} is unable to recover from issues.</td>
    <td>{{site.data.keyword.satelliteshort}} tried unsuccessfully to automatically resolve the issue. Review any further messages to troubleshoot further, such as adding more hosts to the location. If the issue persists for a while, you can [open a support case](/docs/satellite?topic=satellite-get-help).</td>
    </tr>
    <tr>
     <td>R0010 Assign more hosts to the location control plane, or replace unhealthy hosts.<br><br>
     R0030 A zone for the {{site.data.keyword.satelliteshort}} location control plane is reaching a critical capacity. If critical capacity is reached, you cannot add more clusters to location. Add more hosts to the control plane zone, or replace unhealthy hosts.<br><br>
     R0031 A zone for the {{site.data.keyword.satelliteshort}} location control plane is reaching a warning capacity. Add more hosts to the control plane zone, or replace unhealthy hosts.<br><br>
     R0032 Manually assign hosts to the control plane across all 3 zones.</td>
     <td>Your location has no available hosts for {{site.data.keyword.satelliteshort}} to automatically assign to the location control plane, and might be reaching capacity limits. You can choose among the following options:
     <ul><li>[Add more hosts to the location](/docs/satellite?topic=satellite-hosts#add-hosts) with labels for the control plane and zone, so that {{site.data.keyword.satelliteshort}} can automatically assign the hosts to the location control plane.</li>
     <li>[Assign hosts to the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).</li>
     <li>[Update any unhealthy hosts](/docs/satellite?topic=satellite-hosts#host-update) to replace them.</li></ul</td>
    </tr>
    <tr>
    <td>R0011 Make sure that all hosts for your Satellite location are in a normal state. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your {{site.data.keyword.satelliteshort}} location ID.</td>
    <td><ol><li>Check the **Status** of your hosts by running <code>ibmcloud sat host ls --location <location_name_or_ID><code></li>
    <li>If you have no hosts, add hosts to your location.</li>
    <li>Make sure that you have at least 3 hosts that are assigned to the **infrastructure** cluster for the location, to run location control plane operations.</li>
    <li>If your hosts have no status, [log in to debug the host machines](/docs/satellite?topic=satellite-ts-hosts#ts-hosts-login).</li>
    <li>Review the host status to [resolve the host issue](/docs/satellite?topic=satellite-ts-hosts#ts-hosts-debug).</li></ol></td>
    </tr>
    <tr>
    <td>R0012 The location control plane does not have hosts in all 3 zones. Add available hosts to your location for the control plane. Then, wait for {{site.data.keyword.satelliteshort}} to automatically assign the hosts to control plane zones, or you can assign the hosts.</td>
    <td>If you just assigned hosts to the control plane, wait a while for the bootstrapping process to complete. Otherwise, [assign](/docs/satellite?topic=satellite-locations#setup-control-plane) at least one host to each of the three zones for the location itself, to run control plane operations.<ul>
    <li>If you did assign at least one host in each of the 3 zones, check the CPU and memory size of the hosts. The hosts must have at least 4 CPU and 16 memory.</li>
    <li>If you did assign at least one host per zone, make sure that the [hosts meet the minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host) to use in {{site.data.keyword.satelliteshort}}, such as operating system, networking configuration, and public network access.</li>
    <li>If you did assign at least one host in each of the 3 zones but the bootstrapping process failed, [log in to debug the host machines](/docs/satellite?topic=satellite-ts-hosts#ts-hosts-login).</li></ul></td>
    </tr>
    <tr>
    <td>R0013 A zone in the location control plane is unavailable. Add more hosts to the location and assign the hosts to the zone, or replace unhealthy hosts.</td>
    <td>[Assign](/docs/satellite?topic=satellite-locations#setup-control-plane) at least one host to each of the three zones for the location itself, to run control plane operations. If you did assign at least one host in each of the 3 zones:<ul>
    <li>Check the CPU and memory size of the hosts. The hosts must have at least 4 CPU and 16 memory.</li>
    <li>Make sure that the [hosts meet the minimum requirements](/docs/satellite?topic=satellite-limitations#limits-host) to use in {{site.data.keyword.satelliteshort}}, such as operating system, networking configuration, and public network access.</li>
    <li>[Log in to debug the host machines](/docs/satellite?topic=satellite-ts-hosts#ts-hosts-login).</li>
    <li>[Update the host](/docs/satellite?topic=satellite-hosts#host-update). When you update a host, the host is unassigned from the location control plane, and you must assign another host to the zone.</li></ul></td>
    </tr>
    <tr>
    <td>R0014 Verify that the {{site.data.keyword.satelliteshort}} location has a DNS record for load balancing requests to the location control plane.</td>
    <td><ol><li>Run <code>ibmcloud sat host ls --location &lt;location_ID_or_name&gt;</code> and verify that all hosts in your {{site.data.keyword.satelliteshort}} control plane show a **State** of <code>assigned</code> and a **Status** of <code>Ready</code>. </li>
    <li>If all hosts show the correct state and status, the DNS record for your location is not yet created. This process can take up to 30 minutes to complete after all hosts are successfully assigned to your location. </li>
    <li>If one or more hosts do not show the correct state or status, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts#ts-hosts-debug).</li>
    </ol></td>
    </tr>
    <tr>
    <td>R0015 Could not assign hosts because no hosts are available. Attach more hosts to the location and try again. For more information, see the docs: 'http://ibm.biz/sat-loc'<br><br>
    R0016 Unexpected error occurred after assigning host. To debug the host, see 'http://ibm.biz/sat-host-debug'. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your {{site.data.keyword.satelliteshort}} location ID.</td>
    <td>[Add more hosts](/docs/satellite?topic=satellite-hosts#add-hosts) to the location. If you added hosts that are not showing up as available, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts#ts-hosts-debug).</td>
    </tr>
    <tr>
     <td>R0024 The {{site.data.keyword.satelliteshort}} location has {{site.data.keyword.openshiftshort}} clusters in warning health.<br><br>
     R0025 The {{site.data.keyword.satelliteshort}} location has {{site.data.keyword.openshiftshort}} clusters in critical health.</td>
     <td><ol><li>Wait to see if another message is returned, such as a message about host capacity.</li>
     <li>If a host message is returned, try [Debugging hosts](/docs/satellite?topic=satellite-ts-hosts).</li>
     <li>If no further message is returned, try [Debugging your {{site.data.keyword.openshiftlong_notm}} clusters](/docs/openshift?topic=openshift-cs_troubleshoot).</li></ol></td>
    </tr>
    <tr>
     <td>R0026 Hosts in the location control plane are running out of disk space. Assign more hosts to the location control plane, or reload the hosts with disk space issues.</td>
     <td><ol><li>List the hosts that are assigned to the control plane by running `ibmcloud sat host ls --location <location_name_or_ID> | grep infrastructure`.</li>
     <li>Check the details of your host by running `ibmcloud sat host get --host <host_ID> --location <location_name_or_ID>`.</li>
     <li>In the infrastructure provider, check the disk space of your host machine. [Update the host](/docs/satellite?topic=satellite-hosts#host-update) to see if the host issue is resolved.</li>
     <li>If debugging and updating the host do not resolve the issue, the location control plane needs more compute resources to continue running. [Assign more hosts to the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).</li></ol></td>
    </tr>
    <tr>
     <td>R0033 Hosts in the location control plane have critical memory usage issues. Add more hosts to the location control plane and wait for the location to return to normal.<br><br>
     R0034 Hosts in the location control plane have critical CPU usage issues. Add more hosts to the location control plane and wait for the location to return to normal.<br><br>
     R0035 The location control plane is running at max capacity and cannot support any more workloads. Add hosts to each zone and wait for the location to return to normal.</td>
     <td><ol><li>Check the CPU and memory size of the hosts. The hosts must have at least 4 CPU and 16 memory.</li>
     <li>[Add 3 more hosts to the location](/docs/satellite?topic=satellite-hosts#add-hosts).</li>
     <li>[Assign](/docs/satellite?topic=satellite-locations#setup-control-plane) at least one host to each of the three zones for the location itself, to add capacity for control plane operations.</li></ol>
    </td>
    </tr>
    <tr>
     <td>R0036 The location subdomains are not correctly routing traffic to your control plane hosts. Verify that the location subdomains are registered with the correct IP addresses for your control plane hosts with the 'ibmcloud sat location dns' commands.</td>
     <td>See [Location subdomain not routing traffic to control plane hosts](#ts-location-subdomain).</td>
    </tr>
    </tbody>
    </table>

## Debugging the health of the location control plane
{: #ts-locations-control-plane}

When you create a [{{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations#location-concept), IBM automatically sets up a master for the location control plane in {{site.data.keyword.cloud_notm}}. Additionally, you must assign at least three hosts to the {{site.data.keyword.satelliteshort}} location control plane as worker nodes to run location components that IBM configures. If the location control plane that runs on your hosts has issues, you can debug the location control plane.

1.  Get your {{site.data.keyword.satelliteshort}} location ID.
    ```
    ibmcloud sat location ls
    ```
    {: pre}
2.  List the **Hostnames** of the subdomains for your location control plane hosts.
    ```
    ibmcloud sat location dns ls --location <location_name_or_ID>
    ```
    {: pre}

    Example output:
    ```
    Retrieving location subdomains...
    OK
    Hostname                                                                                                 Records                                                                                                Health Monitor   SSL Cert Status   SSL Cert Secret Name                                          Secret  Namespace   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c000.us-east.satellite.appdomain.cloud   169.62.  196.20,169.62.196.23,169.62.196.30                                                                None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c000     default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c001.us-east.satellite.appdomain.cloud   169.62.  196.30                                                                                            None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c001     default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c002.us-east.satellite.appdomain.cloud   169.62.  196.20                                                                                            None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c002     default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c003.us-east.satellite.appdomain.cloud   169.62.  196.23                                                                                            None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c003     default   
    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-ce00.us-east.satellite.appdomain.cloud    ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-c000.us-east.satellite.appdomain.cloud            None             created           ne1d37313068166254bcb-edfc0a8ba65085c5081eced6816c5b9c-ce00      default  
    ```
    {: screen}
   
3.  Check the health of the control plane location subdomains by curling each hostname endpoint. If the endpoint returns a `200` response for each host, the control plane worker node is healthy and serving Kubernetes traffic. If not, continue to the next step.
    ```
    curl -v http://<hostname>:30000
    ```
    {: pre}

    Example output of a failed response:
    ```
    * Rebuilt URL to: http://169.xx.xxx.xxx:30000/
    *   Trying 169.xx.xxx.xxx...
    * TCP_NODELAY set
    * Connection failed
    * connect to 169.xx.xxx.xxx port 30000 failed: Operation timed out
    * Failed to connect to 169.xx.xxx.xxx port 30000: Operation timed out
    * Closing connection 0
    curl: (7) Failed to connect to 169.xx.xxx.xxx port 30000: Operation timed out
    ```
    {: screen}

    Example output of a `200` response:
    ```
    * Rebuilt URL to: http://169.xx.xxx.xxx:30000/
    *   Trying 169.xx.xxx.xxx...
    * TCP_NODELAY set
    * Connected to 169.xx.xxx.xxx (169.xx.xxx.xxx) port 30000 (#0)
    > GET / HTTP/1.1
    > Host: 169.xx.xxx.xxx:30000
    > User-Agent: curl/7.54.0
    > Accept: */*
    >
    < HTTP/1.1 200 OK
    < content-length: 58
    < cache-control: no-cache
    < content-type: text/html
    < connection: close
    <
    <html><body><h1>200 OK</h1>
    Service ready.
    </body></html>
    * Closing connection 0
    ```
    {: screen}
3.  Find the **ID** of the host that did not return a `200` response. You can compare the `Host: 169.xx.xxx.xxx` from the previous step with the **Worker IP** in the output of the following command.
    ```
    ibmcloud sat host ls --location <location_ID> | grep infrastructure
    ```
    {: pre}

    Example output:
    ```
    Name     ID                     State        Status   Cluster          Worker ID                Worker IP   
    host1    aaaaa1a11aaaaaa111aa   assigned     Ready    infrastructure   sat-virtualser-1234...   169.xx.xxx.xxx   
    host2    bbbbbbb22bb2bbb222b2   assigned     Ready    infrastructure   sat-virtualser-1234...   169.xx.xxx.xxx  
    host3    ccccc3c33ccccc3333cc   assigned     Ready    infrastructure   sat-virtualser-1234...   169.xx.xxx.xxx  
    ```
    {: screen}
4.  [Add a host to the control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) in the same zone so that the location control plane has enough compute resources to continue running when you remove the unhealthy host.
5.  [Remove the unhealthy host from the location control plane](/docs/satellite?topic=satellite-hosts#host-remove).
6.  Optional: You can reload the operating system on the unhealthy host and try to attach and assign the host to {{site.data.keyword.satellitelong_notm}} again.

## Location subdomain not routing traffic to control plane hosts
{: #ts-location-subdomain}

{: tsSymptoms}
After you assign hosts to your {{site.data.keyword.satelliteshort}} location control plane, you see a message similar to the following.

```
R0036 The location subdomains are not correctly routing traffic to your control plane hosts. Verify that the location subdomains are registered with the correct IP addresses for your control plane hosts with the 'ibmcloud sat location dns' commands.
```
{: screen}

{: tsCauses}
The location control plane is not publicly accessible. The IP addresses that are registered with the DNS for your location subdomains might have one of the following issues:
* The IP addresses are not the correct public IP addresses of the hosts that run the location control plane.
* The hosts are behind a firewall that blocks traffic to the location control plane.
* The hosts are from cloud providers like AWS or GCP that automatically register the private IP addresses of the hosts, so you must update the DNS records to use the public IP addresses.
* You reused the name of the location, and the location subdomains are still using the IP addresses of the previous location hosts.

{: tsResolve}

Update the location subdomain DNS entries.

1.  Review the location subdomains and check the **Records** for the IP addresses of the hosts that are registered in the DNS for the subdomain.
    ```
    ibmcloud sat location dns ls --location <location_name_or_ID>
    ```
    {: pre}
2.  If you have a firewall, allow traffic from the hosts to the location control plane access through the firewall.
3.  Get the public IP addresses of your hosts. Refer to your provider documentation. For example steps, see the [AWS](/docs/satellite?topic=satellite-providers#aws-reqs-dns-control-plane) or [GCP](/docs/satellite?topic=satellite-providers#gcp-reqs-dns-control-plane) topics.
4.  Update the location subdomain DNS records with the correct public IP addresses of each host in the control plane.
    ```
    ibmcloud sat location dns register --location <location_name_or_ID> --ip <host_IP> --ip <host_IP> --ip <host_IP>
    ```
    {: pre}

    If you use hosts from cloud providers like AWS and GCP, you might also need to update the your cluster Ingress subdomain DNS entries. For more information, see the [AWS](/docs/satellite?topic=satellite-providers#aws-reqs-dns-cluster-nlb) or [GCP](/docs/satellite?topic=satellite-providers#gcp-reqs-dns-cluster-nlb) provider topics.
    {: note}
