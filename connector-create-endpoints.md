---


copyright:
  years: 2024, 2024
lastupdated: "2024-09-10"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Creating and managing Connector endpoints
{: #connector-create-endpoints}

After creating a [Connector](/docs/satellite?topic=satellite-create-connector&interface=cli) and deploying a [Connector agent](/docs/satellite?topic=satellite-run-agent-locally), you can create endpoints to manage the network between your {{site.data.keyword.satellitelong_notm}} location and services, servers, or apps that run outside of the location.
{: shortdesc}



## Creating endpoints from the CLI
{: #create-connector-endpoint-cli}
{: cli}

1. Run the following command to find the ID of your Satellite Connector.

    ```sh
    ibmcloud sat connector ls
    ```
    {: pre}

1. Run the command to create the endpoint.

    ```sh
    ibmcloud sat endpoint create --dest-hostname HOSTNAME --connector-id ID --dest-type location --dest-port PORT --name NAME --source-protocol PROTOCOL [--dest-protocol PROTOCOL] [--sni SNI]
    ```
    {: pre}

    `--connector-id ID`
    :    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

    `--dest-hostname HOSTNAME`
    :    Indicate the fully qualified domain name (FQDN) or the externally accessible IP address of the destination that you want to connect to. For `location` endpoints, this value must resolve from and be reachable from the control plane hosts for Satellite locations or where the agent runs for Satellite Connector.

    `--dest-port PORT`
    :    Provide the port that the destination resource listens on for incoming requests. Make sure that the port matches the destination protocol.

    `--dest-protocol PROTOCOL`
    :    Specify the destination's protocol. If you do not specify this option, the destination protocol is inherited from the source protocol. Available options: `TCP`, `TLS`.

    `--dest-type TYPE`
    :    Specify where the destination resource runs, either in IBM Cloud (`cloud`) or your Satellite location (`location`). Available options: `location`, `cloud`.

    `--name NAME`
    :    Provide a name for the endpoint.

    `--sni SNI`
    :    Specify the server name indicator, if you specify a `TLS` or `HTTPS` source protocol and want a separate hostname to be added to the TLS handshake.

    `--source-protocol PROTOCOL`
    :    Provide the protocol that the source uses to connect the destination resource. For more information, see [Encryption protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols). Available options: `TCP`, `TLS`, `HTTP`, `HTTPS`, `HTTP-tunnel`.

3. Verify that the endpoint is created.

    ```sh
    ibmcloud sat endpoint ls --connector-id ID
    ```
    {: pre}


4. If you are creating an on-cloud endpoint by specifying `--dest-type=cloud`, you must configure your location's Connector agent to expose the target port to requests from the source application. Follow the steps in [Configuring the agent port for on-cloud endpoints](#configure-connector-oncloud-port).


### Example commands for creating endpoints
{: #create-connector-endpoints-comm}

Example command to create an endpoint using HTTPS source and TLS destination protocols and specifying an SNI hostname.

```sh
ibmcloud sat endpoint create --dest-hostname server1.mydomain.com --connector-id A1B0CDefgHilQ11ubmVjdG1yOiJjb11hnTdlWSRE1dnZla1szbDBsZyI --dest-type location --dest-port 443 --name myendpoint --source-protocol HTTPS --dest-protocol TLS --sni mydomain.com
```
{: pre}

Example command to create an endpoint that uses TCP protocol. 

```sh
ibmcloud sat endpoint create --dest-hostname server1.mydomain.com --connector-id A1B0CDefgHilQ11ubmVjdG1yOiJjb11hnTdlWSRE1dnZla1szbDBsZyI --dest-type location --dest-port 443 --name myendpoint --source-protocol TCP --dest-protocol TCP
```
{: pre}

### Configuring the agent port for on-cloud endpoints
{: #configure-connector-oncloud-port}

On-cloud endpoints allow you to access {{site.data.keyword.cloud_notm}} resources outside of your {{site.data.keyword.satelliteshort}} location network. This includes any public or private service that is accessible from your {{site.data.keyword.satelliteshort}} account, such as an endpoint for {{site.data.keyword.cloud_notm}} IAM or a private {{site.data.keyword.cos_full_notm}} bucket.

Calls to an on-cloud endpoint from your source application are first directed to the local Connector agent running on your location. Before your source application can access the endpoint, however, you must ensure that the endpoint's target port, which the agent is listening on, is accessible to your source application.
For container platforms such as Docker or Rancher, this requires you to map the endpoint's target port to an appropriate port that is accessible to clients from outside of the container that the agent is running in. Follow the steps to complete this requirement.

1. Run the command to get the target port. In the output, find the port number listed in the **Address** column. 

    ```sh
    ibmcloud sat endpoints --connector-id <connector ID>
    ```
    {: pre}

    Example output. In this example, the target port number is `29998`.

    ```sh
    OK
    ID                                                              Name      Destination Type   Address      Status
    A1A1AAaaaAAaA11AAAAAaA11aaaAaaaAaA_aaaaa                        test123   cloud              TCP :29998   enabled
    ```
    {: screen}

2. Map the target port to the container using the steps appropriate for your container platform. For most platforms, including Docker and Rancher, you can use the `-p LISTENPORT:CONTAINERPORT` option when starting your Connector agent.

    For example, using Docker, the following command exposes port `8443` for the endpoint's container-internal target port `29998`.

    ```sh
    docker run -d --network host --platform linux/amd64 --env-file ~/agent/env-files/env.txt -v ~/agent/env-files:/agent-env-files icr.io/ibm/satellite-connector/satellite-connector-agent:latest -p 8443:29998
    ```
    {: pre}

    You may also need to work with your container platform and operating system to allow outside traffic into your chosen `LISTENPORT`. In the previous example, the `LISTENPORT` is `8443`.

## Creating an access control list (ACL) rule from the CLI
{: #create-connector-rule-cli}
{: cli}

1. Run the following command to create an ACL rule for one or more subnets and optionally bound to one or more endpoints.

    ```sh
    ibmcloud sat acl create --name NAME --connector-id ID --subnet SUBNET [--subnet SUBNET ...] [--endpoint ENDPOINT ...]
    ```
    {: pre}

    `--connector-id ID`
    :    The ID of the Satellite connector. To find the connector ID, run `ibmcloud sat connector ls`.

    `--name NAME`
    :   The name for the ACL.

    `--subnet SUBNET`
    :   An IP or CIDR block allowed by this ACL. Value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.

    `--endpoint ENDPOINT`
    :   A name or ID of an endpoint to enable for this ACL.

1. Verify that the ACL was created.

    ```sh
    ibmcloud sat acl ls --connector-id ID
    ```
    {: pre}

1. You can also add or remove endpoints or subnets for an existing ACL.

   ```sh
    ibmcloud sat acl endpoint add --connector-id ID --acl-id ID --endpoint ENDPOINT [--endpoint ENDPOINT ...]
    ibmcloud sat acl endpoint rm --connector-id ID --acl-id ID --endpoint ENDPOINT [--endpoint ENDPOINT ...]
    ibmcloud sat acl subnet add --connector-id ID --acl-id ID --subnet SUBNET [--subnet SUBNET ...]
    ibmcloud sat acl subnet rm --connector-id ID --acl-id ID --subnet SUBNET [--subnet SUBNET ...]
    ```
    {: codeblock}

### Example commands for creating ACLs
{: #create-connector-acls-comm}

Example command to create an ACL rule allowing subnet 10.123.76.192/26 access to endpoint 'myendpoint'.

```sh
ibmcloud sat acl create --connector-id A1B0CDefgHilQ11ubmVjdG1yOiJjb11hnTdlWSRE1dnZla1szbDBsZyI --name myrule --subnet 10.123.76.192/26 --endpoint myendpoint
```
{: pre}

This will produce output similar to the following.

```sh
OK
ACL created with ID A1B0CDefgHilQ11ubmVjdG1yOiJjb11hnTdlWSRE1dnZla1szbDBsZyI-Source-vbfea
```
{: screen}

Use the ID of the newly created ACL to run the following command to add a second subnet, 10.194.127.64/26, to the ACL.

```sh
ibmcloud sat acl subnet add --connector-id A1B0CDefgHilQ11ubmVjdG1yOiJjb11hnTdlWSRE1dnZla1szbDBsZyI --acl-id A1B0CDefgHilQ11ubmVjdG1yOiJjb11hnTdlWSRE1dnZla1szbDBsZyI-Source-vbfea --subnet 10.194.127.64/26
```
{: pre}

Run the following command to list ACLs. You will see that the `myrule` ACL now includes two subnets, 10.123.76.192/26 and 10.194.127.64/26.

```sh
ibmcloud sat acl ls --connector-id A1B0CDefgHilQ11ubmVjdG1yOiJjb11hnTdlWSRE1dnZla1szbDBsZyI
```
{: pre}

```sh
OK
ID                                                                      Name        Subnets                             Created
A1B0CDefgHilQ11ubmVjdG1yOiJjb11hnTdlWSRE1dnZla1szbDBsZyI-Source-vbfea   myrule      10.123.76.192/26,10.194.127.64/26   11 minutes ago
```
{: screen}



## Creating endpoints from the console
{: #create-connector-endpoint-console}
{: ui}

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


## Creating an access control list (ACL) rule for your endpoint
{: #create-connector-rule-console}
{: ui}

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click the Connector that you want to create an ACL rule for.

1. On the **Connector overview** page, click the **Access control list** tab, then click **Create rule**.

1. On the ACL rule page, complete the following steps.

    1. Enter a **Rule name**.
    1. Enter the **IP addresses** of the clients that are allowed to connect to the endpoint. This value can be a single IP address, a CIDR block, or a comma-separated list. The value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.

        If no rules are selected, any client that is connected to the IBM Cloud private network can use the endpoint to connect to the destination resource that runs in your location.
        {: important}

    1. Select the endpoint (or multiple endpoints) this rule should control access to in your location.

1. Click **Create**
