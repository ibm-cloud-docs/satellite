---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-15"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Understanding Link endpoints and {{site.data.keyword.satelliteshort}}
{: #link-location-cloud}

Open up {{site.data.keyword.satelliteshort}} endpoints in the {{site.data.keyword.satelliteshort}} control plane to control and audit network traffic between your {{site.data.keyword.satellitelong}} location and services, servers, or apps that run outside of the location.
{: shortdesc}

With {{site.data.keyword.satelliteshort}} Link endpoints, you can allow any client that runs in your {{site.data.keyword.satelliteshort}} location to connect to a service, server, or app that runs outside of the location, or allow a client that is connected to the {{site.data.keyword.cloud_notm}} private network to connect to a service, server, or app that runs in your location.
{: shortdesc}

To establish the connection, you must specify the destination resource's fully qualified domain name (FQDN) or IP address, port, the connection protocol, and any authentication methods in the endpoint. The endpoint is registered with the {{site.data.keyword.satelliteshort}} Link component of your location's {{site.data.keyword.satelliteshort}} control plane. To help you maintain enterprise security and audit compliance, {{site.data.keyword.satelliteshort}} Link additionally provides built-in controls to restrict client access to endpoints and to log and audit traffic that flows over endpoints.

## Architecture
{: #link-architecture}

You can create two types of endpoints, depending on your use case: a cloud endpoint, or a location endpoint.
{: shortdesc}

Cloud endpoint
:    Destination resource runs outside of the {{site.data.keyword.satelliteshort}} location. A cloud endpoint allows you to securely connect to a service, server, or app that runs outside of the location from a client within your {{site.data.keyword.satelliteshort}} location.

Location endpoint
:   Destination resource runs in the {{site.data.keyword.satelliteshort}} location. A location endpoint allows you to securely connect to a server, service, or app that runs in your {{site.data.keyword.satelliteshort}} location from a client that is connected to the {{site.data.keyword.cloud_notm}} private network.

Two {{site.data.keyword.satelliteshort}} Link components, the tunnel server and the connector, proxy network traffic over a secure TLS connection between cloud services and resources in your {{site.data.keyword.satelliteshort}} location. This Link tunnel serves as a communication path over the internet that uses the TCP protocol and port 443, and encrypts payloads via TLS. Three tunnels are created between the tunnel server in {{site.data.keyword.cloud_notm}} and the connector in the location's control plane worker nodes. This redundancy supports the three availability zones of your location, and helps ensure communication in the case of a single zone failure. However, the three tunnels are orchestrated together so that the client that uses a Link endpoint sees a single connection. For more information about the {{site.data.keyword.satelliteshort}} Link components, see the [Satellite architecture](/docs/satellite?topic=satellite-service-architecture#architecture).

### Cloud endpoint
{: #link-cloud-endpoint}

By default, source clients in your {{site.data.keyword.satelliteshort}} location cannot reach destination resources that run outside of the location because the destination resource's IP address is not routable from within the location. Review the following architecture diagram and steps, which demonstrate how {{site.data.keyword.satelliteshort}} Link enables communication from {{site.data.keyword.satelliteshort}} locations to services that run outside of locations through {{site.data.keyword.satelliteshort}} endpoints.

![Network traffic through {{site.data.keyword.satelliteshort}} Link.](/images/sat_link_cloud.png){: caption="Network traffic flow from a source in your {{site.data.keyword.satellitelong_notm}} location to a destination resource in {{site.data.keyword.cloud_notm}} through {{site.data.keyword.satelliteshort}} Link" caption-side="bottom"}

1. When you create an endpoint for your destination resource, a port is opened for the {{site.data.keyword.satelliteshort}} Link connector on your {{site.data.keyword.satelliteshort}} control plane worker nodes. Requests from sources in your {{site.data.keyword.satelliteshort}} location are made to the {{site.data.keyword.satelliteshort}} Link connector host name and the port, such as `nae4dce0eb35957baff66-edfc0a8ba65085c5081eced6816c5b9c-c000.us-east.satellite.appdomain.cloud:30819`. This Link host name and port are mapped to the destination resource's domain and port.

2. The {{site.data.keyword.satelliteshort}} Link connector forwards the request to the {{site.data.keyword.satelliteshort}} Link tunnel server on the {{site.data.keyword.satelliteshort}} control plane master over a secured TLS connection.

3. The {{site.data.keyword.satelliteshort}} Link tunnel server resolves the request to the destination's IP address and port, and forwards the request to the destination resource.

### Location endpoint
{: #link-location-endpoint}

By default, source clients that are connected to the {{site.data.keyword.cloud_notm}} private network cannot reach destination resources that run in your {{site.data.keyword.satelliteshort}} location because the destination resource's IP address is not routable from outside the location. Review the following architecture diagram and steps, which demonstrate how {{site.data.keyword.satelliteshort}} Link enables communication from services that are connected to the {{site.data.keyword.cloud_notm}} private network to locations through {{site.data.keyword.satelliteshort}} endpoints.

![Network traffic through {{site.data.keyword.satelliteshort}} Link.](/images/sat_link_location.png){: caption="Figure 2: Network traffic flow from an {{site.data.keyword.cloud_notm}} source to a destination resource in your location through {{site.data.keyword.satelliteshort}} Link" caption-side="bottom"}

1. When you create an endpoint for a resource that runs in your {{site.data.keyword.satelliteshort}} location, a port is opened on the {{site.data.keyword.satelliteshort}} Link tunnel server and added in the endpoint configuration. Requests from sources that are connected to the {{site.data.keyword.cloud_notm}} private network are made to the {{site.data.keyword.satelliteshort}} Link tunnel server host name and this port, such as `c-01.us-east.link.satellite.cloud.ibm.com:30819`. This Link host name and port are mapped to the destination resource's domain and port.

2. The {{site.data.keyword.satelliteshort}} Link tunnel server resolves the request to the {{site.data.keyword.satelliteshort}} Link connector host name and endpoint port, and forwards the request to the {{site.data.keyword.satelliteshort}} Link connector over a secured TLS connection.

3. The {{site.data.keyword.satelliteshort}} Link connector resolves the request to the destination's IP address and port, and forwards the request to the destination resource.

What happens if {{site.data.keyword.satelliteshort}} Link becomes unavailable?
:    Your on-location workloads continue to run independently even if the location's connectivity to {{site.data.keyword.cloud_notm}} is unavailable. However, if any applications use a Link endpoint to communicate with {{site.data.keyword.cloud_notm}}, communication between those apps and {{site.data.keyword.cloud_notm}} is disrupted. Additionally, any requested changes to your {{site.data.keyword.satelliteshort}} location, such as adding hosts or access control requests to {{site.data.keyword.IBM_notm}} services through {{site.data.keyword.iamshort}}, are disrupted. After connectivity is restored, logs and events are sent to your [{{site.data.keyword.la_full_notm}} and {{site.data.keyword.at_full_notm}} instances](/docs/satellite?topic=satellite-health). Note that {{site.data.keyword.satelliteshort}} Link depends on the underlying connectivity of your hosts' local network to monitor and maintain the managed services for your {{site.data.keyword.satelliteshort}} location.

## External network requirements and security
{: #link-security}

Your {{site.data.keyword.satelliteshort}} location infrastructure is a part of your local network (on-prem hosts) or the network of another cloud provider, but is managed remotely via secure access from {{site.data.keyword.cloud_notm}}. Review the following frequently asked questions about {{site.data.keyword.satelliteshort}} Link network security. For more information about all security options for {{site.data.keyword.satellitelong_notm}}, see [Security and compliance for {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-compliance).
{: shortdesc}

### Do I need to allow any unique inbound traffic from internet-facing ports through firewalls to my location?
{: #link-unique-inbound}

No. {{site.data.keyword.satelliteshort}} Link uses standard web security ports to originate encrypted communication from your location to {{site.data.keyword.cloud_notm}} for location management. {{site.data.keyword.satelliteshort}} creates unique public DNS entries for each location and assigns ports from the 32768 - 52768 range for TCP so that destination addresses can be predictably resolved by {{site.data.keyword.cloud_notm}}. Communication channels over Link endpoints between your {{site.data.keyword.satelliteshort}} location to {{site.data.keyword.cloud_notm}} are permitted through your [existing outbound firewall policies for hosts](/docs/satellite?topic=satellite-reqs-host-network).

### If {{site.data.keyword.IBM_notm}} owns the Link tunnel, how can I validate that our data is inaccessible? My organization's security policy does not allow tunnels from our networks.
{: #link-tunnel-data-inaccessible}

{{site.data.keyword.satelliteshort}} Link uses a zero-trust model; {{site.data.keyword.cloud_notm}} has no access to your workloads by default. Any management of infrastructure in your location that is initiated by {{site.data.keyword.IBM_notm}} Site Reliability Engineers over {{site.data.keyword.satelliteshort}} Link is isolated from your workloads and the network connections, such as the Link endpoints, that your workloads use. For more information about what kinds of access {{site.data.keyword.cloud_notm}} has to your {{site.data.keyword.satelliteshort}} location, see [{{site.data.keyword.IBM_notm}} operational access](/docs/satellite?topic=satellite-compliance#operational-access). For any other connections into your location that your applications require, you can use {{site.data.keyword.satelliteshort}} Link to create layer 4 communications by setting up an endpoint for each destination resource in your location. All connections through your endpoints are always under your control, including completely disabling endpoints.

### How do I make my data secure in transit?
{: #link-data-secure-transit}

Link endpoints between your location and {{site.data.keyword.cloud_notm}} are secured through two levels of encryption: high-security encryption from the location’s connector to {{site.data.keyword.cloud_notm}} that is provided by {{site.data.keyword.IBM_notm}} , and an optional additional encryption layer between the source and destination resources.

All data that is transported over {{site.data.keyword.satelliteshort}} Link is encrypted using TLS 1.3 standards. This level of encryption is managed by {{site.data.keyword.IBM_notm}}.

When you create an endpoint, you can optionally provide another level of encryption by specifying [data encryption protocols](#link-protocols) for the endpoint connection between the client source and destination resource. For example, even if the traffic is not encrypted on the source side, you can specify TLS encryption for the connection that goes over the internet. You can provide your own signed certificates to ensure both internal security and operational auditability without exposing any data contents. {{site.data.keyword.IBM_notm}} only transports the encrypted connection, and your resources must be configured for the data encryption protocols that you specify.

## Encryption protocols
{: #link-protocols}

All communication over {{site.data.keyword.satelliteshort}} Link is encrypted by {{site.data.keyword.IBM_notm}}. When you create an endpoint, you can optionally specify an additional data encryption protocol for the endpoint connection between the client source and destination resource. For example, even if the traffic is not encrypted on the source side, you can specify your own additional TLS encryption for the connection that goes over the internet. Note that your resources must be configured for the data encryption protocols that you specify.
{: shortdesc}

Review the following information about how {{site.data.keyword.satelliteshort}} Link handles each type of connection protocol.

If you use the {{site.data.keyword.satelliteshort}} console to create an endpoint, the destination protocol is inherited from the source protocol that you select. To specify a destination protocol, use the CLI to create an endpoint and include the `--dest-protocol` option in the `ibmcloud sat endpoint create` command.
{: note}

### TCP and TLS
{: #link-tcp-tls}

If your destination resource does not require requests with a specific HTTP or HTTPS host name header, or can accept direct requests to its IP address instead of its host name, use the TCP or TLS protocols. {{site.data.keyword.satelliteshort}} Link uses the same protocol as the request to transfer the request packet to the destination.

### HTTP and HTTPS
{: #link-http-https}

If your destination resource is configured to listen for a specific HTTP or HTTPS host name header, use the HTTP or HTTPS protocols. By using HTTP and HTTPS header remapping, {{site.data.keyword.satelliteshort}} Link is able to correctly route requests for multiple destination resources through TCP ports 80 (HTTP) and 443 (HTTPS).

Cloud endpoint
:   Source requests from your {{site.data.keyword.satelliteshort}} location to your destination resource that runs outside of the location contain an HTTP header such as `linkconnector_hostname:port`. When the request is sent from the {{site.data.keyword.satelliteshort}} Link connector to the {{site.data.keyword.satelliteshort}} Link tunnel server, the {{site.data.keyword.satelliteshort}} Link tunnel server changes the HTTP header on the request to the destination host name and port, such as `dest_hostname:dest_port`. The {{site.data.keyword.satelliteshort}} Link tunnel server then uses the destination's host name and port to forward the request to the correct destination resource.

Location endpoint
:    Source requests from clients that run outside the location to your destination resource in your {{site.data.keyword.satelliteshort}} location contain an HTTP header such as `linkserver_hostname:endpoint_port`. The {{site.data.keyword.satelliteshort}} Link tunnel server changes the HTTP header on the request to the destination host name and port, such as `dest_hostname:dest_port`, and sends the request to the {{site.data.keyword.satelliteshort}} Link connector. The {{site.data.keyword.satelliteshort}} Link connector then uses the destination's host name and port to send the request to the correct destination resource.

### HTTP tunnel
{: #link-http-tunnel}

When you want TLS connections to pass uninterruptedly from the source to your destination resource, such as to pass a certificate to the destination for mutual authentication, use the HTTP tunnel protocol.

The client source makes an HTTP connection request to the {{site.data.keyword.satelliteshort}} Link tunnel server or connector component, depending on whether the destination runs outside of the location or within your {{site.data.keyword.satelliteshort}} location. The Link component then makes the connection to the destination resource. After the initial connection is established, the Link component proxies the TCP connection between the source and destination uninterruptedly.

The {{site.data.keyword.satelliteshort}} Link component is not involved in TLS termination for encrypted traffic, so the destination resource must terminate the TLS connection. For example, if your destination resource requires mutual authentication, the HTTP tunnel protocol allows your client source to pass the required authentication certificate directly to the destination.

### Server-side certificate authentication for TLS and HTTPS
{: #link-server-side-cert}

If you select the TLS or HTTPS protocols, you can optionally require server-side verification of the destination's certificate. The certificate must be valid for the destination's host name and signed by a trusted Certificate Authority.

If your destination resource has a certificate, you do not need to provide the certificate when you create the endpoint. However, if you are testing access to a destination resource that is still in development and you do not have a trusted certificate yet, you can upload a self-signed certificate for verification. This `ssl.crt` file must contain the public, base-64 encoded certificate for your resource's host name and must not contain the private `ssl.key` certificate key. To create a self-signed certificate for testing purposes by using OpenSSL, see this [self-signed SSL certificate tutorial](https://www.akadia.com/services/ssh_test_certificate.html){: external}.

## Access and audit controls
{: #link-audit-about}

{{site.data.keyword.satelliteshort}} Link provides built-in controls to help you restrict which clients can access endpoints, and audit user-initiated events for Link endpoints.
{: shortdesc}

### Restricting access with source lists
{: #link-source-lists}

By default, after you set up an endpoint, any client can connect to the destination resource through the endpoint. For example, for a location endpoint, any client that is connected to the {{site.data.keyword.cloud_notm}} private network can use the endpoint to connect to the destination resource that runs in your {{site.data.keyword.satelliteshort}} location. To limit access to the destination resource, you can [specify a list of source IP ranges](/docs/satellite?topic=satellite-link-cloud-create#link-sources) so that only trusted clients can access the endpoint. Note that currently you can create source lists only for endpoints of type `location` and cannot create source lists for endpoints of type `cloud`.

### Auditing user-initiated events
{: #link-audit-events}

After you set up source lists for endpoints, you can configure auditing to monitor user-initiated events for Link endpoints. {{site.data.keyword.satellitelong_notm}} integrates with {{site.data.keyword.at_full}} to collect and send audit events for all link endpoints in your location to your {{site.data.keyword.at_short}} instance. To get started with auditing, see [Auditing events for endpoint actions](/docs/satellite?topic=satellite-link-cloud-monitor#link-audit).

## Use cases
{: #link-usecases}

Review the following list of general use cases and example use cases for {{site.data.keyword.satelliteshort}} Link endpoints.
{: shortdesc}

### Can I use Link endpoints to
{: #link-use-link-endpoints}



Connect resources within the same {{site.data.keyword.satelliteshort}} location?
:   No. Link endpoints cannot be created between resources in the same location. Instead, resources can access each other directly. For example, an app that runs in a {{site.data.keyword.redhat_openshift_notm}} cluster in {{site.data.keyword.satelliteshort}} does not need to communicate through {{site.data.keyword.satelliteshort}} Link to access a database that exists in the same location, and can instead access that database directly through the location's private network.

Expose apps or services that run in a {{site.data.keyword.redhat_openshift_notm}} cluster in {{site.data.keyword.satelliteshort}}?
:   To see available options, see [Exposing apps in {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-sat-expose-apps).

Bridge networks within the {{site.data.keyword.cloud_notm}} public network, such as VPC spanning?
:   No. Instead, use the bridging solution that is recommended for your network setup. For example, you might use a [{{site.data.keyword.vpn_vpc_full}}](/docs/vpc?topic=vpc-vpn-example) or [{{site.data.keyword.dl_full}}](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl#get-started-with-direct-link-connect).

Connect to other public clouds?
:   Yes. With {{site.data.keyword.satelliteshort}} Link, you can create `cloud` endpoints for resources that run in other public clouds.

### Example: Connect from a {{site.data.keyword.satelliteshort}} location to a service in another cloud provider
{: #link-example-connect-location}

You want to send data from a server that runs on a host in your {{site.data.keyword.satelliteshort}} location to a service that runs in Amazon Web Services. The service must be publicly accessible so that the {{site.data.keyword.satelliteshort}} Link tunnel, which terminates within the {{site.data.keyword.cloud_notm}} network, can access the service in the AWS network.

To establish this connection, you first create a `cloud` endpoint. You specify the service that runs in AWS as the destination resource. Then, the server on your on-location host connects directly to the host name of the {{site.data.keyword.satelliteshort}} Link connector on your location's control plane worker nodes. {{site.data.keyword.satelliteshort}} Link forwards this request to the cloud endpoint that you created for the service that runs in AWS.

### Example: Enable and audit limited access to a {{site.data.keyword.satelliteshort}} location from {{site.data.keyword.cloud_notm}}
{: #link-example-audit-limited-access}

You run a database in your {{site.data.keyword.satelliteshort}} location instead of in {{site.data.keyword.cloud_notm}}, because the database has legal requirements to run in your on-premises data center in a specific country. However, you still need to connect to the database in your {{site.data.keyword.satelliteshort}} location from the {{site.data.keyword.cloud_notm}} private network.

To establish this connection, you first create a `location` endpoint. You specify the database that runs in your {{site.data.keyword.satelliteshort}} location as the destination resource. Then, the client in the {{site.data.keyword.cloud_notm}} private network connects directly to the host name of the {{site.data.keyword.satelliteshort}} Link tunnel server. {{site.data.keyword.satelliteshort}} Link forwards this request to the location endpoint that you created for your on-location database.

Finally, to maintain enterprise security and audit compliance, you specify a list of source IP ranges so that only trusted clients in the public cloud can access your on-location database through the endpoint. Then, you set up an {{site.data.keyword.at_full_notm}} instance so that audit logs can be automatically collected for all endpoints in your {{site.data.keyword.satelliteshort}} location.
