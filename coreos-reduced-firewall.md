---

copyright:
  years: 2023, 2023
lastupdated: "2023-06-21"

keywords: satellite, CoreOS, RHCOS, firewall 

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Creating Red Hat CoreOS enabled Locations with reduced firewall footprint
{: #coreos-reduced-firewall}

Configure a Red Hat CoreOS enabled Location to connect to a single network destination instead of multiple destinations to reduce the number of outbound IP addresses to allow on your firewall. 
{: shortdesc}

To connect to a single network destination, use a host link agent. A host link agent is a binary image of link client that runs as `systemd` on your hosts. The agent connects to the tunnel server and rewrites your hosts DNS so that all Location bootstrap traffic is redirected to use the Link established by the agent to reach IBM back-end services. As a result, all outbound traffic from the Location connects to a single destination, which is the Tunnel server public endpoint on port 443. Therefore, you do not need to allow all the outbound IP addresses that are mentioned in [Required outbound connectivity for hosts in all regions](/docs/satellite?topic=satellite-reqs-host-network-outbound).

Follow these steps to set up a Red Hat CoreOS enabled Location with reduced firewall footprint.

1. Create a Red Hat CoreOS enabled {{site.data.keyword.satelliteshort}} Location. For more information, see [Creating a Satellite location](/docs/satellite?topic=satellite-locations).

1. Get the `healthcheck` Location endpoint by running the `ibmcloud sat endpoint ls --location LOCATION_NAME` command. 
    
    Example command:  
    ```sh
    ibmcloud sat endpoint ls --location my-sat-link
    ```
    {: pre} 

    Example output:
    ```sh
    ID                          Name                                        Destination Type  Address
    cdn1e2dw0vmieu3g98p0_drock  satellite-healthcheck-cdn1e2dw0vmieu3g98p0  location    HTTP  c-01.private.us-south.link.satellite.cloud.ibm.com:32877
    ```
    {: screen}
  
    From the output, take a note of the Location endpoint. For example, `c-01.private.us-south.link.satellite.cloud.ibm.com:32877`. Replace `private` with `ws` and remove the port. For example, `c-01.private.us-south.link.satellite.cloud.ibm.com:32877` becomes `c-01-ws.us-south.link.satellite.cloud.ibm.com`. This value is used as the value for `ENDPOINT_TO_POINT_TO` in the **`sat host attach`** command in the next step.

1. Download the host attachment script for your Location by using the `ibmcloud sat host attach --location LOCATION_NAME --operating-system RHCOS --host-link-agent-endpoint ENDPOINT_TO_POINT_TO` command in the CLI.       

    Example command:  
    ```sh
    ibmcloud sat host attach --location my-sat-link --operating-system RHCOS --host-link-agent-endpoint c-01-ws.region.link.satellite.cloud.ibm.com
    ```
    {: pre}
            
    Example output:
    ```sh
    Creating host registration script...
    OK
    The script to attach hosts to Satellite location 'my-sat-link' was downloaded to the following location:

    /var/folders/7y/90mtvpqj1jx05gvgk1jyk7b80000gn/T/register-host_my-sat-link_1782841498.ign
    ```
    {: screen}

1. Attach your hosts to your Location by running the downloaded script.
    * If your hosts are in another cloud provider, follow the provider-specific steps to run the script and attach your hosts. 
        - [Alibaba Cloud](/docs/satellite?topic=satellite-alibaba)
        - [Amazon Web Services (AWS)](/docs/satellite?topic=satellite-aws)
        - [Google Cloud Platform (GCP)](/docs/satellite?topic=satellite-gcp)
        - [Microsoft Azure](/docs/satellite?topic=satellite-azure)
        - [{{site.data.keyword.cloud_notm}}](/docs/satellite?topic=satellite-ibm)
    * If your hosts are in an on-premises data center, see [Attaching on-premises RHCOS hosts](/docs/satellite?topic=satellite-attach-hosts#attach-rhcos-hosts).

1. Find the IP addresses of the tunnel endpoint by running the `dig c-01-ws.REGION.link.satellite.cloud.ibm.com +short` command. 

    Example command:  
    ```sh
    dig c-01-ws.us-south.link.satellite.cloud.ibm.com +short
    ```
    {: pre}
            
    Example output:
    ```sh
    prod-us-south-sl-935783-6b64a6ccc9c596bf59a86625d8fa2202-0000.us-south.containers.appdomain.cloud.
    prod-us-south-sl-935783-6b64a6ccc9c596bf59a86625d8fa2202-0000.c303u02d04o7tl16uqm0.akadns.net.
    169.61.156.226
    169.61.31.178
    169.46.88.106
    ```
    {: screen}
    
    In this example, the IP addresses of the tunnel endpoint are `169.61.156.226`, `169.61.31.178`, and `169.46.88.106` on `port 443`. 
    
1. Configure your firewall to allow outgoing traffic to the IP addresses of the tunnel endpoint on port 443. The IP addresses can be found in the output of the previous step. 

NTP must also be allowed. You can choose to allow access to the Red Hat network time protocol (NTP) servers listed in [Required outbound connectivity for hosts overview](/docs/satellite?topic=satellite-reqs-host-network-outbound) or you can configure access to a custom Network Time Protocol (NTP) server. See [Specifying a custom Network Time Protocol (NTP) server](/docs/satellite?topic=satellite-config-custom-ntp) if you want to configure a local NTP server.
{: note}
