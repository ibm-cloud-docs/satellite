---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-12"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}



# Connecting {{site.data.keyword.satelliteshort}} locations with external services using Link endpoints
{: #link-location-cloud}

Open up {{site.data.keyword.satelliteshort}} endpoints in the {{site.data.keyword.satelliteshort}} control plane to control network traffic between your {{site.data.keyword.satellitelong}} location and services, servers, or apps that run outside of the location.
{: shortdesc}

## About {{site.data.keyword.satelliteshort}} endpoints
{: #link-about}

With {{site.data.keyword.satelliteshort}} Link endpoints, you can allow any client that runs in your {{site.data.keyword.satelliteshort}} location to connect to a service, server, or app that runs outside of the location, or allow a client that is connected to the {{site.data.keyword.cloud_notm}} private network to connect to a service, server, or app that runs in your location.
{: shortdesc}

To establish the connection, you must specify the destination resource's URL or IP address and port, the connection protocol, and any authentication methods in the endpoint. The endpoint is registered with the {{site.data.keyword.satelliteshort}} Link component of your location's {{site.data.keyword.satelliteshort}} control plane.

### Architecture
{: #link-architecture}

You can create two types of endpoints, depending on your use case: a cloud endpoint, or a location endpoint.
{: shortdesc}

* **Cloud endpoint: Destination resource runs outside of the {{site.data.keyword.satelliteshort}} location.** A cloud endpoint allows you to securely connect to a service, server, or app that runs outside of the location from a client within your {{site.data.keyword.satelliteshort}} location.
* **Location endpoint: Destination resource runs in the {{site.data.keyword.satelliteshort}} location.** A location endpoint allows you to securely connect to a server, service, or app that runs in your {{site.data.keyword.satelliteshort}} location from a client that is connected to the {{site.data.keyword.cloud_notm}} private network.

Two {{site.data.keyword.satelliteshort}} Link components, the tunnel server and the connector, proxy network traffic over a secure TLS connection between cloud services and resources in your {{site.data.keyword.satelliteshort}} location. For more information about the {{site.data.keyword.satelliteshort}} Link components, see the [Satellite architecture](/docs/satellite?topic=satellite-service-architecture#architecture).

**Cloud endpoint**

By default, source clients in your {{site.data.keyword.satelliteshort}} location cannot reach destination resources that run outside of the location because the destination resource's IP address is not routable from within the location. Review the following architecture diagram and steps, which demonstrate how {{site.data.keyword.satelliteshort}} Link enables communication from {{site.data.keyword.satelliteshort}} locations to services that run outside of locations through {{site.data.keyword.satelliteshort}} endpoints.

<p>
<figure>
 <img src="/images/sat_link_cloud.png" alt="Network traffic flow from {{site.data.keyword.satelliteshort}} resources to an {{site.data.keyword.cloud_notm}} resource through an endpoint">
 <figcaption>Network traffic flow from a source in your {{site.data.keyword.satellitelong_notm}} location to a destination resource in {{site.data.keyword.cloud_notm}} through {{site.data.keyword.satelliteshort}} Link</figcaption>
</figure>
</p>

1. When you create an endpoint for your destination resource, a port is opened for the {{site.data.keyword.satelliteshort}} Link connector on your {{site.data.keyword.satelliteshort}} control plane worker nodes. Requests from sources in your {{site.data.keyword.satelliteshort}} location are made to the {{site.data.keyword.satelliteshort}} Link connector host name and the port, such as `nae4dce0eb35957baff66-edfc0a8ba65085c5081eced6816c5b9c-c000.us-east.satellite.appdomain.cloud:30819`. This Link host name and port are mapped to the destination resource's domain and port.

2. The {{site.data.keyword.satelliteshort}} Link connector forwards the request to the {{site.data.keyword.satelliteshort}} Link tunnel server on the {{site.data.keyword.satelliteshort}} control plane master over a secured TLS connection.

3. The {{site.data.keyword.satelliteshort}} Link tunnel server resolves the request to the destination's IP address and port, and forwards the request to the destination resource.

**Location endpoint**

By default, source clients that are connected to the {{site.data.keyword.cloud_notm}} private network cannot reach destination resources that run in your {{site.data.keyword.satelliteshort}} location because the destination resource's IP address is not routable from outside the location. Review the following architecture diagram and steps, which demonstrate how {{site.data.keyword.satelliteshort}} Link enables communication from services that are connected to the {{site.data.keyword.cloud_notm}} private network to locations through {{site.data.keyword.satelliteshort}} endpoints.

<p>
<figure>
 <img src="/images/sat_link_location.png" alt="Network traffic flow from {{site.data.keyword.cloud_notm}} resources to a {{site.data.keyword.satelliteshort}} resource through an endpoint">
 <figcaption>Network traffic flow from an {{site.data.keyword.cloud_notm}} source to a destination resource in your location through {{site.data.keyword.satelliteshort}} Link</figcaption>
</figure>
</p>

1. When you create an endpoint for a resource that runs in your {{site.data.keyword.satelliteshort}} location, a port is opened on the {{site.data.keyword.satelliteshort}} Link tunnel server and added in the endpoint configuration. Requests from sources that are connected to the {{site.data.keyword.cloud_notm}} private network are made to the {{site.data.keyword.satelliteshort}} Link tunnel server host name and this port, such as `c-01.us-east.link.satellite.cloud.ibm.com:30819`. This Link host name and port are mapped to the destination resource's domain and port.

2. The {{site.data.keyword.satelliteshort}} Link tunnel server resolves the request to the {{site.data.keyword.satelliteshort}} Link connector host name and endpoint port, and forwards the request to the {{site.data.keyword.satelliteshort}} Link connector over a secured TLS connection.

3. The {{site.data.keyword.satelliteshort}} Link connector resolves the request to the destination's IP address and port, and forwards the request to the destination resource.

### Security and encryption protocols
{: #link-protocols}

Link endpoints are secured through two levels of encryption: a default encryption from the connector to tunnel server, and an optional encryption between the client source and destination resources.
{: shortdesc}

By default, the {{site.data.keyword.satelliteshort}} Link connector forwards client requests to the {{site.data.keyword.satelliteshort}} Link tunnel server on the {{site.data.keyword.satelliteshort}} control plane master over a secured TLS connection. This level of encryption is managed by IBM.

When you create the endpoint, you can optionally provide another level of encryption by specifying data encryption protocols for the endpoint connection between the client source and destination resource. For example, even if the traffic is not encrypted on the source side, you can specify TLS encryption for the connection that goes over the Internet. IBM only transports the encrypted connection, and your resources must be configured for the data encryption protocols that you specify.

Review the following information about how {{site.data.keyword.satelliteshort}} Link handles each type of connection protocol.

If you use the {{site.data.keyword.satelliteshort}} console to create an endpoint, the destination protocol is inherited from the source protocol that you select. To specify a destination protocol, use the CLI to create an endpoint and include the `--dest-protocol` flag in the `ibmcloud sat endpoint create` command.
{: note}

**UDP, TCP, and TLS**

If your destination resource does not require requests with a specific HTTP or HTTPS host name header, or can accept direct requests to its IP address instead of its host name, use the UDP, TCP, or TLS protocols. {{site.data.keyword.satelliteshort}} Link uses the same protocol as the request to transfer the request packet to the destination.

**HTTP and HTTPS**

If your destination resource is configured to listen for a specific HTTP or HTTPS host name header, use the HTTP or HTTPS protocols. By using HTTP and HTTPS header remapping, {{site.data.keyword.satelliteshort}} Link is able to correctly route requests for multiple destination resources through TCP ports 80 (HTTP) and 443 (HTTPS).

* **Cloud endpoint**: Source requests from your {{site.data.keyword.satelliteshort}} location to your destination resource that runs outside of the location contain an HTTP header such as `linkconnector_hostname:nodeport`. When the request is sent from the {{site.data.keyword.satelliteshort}} Link connector to the {{site.data.keyword.satelliteshort}} Link tunnel server, the {{site.data.keyword.satelliteshort}} Link tunnel server changes the HTTP header on the request to the destination host name and port, such as `dest_hostname:dest_port`. The {{site.data.keyword.satelliteshort}} Link tunnel server then uses the destination's host name and port to forward the request to the correct destination resource.
* **Location endpoint**: Source requests from clients that run outside the location to your destination resource in your {{site.data.keyword.satelliteshort}} location contain an HTTP header such as `linkserver_hostname:endpoint_nodeport`. The {{site.data.keyword.satelliteshort}} Link tunnel server changes the HTTP header on the request to the destination host name and port, such as `dest_hostname:dest_port`, and sends the request to the {{site.data.keyword.satelliteshort}} Link connector. The {{site.data.keyword.satelliteshort}} Link connector then uses the destination's host name and port to send the request to the correct destination resource.

**HTTP tunnel**

When you want TLS connections to pass uninterruptedly from the source to your destination resource, such as to pass a certificate to the destination for mutual authentication, use the HTTP tunnel protocol.

The client source makes an HTTP connection request to the {{site.data.keyword.satelliteshort}} Link tunnel server or connector component, depending on whether the destination runs outside of the location or within your {{site.data.keyword.satelliteshort}} location. The Link component then makes the connection to the destination resource. After the initial connection is established, the Link component proxies the TCP connection between the source and destination uninterruptedly.

The {{site.data.keyword.satelliteshort}} Link component is not involved in TLS termination for encrypted traffic, so the destination resource must terminate the TLS connection. For example, if your destination resource requires mutual authentication, the HTTP tunnel protocol allows your client source to pass the required authentication certificate directly to the destination.

**Server-side certificate authentication for TLS, HTTPS, and HTTP tunnel**

If you select the TLS or HTTPS protocols, you can optionally require server-side verification of the destination's certificate. The certificate must be valid for the destination's host name and signed by a trusted Certificate Authority.

If your destination resource has a certificate, you do not need to provide the certificate when you create the endpoint. However, if you are testing access to a destination resource that is still in development and you do not have a trusted certificate yet, you can upload a self-signed certificate for verification. This file must contain the base-64 encoded certificate for your resource's host name and must not contain the certificate key. To create a self-signed certificate for testing purposes by using OpenSSL, see this [self-signed SSL certificate tutorial](https://www.akadia.com/services/ssh_test_certificate.html){: external}.

### Access controls
{: #link-audit-about}

{{site.data.keyword.satelliteshort}} Link provides built-in controls to help you restrict which clients can access endpoints.
{: shortdesc}

**Restricting access with source lists**

By default, after you set up an endpoint, any client can connect to the destination resource through the endpoint. For example, for a location endpoint, any client that is connected to the {{site.data.keyword.cloud_notm}} private network can use the endpoint to connect to the destination resource that runs in your {{site.data.keyword.satelliteshort}} location. To limit access to the destination resource, you can [specify a list of source IP ranges](#link-sources) so that only trusted clients can access the endpoint. Note that currently you can create source lists only for endpoints of type `location` and cannot create source lists for endpoints of type `cloud`.

### Example use cases
{: #link-usecases}

Review the following example use cases for {{site.data.keyword.satelliteshort}} Link endpoints.
{: shortdesc}

**Example: Connect from a {{site.data.keyword.satelliteshort}} location to a service in another cloud provider**

You want to send data from a server that runs on a host in your {{site.data.keyword.satelliteshort}} location to a service that runs in Amazon Web Services. The service must be publicly accessible so that the {{site.data.keyword.satelliteshort}} Link tunnel, which terminates within the {{site.data.keyword.cloud_notm}} network, can access the service in the AWS network.

To establish this connection, you first create a `cloud` endpoint. You specify the service that runs in AWS as the destination resource. Then, the server on your on-location host connects directly to the host name of the {{site.data.keyword.satelliteshort}} Link connector on your location's control plane worker nodes. {{site.data.keyword.satelliteshort}} Link forwards this request to the cloud endpoint that you created for the service that runs in AWS.

**Example: Enable limited access to a {{site.data.keyword.satelliteshort}} location from {{site.data.keyword.cloud_notm}}**

You run a database in your {{site.data.keyword.satelliteshort}} location instead of in {{site.data.keyword.cloud_notm}}, because the database has legal requirements to run in your on-premises data center in a specific country. However, you still need to connect to the database in your {{site.data.keyword.satelliteshort}} location from the {{site.data.keyword.cloud_notm}} private network.

To establish this connection, you first create a `location` endpoint. You specify the database that runs in your {{site.data.keyword.satelliteshort}} location as the destination resource. Then, the client in the {{site.data.keyword.cloud_notm}} private network connects directly to the host name of the {{site.data.keyword.satelliteshort}} Link tunnel server. {{site.data.keyword.satelliteshort}} Link forwards this request to the location endpoint that you created for your on-location database.

Finally, to maintain enterprise security, you specify a list of source IP ranges so that only trusted clients in the public cloud can access your on-location database through the endpoint.<lb staging>

**Looking to expose apps or services that run in an {{site.data.keyword.openshiftshort}} cluster in {{site.data.keyword.satelliteshort}}?** Instead of using Link endpoints, expose your apps by using Kubernetes-native service discovery options, such as {{site.data.keyword.openshiftshort}} routers. For more information, see []().
{: tip}</lb staging>

**Looking to connect resources within the same {{site.data.keyword.satelliteshort}} location?** Link endpoints cannot be created between resources in the same location. Instead, resources can access each other directly. For example, an app that runs in an {{site.data.keyword.openshiftshort}} cluster in {{site.data.keyword.satelliteshort}} does not need to communicate through {{site.data.keyword.satelliteshort}} Link to access a database that exists in the same location, and can instead access that database directly through the location's private network.
{: tip}

### Default Link endpoints for {{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satelliteshort}} location
{: #default-link-endpoints}

Default {{site.data.keyword.satelliteshort}} link endpoints are created for your location's control plane cluster and for any other {{site.data.keyword.satelliteshort}}-enabled services that you run in your location. These default {{site.data.keyword.satelliteshort}} link endpoints are accessible only from within the {{site.data.keyword.cloud_notm}} private network.
{: shortdesc}

* When you set up a location, a Link endpoint of type `location` is automatically created so that the {{site.data.keyword.satelliteshort}} control plane master can communicate with and make requested changes to your location's control plane cluster. This endpoint is named in the format `automated-control-plane-link-<locationID>-<10_digit_hash>`.
* When you set up a {{site.data.keyword.satelliteshort}}-enabled service in your location, such as a {{site.data.keyword.openshiftlong_notm}} cluster, a Link endpoint of type `location` is automatically created so that the master for the service cluster can communicate with {{site.data.keyword.cloud_notm}} and monitoring through IBM. This endpoint is named in the format `automated-control-plane-link-<clusterID>-<10_digit_hash>`.

Do not disable the automated control plane endpoints. Because these endpoints connect your location to {{site.data.keyword.cloud_notm}}, they cannot be removed.
{: important}<lb staging>

For more information about what kinds of access {{site.data.keyword.cloud_notm}} has to your {{site.data.keyword.satelliteshort}} location, see [link tbd](TBD).</lb staging>

<br />

## Creating `cloud` endpoints to connect to resources outside of the location
{: #link-cloud}

Create an endpoint of type `cloud` so that sources in your {{site.data.keyword.satelliteshort}} location can connect to a service, server, or app that runs outside of the location.
{: shortdesc}

**Before you begin**, ensure that you have the following:
* Source client: A {{site.data.keyword.satelliteshort}} cluster or a host that you attached to your location. For more information about how to create a {{site.data.keyword.satelliteshort}} cluster, see [Creating {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters). To attach a host to your location, see [Attaching hosts to your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-hosts#attach-hosts). Make sure that you do not assign the host to the {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.satelliteshort}} cluster after you attached the host. Assigning the host starts a bootstrapping process that removes SSH access to your host.
* Destination resource: A service, server, or app that runs outside of the location but that is accessible from within {{site.data.keyword.cloud_notm}}. For example, you can use the private service endpoint for an {{site.data.keyword.cloud_notm}} service, because that private service endpoint is routable from within the {{site.data.keyword.cloud_notm}} network. If you want to connect to a service that runs outside of {{site.data.keyword.cloud_notm}}, this service must be accessible from within the {{site.data.keyword.cloud_notm}} network.
* Permissions: The [**Administrator** {{site.data.keyword.cloud_notm}} IAM platform role](/docs/satellite?topic=satellite-iam) for the **Location** resource in {{site.data.keyword.satellitelong_notm}}.

### Creating cloud endpoints by using the console
{: #link-cloud-ui}

Use the console to create a cloud endpoint so that sources in your {{site.data.keyword.satelliteshort}} location can connect to a service, server, or app that runs outside of the location.
{: shortdesc}

1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations), click the name of your location.
2. From the **Overview** tab, verify that your location has a **normal** status.
3. From the **Link endpoints** tab, click **Create an endpoint**.
4. Select **Cloud** to create an endpoint for a service, server, or app that runs outside of the location.
5. Enter an endpoint name, the destination resource's URL or IP address, and the port that your destination resource listens on for incoming requests. Make sure to enter the URL without `http://` or `https://`. The IP address or hostname must resolve to a public IP address or to a private IP address that is accessible within {{site.data.keyword.cloud_notm}}, such as a private service endpoint.
6. Select the protocol that a source must use to connect to the destination URL or IP address. This protocol must match the port for your destination resource. For more information, see [Endpoint protocols](#link-protocols).
  * If you selected the **TLS** or **HTTPS** protocols and want to require server-side authentication of the destination's certificate, select the **Verify destination certificate** checkbox.
  * If you selected the **TLS** or **HTTPS** protocols but the destination resource is still in development, you can click **Upload certificate** to add your self-signed, base-64 encoded certificate file.
7. Configure optional connection settings, such as enabling compression of data that is transferred through the endpoint or setting an inactivity timeout. The inactivity timeout is applied to both the connection between the source and {{site.data.keyword.satelliteshort}} Link and to the connection between {{site.data.keyword.satelliteshort}} Link and the destination. By default, no default inactivity timeout is set.
8. Click **Create**. Wait a few minutes for the {{site.data.keyword.satelliteshort}} Link connector component to assign a port to your endpoint.
9. In the table row for your endpoint, copy the host name for your {{site.data.keyword.satelliteshort}} Link connector and the port for your endpoint in the **Address** field.
10. Use the address to [connect to your destination from a source in your location](#link-cloud-test).

### Creating cloud endpoints by using the CLI
{: #link-cloud-cli}

Use the CLI to create an endpoint so that sources in your {{site.data.keyword.satelliteshort}} location can connect to a service, server, or app that runs outside of the location.
{: shortdesc}

1. Get the ID of your {{site.data.keyword.satelliteshort}} location and verify that your location has a **normal** status.
   ```
   ibmcloud sat location ls
   ```
   {: pre}

   Example output:
   ```
   Name               ID                     Status   Ready   Created       Hosts (used/total)   Managed From
   port-antwerp       brlono42051up3k4htu0   normal   yes     2 weeks ago   6 / 7                London
   ```
   {: screen}

2. Create a `cloud` endpoint.
   ```
   ibmcloud sat endpoint create --location <location_ID> --endpoint <endpoint_name> --dest-type cloud --dest-hostname <hostname_or_IP> --dest-port <port> [--dest-protocol <destination_protocol>] --source-protocol <source_protocol>
   ```
   {: pre}

   <table summary="This table is read from left to right. The first column has the command component. The second column has the description of the component".>
    <caption>Understanding the API request</caption>
      <thead>
      <th>Component</th>
      <th>Description</th>
      </thead>
      <tbody>
      <tr>
      <td><code>--location &lt;location_ID&gt;</code></td>
      <td>Enter the ID of your {{site.data.keyword.satelliteshort}} location that you retrieved earlier.</td>
      </tr>
      <tr>
      <td><code>--endpoint &lt;endpoint_name&gt;</code></td>
      <td>Enter a name for your {{site.data.keyword.satelliteshort}} endpoint. </td>
      </tr>
      <tr>
      <td><code>--dest-type cloud</code></td>
      <td>Enter `cloud` to indicate that the destination resource runs outside of the location.</td>
      </tr>
      <tr>
      <td><code>--dest-hostname &lt;hostname_or_IP&gt;</code></td>
      <td>Enter the URL or the externally accessible IP address of the destination that you want to connect to, such as a public IP address, a public service endpoint, or a private service endpoint. Note that you cannot specify a private IP address. Make sure to enter the URL without <code>http://</code> or <code>https://</code>. </td>
      </tr>
      <tr>
      <td><code>--dest-port &lt;port&gt;</code></td>
      <td>Enter the port that destination resource listens on for incoming requests. Make sure that the port matches the destination protocol.</td>
      </tr>
      <tr>
      <td><code>--dest-protocol &lt;destination-protocol&gt;</code></td>
      <td>Optional: Enter the protocol of the destination resource. If you do not specify this flag, the destination protocol is inherited from the source protocol. Supported protocols include <code>tcp</code>, <code>udp</code>, <code>tls</code>, <code>http</code>, <code>https</code>, and <code>http-tunnel</code>. For more information, see [Endpoint protocols](#link-protocols).</td>
      </tr>
      <tr>
      <td><code>--source-protocol &lt;source-protocol&gt;</code></td>
      <td>Enter the protocol that the source must use to connect to the destination resource. Supported protocols include <code>tcp</code>, <code>udp</code>, <code>tls</code>, <code>http</code>, <code>https</code>, and <code>http-tunnel</code>. For more information, see [Endpoint protocols](#link-protocols).</td>
      </tr>
      </tbody>
    </table>

3. Wait a few minutes for the {{site.data.keyword.satelliteshort}} Link connector component to assign a port to your endpoint.

4. Verify that your endpoint is created. In the output, copy the host name for your {{site.data.keyword.satelliteshort}} Link connector and the port for your endpoint in the **Address** field.
   ```
   ibmcloud sat endpoint ls --location <location_ID>
   ```
   {: pre}

5. Use the address to [connect to your destination from a source in your location](#link-cloud-test).

### Testing connections through cloud endpoints
{: #link-cloud-test}

Use the {{site.data.keyword.satelliteshort}} Link connector host name and port that are assigned to your endpoint to connect to your destination resource from a source in your location. The source can be a {{site.data.keyword.satelliteshort}} cluster that you previously created or a host that you assigned to your location.
{: shortdesc}

**Example for testing the connection from an unassigned host**:
1. Log in to your host. Enter the password to access your host when prompted.
 ```
 ssh root@<ip_address>
 ```
 {: pre}

2. Use the {{site.data.keyword.satelliteshort}} Link connector host name and port to test the connection to your destination resource.
 ```
 curl http://<linkconnector_hostname>:<nodeport>
 ```
 {: pre}
</br>

**Example for testing the connection from a {{site.data.keyword.satelliteshort}} cluster**:
1. Target your cluster. If you are not connected to your location host network, include the `--endpoint link` flag.
 ```
 ibmcloud oc cluster config --cluster <cluster_name> --admin [--endpoint link]
 ```
 {: pre}

2. Deploy a sample app to your cluster. To test the connection from your location to your endpoint, you must be connected to the network that your {{site.data.keyword.satelliteshort}} cluster is connected to. You can connect to the network by deploying an app, logging in to the app, and then running a curl request against your endpoint. The following example deploys `nginx` into your cluster.
   1. Create a configuration file for your deployment.
      ```
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: nginx-deployment
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: nginx
        template:
          metadata:
            labels:
              app: nginx
          spec:
            containers:
            - name: nginx
              image: nginxinc/nginx-unprivileged
              ports:
              - containerPort: 80
      ```
      {: codeblock}

   2. Deploy the app in your cluster.
      ```
      oc apply -f deployment.yaml
      ```
      {: pre}

   3. Verify that the `nginx` app is successfully deployed in your cluster.
      ```
      oc get pods
      ```
      {: pre}

      Example output:
      ```
      NAME                                READY   STATUS    RESTARTS   AGE
      nginx-deployment-85ff79dd56-6lrpg   1/1     Running   0          11s
      ```
      {: screen}

3. Log in to your pod.
 ```
 oc exec <pod_name> -it bash
 ```
 {: pre}

4. Use the {{site.data.keyword.satelliteshort}} Link connector host name and port to test the connection to your destination resource.
 ```
 curl http://<linkconnector_hostname>:<nodeport>
 ```
 {: pre}

<br />

## Creating `location` endpoints to connect to resources in a location
{: #link-location}

Create an endpoint of type `location` so that sources that are connected to the {{site.data.keyword.cloud_notm}} private network can connect to a service, server, or app in your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

**Before you begin**, ensure that you have the following:
* Source client: A service, server, or app that that can access the {{site.data.keyword.cloud_notm}} private network.
* Destination resource: A service, server, or app that runs in a {{site.data.keyword.satelliteshort}} cluster or a host that you attached to your location. For more information about how to create a {{site.data.keyword.satelliteshort}} cluster, see [Creating {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters). To attach a host to your location, see [Attaching hosts to your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-hosts#attach-hosts). Make sure that you do not assign the host to the {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.satelliteshort}} cluster after you attached the host. Assigning the host starts a bootstrapping process that removes SSH access to your host.
* Permissions: The [**Administrator** {{site.data.keyword.cloud_notm}} IAM platform role](/docs/satellite?topic=satellite-iam) for the **Location** resource in {{site.data.keyword.satellitelong_notm}}.

### Creating location endpoints by using the console
{: #link-location-ui}

Use the console to create an endpoint so that sources that are connected to the {{site.data.keyword.cloud_notm}} private network can connect to a service, server, or app in your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations), click the name of your location.
2. From the **Overview** tab, verify that your location has a **normal** status.
3. From the **Link endpoints** tab, click **Create an endpoint**.
4. Select **Satellite location** to create an endpoint for a service, server, or app in your {{site.data.keyword.satelliteshort}} location.
5. Enter an endpoint name, the destination resource's URL or IP address, and the port that your destination resource listens on for incoming requests. Make sure to enter the URL without `http://` or `https://`.
6. Select the protocol that a source must use to connect to the destination URL or IP address. This protocol must match the port for your destination resource. For more information, see [Endpoint protocols](#link-protocols).
  * If you selected the **TLS** or **HTTPS** protocols and want to require server-side authentication of the destination's certificate, select the **Verify destination certificate** checkbox.
  * If you selected the **TLS** or **HTTPS** protocols but the destination resource is still in development, you can click **Upload certificate** to add your self-signed, base-64 encoded certificate file.
7. Configure optional connection settings, such as enabling compression of data that is transferred through the endpoint or setting an inactivity timeout. The inactivity timeout is applied to both the connection between the source and {{site.data.keyword.satelliteshort}} Link and to the connection between {{site.data.keyword.satelliteshort}} Link and the destination. By default, no default inactivity timeout is set.
8. Click **Create**. Wait a few minutes for the {{site.data.keyword.satelliteshort}} Link connector component to assign a port to your endpoint.
9. In the table row for your endpoint, copy the host name for your {{site.data.keyword.satelliteshort}} Link tunnel server and the port for your endpoint in the **Address** field.
10. From your source client in the {{site.data.keyword.cloud_notm}} private network, test the connection to your {{site.data.keyword.satelliteshort}} endpoint by using the address. For example, depending on your source client, you might send a curl request to the endpoint:
   ```
   curl http://<linkserver_hostname>:<nodeport>
   ```
   {: pre}

### Creating location endpoints by using the CLI
{: #link-location-cli}

Use the CLI to create an endpoint so that sources that are connected to the {{site.data.keyword.cloud_notm}} private network can connect to a service, server, or app in your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

1. Get the ID of your {{site.data.keyword.satelliteshort}} location and verify that your location has a **normal** status.
   ```
   ibmcloud sat location ls
   ```
   {: pre}

   Example output:
   ```
   Name               ID                     Status   Ready   Created       Hosts (used/total)   Managed From
   port-antwerp       brlono42051up3k4htu0   normal   yes     2 weeks ago   6 / 7                London
   ```
   {: screen}

2. Create a `location` endpoint.
   ```
   ibmcloud sat endpoint create --location <location_ID> --endpoint <endpoint_name> --dest-type location --dest-hostname <hostname_or_IP> --dest-port <port> [--dest-protocol <destination_protocol>] --source-protocol <source_protocol>
   ```
   {: pre}

   <table summary="This table is read from left to right. The first column has the command component. The second column has the description of the component".>
    <caption>Understanding the API request</caption>
      <thead>
      <th>Component</th>
      <th>Description</th>
      </thead>
      <tbody>
      <tr>
      <td><code>--location &lt;location_ID&gt;</code></td>
      <td>Enter the ID of your {{site.data.keyword.satelliteshort}} location that you retrieved earlier.</td>
      </tr>
      <tr>
      <td><code>--endpoint &lt;endpoint_name&gt;</code></td>
      <td>Enter a name for your {{site.data.keyword.satelliteshort}} endpoint. </td>
      </tr>
      <tr>
      <td><code>--dest-type location</code></td>
      <td>Enter `location` to indicate that the destination resource runs in your {{site.data.keyword.satelliteshort}} location.</td>
      </tr>
      <tr>
      <td><code>--dest-hostname &lt;hostname_or_IP&gt;</code></td>
      <td>Enter the URL or the externally accessible IP address of the destination that you want to connect to, such as a public IP address, a public service endpoint, or a private service endpoint. Note that you cannot specify a private IP address. Make sure to enter the URL without <code>http://</code> or <code>https://</code>. </td>
      </tr>
      <tr>
      <td><code>--dest-port &lt;port&gt;</code></td>
      <td>Enter the port that destination resource listens on for incoming requests. Make sure that the port matches the destination protocol.</td>
      </tr>
      <tr>
      <td><code>--dest-protocol &lt;destination-protocol&gt;</code></td>
      <td>Optional: Enter the protocol of the destination resource. If you do not specify this flag, the destination protocol is inherited from the source protocol. Supported protocols include <code>tcp</code>, <code>udp</code>, <code>tls</code>, <code>http</code>, <code>https</code>, and <code>http-tunnel</code>. For more information, see [Endpoint protocols](#link-protocols).</td>
      </tr>
      <tr>
      <td><code>--source-protocol &lt;source-protocol&gt;</code></td>
      <td>Enter the protocol that the source must use to connect to the destination resource. Supported protocols include <code>tcp</code>, <code>udp</code>, <code>tls</code>, <code>http</code>, <code>https</code>, and <code>http-tunnel</code>. For more information, see [Endpoint protocols](#link-protocols).</td>
      </tr>
      </tbody>
    </table>

3. Wait a few minutes for the {{site.data.keyword.satelliteshort}} Link connector component to assign a port to your endpoint.

4. Verify that your endpoint is created. In the output, copy the host name for your {{site.data.keyword.satelliteshort}} Link tunnel server and the port for your endpoint in the **Address** field.
   ```
   ibmcloud sat endpoint ls --location <location_ID>
   ```
   {: pre}

5. From your source client in the {{site.data.keyword.cloud_notm}} private network, test the connection to your {{site.data.keyword.satelliteshort}} endpoint by using the address. For example, depending on your source client, you might send a curl request to the endpoint:
  ```
  curl http://<linkserver_hostname>:<nodeport>
  ```
  {: pre}

### Setting up source lists to limit access to endpoints
{: #link-sources}

Control which clients can access destination resources by creating a source list for an endpoint.
{: shortdesc}

If no sources are configured, any client can use an endpoint to connect to the destination resource. For example, for a location endpoint, any client that is connected to the {{site.data.keyword.cloud_notm}} private network can use the endpoint to connect to the destination resource that runs in your {{site.data.keyword.satelliteshort}} location. You can restrict access to your destination resource by adding only the IP addresses or subnet CIDRs of specific clients to the endpoint's source list.

Currently, you can create source lists only for endpoints of type `location`. You cannot create source lists for endpoints of type `cloud`.
{: note}



1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations), click the name of your location.
2. From the **Link endpoints** tab, click the name of your endpoint.
3. In the **Source list** section, click **Add source**.
4. Choose an existing source or configure a new source and add it to the source list.
  * To add an existing source, select the source name and click **Add**.
  * To configure a new source, click **Configure source** to enter a source name and the IP address or subnet CIDR for the client that you want to connect to the endpoint, and click **Add**.
5. Use the toggle to enable the source to connect to the destination resource. After you enable a source, network traffic to the destination through the endpoint is permitted only from clients that use an IP address in the range that you specified in the source. Network traffic from other clients that is sent to the destination resource through the endpoint is blocked.
6. Repeat these steps for any sources that you want to grant access to the destination resource through the endpoint.



<br />



## Logging network traffic for endpoints
{: #link-log}



Run a packet capture to view the traffic that is flowing from your source to your destination resource over a {{site.data.keyword.satelliteshort}} endpoint. Packet captures can be useful to help you debug problems with your endpoint connectivity or to monitor sources that access your destination.
{: shortdesc}

**Before you begin**: Install a packet capture tool, such as [`tcpdump`](https://www.tcpdump.org/){: external}, on your local machine.

1. Get the host name and port for your endpoint in the **Address** field. For cloud endpoints, the host name is the {{site.data.keyword.satelliteshort}} Link connector host name. For location endpoints, the host name is the {{site.data.keyword.satelliteshort}} Link tunnel server host name.
   ```
   ibmcloud sat endpoint ls --location <location_ID>
   ```
   {: pre}

2. Using the host name and port, start a packet capture. The following command is an example for using `tcpdump`.
    ```
    tcpdump -i <interface> host <link_host> and port <endpoint_nodeport> [-n] [-w <filename>.pcap]
    ```
    {: pre}

    <table summary="This table is read from left to right. The first column has the command component. The second column has the description of the component.">
     <caption>Understanding this command's components</caption>
       <thead>
       <th>Component</th>
       <th>Description</th>
       </thead>
       <tbody>
       <tr>
       <td><code>-i &lt;interface&gt;</code></td>
       <td>The interface that routes traffic through the endpoint. To view available interfaces, run <code>tcpdump -D</code>. If you do not know which interface is used, specify <code>-i any</code>.</td>
       </tr>
       <tr>
       <td><code>host &lt;link_host&gt;</code></td>
       <td>The host name that was assigned by {{site.data.keyword.satelliteshort}} Link to your endpoint.</td>
       </tr>
       <tr>
       <td><code>port &lt;endpoint_nodeport&gt;</code></td>
       <td>The port that was assigned by {{site.data.keyword.satelliteshort}} Link to your endpoint.</td>
       </tr>
       <tr>
       <td><code>-n</code></td>
       <td>Include this flag if you do not want the IP addresses and port numbers in the output to be converted to DNS host names.</td>
       </tr>
       <tr>
       <td><code>-w &lt;filename&gt;.pcap</code></td>
       <td>Include this flag to print the output of the packet capture into a `.pcap` file.</td>
       </tr>
       </tbody>
     </table>

3. In the output, you can check the sources and destinations of packets that are sent through the endpoint.

    Example output for a cloud endpoint that allows sources in a {{site.data.keyword.satelliteshort}} location (`10.171.52.151`) to access a target resource in {{site.data.keyword.cloud_notm}} (`166.9.12.121`):
    ```
    tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
    listening on any, link-type LINUX_SLL (Linux cooked), capture size 262144 bytes
    05:58:13.471632 IP 10.171.52.151.33666 > 166.9.12.121.filenet-nch: Flags [S], seq 2853445049, win 29200, options [mss 1460,sackOK,TS val 592612262 ecr 0,nop,wscale 7], length 0
    05:58:13.474685 IP 166.9.12.121.filenet-nch > 10.171.52.151.33666: Flags [S.], seq 2264270242, ack 2853445050, win 28960, options [mss 1460,sackOK,TS val 1156479729 ecr 592612262,nop,wscale 9], length 0
    05:58:13.474718 IP 10.171.52.151.33666 > 166.9.12.121.filenet-nch: Flags [.], ack 1, win 229, options [nop,nop,TS val 592612265 ecr 1156479729], length 0
    05:58:13.474806 IP 10.171.52.151.33666 > 166.9.12.121.filenet-nch: Flags [P.], seq 1:115, ack 1, win 229, options [nop,nop,TS val 592612265 ecr 1156479729], length 114
    05:58:13.476559 IP 166.9.12.121.filenet-nch > 10.171.52.151.33666: Flags [.], ack 115, win 57, options [nop,nop,TS val 1156479729 ecr 592612265], length 0
    05:58:13.583080 IP 166.9.12.121.filenet-nch > 10.171.52.151.33666: Flags [P.], seq 1:145, ack 115, win 57, options [nop,nop,TS val 1156479756 ecr 592612265], length 144
    05:58:13.583132 IP 10.171.52.151.33666 > 166.9.12.121.filenet-nch: Flags [.], ack 145, win 237, options [nop,nop,TS val 592612373 ecr 1156479756], length 0
    05:58:13.583399 IP 10.171.52.151.33666 > 166.9.12.121.filenet-nch: Flags [F.], seq 115, ack 145, win 237, options [nop,nop,TS val 592612373 ecr 1156479756], length 0
    05:58:13.585237 IP 166.9.12.121.filenet-nch > 10.171.52.151.33666: Flags [F.], seq 145, ack 116, win 57, options [nop,nop,TS val 1156479756 ecr 592612373], length 0
    05:58:13.585273 IP 10.171.52.151.33666 > 166.9.12.121.filenet-nch: Flags [.], ack 146, win 237, options [nop,nop,TS val 592612375 ecr 1156479756], length 0
    ```
    {: screen}

If you want to quickly generate traffic logs to test your endpoint, you can send 100 requests to your endpoint's host name and port: `for ((i=1;i<=100;i++)); do curl -v --header "Connection: keep-alive" "<host>:<nodeport>"; done`.
{: tip}

## Enabling and disabling endpoints
{: #enable_disable_endpoint}

After you set up an endpoint, you can control the flow of network traffic through the endpoint at any time by enabling or disabling the endpoint.
{: shortdesc}

1. From the [Locations dashboard](https://cloud.ibm.com/satellite/locations){: external}, select the location where you created the {{site.data.keyword.satelliteshort}} endpoint.
2. Select the **Link endpoints** tab and find the endpoint that you want to enable or disable.
3. Use the toggle to enable or disable the endpoint. After you disable an endpoint, network traffic between your location and the destination server, service, or app is blocked for all sources.
