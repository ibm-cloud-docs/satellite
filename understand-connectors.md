---

copyright:
  years: 2023, 2023
lastupdated: "2023-07-05"

keywords: satellite, connector

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.satelliteshort}} Connector overview
{: #understand-connectors}

{{site.data.keyword.satelliteshort}} Connector provides secure TLS tunneling between applications and services that need to communicate in hybrid and multi-cloud environments.
{: shortdesc}
  
A {{site.data.keyword.satelliteshort}} Connector is a deployment model that enables only the secure communications from {{site.data.keyword.cloud_notm}} to on-prem resources with a light weight container, deployed on the client's container platform hosts, such as Docker hosts. This option brings all the security and auditability of {{site.data.keyword.satelliteshort}} communication, but with much less resources required.
  
{{site.data.keyword.satelliteshort}} Connector allows hybrid cloud connectivity for edge devices needing persistent connectivity. It enables advertising of trusted services that are capable of establishing secure end-point connectivity. With {{site.data.keyword.satelliteshort}} Connector, you can maintain data sovereignty with on-premises applications and services while connecting securely over a public network interface.
  
Here are some key concepts for {{site.data.keyword.satelliteshort}} Connector.
  
Connector
:   A connector provides a secure connection between a specific remote location and {{site.data.keyword.cloud_notm}}.
  
Agent
:   Each connector needs an agent running on your location to establish the connection.
  
Endpoint
:   An endpoint allows you to securely connect to a server, service, or app that runs in your {{site.data.keyword.satelliteshort}} location from a client that is connected to the {{site.data.keyword.cloud_notm}} private network.
  
Access control list
:   Access control list (ACL) controls which clients can access location endpoint resources. You can create ACL rules and use them to control which clients can use the endpoint to connect to the destination resource that runs in your location.


## Minimum requirements
{: #min-requirements}

These minimum requirements are for running the agent image only and exclude what's needed to run the container platform.
{: note}
  
To run the {{site.data.keyword.satelliteshort}} Connector agent image, your computing environment must meet the following minimum requirements.
- CPU: 0.40
- Memory: 500M
- Container platform must be on x86 architecture.
- The Connector agent image is for amd64 architecture and only runs on amd64 hardware or hardware that can emulate amd64. If you are on a Mac with Apple Silicon (arm64), the image will work if Rosetta is installed. If Rosetta is not installed on your Mac, you can install it via the `softwareupdate --install-rosetta` command.

Your environment must also meet the following network connectivity requirements. 
- The {{site.data.keyword.satelliteshort}} Connector agent that runs in your environment needs public outbound connectivity to {{site.data.keyword.cloud_notm}}. This can be direct public access or via a proxy. There is no requirement for public inbound access. See the [FAQ](#connector-faq) for more information about using a proxy. The list of endpoints, including URLs and IP Addresses, that must be outbound accessible depends on the region you are using. To see the list of endpoints for your region, choose the relevant page from the following list and review the **Allow Link connectors to connect to the Link tunnel server endpoint** section.
    - [RHCOS enabled locations in Dallas](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-dal#link-connector-dal) 
    - [RHCOS enabled locations in Frankfurt](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-fra#link-connector-fra) 
    - [RHCOS enabled locations in London](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-lon#link-connector-lon) 
    - [RHCOS enabled locations in Osaka](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-osa#link-connector-osa) 
    - [RHCOS enabled locations in Sao Paulo](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-sao#link-connector-sao) 
    - [RHCOS enabled locations in Sydney](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-syd#link-connector-syd) 
    - [RHCOS enabled locations in Tokyo](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-tok#link-connector-tok) 
    - [RHCOS enabled locations in Toronto](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-tor#link-connector-tor) 
    - [RHCOS enabled locations in Washington D.C.](/docs/satellite?topic=satellite-reqs-host-rhcos-outbound-wdc#link-connector-wdc) 
- To pull the {{site.data.keyword.satelliteshort}} Connector agent image, you must allow hosts to communicate with {{site.data.keyword.registrylong_notm}}.
    - Destination IP addresses: N/A 
    - Destination hostnames: `icr.io` 
    - Protocol and ports: HTTPS 443 




## Next steps
{: #connector-understand-next-steps}

- [Create a Connector](/docs/satellite?topic=satellite-create-connector)
- [Running a Connector agent](/docs/satellite?topic=satellite-run-agent-locally)
- [Running your Connector agent as a service in Docker Swarm Mode for high availability](/docs/satellite?topic=satellite-run-agent-swarm)
- [{{site.data.keyword.satelliteshort}} Connector end-to-end example](/docs/satellite?topic=satellite-end-to-end)



