---

copyright:
  years: 2023, 2023
lastupdated: "2023-06-23"

keywords: satellite, connector

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Understanding {{site.data.keyword.satelliteshort}} Connector
{: #understand-connectors}

{{site.data.keyword.satelliteshort}} Connector provides secure TLS tunneling between applications and services that need to communicate in hybrid and multi-cloud environments.
{: shortdesc}

{{site.data.keyword.satelliteshort}} Connector is currently in Beta and has the following limitations: 1. No CLI support. 2. No billing or metering. 3. All {{site.data.keyword.satelliteshort}} Connector instances will be deleted before GA. 4. Some UI and behavior might change before GA as development continues during Beta.
{: beta}
  
A {{site.data.keyword.satelliteshort}} Connector is a deployment model which enables only the secure communications from {{site.data.keyword.cloud_notm}} to on-prem resources via a light weight container deployed on the client's Docker hosts. This option brings all the security and auditability of {{site.data.keyword.satelliteshort}} communication, but with much less resources required.
  
{{site.data.keyword.satelliteshort}} Connector allows hybrid cloud connectivity for edge devices needing persistent connectivity. It enables advertising of trusted services that are capable of establishing secure end-point connectivity. With {{site.data.keyword.satelliteshort}} Connector, you can maintain data sovereignty with on-premises applications and services while connecting securely over a public network interface.
  
Here are some key concepts for {{site.data.keyword.satelliteshort}} Connector.
  
Connector
:   A connector provides a secure connection between a specific remote location and {{site.data.keyword.cloud_notm}}.
  
Agent
:   Each connector needs an agent running on your location to establish the connection.
  
Endpoint
:   An endpoint allows you to securely connect to a server, service, or app that runs in your {{site.data.keyword.satelliteshort}} location from a client that is connected to the {{site.data.keyword.cloud_notm}} private network.
  
Access control list
:   Access control list (ACL) controls which clients can access location endpoint resources. You can create ACL rules and use them to control which clients can use the endpoint to connect to the destination resource that runs in your location.


## Minimum requirements
{: #min-requirements}
  
To run the {{site.data.keyword.satelliteshort}} Connector agent image, your computing environment must meet the following minimum requirements:
- CPU: 0.40
- Memory: 500M
- Container platform must be on x86 architecture

These minimum requirements are for running the agent image only and exclude what's needed to run the container platform.
{: note}

Your environment must also meet the following network connectivity requirements. 
- The {{site.data.keyword.satelliteshort}} Connector agent that runs in your environment needs public outbound connectivity to {{site.data.keyword.cloud_notm}}. This can be direct public access or via a proxy. There is no requirement for public inbound access. See the [FAQ](#connector-faq) for more information about using a proxy. The list of endpoints, including URLs and IP Addresses, that must be outbound accessible depends on the region you are using. To see the list of endpoints for your region, choose the relevant page from the following list and review the **Allow Link connectors to connect to the Link tunnel server endpoint** section.
    - [RHCOS enabled locations in Dallas](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-dal) 
    - [RHCOS enabled locations in Frankfurt](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-fra) 
    - [RHCOS enabled locations in London](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-lon) 
    - [RHCOS enabled locations in Osaka](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-osa) 
    - [RHCOS enabled locations in Sao Paulo](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-sao) 
    - [RHCOS enabled locations in Sydney](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-syd) 
    - [RHCOS enabled locations in Tokyo](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-tok) 
    - [RHCOS enabled locations in Toronto](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-tor) 
    - [RHCOS enabled locations in Washington D.C.](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-wdc) 
- To pull the {{site.data.keyword.satelliteshort}} Connector agent image, you must allow hosts to communicate with {{site.data.keyword.registrylong_notm}}.
    - Destination IP addresses: N/A 
    - Destination hostnames: `icr.io` 
    - Protocol and ports: HTTPS 443 

  
## {{site.data.keyword.satelliteshort}} Connector and Secure Gateway
{: #replace-secure-gateway}

{{site.data.keyword.satelliteshort}} Connector replaces Secure Gateway to provide connection between {{site.data.keyword.cloud_notm}} and your on-prem data center.
{: shortdesc}

If you are familiar with Secure Gateway, see the following table to understand the relevant terminology in {{site.data.keyword.satelliteshort}} Connector.
| Secure Gateway | {{site.data.keyword.satelliteshort}} Connector | Notes |
| --- | --- | --- |
| Secure Gateways | {{site.data.keyword.satelliteshort}} Connector and Agents | Automatically created when you create a Satellite Connector. |
| Secure Gateway Client | {{site.data.keyword.satelliteshort}} Connector Agent | Satellite Connector is a containerized solution. |
| Secure Gateway Destination | {{site.data.keyword.satelliteshort}} Connector Endpoint | They are the same thing. |
| Secure Gateway API | {{site.data.keyword.satelliteshort}} Connector API | The constructs are similar. |
| Secure Gateway Endpoint | {{site.data.keyword.satelliteshort}} Connector API Endpoint | This term in Secure gateway refers to the API endpoint. |
| Secure Gateway Dashboard | {{site.data.keyword.satelliteshort}} Connector Endpoints page in cloud.ibm.com |  |
{: caption="Secure Gateway and {{site.data.keyword.satelliteshort}} Connector terminology." caption-side="bottom"}
  
The following table highlights the key differences between Secure Gateway and {{site.data.keyword.satelliteshort}} Connector.
| Topic | Secure Gateway | {{site.data.keyword.satelliteshort}} Connector | Notes |
| --- | --- | --- | --- |
| Public internet access | Cloud side of a destination is exposed on a public IP address. | Cloud Side of an endpoint is exposed only to the {{site.data.keyword.cloud_notm}} private endpoint network meaning it’s only reachable from within {{site.data.keyword.cloud_notm}}. | {{site.data.keyword.satelliteshort}} Connector Access Control List sets the access. |  
| Integrations | N/A | Integrated when you connect your {{site.data.keyword.satelliteshort}} Connector Agent location to Activity Tracker, LogDNA, and Sysdig. | The agent itself runs on a container platform that isn’t integrated into the {{site.data.keyword.cloud_notm}} tools. For example,  Docker won’t send logs to logDNA.  |  
| Client access | Secure Gateway Client supports Windows, Linux, Mac, Node.js module, and container. | {{site.data.keyword.satelliteshort}} Connector supports container. |  |  
| Clients per instance | Limited to 4 client connections for high availability | Recommended limit of 3 for high availability. However, you can create up to 9 clients. Note that running with 9 clients can causes issues, specifically when you delete and create a new client. If you do not allow time for the client that you deleted to be completely removed, your new client can fail. |  |  
| Client requirements | See [Requirements to run the Client](/docs/SecureGateway?topic=SecureGateway-client-requirements). | 1. **Host type:** Most container hosts can run the client container image, including the Docker Community Edition. \n 2. **Ports:** Port 443 only | |  
| Encryption (TLS support) | TLS version supported is 1.2. Protocols supported are UDP, TCP, HTTP, and HTTPS. | No UDP support.  |  |  
| Authentication | Mutual authentication is supported. | Provided by the target and can be configured with mutual authentication on the {{site.data.keyword.satelliteshort}} Connector parts. |  |  
| Load balancing and high availability | Can connect multiple instances of the Secure Gateway Service client to your gateway to automatically use built-in connection load balancing and connection fail-over if a client instance goes down. | Requires you to manually create more Docker containers. Must have 3 to ensure high availability.  |
{: caption="Secure Gateway and {{site.data.keyword.satelliteshort}} Connector key differences." caption-side="bottom"}
  
## {{site.data.keyword.satelliteshort}} Connector FAQ
{: #connector-faq}

Does {{site.data.keyword.satelliteshort}} Connector support Cloud Endpoints?
:   At this time, {{site.data.keyword.satelliteshort}} Connector only supports On Location endpoints to access your resources on-prem from {{site.data.keyword.cloud_notm}}.
  
How can I restrict access to my endpoints?
:   You can set up ACL rules to restrict access to your endpoints. When you create your link endpoint, select an existing ACL rule or create a new ACL rule to control which clients can access location endpoint resources. If no ACL rule is selected, any client that is connected to the {{site.data.keyword.cloud_notm}} private network can use the endpoint to connect to the destination resource that runs in your location.
  
I created an ACL for my connector, why doesn't it take effect?
:   Make sure you apply the ACL to the endpoint you want to use it against. 
  
What permissions do I need to connect my Agent to the Connector?
:   You need Satellite admin access to be able to see or use {{site.data.keyword.satelliteshort}} Connectors.
  
When does support for Secure Gateway end?
:   Secure Gateway runs on Cloud Foundry and will be deprecated when Cloud foundry reaches end of support. For more information, see [Deprecation of IBM Cloud Foundry](/docs/cloud-foundry-public?topic=cloud-foundry-public-deprecation).



## Next steps
{: #connector-understand-next-steps}

- [Running a Connector agent](/docs/satellite?topic=satellite-run-agent-locally)
- [Running your Connector agent as a service in Docker Swarm Mode for high availability](/docs/satellite?topic=satellite-run-agent-swarm)
- [{{site.data.keyword.satelliteshort}} Connector end-to-end example](/docs/satellite?topic=satellite-end-to-end)



