---

copyright:
  years: 2023, 2023
lastupdated: "2023-07-05"

keywords: satellite, hybrid, multicloud, connector, create connector

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Creating a Connector
{: #create-connector}

A Connector provides a secure connection between a specific remote location and {{site.data.keyword.cloud_notm}}. Use the {{site.data.keyword.satelliteshort}} console to create your Connector.
{: shortdesc}

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Connectors**.

1. Click **Create connector**.

1. For **Name**, enter a unique name for your Connector. The default value is `connector`.

1. For **Tags**, enter one or more tags that can help you organize resources throughout your account by filtering by tags from your resource list. This field is optional. For more information, see [Working with tags](/docs/account?topic=account-tag).

1. For **Resource group**, assign a resource group to your connector. The default value is `Default`.

1. For **Description**, describe what this Connector is used for. This field is optional.

1. For the **Managed from** menu, select the {{site.data.keyword.cloud_notm}} region that is closest to where your infrastructure resides to ensure low network latency.

1. In the **Summary** panel, review your order details, and then click **Create connector**. 


## Next steps
{: #connector-next-steps}

After creating a Connector, you must set up an agent to complete your set up.

- Set up a Connector agent. For more information, see [Running a Connector agent](/docs/satellite?topic=satellite-run-agent-locally).
- [Run the agent image in Swarm mode](/docs/satellite?topic=satellite-run-agent-swarm) for high-availability.
- [Follow the end-to-end example](/docs/satellite?topic=satellite-end-to-end) to run the agent, configure your endpoints, and add TLS support.




