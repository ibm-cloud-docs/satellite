---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-18"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Auditing, logging, and monitoring {{site.data.keyword.satelliteshort}} Link endpoints
{: #link-cloud-monitor}

Add auditing, logging, and monitoring to your link endpoints to help ensure the health and performance of the services and resources attached to your location.
{: shortdesc}

## Auditing events for endpoint actions
{: #link-audit}

{{site.data.keyword.satellitelong_notm}} integrates with {{site.data.keyword.at_full_notm}} to collect and send audit events for all link endpoints in your location to your {{site.data.keyword.at_short}} instance.
{: shortdesc}

1. [Provision an instance of {{site.data.keyword.at_short}}](/docs/log-analysis?topic=log-analysis-provision) in the {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from.
2. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations), click the name of your location.
3. From the **Link endpoints** tab, click the name of your endpoint.
4. From the actions menu, click **Launch Auditing**. The dashboard for your {{site.data.keyword.at_short}} instance is opened, and the events are filtered for your endpoint's ID.

For more information about the types of {{site.data.keyword.satelliteshort}} events that you can track, see [Auditing events for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-at_events).
{: tip}

## Logging and monitoring network traffic for endpoints
{: #link-health}

Log traffic that flows from your source to your destination resource over a {{site.data.keyword.satelliteshort}} endpoint.
{: shortdesc}

### Setting up {{site.data.keyword.mon_short}} for {{site.data.keyword.satelliteshort}} Link metrics
{: #link-mon}

Metrics are available for the {{site.data.keyword.satelliteshort}} Link component of your location to help you monitor the performance of specific Link endpoints or of all Link endpoints for the location. For example, you can monitor the latency or throughput of a specific Link endpoint that you created. To get started, see [Setting up {{site.data.keyword.mon_short}} for {{site.data.keyword.satelliteshort}} location platform metrics](/docs/satellite?topic=satellite-monitor#setup-mon).

### Running a packet capture of endpoint traffic
{: #link-packet-capture}

Run a packet capture to view the traffic that is flowing from your source to your destination resource over a {{site.data.keyword.satelliteshort}} endpoint. Packet captures can be useful to help you debug problems with your endpoint connectivity or to monitor sources that access your destination.
{: shortdesc}

Before you begin, install a packet capture tool, such as [`tcpdump`](https://www.tcpdump.org/){: external}, on your local machine.

1. Get the host name and port for your endpoint in the **Address** field. For cloud endpoints, the host name is the {{site.data.keyword.satelliteshort}} Link connector host name. For location endpoints, the host name is the {{site.data.keyword.satelliteshort}} Link tunnel server host name.
    ```sh
    ibmcloud sat endpoint ls --location <location_ID>
    ```
    {: pre}

    Example output
    ```sh
    ID                           Name                                         Destination Type   Address
    c0mnbnkw0jl8si22djkg_cEomQ   openshift-api-c0mpnn4w0bv28oq2dks0           location           TCP  c-02.us-east.link.satellite.cloud.ibm.com:32823
    c0mnbnkw0jl8si22djkg_6UTZd   satellite-healthcheck-c0mnbnkw0jl8si22djkg   location           HTTP c-02.us-east.link.satellite.cloud.ibm.com:32822
    ```
    {: screen}

2. By using the host name and port, start a packet capture. The following command is an example for using `tcpdump`.

    ```sh
    tcpdump -i <interface> host <link_host> and port <endpoint_port> [-n] [-w <filename>.pcap]
    ```
    {: pre}
    
    | Component | Description | 
    |---------|------------------|
    | `-i <interface>` | The interface that routes traffic through the endpoint. To view available interfaces, run `tcpdump -D`. If you do not know which interface is used, specify `-i any`. | 
    | `host <link_host>` | The host name that was assigned by {{site.data.keyword.satelliteshort}} Link to your endpoint. | 
    | `port <endpoint_port>` | The port that was assigned by {{site.data.keyword.satelliteshort}} Link to your endpoint. | 
    | `-n` | Include this option if you do not want the IP addresses and port numbers in the output to be converted to DNS host names. | 
    | `-w <filename>` | Include this option to print the output of the packet capture into a `.pcap` file. | 
    {: summary="This table is read from left to right. The first column has the API component. The second column has the description of the component."}
    {: caption="Table 1. Understanding the API request" caption-side="top"}

3. In the output, you can check the sources and destinations of packets that are sent through the endpoint.

    Example output for a cloud endpoint that allows sources in a {{site.data.keyword.satelliteshort}} location (`10.171.52.151`) to access a target resource in {{site.data.keyword.cloud_notm}} (`166.9.12.121`).
    ```sh
    tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
    listening on any, link-type LINUX_SLL (Linux cooked), capture size 262144 bytes
    05:58:13.471632 IP 10.171.52.151.33666 > 166.9.12.121.filenet-nch: Flags [S], seq 2853445049, win 29200, options [mss 1460,sackOK,TS val 592612262 ecr 0,nop,wscale 7], length 0
    05:58:13.474685 IP 166.9.12.121.filenet-nch > 10.171.52.151.33666: Flags [S.], seq 2264270242, ack 2853445050, win 28960, options [mss 1460,sackOK,TS val 1156479729 ecr 592612262,nop,wscale 9], length 0
    05:58:13.474718 IP 10.171.52.151.33666 > 166.9.12.121.filenet-nch: Flags [.], ack 1, win 229, options [nop,nop,TS val 592612265 ecr 1156479729], length 0
    05:58:13.474806 IP 10.171.52.151.33666 > 166.9.12.121.filenet-nch: Flags [P.], seq 1:115, ack 1, win 229, options [nop,nop,TS val 592612265 ecr 1156479729], length 114
    05:58:13.476559 IP 166.9.12.121.filenet-nch > 10.171.52.151.33666: Flags [.], ack 115, win 57, options [nop,nop,TS val 1156479729 ecr 592612265], length 0
    05:58:13.583080 IP 166.9.12.121.filenet-nch > 10.171.52.151.33666: Flags [P.], seq 1:145, ack 115, win 57, options [nop,nop,TS val 1156479756 ecr 592612265], length 144
    05:58:13.583132 IP 10.171.52.151.33666 > 166.9.12.121.filenet-nch: Flags [.], ack 145, win 237, options [nop,nop,TS val 592612373 ecr 1156479756], length 0
    05:58:13.583399 IP 10.171.52.151.33666 > 166.9.12.121.filenet-nch: Flags [F.], seq 115, ack 145, win 237, options [nop,nop,TS val 592612373 ecr 1156479756], length 0
    05:58:13.585237 IP 166.9.12.121.filenet-nch > 10.171.52.151.33666: Flags [F.], seq 145, ack 116, win 57, options [nop,nop,TS val 1156479756 ecr 592612373], length 0
    05:58:13.585273 IP 10.171.52.151.33666 > 166.9.12.121.filenet-nch: Flags [.], ack 146, win 237, options [nop,nop,TS val 592612375 ecr 1156479756], length 0
    ```
    {: screen}

If you want to quickly generate traffic logs to test your endpoint, you can send 100 requests to your endpoint's host name and port: `for ((i=1;i<=100;i++)); do curl -v --header "Connection: keep-alive" "<host>:<port>"; done`.
{: tip}
