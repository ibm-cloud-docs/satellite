---


copyright:
  years: 2020, 2024
lastupdated: "2024-06-12"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Creating and managing Connector endpoints
{: #connector-create-endpoints}

After deploying a [Connector agent](/docs/satellite?topic=satellite-run-agent-locally), you can create endpoints to manage the network between your {{site.data.keyword.satellitelong_notm}} location and services, servers, or apps that run outside of the location. For Connector, only **Location** endpoints are supported. Cloud endpoints are not available.
{: shortdesc}

## Creating endpoints from the CLI
{: #create-connector-endpoint-cli}

1. Create and deploy a [connector agent](/docs/satellite?topic=satellite-create-connector&interface=cli) and note the corresponding Connector ID. 

2. Run the command to create the endpoint. For a Connector endpoint, specify the `--destination-type` as `location`.

```sh
ibmcloud sat endpoint create --dest-hostname HOSTNAME --connector-id ID --dest-type location --dest-port PORT  --name NAME --source-protocol PROTOCOL [--dest-protocol PROTOCOL] [--sni SNI] 
```
{: pre}

`--connector-id ID`
:    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat experimental connector ls`.

`--dest-hostname HOSTNAME`
:    Indicate the fully qualified domain name (FQDN) or the externally accessible IP address of the destination that you want to connect to. For `location` endpoints, this value must resolve from and be reachable from the control plane hosts for Satellite locations or where the agent runs for Satellite Connector.

`--dest-port PORT`
:    Provide the port that the destination resource listens on for incoming requests. Make sure that the port matches the destination protocol.

`--dest-protocol PROTOCOL`
:    Specify the destination's protocol. If you do not specify this option, the destination protocol is inherited from the source protocol. Available options: `TCP`, `TLS`.

`--dest-type TYPE`
:    Specify where the destination resource runs. For Connector endpoints, specify `location`.

`--name NAME`
:    Provide a name for the endpoint.

`--sni SNI`
:    Specify the server name indicator, if you specify a `TLS` or `HTTPS` source protocol and want a separate hostname to be added to the TLS handshake.

`--source-protocol PROTOCOL`
:    Provide the protocol that the source uses to connect the destination resource. For more information, see [Encryption protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols). Available options: `TCP`, `TLS`, `HTTP`, `HTTPS`, `HTTP-tunnel`.

3. Verify that the endpoint is created.

```sh
ibmcloud sat endpoint ls --connector-
```
{: pre}

4. Verify that traffic flows to the endpoint. 
    TBD


### Example commands for creating endpoints
{: #create-connector-endpoints-comm}

Example command to create an endpoint using HTTPS source and destination protocols and specifying an SNI. (ASK GERALD do you need hostname AND SNI?)

```sh
ibmcloud sat endpoint create --dest-hostname server1.mydomain.com --connector-id A1B0CDefgHilQ11ubmVjdG1yOiJjb11hnTdlWSRE1dnZla1szbDBsZyI --dest-type location --dest-port 443  --name myendpoint --source-protocol HTTPS --dest-protocol HTTPS --sni SNI mydomain.com
```
{: pre}

Example command to create an endpoint that uses TCP protocol. 

```sh
ibmcloud sat endpoint create --dest-hostname server1.mydomain.com --connector-id A1B0CDefgHilQ11ubmVjdG1yOiJjb11hnTdlWSRE1dnZla1szbDBsZyI --dest-type location --dest-port 443  --name myendpoint --source-protocol TCP --dest-protocol TCP
```
{: pre}




## Creating endpoints from the console
{: #create-connector-endpoint-console}
{: step}

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click the Connector that you want to use create an endpoint.

1. On the **Connector overview** page, click the **User endpoints** tab, then click **Create endpoint**.

1. On the **Resource details** page, enter the following details.

    1. Give your endpoint a name.
    1. Enter the fully qualified domain name or IP address of the destination resource that you want to connect to.
    1. Enter the port that your destination resource listens on for incoming requests.
    1. Click **Next**.

1. On the **Protocol** page, select the protocol that a source must use to connect to the destination FQDN or IP address. This protocol must match the port for your destination resource. For more information, see [Endpoint protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols).
    - If you selected the **TLS** or **HTTPS** protocols and want to require server-side authentication of the destination's certificate, select the **Verify destination certificate** checkbox.
    - If you selected the **TLS** or **HTTPS** protocols but the destination resource is still in development, you can click **Upload certificate** to add your self-signed certificate file. This `ssl.crt` file must contain the public, base-64 encoded certificate for your resource's host name and must not contain the private `ssl.key` certificate key. To create a self-signed certificate for testing purposes by using OpenSSL, see this [self-signed SSL certificate tutorial](https://www.akadia.com/services/ssh_test_certificate.html){: external}.
    - If you selected the **TLS** or **HTTPS** protocols and want to allow a separate hostname to be provided to the TLS handshake of the resource connection, enter the server name indicator (SNI).
1. On the **Access control lists** page, click **Create rule**.

1. **Optional**: On the **ACL rule** page, enter a **Rule name** and enter the IBM Cloud private **IP addresses** of the clients that will be allowed to connect to the endpoint. This value can be a single IP address, a CIDR block, or a comma-separated list. The value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.

    If no rules are selected, any client that is connected to the IBM Cloud private network can use the endpoint to connect to the destination resource that runs in your location.
    {: important}

1. On the **Connection settings** page, set an inactivity timeout between 1 and 600.
1. Click **Create endpoint**.


## Creating an access control list rule for your endpoint
{: #create-connector-rule-console}

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click the Connector that you want to use create an endpoint.

1. On the **Connector overview** page, click the **Access control list** tab, then click **Create rule**.

1. On the ACL rule page, complete the following steps.
    1. Enter a **Rule name**.
    1. Enter the **IP addresses** of the clients that are allowed to connect to the endpoint. This value can be a single IP address, a CIDR block, or a comma-separated list. The value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.

    If no rules are selected, any client that is connected to the IBM Cloud private network can use the endpoint to connect to the destination resource that runs in your location.
    {: important}

    1. **Optional**: Select which endpoints this rule can access in your location.

1. Click **Create**




