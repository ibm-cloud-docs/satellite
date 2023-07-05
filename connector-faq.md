---

copyright:
  years: 2023, 2023
lastupdated: "2023-07-05"

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
  
I created an ACL for my connector, why doesn't it take effect?
:   Make sure you apply the ACL to the endpoint you want to use it against. 
  
What permissions do I need to connect my Agent to the Connector?
:   You need Satellite admin access to be able to see or use {{site.data.keyword.satelliteshort}} Connectors.
  
When does support for Secure Gateway end?
:   Secure Gateway runs on Cloud Foundry and will be deprecated when Cloud foundry reaches end of support. For more information, see [Deprecation of IBM Cloud Foundry](/docs/cloud-foundry-public?topic=cloud-foundry-public-deprecation).


