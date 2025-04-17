---


copyright:
  years: 2020, 2025
lastupdated: "2025-04-17"

keywords: satellite, hybrid, multicloud, direct link, secure direct link

subcollection: satellite

content-type: tutorial
services: satellite, containers, dl
account-plan: paid
completion-time: 2h

---

{{site.data.keyword.attribute-definition-list}}


# Connecting Locations with {{site.data.keyword.cloud_notm}} using {{site.data.keyword.dl_short}}
{: #direct-link-tutorial}
{: toc-content-type="tutorial"}
{: toc-services="satellite, containers, dl"}
{: toc-completion-time="2h"}

Supported location types
:   Red Hat CoreOS (RHCOS)-enabled Locations and Connectors

Supported host operating systems
:   Red Hat CoreOS (RHCOS) and RHEL

Use a secure {{site.data.keyword.dl_full}} connection for {{site.data.keyword.satelliteshort}} Link communications between your services running in an {{site.data.keyword.satellitelong}} Location and {{site.data.keyword.cloud}}.
{: shortdesc}

In this tutorial, you set up your {{site.data.keyword.satelliteshort}} Link to use a {{site.data.keyword.dl_short}} connection. The Link tunnel client at your Location sends traffic over the {{site.data.keyword.dl_short}} connection to a Relay that you create in your  {{site.data.keyword.cloud_notm}} account. This Relay proxies the traffic to Link tunnel server's IP address in the {{site.data.keyword.cloud_notm}} private network.

## FAQ
{: #faq-direct-link}

Is the cost of the relay compute resources included in the {{site.data.keyword.satelliteshort}} service costs?
:   The {{site.data.keyword.cloud_notm}} resources used for the relay and {{site.data.keyword.satelliteshort}} are billed separately.

Are there additional charges to access {{site.data.keyword.cloud_notm}} services over Direct Link?
:   No, there are not additional charges for accessing services over Direct Link. 

Why do I need Direct Link?
:   Normally, outbound traffic from your Location to {{site.data.keyword.cloud_notm}} services might flow over the public internet. When you use Direct Link, outbound traffic from your Location flows through the Direct Link, rather than using the public Internet.

My organization disables Internet access by design. Can I create and maintain Locations and hosts attached to the Location with Direct Link?
:   If you have Direct Link, you can use it for {{site.data.keyword.satelliteshort}} services. With Direct Link, you can create Locations and attach hosts without access to public Internet.

Can I use RHEL hosts to set up my Direct Link?
:   No. You must have both an RHCOS-enabled location and you must use RHCOS hosts in your location to use Direct Link.

Can I redirect all traffic to {{site.data.keyword.cloud_notm}} over Direct Link instead of Internet?
:   Currently, not all services support Direct Link. So, depending on the services you use it might or might not be possible for all traffic to use Direct Link.

What {{site.data.keyword.cloud_notm}} services can I access over Direct Link to avoid accessing them over Internet?
:   After following these instructions, {{site.data.keyword.satelliteshort}} and OpenShift on {{site.data.keyword.satelliteshort}} will work across Direct Link. Additional services deployed into a {{site.data.keyword.satelliteshort}} location might have features that require public Internet access. It is recommended to consult the documentation for each service running in a location to verify their connectivity requirements.

If I have two Locations that use Direct Link, can I use them for Direct Link to fail over from one Location to the other?
:   This functionality is not yet available.

How do I size Direct Link capacity for my Location?
:   There are no additional sizing requirements for using Direct Link. So, you can size your Location like a normal Location, meaning based on the services you will use.  

Can I have one-click deployment of everything needed to enable Direct Link to avoid manual errors?
:   Currently, a one-click deployment for Direct Link is not available. It might be available at a future time.

## Target use case
{: #target-use-case}

Customers who are currently using {{site.data.keyword.dl_short}} between IBM and on-prem or other public clouds, can continue to use it for {{site.data.keyword.satelliteshort}} Link. This allows customers to:

- Access services on {{site.data.keyword.cloud_notm}} from a {{site.data.keyword.satelliteshort}} location over {{site.data.keyword.dl_short}}; examples are backups in {{site.data.keyword.cos_full}}, sending metrics to {{site.data.keyword.mon_full_notm}}, tracking events in {{site.data.keyword.cloudaccesstraillong_notm}}, or sending logs to {{site.data.keyword.loganalysislong_notm}}.
- Access services running in a {{site.data.keyword.satelliteshort}} location from {{site.data.keyword.cloud_notm}}.
- Access public cloud services outside of {{site.data.keyword.cloud_notm}}.

These can be accessed using {{site.data.keyword.satelliteshort}} endpoints addresses created to route traffic over {{site.data.keyword.dl_short}} instead of the internet.

This prevents customer’s sensitive data going over the public internet such as logging, backups or data between integrated services across the hybrid cloud landscape. This also helps optimize ingress/egress charges.

## Overview
{: #dl-overview}

By default, two {{site.data.keyword.satelliteshort}} Link components, the tunnel server and the connector, proxy network traffic between {{site.data.keyword.cloud_notm}} and resources in your {{site.data.keyword.satelliteshort}} location over a secure TLS connection. This document covers the use case of using a TLS connection over Direct Link.

This setup uses the tunnel server's private cloud service endpoint to route traffic over the {{site.data.keyword.cloud_notm}} private network(`166.9.0.0/8`, see [Service network](/docs/infrastructure-hub?topic=infrastructure-hub-ibm-cloud-ip-ranges#service-network). However, communication to the tunnel server's private cloud service endpoint must go through the 166.9.X.X/16 IP address range on the {{site.data.keyword.cloud_notm}} private network, which is not routable from {{site.data.keyword.dl_full_notm}}.

To enable access to the `166.9.X.X/16` range, create a Relay in your {{site.data.keyword.cloud_notm}} account, which will reverse proxy incoming traffic to the tunnel server's private cloud service endpoint. By default, the Relay Ingress has an IP address in the internal `10.X.X.X/8` IP address range, which is accessible via a {{site.data.keyword.dl_short}} connection.

The following diagram demonstrates the flow of traffic.

![{{site.data.keyword.satelliteshort}} Link setup that uses a {{site.data.keyword.dl_short}} connection and a reverse proxy](/images/sat_dl_architecture.svg){: caption="Satellite Link setup that uses a DirectLink connection" caption-side="bottom"}
  
1. Network traffic originating at your Location, such as a request from an {{site.data.keyword.satellitelong_notm}} cluster to an {{site.data.keyword.cloud_notm}} service, is routed via Link Service over Direct Link to the Relay Private Ingress, which has a Direct link routable address.
2. The Relay initiates a new session to forward the request to the private cloud service endpoint of the tunnel server, which terminates to an IP address in the `166.9.X.X/16` range (Link private address).

## Objectives
{: #dl-objectives}

You can create Red Hat CoreOS enabled Locations without accessing public Internet. All traffic is handled by {{site.data.keyword.dl_short}} and stays internal.
{: shortdesc}

High level steps include:

1. Create a Red Hat CoreOS enabled {{site.data.keyword.satelliteshort}} Location with your {{site.data.keyword.cloud_notm}} account that terminates your {{site.data.keyword.dl_short}}.
2. Create a relay, which is a reverse proxy that supports http/https and secure websocket.
3. Provision Red Hat CoreOS hosts. Customize the hosts by using ignition script that are downloaded as attach script for the Location created in Step 1.

## Prerequisites
{: #dl-prereq}

- You must have a Red Hat CoreOS enabled {{site.data.keyword.satelliteshort}} Location. If you don't have one already, follow the instructions in [Creating a Red Hat CoreOS enabled {{site.data.keyword.satelliteshort}} Location](#dl-create-coreos-location) to create it.
- {{site.data.keyword.dl_short}} is available between target {{site.data.keyword.satelliteshort}} location and {{site.data.keyword.cloud_notm}} specific VPC or classic clusters.
- Ensure that your [{{site.data.keyword.dl_full_notm}}](/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) connection can access the `10.X.X.X/8` IP address range. Review network design to avoid IP conflicts between two ends of {{site.data.keyword.dl_short}}.
- [Install the {{site.data.keyword.cloud_notm}} CLI and plug-ins](/docs/satellite?topic=satellite-cli-install) and [install the Kubernetes CLI (`kubectl`)](/docs/containers?topic=containers-cli-install).
- Ensure that your {{site.data.keyword.cloud_notm}} account is Virtual Router Function (VRF) enabled to use service endpoints.
- Ensure you have the following access policies. For more information, see [Checking user permissions](/docs/satellite?topic=satellite-iam-assign-access#checking-perms).
    - **Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role for {{site.data.keyword.containerlong_notm}}
    - **Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role for {{site.data.keyword.registrylong_notm}}
    - **Writer** or **Manager** {{site.data.keyword.cloud_notm}} IAM service access role for {{site.data.keyword.containerlong_notm}}
    - **Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role for {{site.data.keyword.registrylong_notm}}
    - **Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role for {{site.data.keyword.satellitelong_notm}}
    - **Manager** {{site.data.keyword.cloud_notm}} IAM service access role for {{site.data.keyword.satellitelong_notm}}
    - **Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role for {{site.data.keyword.cos_short}}
    - **Writer** or **Manager** {{site.data.keyword.cloud_notm}} IAM platform access role for {{site.data.keyword.cos_full_notm}}
    - **Administrator** {{site.data.keyword.cloud_notm}} IAM platform access role for {{site.data.keyword.cloudcerts_long_notm}}
    - **Writer** or **Manager** {{site.data.keyword.cloud_notm}} IAM platform access role for {{site.data.keyword.cloudcerts_long_notm}}
    - **Viewer** {{site.data.keyword.cloud_notm}} IAM platform access role for the resource group that you plan to use with {{site.data.keyword.satelliteshort}}
    - **Manager** {{site.data.keyword.cloud_notm}} IAM service access role for {{site.data.keyword.bplong_notm}}
- Specifically provision a Kubernetes cluster and deploy NGINX reverse proxy in it to forward to the {{site.data.keyword.dl_short}} endpoints. 
  
## Creating a Red Hat CoreOS enabled {{site.data.keyword.satelliteshort}} Location 
{: #dl-create-coreos-location}
{: step}  

You can skip this step if you already have a Red Hat CoreOS enabled {{site.data.keyword.satelliteshort}} Location.
{: note}

Log in to your {{site.data.keyword.cloud_notm}} account that has {{site.data.keyword.dl_short}} and create a Red Hat CoreOS enabled {{site.data.keyword.satelliteshort}} Location. For more information, see [Creating a Satellite location](/docs/satellite?topic=satellite-locations#verify-coreos-location).
{: shortdesc}
  
## Creating a relay 
{: #dl-create-coreos-relay}  
{: step}

The relay is an http/https reverse proxy that supports secure Websocket connections. It can run on VSI, {{site.data.keyword.redhat_openshift_notm}} or {{site.data.keyword.containerlong_notm}} as Classic or VPC. The following steps demonstrates an example for deploying NGINX reverse proxy on a private-only VPC {{site.data.keyword.redhat_openshift_notm}} cluster (on VPC private nodes). 
{: shortdesc}
  
One essential requirement is to have a valid name that can be assigned to the cluster private ingress (Relay Ingress) and a valid certificate on {{site.data.keyword.cloud_notm}}. On {{site.data.keyword.cloud_notm}}, VPC {{site.data.keyword.redhat_openshift_notm}} clusters on private nodes come with default private host name and certificate. You can use them or bring your custom host name and certificate. This example uses the default private host name and certificates.  
  
VPC clusters considerations for this scenario:

- Zone: Any multizone-capable VPC zone
- Worker node flavor: Any VPC infrastructure flavor
- Version: 4.x.x
- Worker pool: At least 2 worker nodes
- Subnets: Include Ingress Load Balancer IP subnets if the default ranges conflict with the `--pod-subnet` and `--service-subnet` values of the {{site.data.keyword.redhat_openshift_notm}} cluster on Satellite or the network CIDR where the Satellite or {{site.data.keyword.redhat_openshift_notm}} hosts are deployed on-premises.
- Cloud service endpoints: Do not specify the `--disable-public-service-endpoint` option if you want both public and private endpoints. 
- Spread the default worker pool across zones to increase the availability of your classic or VPC cluster.
- Ensure that at least 2 worker nodes exist in each zone, so that the private ALBs that you configure in subsequent steps are highly available and can properly receive version updates.

In the following example, a private-only VPC cluster and private Ingress controller are created by default. However, you can also use a {{site.data.keyword.redhat_openshift_notm}} cluster with a public cloud service endpoint enabled, but in this case your cluster is created with only a public Ingress controller by default. If you want to set up your relay by using a cluster with a public service endpoint, you must first enable the private Ingress controller and register it with a subdomain and certificate by following the steps in [Setting up Ingress](/docs/openshift?topic=openshift-ingress-roks4).
{: note}

1. Create a private-only {{site.data.keyword.redhat_openshift_notm}} cluster on VPC. For more information, see [Creating VPC clusters](/docs/openshift?topic=openshift-cluster-create-vpc-gen2&interface=ui). 

    There are many ways to expose apps in {{site.data.keyword.redhat_openshift_notm}} cluster in a VPC. In this example, the app will be privately exposed with private endpoints only, which is the most common use case for {{site.data.keyword.dl_short}} customers. {{site.data.keyword.redhat_openshift_notm}} clusters that are privately exposed with private endpoints only come with default private name and certificate. They will be used in this example to expose the NGINX reverse proxy pods. You can use the default ones or bring your custom host name and certificate. For more details, see [Privately exposing apps in VPC clusters with a private cloud service endpoint only](/docs/openshift?topic=openshift-ingress-private-expose#priv-se-priv-controller).
    {: note}
    
1. Create a Secret Manager instance and register it to the {{site.data.keyword.redhat_openshift_notm}} cluster that was created in the previous step. For more information, see [Creating a Secrets Manager service instance](/docs/secrets-manager?topic=secrets-manager-create-instance&interface=ui).

1. Get the Ingress details from {{site.data.keyword.dl_short}}.
    ```sh
    ibmcloud oc cluster get --cluster CLUSTER_NAME_OR_ID | grep Ingress
    ```
    {: pre}   

    Example output:
    ```sh
    Ingress Subdomain:      mycluster-i000.us-south.containers.appdomain.cloud
    Ingress Secret:         mycluster-i000
    ```
    {: screen}

    In this scenario, if you run the **`nslookup`** command to the Ingress Subdomain, it resolves to IBM service private IP address (`10.0.0.0/8`). Adding routes to make the Ingress IP address (`10.0.0.0/8`) reachable from customer on-prem is not covered in this document. You are responsible for facilitating routing between on-prem and the Ingress relay on {{site.data.keyword.cloud_notm}}.
    {: note}

1. Get the secret CRN.
    ```sh
    ibmcloud oc ingress secret get -c CLUSTER --name SECRET_NAME --namespace openshift-ingress
    ```
    {: pre}
    
1. Create a namespace for the NGINX reverse proxy.
    ```sh
    kubectl create ns dl-reverse-proxy
    ```
    {: pre} 

1. Copy the default TLS secret from `openshift-ingress` to the project where NGINX is going to deployed.
    ```sh
    ibmcloud oc ingress secret create --cluster CLUSTER_NAME_OR_ID --cert-crn CRN --name SECRET_NAME --namespace dl-reverse-proxy 
    ```
    {: pre}  
  
1. Copy the following Ingress resource file content into your local directory. Replace `VALUE_FROM_INGRESS_SUBDOMAIN` and `VALUE_FROM_INGRESS_SECRET` with your own values.
    ```yaml
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: dl-ingress-resource
      annotations:
        kubernetes.io/ingress.class: "public-iks-k8s-nginx"
        nginx.ingress.kubernetes.io/proxy-read-timeout: "3600"
        nginx.ingress.kubernetes.io/proxy-send-timeout: "3600"
    spec:
      tls:
      - hosts:
        - satellite-dl.VALUE_FROM_INGRESS_SUBDOMAIN
        secretName: VALUE_FROM_INGRESS_SECRET
      rules:
      - host: satellite-dl.VALUE_FROM_INGRESS_SUBDOMAIN
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
    
1. Create the Ingress.
    ```sh
    oc apply -f myingressresource.yaml -n <dl-reverse-proxy>
    ```
    {: pre} 

1. Get the tunnel server {{site.data.keyword.dl_short}} internal Ingress host name by running the following command.
    ```sh
    ibmcloud sat endpoint ls --location LOCATION_ID
    ```
    {: pre}   
  
1. From the output, take a note of the Location endpoint. Replace `c-01`, `c-02`, or `c-03` with `d-01-ws`, `d-02-ws`, or `d-03-ws` and remove the port. For example, `c-01.private.us-south.link.satellite.cloud.ibm.com:40934` becomes `d-01-ws.private.us-south.link.satellite.cloud.ibm.com`. This value can be used as the value for `proxy_pass https` in the ConfigMap file.

1. Copy the NGINX ConfigMap file content into your local directory. This configuration either applies ws-reverse proxy or https reverse proxy to the tunnel server {{site.data.keyword.dl_short}} endpoint. Replace `VALUE_FROM_INGRESS_SUBDOMAIN` and `VALUE_FOR_PROXY_PASS` with your own values.
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
            worker_connections  4096;
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
          server_names_hash_bucket_size  128;
          server {
            listen 80;
            server_name VALUE_FROM_INGRESS_SUBDOMAIN;
            proxy_connect_timeout 180;
            proxy_send_timeout 180;
            proxy_read_timeout 180;
            location /ws {
              proxy_pass https://VALUE_FOR_PROXY_PASS; 
              proxy_ssl_server_name on;
              proxy_http_version 1.1;
              proxy_set_header Upgrade $http_upgrade;
              proxy_set_header Connection "upgrade";
            }
            location / {
              proxy_pass https://VALUE_FOR_PROXY_PASS;
            }
          }
        }
    ```
    {: codeblock}
 
1. Copy the NGINX deployment file.
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
  
1. Create the ConfigMap of the NGINX (`dl-reverse-proxy`).
    ```sh
    oc apply -f confnginx.yaml -n dl-reverse-proxy
    ```
    {: pre}    
  
1. Set the correct `scc` profile and create the NGINX (`dl-reverse-proxy`).
    ```sh
    oc adm policy add-scc-to-user anyuid system:serviceaccount:dl-reverse-proxy:default
    oc apply -f nginx-app.yaml -n dl-reverse-proxy
    ```
    {: pre}
 
1. Double check that the NGINX is running by listing pods.
    ```sh
    oc get pods
    ```
    {: pre}

    ```sh
    NAME                     READY   STATUS    RESTARTS   AGE
    nginx-757fbc9f85-gv2p6   1/1     Running   0          53s
    nginx-757fbc9f85-xvmrj   1/1     Running   0          53s
    ```
    {: screen}   

1. Check logs.
    ```sh
    oc logs -f nginx-757fbc9f85-gv2p6
    ```
    {: pre}

    ```sh
    /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
    /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
    /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
    10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
    10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
    /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
    /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
    /docker-entrypoint.sh: Configuration complete; ready for start up
    ```
    {: screen} 

1. Check Ingress.
    ```sh
    oc get ingress
    ```
    {: pre}

    ```sh    
    NAME                  CLASS    HOSTS                                                                                                       ADDRESS                                                                                                     PORTS     AGE
    dl-ingress-resource   <none>   mysatellite-dl.myname-cluster10-22bfd3cd491bdeb5a0f661fb1e2b0c44-0000.us-south.containers.appdomain.cloud   router-default.myname-cluster10-22bfd3cd491bdeb5a0f661fb1e2b0c44-0000.us-south.containers.appdomain.cloud   80, 443   19m
    ```
    {: screen} 

1. Connect to the reverse proxy URL.
    ```sh
    curl -k https://mysatellite-dl.myname-cluster10-22bfd3cd491bdeb5a0f661fb1e2b0c44-0000.us-south.containers.appdomain.cloud
    ```
    {: pre}

    ```sh   
    {"status":"UP"}
    ```
    {: screen} 
    
  
## Redirect the traffic to use the {{site.data.keyword.dl_short}} Path
{: #dl-use-direct-link-path}
{: step}

Now that the relay is ready to proxy incoming traffic to the tunnel server internal Ingress, you can set up your Location host or Connector to redirect its traffic through the relay. This ensures that all the traffic will stay on the Direct Link path in your private network and no traffic uses the public internet.

Redirect traffic for your Connector agent or Location host by following the applicable instructions below.

### Using a Connector agent (Docker or Windows)
{: #dl-connector-agents}

Follow the instructions in [Configuring a Tunnel server Ingress host for your Satellite Connector agent](/docs/satellite?topic=satellite-connector-agent-path#config-connector-ingress) but set the `SATELLITE_CONNECTOR_DIRECT_LINK_INGRESS` parameter to the relay Ingress host created in step 2 (`mysatellite-dl.myname-cluster10-22bfd3cd491bdeb5a0f661fb1e2b0c44-0000.us-south.containers.appdomain.cloud`) instead of to the internal Ingress host itself. For example:

- On a container platform, in your `env.txt` file.

    ```txt
    SATELLITE_CONNECTOR_DIRECT_LINK_INGRESS=mysatellite-dl.myname-cluster10-22bfd3cd491bdeb5a0f661fb1e2b0c44-0000.us-south.
    ```

- On Windows, in your `config.json` file.

    ```json
    "SATELLITE_CONNECTOR_DIRECT_LINK_INGRESS": "mysatellite-dl.myname-cluster10-22bfd3cd491bdeb5a0f661fb1e2b0c44-0000.us-south.containers.appdomain.cloud"
    ```

### Using a Location Host (CoreOS or RHEL)
{: #dl-provision-coreos-hosts}

1. Run the following CLI command to download the host attachment script for your Location.

    ```sh
    ibmcloud sat host attach --location LOCATION --operating-system SYSTEM --host-link-agent-endpoint ENDPOINT
    ```
    {: pre}

    `--location LOCATION`
    :    The name or ID of the Satellite location.

    `--operating-system SYSTEM`
    :    The operating system of the hosts you want to attach to your location (RHEL or RHCOS).

    `--host-link-agent-endpoint ENDPOINT`
    :    The endpoint that the link agent uses to connect to the link tunnel server. In this case, the relay Ingress host created in step 2 (`mysatellite-dl.myname-cluster10-22bfd3cd491bdeb5a0f661fb1e2b0c44-0000.us-south.containers.appdomain.cloud`).

1. Attach the host agent by following the applicable instructions for your host operating system in [Attaching on-prem hosts to your location](https://cloud.ibm.com/docs/satellite?topic=satellite-attach-hosts).
