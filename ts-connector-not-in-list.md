---

copyright:
  years: 2023, 2023
lastupdated: "2023-07-05"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my agent not showing up in the list of Active Agents?
{: #ts-connector-not-in-list}


My agent is running but it does not show up in the list of Active Agents in the UI.
{: tsSymptoms}

This can be caused by several reasons. Check the following list to determine the cause.
{: tsCauses}

- There is a small delay (approximately 2 minutes) before a running Connector agent shows up in the list. Ensure you’ve waited long enough for the Connector agent to show up in the list.

- Make sure the Connector ID your agent is using matches the Connector ID you are connecting to.

- Make sure the API Key is valid and has the right permissions to the Connector instance. 

- Check the logs of the agent to determine if there are any errors.

- Make sure the region specified for your Connector is accurate and matches one of the values in [Supported IBM Cloud regions](/docs/satellite?topic=satellite-sat-regions).
{: tsResolve}


