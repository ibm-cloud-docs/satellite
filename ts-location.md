

# Managing location health
{: #locations-health}

By default, {{site.data.keyword.satellitelong_notm}} monitors the health of your locations and tries to resolve issues automatically for you. For issues that cannot be resolved automatically, you can debug the location by reviewing the provided health information.
{: shortdesc}

## Debugging location health
{: #locations-health-debug}

Review the health of your location, and use the error messages to resolve any issues that you might have.
{: shortdesc}


1.  List your locations and review the **Status**. If the status is not healthy, continue to the next step.
    ```
    ibmcloud sat location ls
    ```
    {: pre}

    Example output:
    ```
    Name         ID                     Status            Ready   Created      Hosts (used/total)   Managed From   
    Port-North   aaaaa1a11aaaaaa111aa   action required   no      6 days ago   3 / 5                Dallas  
    ```
    {: screen}
2.  Get the details for your location, and review the **Message**.
    ```
    ibmcloud sat location get --location <location_name_or_ID>
    ```
    {: pre}

    Example output:
    ```
    Name:                           Port-North   
    ID:                             aaaaa1a11aaaaaa111aa   
    Created:                        2020-06-05 13:50:58 -0400 (6 days ago)   
    Creator:                        name@email.com   
    Managed From:                   Dallas   
    State:                          action required   
    Ready for deployments:          no   
    Message:                        There was an issue with the client heartbeat. No heartbeat found from client. Please ensure the location has assigned hosts in a 'normal' state 
    ```
    {: screen}
3.  Review the following messages and the steps to resolve the issue.
    
    <table summary="This table is read from left to right. The first column has the error message. The second column has the description of the how to resolve the error.">
    <caption>Understanding this command's components</caption>
    <thead>
    <th>Message</th>
    <th>Description</th>
    </thead>
    <tbody>
    <tr>
    <td>There was an issue with the client heartbeat. No heartbeat found from client. Please ensure the location has assignedhosts in a 'normal' state</td>
    <td><ol><li>Check the **Status** of your hosts by running <code>ibmcloud sat host ls --location <location_name_or_ID><code></li>
    <li>If you have no hosts, add hosts to your location.</li>
    <li>Make sure that you have at least 3 hosts that are assigned to the **infrastructure** cluster for the location, to runlocation control plane operations.</li>
    <li>If your hosts have no status, [log in to debug the host machines](/docs/satellite?topic=satellite-hosts#hosts-health-login-debug).</li>
    <li>Review the host status to [resolve the host issue]((/docs/satellite?topic=satellite-hosts#hosts-health-debug).</li></ol></td>
    </tr>
    <tr>
    <td>There are less than 3 zones available. Add more zones to the location</td>
    <td><ul><li>Assign at least one host to each of the three zones for the location itself, to run control plane operations</li>
    <li>If you did assign at least one host in each of the 3 zones, check the CPU and memory size of the hosts. The hostsmust have at least 4 CPU and 16 memory.</li>
    <li>If you did assign at least one host per zone, make sure that the [hosts meet the minimum requirements](/docssatellite?topic=satellite-limitations#limits-host) to use in {{site.data.keyword.satelliteshort}}, such as operatingsystem, networking configuration, and public network access.</li>
    <li>If you did assign at least one host in each of the 3 zones, [log in to debug the host machines](/docs/satellite?topic=satellite-hosts#hosts-health-login-debug).<li></ul></td>
    </tr>
    </tbody>
    </table>

## Automatically monitoring and resolving location alerts
{: #locations-health-automatic} 


**TODO Do we need to describe the alerts if they're internal?**<br>
{{site.data.keyword.satelliteshort}} monitors resources in your location such as hosts and clusters, and acts on alerts such as the following:
* Cluster capacity exceeds 80% in a zone.
* {{site.data.keyword.openshiftshort}} clusters are in an unhealthy state.
* Default monitoring tools like Prometheus do not work.
* Ingress subdomain registration fails.

To resolve these alerts, {{site.data.keyword.satelliteshort}} automatically takes actions such as the following:
* Prevent or allow {{site.data.keyword.openshiftshort}} clusters to be created.
* Assign hosts to a location.
* Resolve certain healthy issues with {{site.data.keyword.openshiftshort}} clusters.
* Send alerts to your {{site.data.keyword.la_full_notm}} instance.
* Alert IBM engineers to troubleshoot the issues further.

**TODO Or could we do a table like this?**<br>

| Alert | Action |
| --- | --- | 
| Cluster capacity exceeds 80% in a zone. | <ul><li>Prevent or allow {{site.data.keyword.openshiftshort}} clusters to be created.</li><li>Assign hosts to a location.</li></ul> |
| {{site.data.keyword.openshiftshort}} clusters are in an unhealthy state. | Resolve certain healthy issues with {{site.data.keyword.openshiftshort}} clusters. |
| Default monitoring tools like Prometheus do not work. | Send alerts to your {{site.data.keyword.la_full_notm}} instance. |
| Ingress subdomain registration fails. | Alert IBM engineers to troubleshoot the issues further. | 
| Other alerts | Alert IBM engineers to troubleshoot the issues further. |
{: caption="{{site.data.keyword.satelliteshort}} alerts and automatic actions to address the alerts." caption-side="top"}
{: summary="Read this table from left to right. In the first column is the alert. In the second column is the action that {{site.data.keyword.satelliteshort}} automatically takes to address the alert."}
