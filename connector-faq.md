---


copyright:
  years: 2023, 2024
lastupdated: "2024-08-01"

keywords: satellite, connector, faq, frequently asked questions

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.satelliteshort}} Connector FAQ
{: #connector-faq}

## Does {{site.data.keyword.satelliteshort}} Connector support Cloud Endpoints?
{: #connector-faq-endpoints}

Yes, {{site.data.keyword.satelliteshort}} Connector supports both Location and Cloud endpoints. For more information, see [Creating Connector endpoints in the CLI](/docs/satellite?topic=satellite-connector-create-endpoints&interface=cli).
  
## How can I restrict access to my endpoints?
{: #connector-faq-restrict}

You can set up ACL rules to restrict access to your endpoints. When you create your link endpoint, select an existing ACL rule or create a new ACL rule to control which clients can access location endpoint resources. If no ACL rule is selected, any client that is connected to the {{site.data.keyword.cloud_notm}} private network can use the endpoint to connect to the destination resource that runs in your location.
  
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


