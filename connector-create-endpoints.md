---


copyright:
  years: 2024, 2024
lastupdated: "2024-06-13"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Creating and managing Connector endpoints
{: #connector-create-endpoints}

After deploying a [Connector agent](/docs/satellite?topic=satellite-run-agent-locally), you can create endpoints to manage the network between your {{site.data.keyword.satellitelong_notm}} location and services, servers, or apps that run outside of the location. For Connector, only **Location** endpoints are supported. Cloud endpoints are not available.
{: shortdesc}



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


## Creating an access control list rule for your endpoint
{: #create-connector-rule-console}
{: ui}

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click the Connector that you want to use create an endpoint.

1. On the **Connector overview** page, click the **Access control list** tab, then click **Create rule**.

1. On the ACL rule page, complete the following steps.
    1. Enter a **Rule name**.
    1. Enter the **IP addresses** of the clients that are allowed to connect to the endpoint. This value can be a single IP address, a CIDR block, or a comma-separated list. The value must be fully contained in the following CIDRs: 10.0.0.0/8, 161.26.0.0/16, 166.8.0.0/14, 172.16.0.0/12.

    If no rules are selected, any client that is connected to the IBM Cloud private network can use the endpoint to connect to the destination resource that runs in your location.
    {: important}

    1. **Optional**: Select which endpoints this rule can access in your location.

1. Click **Create**




