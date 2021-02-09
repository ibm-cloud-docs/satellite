---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-09"

keywords: satellite, hybrid, multicloud

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
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
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
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
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
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
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


# Debugging location health
{: #ts-locations-debug}

By default, {{site.data.keyword.satellitelong_notm}} monitors the health of your locations and tries to resolve issues automatically for you. For issues that cannot be resolved automatically, you can debug the location by reviewing the provided health information.
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
     <td><a name="R0001"></a>R0001 The {{site.data.keyword.satelliteshort}} location is ready for operations.</td>
     <td>Your {{site.data.keyword.satelliteshort}} location has no critical alerts, and the IBM monitoring component in the location control plane is monitoring the health of your location. You might still see some warning messages for actions that you can take to improve the state of resources in your location, such as hosts.</td>
    </tr>
    <tr>
    <td><a name="R0002"></a>R0002 The {{site.data.keyword.satelliteshort}} location has issues that {{site.data.keyword.cloud_notm}} Support is working to resolve. Check back later.<br><br>
    <a name="R0018"></a>R0018 {{site.data.keyword.satelliteshort}} is attempting to recover.<br><br>
    <a name="R0020"></a>R0020 Wait while {{site.data.keyword.satelliteshort}} completes a recovery action.<br><br>
    <a name="R0023"></a>R0023 Wait while {{site.data.keyword.satelliteshort}} sets up the location control plane.<br><br>
    <a name="R0029"></a>R0029 Successfully initiated recovery action.<br><br>
    <a name="R0037"></a>R0037 The {{site.data.keyword.satelliteshort}} location has clusters that are in a failed state. {{site.data.keyword.cloud_notm}} Support is working to resolve. Check back later.<br><br>
    <a name="R0039"></a>R0039 The {{site.data.keyword.satelliteshort}} location control plane is currently unhealthy. {{site.data.keyword.cloud_notm}} Support is working to resolve. Check back later.</td>
    <td>Check back later to see if the issue is resolved. If the issue persists for a while, you can [open a support case](/docs/satellite?topic=satellite-get-help).</td>
    </tr>
    <tr>
    <td><a name="R0009"></a>R0009 {{site.data.keyword.satelliteshort}} is unable to recover from issues.</td>
    <td>{{site.data.keyword.satelliteshort}} tried unsuccessfully to automatically resolve the issue. Review any further messages to troubleshoot further, such as adding more hosts to the location. If the issue persists for a while, you can [open a support case](/docs/satellite?topic=satellite-get-help).</td>
    </tr>
    <tr>
     <td><a name="R0010"></a>R0010 Assign more hosts to the location control plane, or replace unhealthy hosts.<br><br>
     <a name="R0030"></a>R0030 A zone for the {{site.data.keyword.satelliteshort}} location control plane is reaching a critical capacity. If critical capacity is reached, you cannot add more clusters to location. Add more hosts to the control plane zone, or replace unhealthy hosts.<br><br>
     <a name="R0031"></a>R0031 A zone for the {{site.data.keyword.satelliteshort}} location control plane is reaching a warning capacity. Add more hosts to the control plane zone, or replace unhealthy hosts.<br><br>
     <a name="R0032"></a>R0032 Manually assign hosts to the control plane across all 3 zones.</td>
     <td>Your location has no available hosts for {{site.data.keyword.satelliteshort}} to automatically assign to the location control plane, and might be reaching capacity limits. You can choose among the following options:
     <ul><li>[Attach](/docs/satellite?topic=satellite-hosts#attach-hosts) and [assign](/docs/satellite?topic=satellite-locations#setup-control-plane) more hosts to the location control plane. Keep in mind that when you scale up the location control plane, scale evenly in multiples of 3, and assign the hosts evenly across zones.</li>
     <li>[Remove](/docs/satellite?topic=satellite-hosts#host-remove) and [reattach the host](/docs/satellite?topic=satellite-hosts#attach-hosts).</li></ul</td>
    </tr>
    <tr>
    <td><a name="R0011"></a>R0011 Make sure that all hosts for your {{site.data.keyword.satelliteshort}} location are in a normal state. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your {{site.data.keyword.satelliteshort}} location ID.<br><br>
    <a name="R0040"></a>R0040 The {{site.data.keyword.satelliteshort}} location data plane is currently unhealthy. To debug the host, see 'http://ibm.biz/sat-host-debug'. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your {{site.data.keyword.satelliteshort}} location ID.<br><br>
    <a name="R0041"></a>R0041 Unknown issues are detected with the {{site.data.keyword.satelliteshort}} location control plane hosts. Ensure that hosts meet the minimum requirements, http://ibm.biz/sat-host-reqs. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your {{site.data.keyword.satelliteshort}} location ID.</td>
    <td><ol><li>Check the **Status** of your hosts by running <code>ibmcloud sat host ls --location &lt;location_name_or_ID&gt;</code></li>
    <li>If you have no hosts, attach hosts to your location.</li>
    <li>Make sure that you have at least 6 hosts (2 hosts per zone across 3 zones) that are assigned to the **infrastructure** cluster for the location, to run location control plane operations.</li>
    <li>If your hosts have no status and are unassigned, [log in to debug the host machines](/docs/satellite?topic=satellite-ts-hosts-login).</li>
    <li>Review the host status to [resolve the host issue](/docs/satellite?topic=satellite-ts-hosts-debug).</li></ol></td>
    </tr>
    <tr>
    <td><a name="R0012"></a>R0012 The location control plane does not have hosts in all 3 zones. Add available hosts to your location for the control plane.</td>
    <td>If you just assigned hosts to the control plane, wait a while for the bootstrapping process to complete. Otherwise, [assign](/docs/satellite?topic=satellite-locations#setup-control-plane) at least one host to each of the three zones for the location itself, to run control plane operations.<ul>
    <li>If you did assign at least 2 hosts in each of the 3 zones, check the CPU and memory size of the hosts. The hosts must have at least 4 vCPU and 16 memory.</li>
    <li>If you did assign at least one host per zone, make sure that the [hosts meet the minimum requirements](/docs/satellite?topic=satellite-host-reqs) to use in {{site.data.keyword.satelliteshort}}, such as operating system and networking configuration.</li>
    <li>If you did assign at least 2 hosts in each of the 3 zones but the bootstrapping process failed, [log in to debug the host machines](/docs/satellite?topic=satellite-ts-hosts-login).</li></ul></td>
    </tr>
    <tr>
    <td><a name="R0013"></a>R0013 A zone in the location control plane is unavailable. Attach more hosts to the location and assign the hosts to the zone, or replace unhealthy hosts.</td>
    <td>[Assign](/docs/satellite?topic=satellite-locations#setup-control-plane) at least 2 hosts to each of the 3 zones for the location itself, to run control plane operations. If you did assign at least one host in each of the 3 zones:<ul>
    <li>Check the CPU and memory size of the hosts. The hosts must have at least 4 vCPU and 16 memory.</li>
    <li>Make sure that the [hosts meet the minimum requirements](/docs/satellite?topic=satellite-host-reqs) to use in {{site.data.keyword.satelliteshort}}, such as operating system and networking configuration.</li>
    <li>[Log in to debug the host machines](/docs/satellite?topic=satellite-ts-hosts-login).</li>
    <li>[Remove](/docs/satellite?topic=satellite-hosts#host-remove) and [reattach the host](/docs/satellite?topic=satellite-hosts#attach-hosts). When you remove a host, the host is unassigned from the location control plane, and you must assign another host to the zone.</li></ul></td>
    </tr>
    <tr>
    <td><a name="R0014"></a>R0014 Verify that the {{site.data.keyword.satelliteshort}} location has a DNS record for load balancing requests to the location control plane.</td>
    <td><ol><li>Run <code>ibmcloud sat host ls --location &lt;location_ID_or_name&gt;</code> and verify that all hosts in your {{site.data.keyword.satelliteshort}} control plane show a **State** of <code>assigned</code> and a **Status** of <code>Ready</code>.</li>
    <li>If all hosts show the correct state and status, the DNS record for your location is not yet created. This process can take up to 30 minutes to complete after all hosts are successfully assigned to your location. </li>
    <li>If one or more hosts do not show the correct state or status, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts-debug).</li>
    </ol></td>
    </tr>
    <tr>
    <td><a name="R0015"></a>R0015 Could not assign hosts because no hosts are available. Attach more hosts to the location and try again. For more information, see the docs: 'http://ibm.biz/sat-loc'<br><br>
    <a name="R0016"></a>R0016 Unexpected error occurred after assigning host. To debug the host, see 'http://ibm.biz/sat-host-debug'. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your {{site.data.keyword.satelliteshort}} location ID.</td>
    <td>[Attach more hosts](/docs/satellite?topic=satellite-hosts#attach-hosts) to the location. If you attached hosts that are not showing up as available, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts-debug).</td>
    </tr>
    <tr>
     <td><a name="R0024"></a>R0024 The {{site.data.keyword.satelliteshort}} location has {{site.data.keyword.openshiftshort}} clusters in warning health.<br><br>
     <a name="R0025"></a>R0025 The {{site.data.keyword.satelliteshort}} location has {{site.data.keyword.openshiftshort}} clusters in critical health.<br><br>
     <a name="R0038"></a>R0038 The {{site.data.keyword.satelliteshort}} location has clusters in the middle of an operation. Wait for them to finish and check back later</td>
     <td><ol><li>Wait to see if another message is returned, such as a message about host capacity.</li>
     <li>If a host message is returned, try [Debugging hosts](/docs/satellite?topic=satellite-ts-hosts-debug).</li>
     <li>If no further message is returned, try [Debugging your {{site.data.keyword.openshiftlong_notm}} clusters](/docs/openshift?topic=openshift-cs_troubleshoot).</li></ol></td>
    </tr>
    <tr>
     <td><a name="R0026"></a>R0026 Hosts in the location control plane are running out of disk space. Assign more hosts to the location control plane, or reload the hosts with disk space issues.</td>
     <td><ol><li>List the hosts that are assigned to the control plane by running `ibmcloud sat host ls --location <location_name_or_ID> | grep infrastructure`.</li>
     <li>Check the details of your host by running `ibmcloud sat host get --host <host_ID> --location <location_name_or_ID>`.</li>
     <li>In the infrastructure provider, check the disk space of your host machine. [Remove](/docs/satellite?topic=satellite-hosts#host-remove) and [reattach the host](/docs/satellite?topic=satellite-hosts#attach-hosts).</li>
     <li>If debugging and reattaching the host do not resolve the issue, the location control plane needs more compute resources to continue running. [Assign more hosts to the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).</li></ol></td>
    </tr>
    <tr>
     <td><a name="R0033"></a>R0033 Hosts in the location control plane have critical memory usage issues. Add more hosts to the location control plane and wait for the location to return to normal.<br><br>
     <a name="R0034"></a>R0034 Hosts in the location control plane have critical CPU usage issues. Add more hosts to the location control plane and wait for the location to return to normal.<br><br>
     <a name="R0035"></a>R0035 The location control plane is running at max capacity and cannot support any more workloads. Add hosts to each zone and wait for the location to return to normal.</td>
     <td><ol><li>Check the CPU and memory size of the hosts. The hosts must have at least 4 vCPU and 16 memory.</li>
     <li>[Attach 3 more hosts to the location](/docs/satellite?topic=satellite-hosts#attach-hosts).</li>
     <li>[Assign](/docs/satellite?topic=satellite-locations#setup-control-plane) at least one host to each of the three zones for the location itself, to add capacity for control plane operations. Keep in mind that when you scale up the location control plane, scale evenly in multiples of 3, and assign the hosts evenly across zones.</li></ol>
    </td>
    </tr>
    <tr>
     <td><a name="R0036"></a>R0036 The location subdomains are not correctly routing traffic to your control plane hosts. Verify that the location subdomains are registered with the correct IP addresses for your control plane hosts with the 'ibmcloud sat location dns' commands.</td>
     <td>See [Location subdomain not routing traffic to control plane hosts](/docs/satellite?topic=satellite-ts-location-subdomain).</td>
    </tr>
    <tr>
     <td><a name="R0043"></a>R0043 The location does not meet the following requirement: Hosts must have TCP/UDP/ICMP Layer 3 connectivity for all ports across hosts. If you still have issues, contact IBM Cloud Support and include your Satellite location ID.</td>
     <td>Hosts must have TCP/UDP/ICMP Layer 3 connectivity for all ports across hosts. You cannot block access to certain ports that might block communication across hosts. See [Host network requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network).</td>
    </tr>
    <tr>
     <td><a name="R0044"></a>R0044 DNS issues have been detected on one or more hosts. Verify that your DNS solution is working as expected. If you still have issues, contact IBM Cloud Support and include your Satellite location ID.</td>
     <td>One or more hosts in your locations is unable to resolve DNS queries or a search domain is causing unexpected issues. Verify that your DNS solution is working as expected and that all hosts meet the network host requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network).</td>
    </tr>
    <tr>
     <td><a name="R0045"></a>R0045 A read-only file system has been detected on one or more hosts. Replace the affected host(s).<br><br>
     <a name="R0046"></a>R0046 An NTP issue has been detected on one or more hosts. Verify that your NTP solution is working as expected.</td>
     <td><ol><li>[Set up LogDNA for {{site.data.keyword.satelliteshort}} location logs](/docs/satellite?topic=satellite-health#setup-logdna) for more information about which hosts are affected.</li>
     <li>[Remove](/docs/satellite?topic=satellite-hosts#host-remove) the affected hosts and [reattach new hosts](/docs/satellite?topic=satellite-hosts#attach-hosts).</li>
     <li>If you still have issues, [open a support case](/docs/satellite?topic=satellite-get-help) and include your Satellite location ID.</li></ol>
    </td>
    </tr>
    </tbody>
    </table>
