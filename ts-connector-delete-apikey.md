---

copyright:
  years: 2023, 2023
lastupdated: "2023-07-05"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I see Connector logs?
{: #ts-connector-delete-apikey}


When trying to view logs for your Connector agents, you see an error similar to the following.
{: tsSymptoms}

```sh
Failed to get configuration from API /v1/connectors/U2F0ZWxsaXRlQ29ubmVjdG9yOiJjaTExMGxpdzFwazluMGdybXUyMCI, region us-east, code: 401. IAM Error: "status code: 400. Provided API key could not be found.", API Error: "null"
```
{: screen}

If your API key is deleted while a Connector agent is running, the change is detected by the Connector agent after about 45 minutes. Without a valid API key, the Connector agent close the tunnel. As a result, any applications using an endpoint will receive a communication error.
{: tsCauses}

You must update your API key and restart the Connector agent before you can use the tunnel or endpoints again.
{: tsResolve}


