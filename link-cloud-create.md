---

copyright:
  years: 2020, 2022
lastupdated: "2022-03-10"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Creating and managing link endpoints
{: #link-cloud-create}

Create your {{site.data.keyword.satelliteshort}} endpoints to manage the network between your {{site.data.keyword.satellitelong_notm}} location and services, servers, or apps that run outside of the location. 
{: shortdesc}

## Creating `cloud` endpoints to connect to resources outside of the location
{: #link-cloud}

Create an endpoint of type `cloud` so that sources in your {{site.data.keyword.satelliteshort}} location can connect to a service, server, or app that runs outside of the location.
{: shortdesc}

Before you begin, ensure that you have the following items.

Source client
:  A {{site.data.keyword.satelliteshort}} cluster or a host that you attached to your location. For more information about how to create a {{site.data.keyword.satelliteshort}} cluster, see [Creating {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters). To use a host, [attach a host to your location](/docs/satellite?topic=satellite-attach-hosts) but do not assign the host to the {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.satelliteshort}} cluster. Assigning the host starts a bootstrapping process that removes SSH access to your host.

Destination resource
:   A service, server, or app that runs outside of the location but that is accessible from within {{site.data.keyword.cloud_notm}}. For example, you can use the private service endpoint for an {{site.data.keyword.cloud_notm}} service, because that private service endpoint is routable from within the {{site.data.keyword.cloud_notm}} network. If you want to connect to a service that runs outside of {{site.data.keyword.cloud_notm}}, this service must be accessible from within the {{site.data.keyword.cloud_notm}} network.

Permissions
:   The **Administrator** {{site.data.keyword.cloud_notm}} IAM platform role for the **Link** resource in {{site.data.keyword.satellitelong_notm}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).


### Creating cloud endpoints by using the console
{: #link-cloud-ui}

Use the console to create a cloud endpoint so that sources in your {{site.data.keyword.satelliteshort}} location can connect to a service, server, or app that runs outside of the location.
{: shortdesc}

1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations), click the name of your location.
2. From the **Overview** tab, verify that your location has a **normal** status.
3. From the **Link endpoints** tab, click **Create an endpoint**.
4. Select **Cloud** to create an endpoint for a service, server, or app that runs outside of the location.
5. Enter an endpoint name, the destination resource's fully qualified domain name (FQDN) or IP address, and the port that your destination resource listens on for incoming requests. The IP address or FQDN must resolve to a public IP address or to a private IP address that is accessible within {{site.data.keyword.cloud_notm}}, such as a private service endpoint.
6. Select the protocol that a source must use to connect to the destination FQDN or IP address. This protocol must match the port for your destination resource. For more information, see [Endpoint protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols).
    - If you selected the **TLS** or **HTTPS** protocols and want to require server-side authentication of the destination's certificate, select the **Verify destination certificate** checkbox.
    - If you selected the **TLS** or **HTTPS** protocols but the destination resource is still in development, you can click **Upload certificate** to add your self-signed certificate file. This `ssl.crt` file must contain the public, base-64 encoded certificate for your resource's host name and must not contain the private `ssl.key` certificate key. To create a self-signed certificate for testing purposes by using OpenSSL, see this [self-signed SSL certificate tutorial](https://www.akadia.com/services/ssh_test_certificate.html){: external}.
    - If you selected the **TLS** or **HTTPS** protocols and want to allow a separate hostname to be provided to the TLS handshake of the resource connection, enter the server name indicator (SNI).
7. Configure optional connection settings, such as setting an inactivity timeout. Choose a value between 1 and 60 seconds. The default value is `60` seconds.
8. Click **Create**. Wait a few minutes for the {{site.data.keyword.satelliteshort}} Link connector component to assign a port to your endpoint.
9. In the table row for your endpoint, copy the host name for your {{site.data.keyword.satelliteshort}} Link connector and the port for your endpoint in the **Address** field.
10. Use the address to [connect to your destination from a source in your location](#link-cloud-test).

### Creating cloud endpoints with the CLI
{: #link-cloud-cli}

Use the CLI to create an endpoint so that sources in your {{site.data.keyword.satelliteshort}} location can connect to a service, server, or app that runs outside of the location.
{: shortdesc}

1. Get the ID of your {{site.data.keyword.satelliteshort}} location and verify that your location has a **normal** status.
    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

    Example output
    ```sh
    Name               ID                     Status   Ready   Created       Hosts (used/total)   Managed From
    port-antwerp       brlono42051up3k4htu0   normal   yes     2 weeks ago   6 / 7                London
    ```
    {: screen}

2. Create a `cloud` endpoint.
    ```sh
    ibmcloud sat endpoint create --location <location_ID> --name <endpoint_name> --dest-type cloud --dest-hostname <FQDN_or_IP> --dest-port <port> [--dest-protocol <destination_protocol>] --source-protocol <source_protocol>  [--sni <sni>]
    ```
    {: pre}

    | Component             | Description      | 
    |--------------------|------------------|
    | `--location <location_ID>` | Enter the ID of your {{site.data.keyword.satelliteshort}} location that you retrieved earlier. | 
    | `--name <endpoint_name>` | Enter a name for your {{site.data.keyword.satelliteshort}} endpoint. | 
    | `--dest-type cloud` | Enter `cloud` to indicate that the destination resource runs outside of the location. | 
    | `--dest-hostname <FQDN_or_IP>` | Enter the fully qualified domain name (FQDN) or the externally accessible IP address of the destination that you want to connect to, which must resolve to a public IP address or to a private IP address that is accessible within {{site.data.keyword.cloud_notm}} such as a private cloud service endpoint. | 
    | `--dest-port <port>` | Enter the port that destination resource listens on for incoming requests. Make sure that the port matches the destination protocol. | 
    | `--dest-protocol <destination-protocol>` | Optional: Enter the protocol of the destination resource. If you do not specify this option, the destination protocol is inherited from the source protocol. Supported protocols include `tcp` and `tls`. For more information, see [Endpoint protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols). | 
    | `--source-protocol <source-protocol>` | Enter the protocol that the source must use to connect to the destination resource. Supported protocols include `tcp`, `tls`, `http`, `https`, and `http-tunnel`. For more information, see [Endpoint protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols). | 
    |  `--sni <sni>` | Optional. If you specify a `tls` or `https` source protocol and want a separate hostname to be added to the TLS handshake, include the server name indicator. |
    {: summary="This table is read from left to right. The first column has the command component. The second column has the description of the component."}
    {: caption="Table 1. Understanding the API request" caption-side="top"}

3. Wait a few minutes for the {{site.data.keyword.satelliteshort}} Link connector component to assign a port to your endpoint.

4. Verify that your endpoint is created. In the output, copy the host name for your {{site.data.keyword.satelliteshort}} Link connector and the port for your endpoint in the **Address** field.

    ```sh
    ibmcloud sat endpoint ls --location <location_ID>
    ```
    {: pre}

    Example output
    
    ```sh
    ID                           Name                                         Destination Type   Address
    c0mnbnkw0jl8si22djkg_cEomQ   openshift-api-c0mpnn4w0bv28oq2dks0           location           TCP  c-02.us-east.link.satellite.cloud.ibm.com:32823
    c0mnbnkw0jl8si22djkg_6UTZd   satellite-healthcheck-c0mnbnkw0jl8si22djkg   location           HTTP c-02.us-east.link.satellite.cloud.ibm.com:32822
    c0mnbnkw0jl8si22djkg_GzstO   test-endpoint                                cloud              TLS  nae4dce0eb35957baff66-edfc0a8ba65085c5081eced6816c5b9c-c000.us-east.satellite.appdomain.cloud:30819
    ```
    {: screen}

5. Use the address to [connect to your destination from a source in your location](#link-cloud-test).

## Testing connections through cloud endpoints
{: #link-cloud-test}

Use the {{site.data.keyword.satelliteshort}} Link connector host name and port that are assigned to your endpoint to connect to your destination resource from a source in your location. The source can be a {{site.data.keyword.satelliteshort}} cluster that you previously created or a host that you assigned to your location.
{: shortdesc}

### Example for testing the connection from an unassigned host
{: #link-example-unassigned-host}

1. Log in to your host. Enter the password to access your host when prompted.
    ```sh
    ssh root@<ip_address>
    ```
    {: pre}

2. Use the {{site.data.keyword.satelliteshort}} Link connector host name and port to test the connection to your destination resource.
    ```sh
    curl http://<linkconnector_hostname>:<port>
    ```
    {: pre}



### Example for testing the connection from a {{site.data.keyword.satelliteshort}} cluster
{: #link-example-connection-cluster}

1. Target your cluster. If you are not connected to your location host network, include the `--endpoint link` option.
    ```sh
    ibmcloud oc cluster config --cluster <cluster_name> --admin [--endpoint link]
    ```
    {: pre}

2. Deploy a sample app to your cluster. To test the connection from your location to your endpoint, you must be connected to the network that your {{site.data.keyword.satelliteshort}} cluster is connected to. You can connect to the network by deploying an app, logging in to the app, and then running a curl request against your endpoint. The following example deploys `nginx` into your cluster.
    1. Create a configuration file for your deployment.
        ```yaml
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
        ```sh
        oc apply -f deployment.yaml
        ```
        {: pre}

    3. Verify that the `nginx` app is successfully deployed in your cluster.
        ```sh
        oc get pods
        ```
        {: pre}

        Example output
        ```sh
        NAME                                READY   STATUS    RESTARTS   AGE
        nginx-deployment-85ff79dd56-6lrpg   1/1     Running   0          11s
        ```
        {: screen}

3. Log in to your pod.
    ```sh
    oc exec <pod_name> -it bash
    ```
    {: pre}

4. Use the {{site.data.keyword.satelliteshort}} Link connector host name and port to test the connection to your destination resource.
    ```sh
    curl http://<linkconnector_hostname>:<port>
    ```
    {: pre}


## Creating `location` endpoints to connect to resources in a location
{: #link-location}

Create an endpoint of type `location` so that sources that are connected to the {{site.data.keyword.cloud_notm}} private network can connect to a service, server, or app in your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

Before you begin, ensure that you have the following items.

Source client
:   A service, server, or app that that can access the {{site.data.keyword.cloud_notm}} private network.
Destination resource

:   A service, server, or app that runs in a {{site.data.keyword.satelliteshort}} cluster or a host that you attached to your location. For more information about how to create a {{site.data.keyword.satelliteshort}} cluster, see [Creating {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-satellite-clusters). To use a host, [attach a host to your location](/docs/satellite?topic=satellite-attach-hosts) but do not assign the host to the {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.satelliteshort}} cluster. Assigning the host starts a bootstrapping process that removes SSH access to your host.

Permissions
:   The **Administrator** {{site.data.keyword.cloud_notm}} IAM platform role for the **Link** resource in {{site.data.keyword.satellitelong_notm}}. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).


### Creating location endpoints by using the console
{: #link-location-ui}

Use the console to create an endpoint so that sources that are connected to the {{site.data.keyword.cloud_notm}} private network can connect to a service, server, or app in your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

1. From the [{{site.data.keyword.satelliteshort}} **Locations** dashboard](https://cloud.ibm.com/satellite/locations), click the name of your location.
2. From the **Overview** tab, verify that your location has a **normal** status.
3. From the **Link endpoints** tab, click **Create an endpoint**.
4. Select **Satellite location** to create an endpoint for a service, server, or app in your {{site.data.keyword.satelliteshort}} location.
5. Enter an endpoint name, the destination resource's fully qualified domain name (FQDN) or IP address, and the port that your destination resource listens on for incoming requests.
6. Select the protocol that a source must use to connect to the destination FQDN or IP address. This protocol must match the port for your destination resource. For more information, see [Endpoint protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols).
    - If you selected the **TLS** or **HTTPS** protocols and want to require server-side authentication of the destination's certificate, select the **Verify destination certificate** checkbox.
    - If you selected the **TLS** or **HTTPS** protocols but the destination resource is still in development, you can click **Upload certificate** to add your self-signed certificate file. This `ssl.crt` file must contain the public, base-64 encoded certificate for your resource's host name and must not contain the private `ssl.key` certificate key. To create a self-signed certificate for testing purposes by using OpenSSL, see this [self-signed SSL certificate tutorial](https://www.akadia.com/services/ssh_test_certificate.html){: external}.
    - If you selected the **TLS** or **HTTPS** protocols and want to allow a separate hostname to be provided to the TLS handshake of the resource connection, enter the server name indicator (SNI).
7. Configure optional connection settings, such as setting an inactivity timeout.  Choose a value between 1 and 60 seconds. The default value is `60` seconds.
8. Click **Create**. Wait a few minutes for the {{site.data.keyword.satelliteshort}} Link connector component to assign a port to your endpoint.
9. In the table row for your endpoint, copy the host name for your {{site.data.keyword.satelliteshort}} Link tunnel server and the port for your endpoint in the **Address** field.
10. From your source client in the {{site.data.keyword.cloud_notm}} private network, test the connection to your {{site.data.keyword.satelliteshort}} endpoint by using the address. For example, depending on your source client, you might send a curl request to the endpoint:
    ```sh
    curl http://<linkserver_hostname>:<port>
    ```
    {: pre}

### Creating location endpoints by using the CLI
{: #link-location-cli}

Use the CLI to create an endpoint so that sources that are connected to the {{site.data.keyword.cloud_notm}} private network can connect to a service, server, or app in your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

1. Get the ID of your {{site.data.keyword.satelliteshort}} location and verify that your location has a **normal** status.
    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

    Example output
    ```sh
    Name               ID                     Status   Ready   Created       Hosts (used/total)   Managed From
    port-antwerp       brlono42051up3k4htu0   normal   yes     2 weeks ago   6 / 7                London
    ```
    {: screen}

2. Create a `location` endpoint.
    ```sh
    ibmcloud sat endpoint create --location <location_ID> --name <endpoint_name> --dest-type location --dest-hostname <FQDN_or_IP> --dest-port <port> [--dest-protocol <destination_protocol>] --source-protocol <source_protocol>
    ```
    {: pre}
  
    | Component             | Description      | 
    |--------------------|------------------|
    | `--location <location_ID>` | Enter the ID of your {{site.data.keyword.satelliteshort}} location that you retrieved earlier. | 
    | `--name <endpoint_name>` | Enter a name for your {{site.data.keyword.satelliteshort}} endpoint. | 
    | `--dest-type cloud` | Enter `cloud` to indicate that the destination resource runs outside of the location. | 
    | `--dest-hostname <FQDN_or_IP>` | Enter the fully qualified domain name (FQDN) or the externally accessible IP address of the destination that you want to connect to, which must resolve to a public IP address or to a private IP address that is accessible within {{site.data.keyword.cloud_notm}} such as a private cloud service endpoint. | 
    | `--dest-port <port>` | Enter the port that destination resource listens on for incoming requests. Make sure that the port matches the destination protocol. | 
    | `--dest-protocol <destination-protocol>` | Optional: Enter the protocol of the destination resource. If you do not specify this option, the destination protocol is inherited from the source protocol. Supported protocols include `tcp` and `tls`. For more information, see [Endpoint protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols). | 
    | `--source-protocol <source-protocol>` | Enter the protocol that the source must use to connect to the destination resource. Supported protocols include `tcp`, `tls`, `http`, `https`, and `http-tunnel`. For more information, see [Endpoint protocols](/docs/satellite?topic=satellite-link-location-cloud#link-protocols). | 
    |  `--sni <sni>` | Optional. If you specify a `tls` or `https` source protocol and want a separate hostname to be added to the TLS handshake, include the server name indicator. |
    {: summary="This table is read from left to right. The first column has the API component. The second column has the description of the component."}
    {: caption="Table 1. Understanding the API request" caption-side="top"}

3. Wait a few minutes for the {{site.data.keyword.satelliteshort}} Link connector component to assign a port to your endpoint.

4. Verify that your endpoint is created. In the output, copy the host name for your {{site.data.keyword.satelliteshort}} Link tunnel server and the port for your endpoint in the **Address** field.

    ```sh
    ibmcloud sat endpoint ls --location <location_ID>
    ```
    {: pre}

    Example output
    
    ```sh
    ID                           Name                                         Destination Type   Address
    c0mnbnkw0jl8si22djkg_cEomQ   openshift-api-c0mpnn4w0bv28oq2dks0           location           TCP  c-02.us-east.link.satellite.cloud.ibm.com:32823
    c0mnbnkw0jl8si22djkg_6UTZd   satellite-healthcheck-c0mnbnkw0jl8si22djkg   location           HTTP c-02.us-east.link.satellite.cloud.ibm.com:32822
    ```
    {: screen}

5. From your source client in the {{site.data.keyword.cloud_notm}} private network, test the connection to your {{site.data.keyword.satelliteshort}} endpoint by using the address. For example, depending on your source client, you might send a curl request to the endpoint.

    ```sh
    curl http://<linkserver_hostname>:<port>
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
    * To configure a new source, click **Configure source** to enter a source name and the IP address or subnet CIDR for the client that you want to connect to the endpoint, and click **Add**. Separate multiple IP addresses or subnet CIDRs with a comma (`,`).
5. Use the toggle to enable the source to connect to the destination resource. After you enable a source, network traffic to the destination through the endpoint is permitted only from clients that use an IP address in the range that you specified in the source. Network traffic from other clients that is sent to the destination resource through the endpoint is blocked.
6. Repeat these steps for any sources that you want to grant access to the destination resource through the endpoint.

To see the status of sources for each endpoints, such as the last time that a source was modified for an endpoint, click the **Link endpoints** tab, and click the **Sources** tab.
{: tip}



## Enabling and disabling endpoints
{: #enable_disable_endpoint}

After you set up an endpoint, you can control the flow of network traffic through the endpoint at any time by enabling or disabling the endpoint.
{: shortdesc}

1. From the [Locations dashboard](https://cloud.ibm.com/satellite/locations){: external}, select the location where you created the {{site.data.keyword.satelliteshort}} endpoint.
2. Select the **Link endpoints** tab and find the endpoint that you want to enable or disable.
3. Use the toggle to enable or disable the endpoint. After you disable an endpoint, network traffic between your location and the destination server, service, or app is blocked for all sources.
