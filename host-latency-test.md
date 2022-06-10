---

copyright:
  years: 2020, 2022
lastupdated: "2022-06-10"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Host latency
{: #host-latency-test}

Review the network latency requirements for the hosts that you add to your {{site.data.keyword.satellitelong_notm}} location.
{: shortdesc}

## {{site.data.keyword.IBM_notm}}-managed master to customer-provided worker nodes for the {{site.data.keyword.satelliteshort}} location control plane
{: #host-latency-test-master-worker}

The hosts that you want to attach to the {{site.data.keyword.satelliteshort}} location control plane must have a low latency connection of less than or equal to 200 milliseconds (`<= 200ms`) round-trip time (RTT) to the {{site.data.keyword.cloud_notm}} region that your {{site.data.keyword.satelliteshort}} location is managed from. As latency increases, you might see impacts to performance, including {{site.data.keyword.satelliteshort}} Link throughput, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service provisioning time, host failure recovery time, and in extreme cases, the availability of resources that run in the {{site.data.keyword.satelliteshort}} location control plane, such as {{site.data.keyword.redhat_openshift_notm}} cluster masters. For more information, see [Testing the latency between {{site.data.keyword.cloud_notm}} and the {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-host-latency-test#host-latency-mzr).

## Customer-provided worker nodes in the {{site.data.keyword.satelliteshort}} location control plane to worker nodes that run {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.redhat_openshift_notm}} clusters in the same location
{: #host-latency-test-woker-worker}

Your host infrastructure setup must have a low latency connection of less than or equal to 100 milliseconds (`<= 100ms`) round-trip time (RTT) between the hosts that are used for the {{site.data.keyword.satelliteshort}} location control plane worker nodes and the hosts that are used for other resources in the location, like clusters or [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services). For example, in cloud providers such as AWS, this setup typically means that all the hosts in the {{site.data.keyword.satelliteshort}} location are from the same cloud region, like `us-east-1`. As latency increases, you might see impacts to performance, including provisioning and recovery times, reduced worker nodes in the cluster, {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service degradation, and in extreme cases, failures in your cluster applications.

## Customer-provided worker nodes that are assigned to the same resource, such as the {{site.data.keyword.satelliteshort}} location control plane or a cluster
{: #host-latency-test-customer-provided}

Your host infrastructure setup must have a low latency connection of less than or equal to 10 milliseconds (`<= 10ms`) round-trip time (RTT) among all the hosts that are assigned to the same {{site.data.keyword.satelliteshort}} resource, such as the {{site.data.keyword.satelliteshort}} location control plane, a {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service, or cluster. As latency increases, you might see impacts to performance, including {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services like databases or cluster application failures.

## Testing the latency between {{site.data.keyword.cloud_notm}} and the {{site.data.keyword.satelliteshort}} location control plane hosts
{: #host-latency-mzr}

Each {{site.data.keyword.satelliteshort}} location is [managed from an {{site.data.keyword.cloud_notm}} multizone region](/docs/satellite?topic=satellite-sat-regions#understand-supported-regions). You can test the latency between your hosts and the region to make sure you use a low latency connection of less than or equal to 200 milliseconds (`<= 200ms`) round-trip time (RTT).
{: shortdesc}

1. In your infrastructure provider, log in to a host machine that you want to add to a {{site.data.keyword.satelliteshort}} location. For example, you might SSH into the machine from a command line.

2. Note the IP addresses for the {{site.data.keyword.cloud_notm}} region that you want to test.
    - **Dallas**

      52.117.39.146, 169.48.134.66, 169.63.36.210

    - **Frankfurt**

      149.81.188.122, 158.177.88.18, 161.156.38.122

    - **London**

      158.175.120.210, 141.125.97.106, 158.176.139.66
      
    - **Sao Paulo**
    
      163.107.67.18, 163.109.71.82, 169.57.144.42

    - **Tokyo**

      161.202.104.226, 128.168.67.106, 165.192.108.10

    - **Toronto**
    
      163.74.65.138, 163.75.70.50, 169.53.160.154

    - **Washington, DC**

      169.63.123.154, 169.63.110.114, 169.62.13.2, 169.60.123.162, 169.59.152.58, 52.117.93.26

3. From your host, ping the IP addresses of the {{site.data.keyword.cloud_notm}} region.
    ```sh
    ping <ip_address>
    ```
    {: pre}

4. After a few packets complete transmission, close the connection. For example, from the command line, you might enter `ctrl+c`.
5. In the `ping statistics` output, note the average (`avg`) round-trip distance in milliseconds (ms) between the host and the {{site.data.keyword.cloud_notm}} region, and compare whether the connection meets the latency requirement of less than or equal to 200 milliseconds (`<= 200ms`).

    Example of a connection that meets the latency requirements
    ```sh
    --- 169.63.123.154 ping statistics ---
    25 packets transmitted, 25 packets received, 0.0% packet loss
    round-trip min/avg/max/stddev = 48.131/77.716/181.397/27.893 ms
    ```
    {: screen}

    Example of a connection that does not meet the latency requirements
    ```sh
    --- 158.175.120.210 ping statistics ---
    9 packets transmitted, 9 packets received, 0.0% packet loss
    round-trip min/avg/max/stddev = 138.453/217.370/419.901/108.211 ms
    ```
    {: screen}
