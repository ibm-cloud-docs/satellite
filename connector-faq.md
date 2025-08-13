---


copyright:
  years: 2023, 2025
lastupdated: "2025-08-13"

keywords: satellite, connector, faq, frequently asked questions

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.satelliteshort}} Connector FAQ
{: #connector-faq}

## Does {{site.data.keyword.satelliteshort}} Connector support Cloud Endpoints?
{: #connector-faq-endpoints}

Yes, {{site.data.keyword.satelliteshort}} Connector supports both Location and Cloud endpoints. For more information, see [Creating Connector endpoints in the CLI](/docs/satellite?topic=satellite-connector-create-endpoints&interface=cli).
  
## How can I restrict access to my location endpoints?
{: #connector-faq-restrict}

You can set up ACL rules to restrict access to your endpoints. When you create your link endpoint, select an existing ACL rule or create a new ACL rule to control which clients can access location endpoint resources. If no ACL rule is selected, any client that is connected to the {{site.data.keyword.cloud_notm}} private network can use the endpoint to connect to the destination resource that runs in your location.

## How can I restrict the Connector Agent access on my network?
{: #connector-faq-agent-restrict}

Use any standard firewall in your location to restrict the endpoints on your network that are accessible to the Connector Agent. On Windows, for example, you can configure Windows firewall to allow the agent to only access the specific set of endpoints needed by your application. However, when using a firewall, make sure the firewall also allows access to the required endpoints described in [network requirements](/docs/satellite?topic=satellite-understand-connectors#network-requirements).

## I created an ACL for my Connector, why doesn't it take effect?
{: #connector-faq-acl-implementation}

Make sure you apply the ACL to the endpoint you want to use it against. 
  
## What IAM permissions do I need for Connectors?
{: #conector-faq-permissions}

To create a Connector, you need **Administrator** Platform role for {{site.data.keyword.satelliteshort}}. To connect an Agent to an existing Connector, you need **Viewer** Platform role for {{site.data.keyword.satelliteshort}}.

## How many Connectors are supported per account per region?
{: #connector-faq-con-per-region}

You can have a maximum of 25 Connectors per account per region.

## How many endpoints are supported per Connector?
{: #connector-faq-endpoints-per-conn}

`cloud` endpoints
:   1000 total. For example, you might create up to 650 TLS endpoints and 350 HTTP endpoints through which clients in your location can connect to resources outside of the location network.

`location` endpoints
:   25 total. For example, you might create up to 20 TLS endpoints and 5 HTTP endpoints through which clients outside of your location network can connect to resources inside the location.

## How many instances of Connector agent can I run?
{: #connector-faq-instance-limits}

As a best practice, run no more than 6 agents per Connector.

## Can I deploy {{site.data.keyword.satelliteshort}} Connectors within {{site.data.keyword.cloud_notm}}?
{: #connector-ibm-cloud}

Connectors are generally not recommended for use within {{site.data.keyword.cloud_notm}} except for specific use cases. Contact your IBM account team if you believe you need this support for your solution.

## What version of TLS does Connector use?
{: #conector-faq-tls}

Connector uses TLS version 1.3.




## How does load balancing work across Satellite connector?
{: #conector-faq-lbs}

The TCP streams flowing through Satellite Connector are round robin through the agents connected. Multiple TCP streams are required to take the most advantage of the agents.

## What are the failover or disaster recovery expectations of agents and Connector?
{: #conector-faq-dr}

IBM manages the Cloud side of the service which is split across IBM Cloud Multi-Zone Regions (MZRs) so the agents automatically have redundancy talking to different resources in each zone. For Connector agents, it is recommended that you deploy at least 3 in whatever availability zone fits your model. For example, 3 VMware hosts if you're on VMware or aligning to another cloud providers availability zones.

## How do I measure and monitor connections that are using Satellite Connector?
{: #conector-faq-connections}

There are many options to monitor Satellite Connector and the things that use Satellite Connector. Specifically, you can use [IBM Cloud Monitoring](https://cloud.ibm.com/docs/monitoring) and [Cloud Logs](https://cloud.ibm.com/docs/cloud-logs) which can provide many metrics. The logs always have "flowlog" entries which are meant to track traffic using Satellite Connector. Agents are either containers or running on a Windows host and both of these platforms offer methods of observing traffic flowing through the Satellite Connector service.

## What happens when an agent goes down?
{: #conector-faq-recover}

The tunnel and agent have recurring heartbeat message. When an agent goes down, the tunnel will drop the connection to the agent and stop routing traffic through that agent. Existing live TCP streams will be interrupted but retries and reconnects use the other agents.
