---

copyright:
  years: 2023, 2023
lastupdated: "2023-10-20"

keywords: satellite, connector, secure gateway

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}




# {{site.data.keyword.satelliteshort}} Connector and Secure Gateway
{: #connector-and-secure-gateway}

{{site.data.keyword.satelliteshort}} Connector replaces Secure Gateway to provide connection between {{site.data.keyword.cloud_notm}} and your on-prem data center.
{: shortdesc}

## Terminology mapping
{: #secure-gateway-connector-terms}

See the following table to compare terminology between Secure Gateway and Satellite Connector.


| Secure Gateway | {{site.data.keyword.satelliteshort}} Connector | Notes |
| --- | --- | --- |
| Secure Gateways | {{site.data.keyword.satelliteshort}} Connector and Agents | Automatically created when you create a Satellite Connector. |
| Secure Gateway Client | {{site.data.keyword.satelliteshort}} Connector Agent | Satellite Connector is a containerized solution. |
| Secure Gateway Destination | {{site.data.keyword.satelliteshort}} Connector Endpoint | They are the same thing. |
| Secure Gateway API | {{site.data.keyword.satelliteshort}} Connector API | The constructs are similar. |
| Secure Gateway Endpoint | {{site.data.keyword.satelliteshort}} Connector API Endpoint | This term in Secure gateway refers to the API endpoint. |
| Secure Gateway Dashboard | {{site.data.keyword.satelliteshort}} Connector Endpoints page in cloud.ibm.com |  |
{: caption="Secure Gateway and {{site.data.keyword.satelliteshort}} Connector terminology." caption-side="bottom"}
{: #connector-sg-comp-table}

## Capabilities
{: #capability-comparison}

Satellite Connector offers a lot of the same capabilities as Secure Gateway and some additional features which provide a more seamless integration into IBM Cloud.
  
The following table highlights how capabilities are provided in Secure Gateway and {{site.data.keyword.satelliteshort}} Connector.


| Topic | Secure Gateway | {{site.data.keyword.satelliteshort}} Connector | Notes |
| --- | --- | --- | --- |
| Public internet access | Cloud side of a destination is exposed on a public IP address. | Cloud side of an endpoint is exposed only to the {{site.data.keyword.cloud_notm}} private endpoint network so that it's reachable only from within {{site.data.keyword.cloud_notm}}. | {{site.data.keyword.satelliteshort}} Connector Access Control List sets the access. |  
| Integrations | N/A | Integrated when you connect your {{site.data.keyword.satelliteshort}} Connector Agent location to Activity Tracker, LogDNA, and Sysdig. | The agent itself runs on a container platform that isn’t integrated into the {{site.data.keyword.cloud_notm}} tools. For example,  Docker won’t send logs to logDNA.  |  
| Client access | Secure Gateway Client supports Windows, Linux, Mac, Node.js module, and container. | {{site.data.keyword.satelliteshort}} Connector supports container.  |  |  
| Clients per instance | Limited to 4 client connections for high availability | For high availability support, use 3 clients. Up to 9 clients allowed to scale containers over time. |  |  
| Client requirements | See [Requirements to run the Client](/docs/SecureGateway?topic=SecureGateway-client-requirements). | - **Host type:** Most container hosts can run the client container image, including the Docker Community Edition.  \n - **Ports:** Only requires port 443 to be opened. | |  
| Encryption (TLS support) | TLS version supported is 1.2. Protocols supported are UDP, TCP, HTTP, and HTTPS. | TCP, TLS (version 1.3), HTTP, HTTPS, and HTTP Tunnel. No UDP support. |  |  
| Authentication | Mutual authentication is supported. | Provided by the target and can be configured with mutual authentication on the {{site.data.keyword.satelliteshort}} Connector parts. |  |  
| Load balancing and high availability | Can connect multiple instances of the Secure Gateway Service client to your gateway to automatically use built-in connection load balancing and connection fail-over if a client instance goes down. | Can connect multiple Connector agents to your connector instance in Cloud to automatically use built-in load balancing and connection failover if an container goes down. |
{: caption="Secure Gateway and {{site.data.keyword.satelliteshort}} Connector key differences." caption-side="bottom"}
{: #connector-capabilities-table}
