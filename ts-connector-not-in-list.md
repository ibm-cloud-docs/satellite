---


copyright:
  years: 2023, 2025
lastupdated: "2025-04-22"

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

- There is a small delay (approximately several seconds) before a running Connector agent shows up in the list.

- Make sure the Connector ID your agent is using matches the Connector ID you are connecting to.

- Make sure the API Key is valid and has the correct permissions to the Connector instance. 

- Check the logs of the agent to determine if there are any errors.

- Make sure the Connector service and processes are running (Windows agent only).
  - From the **Services** control panel, all services with a name that starts with `SatelliteConnectorService` or `Satellite Connector Service` (< 1.2.0) must have the `Running` status and the `Automatic` startup type.
  - From TaskManager, verify that there are two `Node.js JavaScript Runtime(32 bit)` processes and one `Windows Service Wrapper` process for each agent.
    
{: tsResolve}
