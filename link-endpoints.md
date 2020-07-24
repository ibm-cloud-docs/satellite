---

copyright:
  years: 2020, 2020
lastupdated: "2020-07-24"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}


# Connecting your {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}} with endpoints
{: #link-location-cloud}

Open up {{site.data.keyword.satelliteshort}} endpoints in the {{site.data.keyword.satelliteshort}} control plane to allow network traffic between your {{site.data.keyword.satellitelong}} location and services, servers, or apps that run in {{site.data.keyword.cloud_notm}}.
{: shortdesc}

## About {{site.data.keyword.satelliteshort}} endpoints
{: #link-about}

With {{site.data.keyword.satelliteshort}} endpoints, you can allow any client that runs in your {{site.data.keyword.satelliteshort}} location to connect to a service, server, or app that is exposed in {{site.data.keyword.cloud_notm}}, or vice versa. To establish the connection, you must specify the target resource's URL or IP address and port, the connection protocol, and any authentication methods in the endpoint. The endpoint is registered with the {{site.data.keyword.satelliteshort}} Link component of your location's {{site.data.keyword.satelliteshort}} control plane.

### Use cases
{: #link-usecases}

You can create two types of endpoints, depending on your use case: a cloud endpoint, or a location endpoint.
{: shortdesc}

* Cloud endpoint: You want to securely connect to a service, server, or app that is exposed in {{site.data.keyword.cloud_notm}} from a client in your {{site.data.keyword.satelliteshort}} location. For example, you might want to send logs for an app in your {{site.data.keyword.satelliteshort}} location to your {{site.data.keyword.loganalysislong}} instance. You create a cloud endpoint for your {{site.data.keyword.cloud_notm}} target resource, which a client in your {{site.data.keyword.satelliteshort}} location uses to connect to the target resource.
* Location endpoint: You want to securely connect to a server, service, or app that runs in your {{site.data.keyword.satelliteshort}} location from a client in {{site.data.keyword.cloud_notm}}. For example, you might run a service in your {{site.data.keyword.satelliteshort}} location instead of in {{site.data.keyword.cloud_notm}} because the service has legal requirements to run in your on-premises data center in a specific country. You create a location endpoint for your target resource, which a client in {{site.data.keyword.cloud_notm}}, can use to connect to the target resource.

After you set up an endpoint, you can specify a list of source IP ranges so that only specific clients can access the target resource through the endpoint.

### Architecture
{: #link-architecture}

Two {{site.data.keyword.satelliteshort}} Link components, the tunnel server and the connector, proxy network traffic over a secure TLS connection between services in {{site.data.keyword.cloud_notm}} and resources in your {{site.data.keyword.satelliteshort}} location. For more information about the {{site.data.keyword.satelliteshort}} Link components, see [Satellite architecture](/docs/satellite?topic=satellite-service-architecture#architecture).
{: shortdesc}

**Target resource runs in {{site.data.keyword.cloud_notm}}**

By default, clients in your {{site.data.keyword.satelliteshort}} location cannot reach target resources that are privately exposed in {{site.data.keyword.cloud_notm}} because the target resource's IP address is not routable from within the location. Review the following architecture diagram and steps, which demonstrate how {{site.data.keyword.satelliteshort}} Link enables communication from {{site.data.keyword.satelliteshort}} locations to {{site.data.keyword.cloud_notm}} through {{site.data.keyword.satelliteshort}} endpoints.

<p>
<figure>
 <img src="/images/sat_link_cloud.png" alt="Network traffic flow from {{site.data.keyword.satelliteshort}} resources to a {{site.data.keyword.cloud_notm}} resource through an endpoint">
 <figcaption>Network traffic flow from a client in your {{site.data.keyword.satellitelong_notm}} location to a target resource in {{site.data.keyword.cloud_notm}} through {{site.data.keyword.satelliteshort}} Link</figcaption>
</figure>
</p>

1. When you create an endpoint for your {{site.data.keyword.cloud_notm}} resource, you open up a node port for the {{site.data.keyword.satelliteshort}} Link connector on your {{site.data.keyword.satelliteshort}} control plane worker nodes. Requests from clients in your {{site.data.keyword.satelliteshort}} location are made to the {{site.data.keyword.satelliteshort}} Link connector host name and this endpoint's node port, such as `nae4dce0eb35957baff66-edfc0a8ba65085c5081eced6816c5b9c-c000.us-south.containers.appdomain.cloud:30819`.

2. The {{site.data.keyword.satelliteshort}} Link connector forwards the request to the {{site.data.keyword.satelliteshort}} Link tunnel server on the {{site.data.keyword.satelliteshort}} control plane master over a secured VPN connection.

3. The {{site.data.keyword.satelliteshort}} Link tunnel server resolves the request to the target resource's IP address and port in {{site.data.keyword.cloud_notm}}, and forwards the request to the target resource.

**Target resource runs in {{site.data.keyword.satelliteshort}} location**

By default, clients in {{site.data.keyword.cloud_notm}} cannot reach target resources that are privately exposed in your {{site.data.keyword.satelliteshort}} location because the target resource's IP address is not routable from {{site.data.keyword.cloud_notm}}. Review the following architecture diagram and steps, which demonstrate how {{site.data.keyword.satelliteshort}} Link enables communication from {{site.data.keyword.cloud_notm}} to {{site.data.keyword.satelliteshort}} locations through {{site.data.keyword.satelliteshort}} endpoints.

<p>
<figure>
 <img src="/images/sat_link_location.png" alt="Network traffic flow from {{site.data.keyword.cloud_notm}} resources to a {{site.data.keyword.satelliteshort}} resource through an endpoint">
 <figcaption>Network traffic flow from a {{site.data.keyword.cloud_notm}} client to a target resource in your {{site.data.keyword.satellitelong_notm}} location through {{site.data.keyword.satelliteshort}} Link</figcaption>
</figure>
</p>

1. When you create an endpoint for your {{site.data.keyword.satelliteshort}} resource, you open up a node port for the {{site.data.keyword.satelliteshort}} Link connector on your {{site.data.keyword.satelliteshort}} control plane worker nodes. Requests from clients in {{site.data.keyword.cloud_notm}} are made to the {{site.data.keyword.satelliteshort}} Link tunnel server host name and this endpoint's node port, such as `c-01.us-south.link.satellite.cloud.ibm.com:30819`.

2. The {{site.data.keyword.satelliteshort}} Link tunnel server resolves the request to the {{site.data.keyword.satelliteshort}} Link connector host name and endpoint node port, and forwards the request to the {{site.data.keyword.satelliteshort}} Link connector over a secured VPN connection.

3. The {{site.data.keyword.satelliteshort}} Link connector resolves the request to the target resource's IP address and port, and forwards the request to the target resource.

### Endpoint protocols
{: #link-protocols}

When you create the endpoint, you must select the protocol that the client uses to connect to the endpoint. Review the following information about how {{site.data.keyword.satelliteshort}} Link handles each type of connection protocol.
{: shortdesc}

**UDP, TCP, and TLS**

If your target resource does not require requests with a specific HTTP or HTTPS host name header, or can accept direct requests to its IP address instead of its host, use the UDP, TCP, or TLS protocols. {{site.data.keyword.satelliteshort}} Link uses the same protocol as the request to transfer the request packet to the target resource.

**HTTP and HTTPS**

If your target resource is configured to listen for a specific HTTP or HTTPS host name header, use the HTTP or HTTPS protocols. By using HTTP header remapping, {{site.data.keyword.satelliteshort}} Link is able to correctly route requests for multiple target resources through TCP ports 80 (HTTP) and 443 (HTTPS).

Client requests from your {{site.data.keyword.satelliteshort}} location to your target resource in {{site.data.keyword.cloud_notm}} contain an HTTP header such as `linkconnector_hostname:nodeport`. When the request is sent from the {{site.data.keyword.satelliteshort}} Link connector to the {{site.data.keyword.satelliteshort}} Link tunnel server, the {{site.data.keyword.satelliteshort}} Link tunnel server changes the HTTP header on the request so that it is remapped to `target_host:target_port`. The {{site.data.keyword.satelliteshort}} Link tunnel server then uses the target resource's host name and port to send the request to the correct target resource.

Client requests from {{site.data.keyword.cloud_notm}} to your target resource in your {{site.data.keyword.satelliteshort}} location contain an HTTP header such as `linkserver_host:endpoint_nodeport`. The {{site.data.keyword.satelliteshort}} Link tunnel server changes the HTTP header on the request so that it is remapped to `target_host:target_port`, and sends the request to the {{site.data.keyword.satelliteshort}} Link tunnel server,  The {{site.data.keyword.satelliteshort}} Link connector. The {{site.data.keyword.satelliteshort}} Link connector then uses the target resource's host name and port to send the request to the correct target resource.

**Certificates for TLS and HTTPS**

If you select the TLS or HTTPS protocols, you can optionally require server-side verification of the target resource's certificate. The certificate must be valid for the target resource's hostname and signed by a trusted Certificate Authority.

If your target resource has a certificate, you do not need to provide the certificate when you create the endpoint. However, if you are testing access to a target resource that is still in development and does not yet have a trusted certificate, you can upload a self-signed certificate for verification. This file must be the `.crt` file type, must contain the base-64 encoded certificate for your resource's hostname, and must not contain the certificate key. To create a self-signed certificate for testing purposes by using OpenSSL, see this [self-signed SSL certificate tutorial](https://www.akadia.com/services/ssh_test_certificate.html){: external}.

<br />


## Creating {{site.data.keyword.satelliteshort}} endpoints for resources in {{site.data.keyword.cloud_notm}}
{: #link-cloud}

Create an endpoint of type `cloud` so that clients in your {{site.data.keyword.satelliteshort}} location can connect to an {{site.data.keyword.cloud_notm}} service, server, or app.
{: shortdesc}

**Before you begin**, ensure that you have the following:
* A {{site.data.keyword.satelliteshort}} cluster or a host that you added to your location. For more information about how to create a {{site.data.keyword.satelliteshort}} cluster, see [Creating {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters). To add a host to your location, see [Adding hosts to your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations#add-hosts). Make sure that you do not assign the host to the {{site.data.keyword.satelliteshort}} control plan or a {{site.data.keyword.satelliteshort}} cluster after you added the host. Assigning the host starts a bootstrapping process that removes SSH access to your host.
* A service, server, or app that is externally accessible in {{site.data.keyword.cloud_notm}}. For example, to use an {{site.data.keyword.cloud_notm}} service, the service must be enabled with private service endpoints.
* The [**Administrator** {{site.data.keyword.cloud_notm}} IAM platform role](/docs/satellite?topic=satellite-iam) for the **Link** resource in {{site.data.keyword.satellitelong_notm}}

### Creating endpoints by using the console
{: #link-cloud-ui}

To use the console to create an endpoint so that clients in your {{site.data.keyword.satelliteshort}} location can connect to an {{site.data.keyword.cloud_notm}} service, server, or app:
{: shortdesc}

1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations), click the name of your location.
2. From the **Endpoints** tab, click **Create endpoint**.
3. Select **Cloud** to create an endpoint for an {{site.data.keyword.cloud_notm}} service, server, or app.
4. Enter an endpoint name, the target resource's URL or externally accessible IP address, the port that your target resource listens on for incoming requests, and select the protocol that a client must use to connect to the target URL or IP address. Make sure to enter the URL without `http://` or `https://`, and that the port matches protocol. For more information, see [Endpoint protocols](#link-protocols).
5. If you selected the **TLS** or **HTTPS** protocols and want to require server-side authentication of the target resource's certificate, select the **Verify server certificate** checkbox.
6. If you selected the **TLS** or **HTTPS** protocols but the target resource is still in development, you can click **Upload certificate** to add your self-signed certificate that is base-64 encoded in a `.crt` file.
7. Configure optional connection settings, such as enabling compression of data that is transferred through the endpoint or setting an inactivity timeout. The inactivity timeout is applied to both the connection between the client and {{site.data.keyword.satelliteshort}} Link and to the connection between {{site.data.keyword.satelliteshort}} Link and the target resource. By default, no default inactivity timeout is set.
8. Click **Create**.
9. Wait a few minutes for the {{site.data.keyword.satelliteshort}} Link connector component to assign a node port to your endpoint.
10. In the table row for your endpoint, copy the host name for your {{site.data.keyword.satelliteshort}} Link connector in the **Host** field and copy the node port for your endpoint in the **Port** field.
11. Use the host and node port to [connect to your target resource from a client in your location](#link-cloud-test).



### Creating endpoints by using the API
{: #link-cloud-api}

To use the API to create an endpoint so that clients in your {{site.data.keyword.satelliteshort}} location can connect to an {{site.data.keyword.cloud_notm}} service, server, or app:
{: shortdesc}

1. Verify that your {{site.data.keyword.satelliteshort}} control plane is correctly set up and that endpoints were created in the {{site.data.keyword.satelliteshort}} link connector component for each of the hosts that you added to the control plane.
   1. Get the ID of your {{site.data.keyword.satelliteshort}} location and verify that your location is in a **normal** state.
      ```
      ibmcloud sat location ls
      ```
      {: pre}

   2. Retrieve your {{site.data.keyword.cloud_notm}} IAM access token.
      ```
      ibmcloud iam oauth-tokens
      ```
      {: pre}

   3. Retrieve the details of your location. If your {{site.data.keyword.satelliteshort}} control plane is correctly set up, you find 3 endpoints in the `connected_connectors_arr` property for every host that you added to your {{site.data.keyword.satelliteshort}} control plane. To allow network traffic to your {{site.data.keyword.satelliteshort}} location, your API output must also show `"connected": true`.
      ```
      curl -X GET -H "authorization: <iam_access_token>" https://api.hybrid-link.test.cloud.ibm.com/v1/locations/<location_ID>
      ```
      {: pre}

      Example output:
      ```
      {
       "location_id": "brlono42051up3k4htu0",
       "crn": "crn:v1:staging:public:satellite:us-south:a/e32663b2aaacd77c072dd97b4bb0191e::location:brlono42051up3k4htu0",
       "desc": "brlono42051up3k4htu0",
       "satellite_link_host": "c-01.us-south.hybrid-link.test.cloud.ibm.com",
       "status": "enabled",
       "connected": true,
       "created_at": "2020-06-18T15:44:33.316Z",
       "last_status_change": "2020-06-18T15:44:33.316Z",
       "connected_connectors_arr": [
          {
            "id": "brlono42051up3k4htu0-Client-kN8",
            "version": 1,
            "version_detail": "Version 1.2.0",
            "connector_host": "satellite-link-connector-55b9dc84d7-mhvdp",
            "type": "Kubernetes",
            "host": "hybrid-link-tunnel-hl-tunnel-75c858678c-pcllb"
          },
          {
            "id": "brlono42051up3k4htu0-Client-jXP",
            "version": 1,
            "version_detail": "Version 1.2.0",
            "connector_host": "satellite-link-connector-55b9dc84d7-d6m7g",
            "type": "Kubernetes",
            "host": "hybrid-link-tunnel-hl-tunnel-75c858678c-pcllb"
          },
          {
            "id": "brlono42051up3k4htu0-Client-f2l",
            "version": 1,
            "version_detail": "Version 1.2.0",
            "connector_host": "satellite-link-connector-55b9dc84d7-8vmtj",
            "type": "Kubernetes",
            "host": "hybrid-link-tunnel-hl-tunnel-75c858678c-9m84q"
          }
       ]
      }
      ```
      {: screen}

2. Create a {{site.data.keyword.satelliteshort}} endpoint to allow network traffic from your location to a service or app that runs in {{site.data.keyword.cloud_notm}}.  

   ```
   curl -X POST -H "authorization: <iam_access_token>" -H "Content-Type: application/json" https://api.hybrid-link.test.cloud.ibm.com/v1/locations/<location_ID>/endpoints -d '{"conn_type":"cloud","display_name":"<endpoint_name>","server_protocol":"<target_protocol>","connection_info":{"server_host":"<target_url>","server_port":"<target_port>","sni":"<target_url>"},"client_protocol":"<target_protocol>"}'
   ```
   {: pre}

   Example output:
   ```
   {
            "location_id": "brlono42051up3k4htu0",
            "crn": "crn:v1:staging:public:satellite:us-south:a/e32663b2aaacd77c072dd97b4bb0191e::location:brlono42051up3k4htu0",
            "conn_type": "cloud",
            "display_name": "LibertyNew",
            "service_name": "libertynew",
            "satellite_link_port": null,
            "connection_info": {
                "server_host": "rlgcluster-35366fb2d3d90fd50548180f69e7d12a-0002.us-south.containers.appdomain.cloud",
                "server_port": "80",
                "connector_port": 29997,
                "sni": "rlgcluster-35366fb2d3d90fd50548180f69e7d12a-0002.us-south.containers.appdomain.cloud"
            },
            "client_protocol": "tcp",
            "client_mutual_auth": false,
            "server_protocol": "tcp",
            "server_mutual_auth": false,
            "reject_unauth": true,
            "enforce_proxy": false,
            "proxy": null,
            "sources": [],
            "timeout": 0,
            "compress_data": true,
            "certs": {},
            "status": "enabled",
            "created_at": "2020-06-25T12:57:54.715Z",
            "last_change": "2020-06-25T12:58:16.772Z",
            "performance": {
                "connection": 0,
                "rx_bandwidth": 2,
                "tx_bandwidth": 218,
                "bandwidth": 220,
                "connectors": [
                    {
                        "connector": "satellite-link-connector-9487bf46c-sp5m7",
                        "connections": 0,
                        "rxBW": 1,
                        "txBW": 109
                    },
                    {
                        "connector": "satellite-link-connector-9487bf46c-82j8v",
                        "connections": 0,
                        "rxBW": 1,
                        "txBW": 109
                    }
                ]
            },
            "satellite_link_host": "c-01.us-south.hybrid-link.test.cloud.ibm.com",
            "region": "us-south",
            "endpoint_id": "brlono42051up3k4htu0_CcVxH"
        }
   ```
   {: screen}

   <table summary="This table is read from left to right. The first column has the command component. The second column has the description of the component".>
    <caption>Understanding the API request</caption>
      <thead>
      <th>Component</th>
      <th>Description</th>
      </thead>
      <tbody>
      <tr>
        <td><code><em>&lt;iam_access_token&gt;</em></code></td>
      <td>Enter the IAM access token that you retrieved earlier.</td>
      </tr>
      <tr>
      <td><code><em>&lt;endpoint_name&gt;</em></code></td>
      <td>Enter a name for your {{site.data.keyword.satelliteshort}} endpoint. </td>
      </tr>
      <tr>
      <td><code><em>&lt;target_protocol&gt;</em></code></td>
      <td>Enter the protocol to use when connecting to the target server. Supported protocols include <code>tcp</code>, <code>udp</code>, <code>tls</code>, <code>http</code>, and <code>https</code>.</td>
      </tr>
      <tr>
      <td><code><em>&lt;target_url&gt;</em></code></td>
      <td>Enter the public URL of the service, server, or app that you want to connect to. Make sure to enter the URL without <code>http://</code> or <code>https://</code>. </td>
      </tr>
      <tr>
      <td><code><em>&lt;target_port&gt;</em></code></td>
      <td>Enter the port that your service, server, or apps listens for incoming requests. Make sure that the port matches the protocol that you specified.</td>
      </tr>
      </tbody>
    </table>

3. Wait a few minutes for the {{site.data.keyword.satelliteshort}} link connector component to assign a node port to your endpoint. You use this node port and the hostname that was assigned to your location to connect to your service or app from a client in your location later.

4. Retrieve the details of your endpoint and verify that a node port is assigned to your endpoint (`connection_info.node_port`).
   ```
   curl -X GET -H "authorization: <iam_access_token>" https://api.hybrid-link.test.cloud.ibm.com/v1/locations/<location_ID>/endpoints
   ```
   {: pre}

   Example output:
   ```
   {
            "location_id": "brlono42051up3k4htu0",
            "crn": "crn:v1:staging:public:satellite:us-south:a/e32663b2aaacd77c072dd97b4bb0191e::location:brlono42051up3k4htu0",
            "conn_type": "cloud",
            "display_name": "LibertyNew",
            "service_name": "libertynew",
            "satellite_link_port": null,
            "connection_info": {
                "server_host": "rlgcluster-35366fb2d3d90fd50548180f69e7d12a-0002.us-south.containers.appdomain.cloud",
                "server_port": "80",
                "connector_port": 29997,
                "node_port": 30405,
                "sni": "rlgcluster-35366fb2d3d90fd50548180f69e7d12a-0002.us-south.containers.appdomain.cloud"
            },
            "client_protocol": "tcp",
            "client_mutual_auth": false,
            "server_protocol": "tcp",
            "server_mutual_auth": false,
            "reject_unauth": true,
            "enforce_proxy": false,
            "proxy": null,
            "sources": [],
            "timeout": 0,
            "compress_data": true,
            "certs": {},
            "status": "enabled",
            "created_at": "2020-06-25T12:57:54.715Z",
            "last_change": "2020-06-25T12:58:16.772Z",
            "performance": {
                "connection": 0,
                "rx_bandwidth": 2,
                "tx_bandwidth": 218,
                "bandwidth": 220,
                "connectors": [
                    {
                        "connector": "satellite-link-connector-9487bf46c-sp5m7",
                        "connections": 0,
                        "rxBW": 1,
                        "txBW": 109
                    },
                    {
                        "connector": "satellite-link-connector-9487bf46c-82j8v",
                        "connections": 0,
                        "rxBW": 1,
                        "txBW": 109
                    }
                ]
            },
            "satellite_link_host": "c-01.us-south.hybrid-link.test.cloud.ibm.com",
            "region": "us-south",
            "endpoint_id": "brlono42051up3k4htu0_CcVxH"
        }
   ```
   {: screen}

5. Retrieve the **HOSTNAME** that was assigned to your location. You use the hostname and the node port that was assigned to your endpoint to connect to the endpoint from a client in your location.
   ```
   ibmcloud sat location dns ls --location <location_name_or_ID>
   ```
   {: pre}

### Testing connections through cloud endpoints
{: #link-cloud-test}

Use the host and node port that are assigned to your endpoint to connect to your target {{site.data.keyword.cloud_notm}} resource from a client in your location. The client can be a {{site.data.keyword.satelliteshort}} cluster that you previously created or a host that you added to your location.
{: shortdesc}

**Example for testing the connection from a host**:
1. Log in to your host. Enter the password to access your host when prompted.
 ```
 ssh root@<pulic_IP_address>
 ```
 {: pre}

2. Test the connection to your {{site.data.keyword.satelliteshort}} endpoint by using the {{site.data.keyword.satelliteshort}} Link connector host name and the node port that was assigned to your endpoint.
 ```
 curl http://<linkconnector_hostname>:<nodeport>
 ```
 {: pre}
</br>

**Example for testing the connection from a {{site.data.keyword.satelliteshort}} cluster**:
1. Target your cluster.
 ```
 ibmcloud oc cluster config --cluster <cluster_name> --admin
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
            image: nginx
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

4. Test the connection to your {{site.data.keyword.satelliteshort}} endpoint by using the hostname of the location and the node port that was assigned to your endpoint.
 ```
 curl http://<location_hostname>:<nodeport>
 ```
 {: pre}

### Managing client source access to endpoints for cloud resources
{: #link-sources}

Control which clients in your {{site.data.keyword.satelliteshort}} location can access target resources in {{site.data.keyword.cloud_notm}} by creating a source list for an endpoint.
{: shortdesc}

If no sources are configured, any client in your {{site.data.keyword.satelliteshort}} location can connect to the endpoint for a target resource in {{site.data.keyword.cloud_notm}}. You can restrict access to your {{site.data.keyword.cloud_notm}} resource by adding only the IP addresses or subnet CIDRs of specific clients in your {{site.data.keyword.satelliteshort}} location to the endpoint's source list.

Currently, you can create client source lists only for endpoints of type `cloud`. You cannot create client source lists for endpoints of type `location`.
{: note}


1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations), click the name of your location.
2. From the **Endpoints** tab, click the name of your cloud endpoint.
3. In the **Client sources** section, click **Add a client source**.
4. Enter a name for the source, and enter the IP address or subnet CIDR for the client that you want to connect to the endpoint.
5. Click **Add**.
6. After the endpoint is created, use the toggle to enable the source. After you enable a source, network traffic to the target resource through the endpoint is permitted only from clients that use an IP address from that source or from other sources that you enable. All network traffic from other clients through the endpoint is blocked.
7. Repeat these steps for any clients that you want to grant access to the endpoint.



<br />


## Creating {{site.data.keyword.satelliteshort}} endpoints for resources in a location
{: #link-location}

Create an endpoint of type `location` so that clients in {{site.data.keyword.cloud_notm}} can connect to a service, server, or app in your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

**Before you begin**, ensure that you have the following:
* A service, server, or app that is externally accessible in a {{site.data.keyword.satelliteshort}} cluster or a host that you added to your location. For more information about how to create a {{site.data.keyword.satelliteshort}} cluster, see [Creating {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters). To add a host to your location, see [Adding hosts to your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations#add-hosts). Make sure that you do not assign the host to the {{site.data.keyword.satelliteshort}} control plan or a {{site.data.keyword.satelliteshort}} cluster after you added the host. Assigning the host starts a bootstrapping process that removes SSH access to your host.
* The client service in {{site.data.keyword.cloud_notm}} must be enabled with private cloud service endpoints. For more information, see [Using service endpoints](/docs/account?topic=account-vrf-service-endpoint#use-service-endpoint).
* The [**Administrator** {{site.data.keyword.cloud_notm}} IAM platform role](/docs/satellite?topic=satellite-iam) for the **Link** resource in {{site.data.keyword.satellitelong_notm}}

### Creating endpoints by using the console
{: #link-location-ui}

To use the console to create an endpoint so that clients in {{site.data.keyword.cloud_notm}} can connect to a service, server, or app in your {{site.data.keyword.satelliteshort}} location:
{: shortdesc}

1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations), click the name of your location.
2. From the **Endpoints** tab, click **Create endpoint**.
3. Select **Location** to create an endpoint for a service, server, or app in your {{site.data.keyword.satelliteshort}} location.
4. Enter an endpoint name, the target resource's URL or externally accessible IP address, the port that your target resource listens on for incoming requests, and select the protocol that a client must use to connect to the target URL or IP address. Make sure to enter the URL without `http://` or `https://`, and that the port matches protocol. For more information, see [Endpoint protocols](#link-protocols).
5. If you selected the **TLS** or **HTTPS** protocols and want to require server-side authentication of the target resource's certificate, select the **Verify server certificate** checkbox.
6. If you selected the **TLS** or **HTTPS** protocols but the target resource is still in development, you can click **Upload certificate** to add your self-signed certificate that is base-64 encoded in a `.crt` file.
7. Configure optional connection settings, such as enabling compression of data that is transferred through the endpoint or setting an inactivity timeout. The inactivity timeout is applied to both the connection between the client and {{site.data.keyword.satelliteshort}} Link and to the connection between {{site.data.keyword.satelliteshort}} Link and the target resource. By default, no default inactivity timeout is set.
8. Click **Create**.
9. Wait a few minutes for the {{site.data.keyword.satelliteshort}} Link connector component to assign a node port to your endpoint.
10. In the table row for your endpoint, copy the host name for your {{site.data.keyword.satelliteshort}} Link tunnel server in the **Host** field and copy the node port for your endpoint in the **Port** field.
11. From your client in {{site.data.keyword.cloud_notm}}, test the connection to your {{site.data.keyword.satelliteshort}} endpoint by using host and node port. For example, depending on your {{site.data.keyword.cloud_notm}} service, you might send a curl request to the endpoint:
   ```
   curl http://<linkserver_hostname>:<nodeport>
   ```
   {: pre}



<br />




## Logging network traffic for endpoints
{: #link-log}



Run a packet capture to view the traffic that is flowing from your client to your target resource over a {{site.data.keyword.satelliteshort}} endpoint. Packet captures can be useful to help you debug problems with your endpoint connectivity or to monitor clients that access your target resource.
{: shortdesc}

**Before you begin**: Install a packet capture tool, such as [`tcpdump`](https://www.tcpdump.org/){: external}, on your local machine.

1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations), click the name of your location.
2. From the **Endpoints** tab, copy the host name for your endpoint in the **Host** field and the node port for your endpoint in the **Port** field. For cloud endpoints, the host name is the {{site.data.keyword.satelliteshort}} Link connector host name. For location endpoints, the host name is the {{site.data.keyword.satelliteshort}} Link tunnel server host name.

2. Using the host name and node port, start a packet capture. The following command is an example for using `tcpdump`.
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
       <td>The node port that was assigned by {{site.data.keyword.satelliteshort}} Link to your endpoint.</td>
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

    Example output for a cloud endpoint that allows clients in a {{site.data.keyword.satelliteshort}} location (`10.171.52.151`) to access a target resource in {{site.data.keyword.cloud_notm}} (`166.9.12.121`):
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

If you want to quickly generate traffic logs to test your endpoint, you can send 100 requests to your endpoint's host name and node port: `for ((i=1;i<=100;i++)); do curl -v --header "Connection: keep-alive" "<host>:<nodeport>"; done`.
{: tip}

## Enabling and disabling endpoints
{: #enable_disable_endpoint}

After you set up an endpoint, you can control the flow of network traffic through the endpoint at any time by enabling or disabling the endpoint.
{: shortdesc}

1. From the [Locations dashoard](https://cloud.ibm.com/satellite/locations){: external}, select the location where you created the {{site.data.keyword.satelliteshort}} endpoint.
2. Select the **Endpoints** tab and find the endpoint that you want to enable or disable.
3. Use the toggle to enable or disable the endpoint. After you disable an endpoint, network traffic between your location and the target server, service, or app is blocked for all source clients that you set up in your endpoint.
