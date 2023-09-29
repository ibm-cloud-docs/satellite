---

copyright:
  years: 2023, 2023
lastupdated: "2023-09-29"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I reach my endpoint from {{site.data.keyword.cloud_notm}}?
{: #ts-connector-cannot-reach}


I created an endpoint and I cannot access it from {{site.data.keyword.cloud_notm}}.
{: tsSymptoms}

Follow this list to check for errors.
{: tsResolve}
  
- Check your Connector logs. A `flowlog` entry is generated for each connection and you can determine why the connection is not working.

- Check the Connector agent log for errors while the agent connects to your endpoint.

- Make sure that your firewall is not blocking the connection from the Connector agent that is running in Docker to the endpoint.

- If an ACL is assigned to the endpoint, make sure it is set up properly with the correct source IP address or CIDR. The source IP might not be what you think it is especially if your source is coming from a Kubernetes cluster or instance in VPC. The `flowlog` entry shows the source IP address; for example,
    ```sh
    May 12 15:57:49  satellite-link-tunnel-5476c9bbdf-vv6m2  satellite-link-tunnel-container 50  flowlog: rejected by Sources when client 10.39.62.229:36062 connecting to 172.18.197.208:35665, conn_type: location
    ```
    {: screen}
  

