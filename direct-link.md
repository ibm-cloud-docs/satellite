<staging>---

copyright:
  years: 2020, 2021
lastupdated: "2021-12-02"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Connecting {{site.data.keyword.satelliteshort}} locations with {{site.data.keyword.cloud_notm}} using {{site.data.keyword.dl_short}}
{: #direct-link-tutorial}
{: toc-content-type="tutorial"}
{: toc-services="satellite, containers, dl"}
{: toc-completion-time="2h"}

Use a secure {{site.data.keyword.dl_full}} connection for {{site.data.keyword.satelliteshort}} Link communications between your on-premises {{site.data.keyword.satellitelong}} location and {{site.data.keyword.cloud}}.
{: shortdesc}

In this tutorial, you configure your {{site.data.keyword.satelliteshort}} Link setup to use a {{site.data.keyword.dl_short}} connection. Your location's Link connector sends traffic over the {{site.data.keyword.dl_short}} connection to an {{site.data.keyword.containerlong_notm}} cluster, which proxies the traffic to Link tunnel server's IP address in the {{site.data.keyword.cloud_notm}} private network.

The features in this tutorial are available as closed technical preview that is subject to change without prior notice.
{: preview}

## Overview
{: #dl-overview}

By default, two {{site.data.keyword.satelliteshort}} Link components, the tunnel server and the connector, proxy network traffic between {{site.data.keyword.cloud_notm}} and resources in your on-premises {{site.data.keyword.satelliteshort}} location over a secure TLS connection. Instead of using a TLS connection over the internet, you can set up an [{{site.data.keyword.dl_full_notm}}](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) connection for all network traffic between the Link tunnel server and the Link connector.

This setup uses the tunnel server's private cloud service endpoint to route traffic over the {{site.data.keyword.cloud_notm}} private network. However, communication to the tunnel server's private cloud service endpoint must go through the `166.9.X.X/16` IP address range, which is not routable from {{site.data.keyword.dl_full_notm}}.

To make the `166.9.X.X/16` range accessible, you can create an {{site.data.keyword.containerlong_notm}} cluster in your {{site.data.keyword.cloud_notm}} account. Then, you create an NGINX reverse proxy that points to the tunnel server's private cloud service endpoint. Finally, you use the {{site.data.keyword.containerlong_notm}} cluster's private Ingress application load balancers (ALBs) to expose the reverse proxy as an internal `10.X.X.X/8` IP address range, which is accessible through a {{site.data.keyword.dl_short}} connection.

The following diagram demonstrates the flow of traffic.

![{{site.data.keyword.satelliteshort}} Link setup that uses a {{site.data.keyword.dl_short}} connection and a reverse proxy](/images/dl-unformatted.png)
TODO: need to format image using ibmcloud docs style set

1. Network traffic that originates from your location, such as a request from an {{site.data.keyword.satellitelong_notm}} cluster to an {{site.data.keyword.cloud_notm}} service, is forwarded to the location's Link connector.
2. The location's Link connector routes the request over {{site.data.keyword.dl_short}} to the private Ingress subdomain, which terminates to the `10.X.X.X` IP address of a private ALB in your {{site.data.keyword.containerlong_notm}} cluster.
3. The private ALB forwards the request to the NGINX reverse proxy service.
4. The reverse proxy starts a new session to forward the request to the tunnel server's private cloud service endpoint, which terminates to an IP address in the `166.9.X.X/16` range.

## Objectives
{: #dl-objectives}

- Create an {{site.data.keyword.containerlong_notm}} cluster in your {{site.data.keyword.cloud_notm}} account.
- Set up the private Ingress ALBs in the cluster.
- Deploy an NGINX reverse proxy in the cluster that is exposed on the private network by the Ingress ALBs.
- Configure the reverse proxy to point to the private cloud service endpoint for the {{site.data.keyword.satelliteshort}} Link tunnel server.
- Configure the Link connector in your location to send traffic over the {{site.data.keyword.dl_short}} connection to the reverse proxy.

## Prerequisites
{: #dl-prereq}

- [Create](/docs/satellite?topic=satellite-locations) or [identify an existing](/docs/satellite?topic=satellite-satellite-cli-reference#location-ls) {{site.data.keyword.satelliteshort}} location in your on-premises network.
- Ensure that your [{{site.data.keyword.dl_full_notm}}](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) connection can access the `10.X.X.X/8` IP address range.
- [Install the {{site.data.keyword.cloud_notm}} CLI and plug-ins](/docs/containers?topic=containers-cs_cli_install#cs_cli_install).
- [Install the Kubernetes CLI (`kubectl`)](/docs/containers?topic=containers-cs_cli_install#kubectl).
-  Ensure you have the following access policies. For more information, see [Checking user permissions](/docs/openshift?topic=openshift-users#checking-perms).
    - The **Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role for {{site.data.keyword.containerlong_notm}}
    - The **Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role for {{site.data.keyword.registrylong_notm}}
    - The **Writer** or **Manager** {{site.data.keyword.cloud_notm}} IAM service access role for {{site.data.keyword.containerlong_notm}}

## Create an {{site.data.keyword.containerlong_notm}} cluster
{: #dl-cluster}
{: step}

Create an {{site.data.keyword.containerlong_notm}} cluster in your {{site.data.keyword.cloud_notm}} account, which serves as the connection between your on-premises {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}} on the private network.
{: shortdesc}

1. Review the networking basics of [classic {{site.data.keyword.containerlong_notm}} clusters](/docs/containers?topic=containers-plan_clusters#plan_basics) or [VPC {{site.data.keyword.containerlong_notm}} clusters](/docs/containers?topic=containers-plan_clusters#plan_vpc_basics). In particular, ensure that you prepare the following:
    - VLAN management (classic clusters only): Manage and choose both a public and private VLAN for your cluster's network connectivity.
    - Subnet routing: Enable a Virtual Router Function (VRF) or VLAN spanning for your {{site.data.keyword.cloud_notm}} infrastructure account so your worker nodes can communicate with each other on the private network and communicate with private cloud service endpoints internally.
    - IP address schema: Ensure that no subnet conflicts exist between the cluster and your on-premises network.

2. Review the steps you need to take to [prepare to create a cluster](/docs/containers?topic=containers-clusters#cluster_prepare).

3. Create a standard [classic cluster](/docs/containers?topic=containers-clusters#clusters_cli_steps) or [VPC cluster](/docs/containers?topic=containers-clusters#cluster_vpcg2_cli) in the CLI. Create the cluster with the following features.
    - Classic clusters:
        - Zone: Any [multizone-capable zone](/docs/containers?topic=containers-regions-and-zones#zones-mz)
        - Worker node flavor: Any classic infrastructure flavor
        - Network connectivity: Public and private VLANs
        - Worker pool: At least 2 worker nodes
        - Version: 1.20.7 or later
        - Cloud service endpoints: Both public and private endpoints
        - Subnets: Include subnets in the `--pod-subnet` and `--service-subnet` flags if the default ranges conflict with your on-premises subnets
    - VPC clusters:
        - Zone: Any [multizone-capable VPC zone](/docs/containers?topic=containers-regions-and-zones#zones-vpc)
        - Worker node flavor: Any VPC infrastructure flavor
        - Version: 1.20.7 or later
        - Worker pool: At least 2 worker nodes
        - Subnets: Include subnets in the `--pod-subnet` and `--service-subnet` flags if the default ranges conflict with your on-premises subnets
        - Cloud service endpoints: Do _not_ specify the `--disable-public-service-endpoint` flag to ensure that both public and private endpoints are created

4. Spread the default worker pool across zones to increase the availability of your [classic](/docs/containers?topic=containers-add_workers#add_zone) or [VPC](docs/containers?topic=containers-add_workers#vpc_add_zone) cluster. Ensure that at least 2 worker nodes exist in each zone, so that the private ALBs that you configure in subsequent steps are highly available and can properly receive version updates.

5. Set the {{site.data.keyword.containerlong_notm}} cluster as the context for this session.
    ```sh
    ibmcloud ks cluster config --cluster <cluster_name_or_ID>
    ```
    {: pre}

6. Create a namespace for the NGINX reverse proxy.
    ```sh
    kubectl create ns dl-reverse-proxy
    ```
    {: pre}

## Set up private Ingress ALBs in the cluster
{: #dl-ingress}
{: step}

Set up the private Ingress application load balancers (ALBs) for the {{site.data.keyword.containerlong_notm}} cluster, which expose the service for an NGINX reverse proxy on the private network. For more information, you can review the [considerations for exposing an app in your cluster to the private network only](/docs/containers?topic=containers-cs_network_planning#private_access) and specific guidance for [privately exposing apps with Ingress ALBs](/docs/containers?topic=containers-ingress-types#alb-comm-create-private).
{: shortdesc}

1. Verify that at least one ALB with a **Type** of `private` exists in each zone.
    ```sh
    ibmcloud ks alb ls -c <cluster_name_or_ID>
    ```
    {: pre}

2. Ensure that the private ALBs have a **Status** of `enabled`. If they are disabled, run the following command to enable each private ALB.
    ```sh
    ibmcloud ks ingress alb enable classic --alb <ALB_ID> -c <cluster_name_or_ID> --version 0.45.0_1228_iks
    ```
    {: pre}

3. Optional: Disable each public ALB.
    ```sh
    ibmcloud ks ingress alb disable classic --alb <ALB_ID> -c <cluster_name_or_ID>
    ```
    {: pre}

4. Set up a custom domain for the private ALBs that the NGINX reverse proxy is accessible through, and optionally set up TLS for the domain.
    1. Create a custom domain through your DNS service provider. Your custom domain must be 130 characters or fewer to meet Ingress requirements.
    2. Map your custom domain to the private ALBs by adding their IP addresses as A records (classic clusters) or their VPC hostname as a CNAME (VPC clusters). To find the ALB IP addresses (classic) or hostname (VPC), run `ibmcloud ks ingress alb ls -c <cluster_name_or_ID>`. Note that in VPC clusters, a hostname is assigned to the ALBs because the `10.X.X.X` IP addresses are not static and might change over time.
    3. To use TLS termination, create a secret in the `dl-reverse-proxy` namespace that contains a TLS certificate for your custom domain. For example, if a TLS certificate is stored in {{site.data.keyword.cloudcerts_long_notm}} that you want to use, you can import its associated secret into your cluster by running the following command. For more information, see [Using a TLS certificate for a custom subdomain](/docs/containers?topic=containers-ingress-types#manage_certs_custom).
    ```sh
    ibmcloud ks ingress secret create --name <secret_name> --cluster <cluster_name_or_ID> --cert-crn <certificate_crn> --namespace dl-reverse-proxy
    ```
    {: pre}

5. Define an Ingress resource file that uses your custom domain to route incoming network traffic to an `nginxsvc` that you create in subsequent steps. Replace `<custom_ingress_domain>` with the domain that you registered, and `<secret_name>` with the secret that you created for your domain's TLS certificate.
    ```yaml
    apiVersion: networking.k8s.io/v1beta1
    kind: Ingress
    metadata:
      name: dl-ingress-resource
      annotations:
        kubernetes.io/ingress.class: "private-iks-k8s-nginx"
        nginx.ingress.kubernetes.io/proxy-read-timeout: "3600"
        nginx.ingress.kubernetes.io/proxy-send-timeout: "3600"
    spec:
      tls:
      - hosts:
        - <custom_ingress_domain>
        secretName: <secret_name>
      rules:
      - host: <custom_ingress_domain>
      http:
        paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: nginxsvc
              port:
                number: 80
    ```
    {: codeblock}

6. Create the Ingress resource in your cluster.
    ```sh
    kubectl apply -f dl-ingress-resource.yaml -n dl-reverse-proxy
    ```
    {: pre}

The private Ingress ALBs in your cluster are now configured to expose a reverse proxy service with your custom domain on the {{site.data.keyword.cloud_notm}} private network.

## Deploy an NGINX reverse proxy
{: #dl-reverse-proxy}
{: step}

Deploy an NGINX reverse proxy in the cluster that is exposed on the private network by the Ingress ALBs, and configure the reverse proxy to point to the {{site.data.keyword.satelliteshort}} Link tunnel server for your location.
{: shortdesc}

The following steps include editing and using local YAML files to create a configmap, an NGINX deployment, and a service. As an alternative, you can clone an NGINX HTTPS sample repository, such as the [`https-nginx` directory of the `kubernetes/examples` repository](https://github.com/kubernetes/examples/tree/master/staging/https-nginx){: external}, and [push the Docker image to a namespace in {{site.data.keyword.registrylong_notm}}](/docs/Registry?topic=Registry-getting-started). If you use a sample repository, ensure that the NGINX configuration, such as in the `default.conf` file, includes the server block that is specified in step 2 of this section.
{: tip}

1. Get the private service endpoint for the Link tunnel server. In the output, look for the `Address` that is listed for an endpoint of type `location`.
    ```sh
    ibmcloud sat endpoint ls --location <location_ID>
    ```
    {: pre}

    In this example output, the tunnel server endpoint is `c-04.private.us-east.link.satellite.cloud.ibm.com`. Do not include a port.
    ```sh
    ID                           Name                                            Destination Type   Address
    c1hnscnw0h7i5uf0t8eg_zE6Nx   openshift-api-c1muom3w0kfdne2kb37g              location           TCP   c-04.private.us-east.link.satellite.cloud.ibm.com:33809
    c1hnscnw0h7i5uf0t8eg_2F3Xo   openshift-api-c2e3ishw0sdo08f5902g              location           TCP   c-04.private.us-east.link.satellite.cloud.ibm.com:34222
    c1hnscnw0h7i5uf0t8eg_EczUw   satellite-cos-c1hnscnw0h7i5uf0t8eg              cloud              TLS   m65f0b26d6c5f695647f5-6b64a6ccc9c596bf59a86625d8fa2202-c000.us-east.satellite.appdomain.cloud:30235
    c1hnscnw0h7i5uf0t8eg_56zpT   satellite-cosCrossRegion-c1hnscnw0h7i5uf0t8eg   cloud              TLS   m65f0b26d6c5f695647f5-6b64a6ccc9c596bf59a86625d8fa2202-c000.us-east.satellite.appdomain.cloud:31774
    ...
    ```
    {: screen}

2. Create a configmap for the NGINX reverse proxy and save the file as `confnginx.yaml`. In the `server` block, replace `<custom_ingress_domain>` with the domain that you registered for the private Ingress ALBs and `<tunnel_server_ep>` with the tunnel server endpoint that you found in step 1.
    ```yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: confnginx
    data:
      nginx.conf: |
        user  nginx;
        worker_processes  1;
        error_log  /var/log/nginx/error.log warn;
        events {
            worker_connections  1024;
        }
        http {
          include       /etc/nginx/mime.types;
          default_type  application/octet-stream;
          log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                              '$status $body_bytes_sent "$http_referer" '
                              '"$http_user_agent" "$http_x_forwarded_for"';
          access_log  /var/log/nginx/access.log  main;
          sendfile        on;
          keepalive_timeout  65;
          server {
            listen 80;

            server_name <custom_ingress_domain>;

            proxy_connect_timeout 180;
            proxy_send_timeout 180;
            proxy_read_timeout 180;

            location / {
              proxy_pass https://<tunnel_server_ep>;
              proxy_ssl_server_name on;
              proxy_http_version 1.1;
              proxy_set_header Upgrade $http_upgrade;
              proxy_set_header Connection "upgrade";
            }
          }
        }
    ```
    {: codeblock}

3. Create the configmap in your {{site.data.keyword.containerlong_notm}} cluster.
    ```sh
    kubectl apply -f confnginx.yaml -n dl-reverse-proxy
    ```
    {: pre}

4. Create a deployment configuration for the NGINX reverse proxy and a service so that the deployment is included in the private Ingress load balancing. Save the file as `nginx-app.yaml`.
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: nginx
      labels:
        app: nginx
    spec:
      selector:
        matchLabels:
          app: nginx
      replicas: 2
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
            - name: nginx
              image: nginx:alpine
              ports:
              - containerPort: 80
              volumeMounts:
                - name: nginx-config
                  mountPath: /etc/nginx/nginx.conf
                  subPath: nginx.conf
          volumes:
            - name: nginx-config
              configMap:
                name: confnginx
    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: nginxsvc
      labels:
        app: nginx
    spec:
      type: NodePort
      ports:
      - port: 80
        protocol: TCP
        name: http
      - port: 443
        protocol: TCP
        name: https
      - port: 8080
        protocol: TCP
        name: tcp
      selector:
        app: nginx
    ```
    {: codeblock}

5. Create the deployment and service in your {{site.data.keyword.containerlong_notm}} cluster.
    ```sh
    kubectl apply -f nginx-app.yaml -n dl-reverse-proxy
    ```
    {: pre}

The reverse proxy is now configured to terminate incoming connections to your custom Ingress subdomain, open a new connection to the {{site.data.keyword.satelliteshort}} Link tunnel server, and forward requests to the tunnel server.

## Configure Link to use the reverse proxy
{: #dl-config-satlink}
{: step}

Configure the Link connector in your {{site.data.keyword.satelliteshort}} location to send traffic over the {{site.data.keyword.dl_short}} connection to the reverse proxy.
{: shortdesc}

1. Edit the endpoint for your location's Link tunnel server.
    ```sh
    curl -X PATCH "https://api.link.satellite.cloud.ibm.com/v1/locations/<location_ID>" -H "accept: application/json" -H "Authorization: Bearer <IAM_token>" -H "Content-Type: application/json" -d "{\"ws_endpoint\":\"<custom_ingress_domain>\"}"
    ```
    {: pre}

    | Component | Description |
    | -------------- | -------------- |
    | `<location_ID>` | The ID of your {{site.data.keyword.satelliteshort}} location, which you can find by running `ibmcloud sat location ls`. |
    | `<IAM_token>` | Your {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) token, which you can find by running `ibmcloud iam oauth-tokens`. |
    | `<custom_ingress_domain>` | The custom domain that you registered for the private Ingress ALBs. |
    {: caption="Table 1. Understanding this request's components" caption-side="top"}

2. To ensure that the {{site.data.keyword.satelliteshort}} Link configuration change was successful, verify that the location's **State** is `normal`.
    ```sh
    ibmcloud sat location get --location <location_name_or_ID>
    ```
    {: pre}

3. Verify that traffic flows from your {{site.data.keyword.satelliteshort}} location to {{site.data.keyword.cloud_notm}} by [creating a Link endpoint of type `cloud`](/docs/satellite?topic=satellite-link-location-cloud#link-cloud). For example, you might create a `cloud` endpoint for the private service endpoint of an {{site.data.keyword.cloud_notm}} service, and then [test the connection to the service from a {{site.data.keyword.openshiftlong_notm}} cluster that runs in your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-link-location-cloud#link-cloud-test).

4. Verify that traffic flows from {{site.data.keyword.cloud_notm}} to your {{site.data.keyword.satelliteshort}} location by [creating a Link endpoint of type `location`](/docs/satellite?topic=satellite-link-location-cloud#link-location). For example, you might create a `location` endpoint for the IP address of an app in a {{site.data.keyword.satelliteshort}} cluster in your location, and then test the connection to the app from a service in the {{site.data.keyword.cloud_notm}} private network.

You've successfully configured your {{site.data.keyword.satelliteshort}} Link setup so that all traffic that flows over Link endpoints now uses your {{site.data.keyword.dl_short}} connection!

TODO: what happens if customers add hosts to their location after these steps? Do they need to revert the ws endpoint first, add the hosts, and then change the ws endpoint again? Test from Dalal to come.

</staging>




