---


copyright:
  years: 2023, 2024
lastupdated: "2024-10-29"

keywords: satellite, hybrid, multicloud, connector, private tunnel

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Configuring the request path from your Connector agent
{: #connector-agent-path}

This document shows how to configure the target destination for outgoing requests from your Connector agent. You can configure the agent to send its outgoing requests through a proxy server and you can configure the Satellite Tunnel server Ingress host to which your Connector agent will forward traffic.
{: shortdesc}


## Configuring a proxy for your {{site.data.keyword.satelliteshort}} Connector agent
{: #config-connector-proxy}

You can use a forward proxy to establish the tunnel connection for your {{site.data.keyword.satelliteshort}} Connector agent.

There are various ways to set up a proxy. These instructions assume that you have a properly configured proxy, which is accessible from the machine running the Connector agent. The following instructions have been tested with the Connector agent machine running Ubuntu 22.04 with an explicit Squid proxy running on a different machine from the Connector agent.

The `HTTP_PROXY` and `HTTPS_PROXY` environment variables are used to redirect traffic to your proxy. For example:

```txt
HTTP_PROXY=http://my.proxy.example.com:3128
HTTPS_PROXY=https://my.proxy.example.com:3129
```
{: codeblock}

Setting these environment variables for your Connector agent to use depends on your runtime environment.

On a container platform, add them to your `env.txt` file.

```txt
SATELLITE_CONNECTOR_ID=U2.....wZyI
SATELLITE_CONNECTOR_IAM_APIKEY=/agent-env-files/apikey
SATELLITE_CONNECTOR_TAGS=test
HTTP_PROXY=http://192.168.3.87:3128
HTTPS_PROXY=https://192.168.3.87:3129
```
{: codeblock}

On Windows, add them to your `config.json` file.

```json
{
  "SATELLITE_CONNECTOR_ID": "U2.....wZyI",
  "SATELLITE_CONNECTOR_IAM_APIKEY": "C:\\path\\to\\apikey",
  "SATELLITE_CONNECTOR_TAGS": "test",
  "PRETTY_LOG": true,
  "HTTP_PROXY": "http://192.168.3.87:3128",
  "HTTPS_PROXY": "https://192.168.3.87:3129"
}
```
{: codeblock}

If your Connector agent is running on a container platform, ensure that your container platform runtime is also using the proxy when pulling images from `icr.io`. For more information, see [Configure the Docker daemon to use a proxy server](https://docs.docker.com/engine/daemon/proxy/#httphttps-proxy){: external}.
{: important}


## Configuring a Tunnel server Ingress host for your {{site.data.keyword.satelliteshort}} Connector agent
{: #config-connector-ingress}

You can configure the Satellite Tunnel server Ingress host that your Connector agent will forward traffic to. The Connector agent supports specifying either public or private Ingress hosts. To help with compliance management, you can configure a single network destination to reduce the number of outbound IP addresses to allow on your firewall. Also, by specifying an internal Ingress host, you can ensure that the traffic between your Connector agent and Tunnel server stays in your private network and no traffic uses the public internet.

### Specifying an internal host
{: #specify-host}


Internal Ingress hosts are regional with the format `d-{nn}-ws.private.{mzr}.link.satellite.cloud.ibm.com` (for example, a tunnel server private Ingress in us-south is `d-01-ws.private.us-south.link.satellite.cloud.ibm.com`).
These hosts resolve to Cloud Service Endpoint (CSE) IP addresses which are IBM reserved addresses in the 166.9.x.x range. By connecting the agent using these addresses, you can ensure all traffic to the Tunnel server will stay in IBM's internal private network.

Follow the steps to configure your Connector agent to use an internal Ingress host.

1. Review the [prereqs](/docs/satellite?topic=satellite-run-agent-locally#agent-prepreqs) for running a Connector agent.

1. If the Connector agent is unable to reach the CSE endpoint IP addresses of the tunnel server directly from your local network, you may need to create a relay through which to send the agent requests. For more information, refer to [creating a relay](/docs/satellite?topic=satellite-direct-link-tutorial#dl-create-coreos-relay).

1. Set the `SATELLITE_CONNECTOR_DIRECT_LINK_INGRESS` parameter along with your other required [agent parameters](/docs/satellite?topic=satellite-run-agent-locally#review-parameters). For example:

    On a container platform, in your `env.txt` file.

    ```txt
    SATELLITE_CONNECTOR_ID=U2.....wZyI
    SATELLITE_CONNECTOR_IAM_APIKEY=/agent-env-files/api-key
    SATELLITE_CONNECTOR_TAGS=test
    SATELLITE_CONNECTOR_DIRECT_LINK_INGRESS=d-01-ws.private.us-south.link.satellite.cloud.ibm.com
    ```
    {: codeblock}

    On Windows, in your `config.json` file.

    ```json
    {
      "SATELLITE_CONNECTOR_ID": "U2.....wZyI",
      "SATELLITE_CONNECTOR_IAM_APIKEY": "C:\\path\\to\\apikey",
      "SATELLITE_CONNECTOR_TAGS": "test",
      "PRETTY_LOG": true,
      "SATELLITE_CONNECTOR_DIRECT_LINK_INGRESS": "d-01-ws.private.us-south.link.satellite.cloud.ibm.com"
    }
    ```
    {: codeblock}

1. Follow the steps to run your Connector agent [on your container platform](/docs/satellite?topic=satellite-run-agent-locally#connector-agent-container-platform) or [on Windows](/docs/satellite?topic=satellite-run-agent-locally#run-agent-windows).

### Specifying a single network destination
{: #specify-dest}

You can specify a single network destination instead of multiple destinations to reduce the number of outbound IP addresses to allow on your firewall. You can limit traffic to only use the tunnel server Ingress IP addresses in a single region by setting the `SATELLITE_CONNECTOR_DIRECT_LINK_INGRESS` parameter to a specific regional host.

Public Ingress hosts are regional with the format `c-{nn}-ws.{mzr}.link.satellite.cloud.ibm.com` (for example, a tunnel server public Ingress in us-south is `c-01-ws.us-south.link.satellite.cloud.ibm.com`). To use the public Ingress in us-south, include the following parameter:

- On a container platform, in your `env.txt` file.

    ```txt
    SATELLITE_CONNECTOR_DIRECT_LINK_INGRESS=c-01-ws.us-south.link.satellite.cloud.ibm.com
    ```

- On Windows, in your `config.json` file.

    ```json
    "SATELLITE_CONNECTOR_DIRECT_LINK_INGRESS": "c-01-ws.us-south.link.satellite.cloud.ibm.com"
    ```

This setting will allow you to limit your firewall to only allow the us-south IP addresses (169.46.88.106, 169.61.31.178, 169.61.156.226) as described in [network requirements](/docs/satellite?topic=satellite-understand-connectors#network-requirements).
