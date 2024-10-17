---


copyright:
  years: 2023, 2024
lastupdated: "2024-10-16"

keywords: satellite, hybrid, multicloud, connector, private tunnel

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Configuring the request path from your connector agent
{: #connector-agent-path}

You can configure the Satellite Tunnel server Ingress host that your Connector agent will forward traffic to. The Connector agent supports specifying either public or private Ingress hosts. For example, to help with compliance management, you can specify an internal Ingress host. This will ensure that the traffic between your Connector agent and Tunnel server stays in your private network and no traffic uses the public internet.
{: shortdesc}

This document shows how to configure the target destination for outgoing requests from your Connector agent. If you want to configure the agent to send its outgoing requests through a proxy server, refer to [Configuring a proxy for your Satellite Connector](/docs/satellite?topic=satellite-config-connector-proxy) instead.
{: note}

Internal Ingress hosts are regional with the format `d-{nn}-ws.private.{mzr}.link.satellite.cloud.ibm.com` (for example, a tunnel server private Ingress in us-south is `d-01-ws.private.us-south.link.satellite.cloud.ibm.com`).
These hosts resolve to Cloud Service Endpoint (CSE) IP addresses which are IBM reserved addresses in the 166.9.x.x range. By connecting the agent using these addresses, you can ensure all traffic to the Tunnel server will stay in IBM's internal private network.

Follow the steps to configure your Connector agent to use an internal Ingress host.

1. Review the [prereqs](/docs/satellite?topic=satellite-run-agent-locally#agent-prepreqs) for running a Connector agent.

1. If the Connector agent is unable to reach the CSE endpoint IP addresses of the tunnel server directly from your local network, you may need to create a relay through which to send the agent requests. For more information, refer to [creating a relay](/docs/satellite?topic=satellite-direct-link-tutorial#dl-create-coreos-relay).

1. Set the `SATELLITE_CONNECTOR_DIRECT_LINK_INGRESS` parameter along with your other required [agent parameters](/docs/satellite?topic=satellite-run-agent-locally#review-parameters). For example:

    ```txt
    SATELLITE_CONNECTOR_ID=U2F0ZWxsaXRlQ29ubmVjdG9yOiJjaHIwbDFnMjFyNzRlMzRqdDVkZyI
    SATELLITE_CONNECTOR_IAM_APIKEY=
    SATELLITE_CONNECTOR_DIRECT_LINK_INGRESS=d-01-ws.private.us-south.link.satellite.cloud.ibm.com
    SATELLITE_CONNECTOR_TAGS=test
    ```
    {: codeblock}

1. Follow the steps to run your Connector agent [on your container platform](/docs/satellite?topic=satellite-run-agent-locally#connector-agent-container-platform) or [on Windows](/docs/satellite?topic=satellite-run-agent-locally#run-agent-windows).
