---

copyright:
  years: 2022, 2022
lastupdated: "2022-11-18"

keywords: satellite, http proxy, http, proxy, mirror

subcollection: satellite

---


{{site.data.keyword.attribute-definition-list}}

# Configuring an HTTP proxy for your {{site.data.keyword.satelliteshort}} hosts
{: #config-http-proxy}

You can configure an HTTP proxy so that all outbound traffic from your {{site.data.keyword.satelliteshort}} hosts is routed through the proxy.
{: shortdesc}

Setting up an HTTP proxy is available only for allowlisted accounts.
{: important}

## What type of location do I need to use HTTP proxy?
{: #consider-http-proxy}

Consider the following types of locations.

Existing RHEL-based locations
:   To set up a proxy, your location must be enabled for Red Hat CoreOS (RHCOS). If your existing location is not RHCOS-enabled, then you can't configure an HTTP proxy. Create a RHCOS-enabled location, then [configure your HTTP proxy](#http-proxy-config).

Existing Red Hat CoreOS-enabled locations with attached hosts
:   To set up an HTTP proxy, you must first [remove your hosts](/docs/satellite?topic=satellite-host-remove) from your location. After you remove your hosts, see [Configuring your HTTP proxy](#http-proxy-config). Note that you must also update the hosts that make up your location control plane. See [Updating Satellite location control plane hosts](/docs/satellite?topic=satellite-host-update-location).

New Red Hat CoreOS-enabled locations
:   Before attaching your hosts to your location, [configure your HTTP proxy](#http-proxy-config).



## What type of hosts can I use?
{: #consider-http-proxy-host}

You can use RHEL or Red Hat CoreOS hosts when you set up an HTTP proxy. You must edit each host that is attached to your location, including the hosts that make up the control plane.

## What else do I need to know about HTTP proxy?
{: #additional-http-proxy}

For your Satellite location and clusters to work with a proxy, the kubelet on the control plane infrastructure nodes that are deployed to a Satellite location must be able to communicate to the IBM Cloud control plane node API server. To enable this communication, you must meet one of the following requirements.

- Option 1: The current proxy can support long lived TCP connections (TCP tunneling).

- Option 2: You can create a secondary proxy on a VSI in same network as your Satellite hosts that supports long lived TCP connections.

- Option 3: You can open the firewall outbound to allow the TCP connections. For more information, see [Required outbound connectivity for hosts in all regions]{/docs/satellite?topic=satellite-reqs-host-network-outbound) and then find the specific outbound network requirements for your region.

You cannot configure an HTTP proxy for worker to master communications or for connecting to the package mirrors.
{: note}

## Setting up TCP tunneling
{: #setup-tcp-http-proxy}

Your proxy must be set up with TCP tunneling. While specific steps might vary depending on your provider, follow these general steps to set up TCP tunneling.

1. Set up your HTTP proxy to tunnel traffic for all four of your location public service endpoints. To find your endpoints, 
        
    ```sh
    ibmcloud sat location get --location LOCATION_NAME
    ```
    {: pre}
        
    In the output, find the field **Public Service Endpoint URL**. From that field, you can derive the endpoints. For example, if the field value is `https://c131-e.us-south.satellite.cloud.ibm.com:31726`, then the endpoints are `https://c131-1.us-south.satellite.cloud.ibm.com:31726`, `https://c131-2.us-south.satellite.cloud.ibm.com:31726` and `https://c131-3.us-south.satellite.cloud.ibm.com:31726`.
        
2. Make sure the listener port on the HTTP proxy is the same as on {{site.data.keyword.cloud_notm}}.
3. Update the `/etc/hosts` on all your {{site.data.keyword.satelliteshort}} hosts to include the location public service endpoints forward traffic to the proxy, rather than to {{site.data.keyword.cloud_notm}} endpoints.

Your configuration might vary by provider. Consider setting up your proxy outside of the {{site.data.keyword.satelliteshort}} environment to ensure that the configuration works for your infrastructure. Then, configure your proxy in the {{site.data.keyword.satelliteshort}} environment. For more information about setting up and configuring your HTTP proxy, see the blog [Proxying In Cluster Kube-APIServer Traffic in IBM Cloud Satellite](https://lisowski0925.medium.com/proxying-in-cluster-kube-apiserver-traffic-in-ibm-cloud-satellite-162ee07d6e0d){: external}.
{: tip}

## Requesting access to the allowlist
{: #access-http-proxy}

To gain access to the allowlist for HTTP proxy, [create a ticket](https://cloud.ibm.com/unifiedsupport/cases/form){: external} with IBM Support.

For example, use the following request as a template.

```txt
Title: Request for addition of HTTP_PROXY config to 
       location <LOCATION_ID>

Request Body:
We are requesting the following HTTP_PROXY info be added to 
the location_ID listed in the title of this ticket.

Use the following HTTP_PROXY info 
BE SURE to include the protocol (http:// or https://) 
AND the port (`:PORT_NUMBER`) in the endpoint.


HTTP_PROXY: https://my-proxy-endpoint.com:PORT_NUMBER
HTTPS_PROXY: https://my-proxy-endpoint.com:PORT_NUMBER
```
{: screen}
    
After support processes the ticket, you will receive a notification that your location is updated. If a change is required, a new ticket must be opened stating the new parameters. To find your `LOCATIONID` by running `ibmcloud sat locations`.


## Configuring your HTTP proxy
{: #http-proxy-config}

To configure an HTTP proxy, you must edit each of your hosts, including the hosts in the control plane. If your hosts are already attached to a location, including those hosts that make up the control plane, you must [remove them from the location](/docs/satellite?topic=satellite-host-remove) before you can edit them. After you configure the proxy, [reattach your hosts to the location](/docs/satellite?topic=satellite-attach-hosts). For information about updating your control plane hosts, see [Updating Satellite location control plane hosts](/docs/satellite?topic=satellite-host-update-location).

1. Choose a [mirror location](#common-mirror-locations) you want to use for a proxy. This mirror location is used when you set up your proxy.
2. Find the value for `NO_PROXY`. 
    - For control plane hosts, use `172.20.0.1` for RHCOS and `NO_PROXY=172.20.0.1,$<REDHHAT_PACKAGE_MIRROR_LOCATION>` for RHEL.
    - For {{site.data.keyword.redhat_openshift_notm}} hosts, the `NO_PROXY` for {{site.data.keyword.redhat_openshift_notm}} hosts must include the first IP of the service subnet that is used for the {{site.data.keyword.redhat_openshift_notm}} cluster. To find this IP, run the **`cluster get`** command.
        
        ```sh
        ibmcloud ks cluster get --cluster <ClusterID>
        ```
        {: pre}
        
        Example output
        
        ```sh
        Name:                           hyp-20220306-1-d2   
        ID:                             <ClusterID>  
        ...
        Service Subnet:                 172.21.0.0/16 
        ...
        ```
        {: screen}
        
        From this output, note that the first IP is `172.21.0.1`, which makes the full output for hosts associated with this specific cluster in this example `NO_PROXY=172.20.0.1,172.21.0.1,$REDHAT_PACKAGE_MIRROR_LOCATION` for RHEL hosts and `NO_PROXY=172.20.0.1,172.21.0.1,.REGION.satellite.appdomain.cloud` for RHCOS hosts. For example, `NO_PROXY=172.20.0.1,172.21.0.1,.eu-gb.satellite.appdomain.cloud` is correct for a London mirror location for RHCOS hosts. Note that the RHCOS value includes `.` before the region.
        
        Remember that any traffic to cluster services from the worker node must be included in `NO_PROXY`. For example, to use the image registry service to store images,  add `image-registry.openshift-image-registry.svc` to `NO_PROXY` for each worker node; this value doesn't need to be included for the control plane.



3. Navigate to `/etc/systemd/system.conf.d` on your host. If that file does not exist, create it. Enter the `<VALUE>` for `NO_PROXY` from step 2.
    
    ```sh
    mkdir -p /etc/systemd/system.conf.d
    cat >"/etc/systemd/system.conf.d/proxy.conf" <<EOF
    [Manager]
    DefaultEnvironment="HTTP_PROXY=xxxx" "HTTPS_PROXY=xxxx" "NO_PROXY=<VALUE>"
    EOF
    chmod 0644 /etc/systemd/system.conf.d/proxy.conf
    ```
    {: pre}
    
4. Navigate to the `/etc/environment` file on your host. Enter the `<VALUE>` for `NO_PROXY` from step 2. If that file does not exist, create it. 
    
    ```sh
    mkdir -p /etc/profile.d
    cat >"/etc/profile.d/ibm-proxy.sh" <<EOF
    #!/usr/bin/env bash
    HTTP_PROXY="xxxx"
    HTTPS_PROXY="xxxx"
    NO_PROXY="<VALUE>"
    export HTTP_PROXY
    export HTTPS_PROXY
    export NO_PROXY
    EOF
    chmod 0755 /etc/profile.d/ibm-proxy.sh
    ```
    {: pre}
    
5. Reboot your host to pick up this change.
6. [Attach or reattach](/docs/satellite?topic=satellite-attach-hosts) your host to the location.
7. Assign the host back to the [control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or to [the service](/docs/satellite?topic=satellite-assigning-hosts) where it was previously assigned.
8. Repeat these steps for each host.
    
The value for `REDHHAT_PACKAGE_MIRROR_LOCATION` depends on the location of the Red Hat package mirrors. The `REDHHAT_PACKAGE_MIRROR_LOCATION` can be a wild-card if multiple mirrors are used. For more information, see [How to apply a system wide proxy](https://access.redhat.com/solutions/1351253){: external}.
{: note}

## Common mirror locations
{: #common-mirror-locations}

The following list provides some common mirror locations. 

Azure: 
:    separately defining: `rhui-1.microsoft.com`, `rhui-2.microsoft.com`, `rhui-3.microsoft.com`
:    `/etc/yum.repos.d/rh-cloud.repo` under `baseurl`

Google Cloud Provider
:    `cds.rhel.updates.googlecloud.com`
:    `/etc/yum.repos.d/rh-cloud.repo` under `mirrorlist`

Amazon Web Service:
:    wildcard: `aws.ce.redhat.com`
:    `rhui3.REGION.aws.ce.redhat.com`
:    `/etc/yum.repos.d/redhat-rhui.repo` under `mirrorlist`
