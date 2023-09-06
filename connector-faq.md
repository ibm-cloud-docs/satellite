---

copyright:
  years: 2023, 2023
lastupdated: "2023-09-06"

keywords: satellite, connector, faq, frequently asked questions

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.satelliteshort}} Connector FAQ
{: #connector-faq}

Does {{site.data.keyword.satelliteshort}} Connector support Cloud Endpoints?
:   At this time, {{site.data.keyword.satelliteshort}} Connector only supports On Location endpoints to access your resources on-prem from {{site.data.keyword.cloud_notm}}.
  
How can I restrict access to my endpoints?
:   You can set up ACL rules to restrict access to your endpoints. When you create your link endpoint, select an existing ACL rule or create a new ACL rule to control which clients can access location endpoint resources. If no ACL rule is selected, any client that is connected to the {{site.data.keyword.cloud_notm}} private network can use the endpoint to connect to the destination resource that runs in your location.
  
I created an ACL for my Connector, why doesn't it take effect?
:   Make sure you apply the ACL to the endpoint you want to use it against. 
  
What IAM permissions do I need for Connectors?
:   To create a Connector, you need **Administrator** Platform role for {{site.data.keyword.satelliteshort}}. To connect an Agent to an existing Connector, you need **Viewer** Platform role or **Reader** Service role for {{site.data.keyword.satelliteshort}}.

How many Connectors are supported per account per region?
:   You can have a maximum of 25 Connectors per account per region.

How many endpoints are supported per Connector?
:   You can have a maximum of 25 endpoints per Connector.

How many instances of Connector agent can I run?
:   As a best practice, run no more than 6 agents per Connector.

