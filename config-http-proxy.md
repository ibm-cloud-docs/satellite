---

copyright:
  years: 2022, 2022
lastupdated: "2022-05-13"

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


You cannot configure an HTTP proxy for worker to master communications or for connecting to the package mirrors. You must edit each host that is attached to your location, including the hosts that make up the control plane. Your configuration might vary by provider. Consider setting up your proxy outside of the {{site.data.keyword.satelliteshort}} environment to ensure that the configuration works for your infrastructure. Then, configure your proxy in the {{site.data.keyword.satelliteshort}} environment.
{: note}



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
        
        From this output, the first IP is `172.21.0.1`, which makes the full output for hosts associated with this specific cluster in this example `NO_PROXY=172.20.0.1,172.21.0.1,$REDHAT_PACKAGE_MIRROR_LOCATION` for RHEL hosts and `NO_PROXY=172.20.0.1,172.21.0.1,$REDHAT_PACKAGE_MIRROR_LOCATION` for RHCOS hosts.

3. Navigate to `/etc/systemd/system.conf.d` on your host. If that file does not exist, create it. 
    
    ```sh
    mkdir -p /etc/systemd/system.conf.d
    ```
    {: pre}

4. Edit the file and add the following information. Enter the `<VALUE>` for `NO_PROXY` from step 2.

    ```sh
    [Manager]
    DefaultEnvironment="HTTP_PROXY=xxxx" "HTTPS_PROXY=xxxxx" "NO_PROXY=<VALUE>"
    ```
    {: codeblock}
    
5. Save the file.
6. Navigate to the `/etc/environment` file on your host. If that file does not exist, create it. 
    
    ```sh
    mkdir -p /etc/environment
    ```
    {: pre}
    
7. Edit the `/etc/environment` file and add the following information. Enter the `<VALUE>` for `NO_PROXY` from step 2.

    ```sh
    HTTP_PROXY=xxxxx
    HTTPS_PROXY=xxxxx
    NO_PROXY="<VALUE>"
    ```
    {: codeblock}
    
8. Save the file.
9. Reboot your host to pick up this change.
10. [Attach or reattach](/docs/satellite?topic=satellite-attach-hosts) your host to the location.
11. Repeat these steps for each host.
12. [Create a ticket](https://cloud.ibm.com/unifiedsupport/cases/form){: external} with IBM Support with the following format.
    
    ```sh
    Title: Request for addition of HTTP_PROXY config to location <LOCATIONID>

    Request Body:
    We are requesting the following information HTTP_PROXY info be added to the locationID listed in the title of this ticket.

    The HTTP_PROXY info is as follows:

    HTTP_PROXY: <endpoint>
    HTTPS_PROXY: <endpoint>
    ```
    {: screen}
    
    After support processes the ticket, you will receive a notification that your location is updated. If a change is required, a new ticket must be opened stating the new parameters. To find your `LOCATIONID` by running `ibmcloud sat locations`.

    For example

    ```sh
    Title: Request for addition of HTTP_PROXY config to location c8idhiu1040dksv67fgg

    Request Body:
    We are requesting the following information HTTP_PROXY info be added to the locationID listed in the title of this ticket.

    The HTTP_PROXY info is as follows:

    HTTP_PROXY: https://my-proxy-endpoint.com:2090
    HTTPS_PROXY: https://my-proxy-endpoint.com:2090
    ```
    {: screen}
    
The value for `REDHHAT_PACKAGE_MIRROR_LOCATION` depends on the location of the Red Hat package mirrors. The `REDHHAT_PACKAGE_MIRROR_LOCATION` can be a wild-card if multiple mirrors are used. For more information, see [How to apply a system wide proxy](https://access.redhat.com/articles/2133021){: external}.
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
