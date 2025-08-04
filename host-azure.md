---


copyright:
  years: 2020, 2025
lastupdated: "2025-08-04"

keywords: satellite, hybrid, multicloud, microsoft azure, azure, azure host

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}



# Attaching Microsoft Azure hosts
{: #azure}

Learn how to attach Microsoft Azure cloud hosts to {{site.data.keyword.satellitelong}}.
{: shortdesc}

To attach Red Hat CoreOS (RHCOS) hosts, your location must be enabled for Red Hat CoreOS. For more information, see [Is my location enabled for Red Hat CoreOS?](/docs/satellite?topic=satellite-locations#verify-coreos-location). Note that you can still attach Red Hat Enterprise Linux hosts to a location that is enabled for Red Hat CoreOS.

Before you begin, make sure that you create host machines that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-host-reqs) in your on-prem data center, in {{site.data.keyword.cloud_notm}}, or in public cloud providers.

After you attach a host to your location, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host as root with SSH for security purposes. You might see error messages if you try to SSH as root into a host that is attached successfully to a location. To restore the ability to SSH into the machine, you can [remove the host](/docs/satellite?topic=satellite-host-remove) and reload the operating system.

Not sure how many hosts to attach to your location? See [Sizing your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-location-sizing).
{: tip}


## Adding Azure hosts to {{site.data.keyword.satelliteshort}}
{: #azure-host-attach}

You can create your {{site.data.keyword.satellitelong_notm}} location by using hosts that you added from Microsoft Azure.
{: shortdesc}


If you want to use Red Hat CoreOS (RHCOS) hosts in your location, provide your RHCOS image file to your Azure account. For more information, see [Creating custom Linux images](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/imaging){: external}. To find RHCOS images, see the list of [available images](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/). Note that you must use at least version 4.9.
{: important}


All hosts that you want to add must meet the general host requirements, such as the RHEL 8 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

Before you begin
* [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
* [Install the Azure command line interface (`az`)](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli){: external}.
* Make sure that you have user admin credentials to your Azure account.

To add hosts from Azure to your {{site.data.keyword.satelliteshort}} location,

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to add Azure hosts.
2. Retrieve the host registration script that you must run on your hosts to make them visible to your {{site.data.keyword.satellitelong_notm}} location.
    1. From the **Hosts** tab, click **Attach host**.
    2. Optional: Add host labels that are used later to [automatically assign](/docs/satellite?topic=satellite-host-autoassign-ov) hosts to {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services in the location. Labels must be provided as key-value pairs, and must match the request from the service. By default, your hosts get a `cpu`, `os`, and `memory` label, but you might want to add more to control the auto assignment, such as `env=prod` or `service=database`.
    3. Enter a file name for your script or use the name that is generated for you.
    4. Click **Download script** to generate the host script and download the script to your local machine. Note that the token in the script is an API key, which should be treated and protected as sensitive information.

3. From your local command line, [sign in to your Azure account](https://learn.microsoft.com/en-us/cli/azure/authenticate-azure-cli?view=azure-cli-latest){: external}.
    ```sh
    az login
    ```
    {: pre}

4. Create a network security group that meets the host networking requirements for {{site.data.keyword.satelliteshort}}.
    1. Create a network security group in your resource group. For more information, see the [Azure CLI documentation](https://learn.microsoft.com/en-us/cli/azure/network/nsg?view=azure-cli-latest#az_network_nsg_create){: external}.
        ```sh
        az network nsg create --name <network_security_group_name> --resource-group <resource_group_name>
        ```
        {: pre}

    2. Create a rule in the network security group to allow SSH into virtual machines. For more information, see the [Azure CLI documentation](https://learn.microsoft.com/en-us/cli/azure/network/nsg/rule?view=azure-cli-latest#az_network_nsg_rule_create){: external}.
        ```sh
        az network nsg rule create --name ssh --nsg-name <network_security_group_name> --priority 1000 --resource-group <resource_group_name> --destination-port-ranges 22 --access Allow --protocol Tcp
        ```
        {: pre}

    3. Create a rule in the network security group to meet the [minimum host networking requirements](/docs/satellite?topic=satellite-reqs-host-network). For more information, see the [Azure CLI documentation](https://learn.microsoft.com/en-us/cli/azure/network/nsg/rule?view=azure-cli-latest#az_network_nsg_rule_create){: external}.
        ```sh
        az network nsg rule create --name satellite --nsg-name <network_security_group_name> --priority 1010 --resource-group <resource_group_name> --destination-port-ranges 80 443 30000-32767 --access Allow
        ```
        {: pre} 

    4. Optional: Verify that your network security group meets the host networking requirements, such as in the [example settings](#azure-reqs-firewall).
    
5. Create virtual machines to serve as the hosts for your {{site.data.keyword.satelliteshort}} location resources, including the control plane and any {{site.data.keyword.redhat_openshift_notm}} clusters that you want to create. The following command creates 6 VMs at the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs) for compute, disks, and image. The VMs are created in the resource group and network security group that you previously created. For more information, see the [Azure CLI documentation](https://learn.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest#az_vm_create){: external}.
    
    - Create Red Hat Enterprise Linux hosts
        ```sh
        az vm create --name <vm_name> --resource-group RESOURCE-GROUP --admin-user USERNAME --admin-password PASSWORD --image RedHat:RHEL:7-LVM:latest --nsg <network_security_group> --os-disk-name DISK_NAME --os-disk-size-gb 128 --size Standard_D4s_v3 --count 6 --custom-data FILEPATH_TO_HOST_REGISTRATION_SCRIPT
        ```
        {: pre}
        
    - Create Red Hat CoreOS hosts
        ```sh
        az vm create --name VM --resource-group RESOURCE-GROUP --image RHCOS-IMAGE --admin-username USERNAME  --admin-password PASSWORD--size Standard_B8ms --data-disk-sizes-gb 100 --custom-data FILEPATH-IGNITION-SCRIPT-LOCATION --os-disk-size-gb 100 --public-ip-sku Standard --generate-ssh-keys
        ```
        {: pre}
    
    If you don't want to pass the `--custom-data` command option during VM creation, you can run the host registration script on each VM after provisioning.
    {: tip}

6. Wait for the instances to create. During the creation of your instance, the script runs automatically. This process takes a few minutes to complete.
7. Monitor the progress of the registration script.
    1.  Get the public IP address of one of your instances. For more information, see the [Azure CLI documentation](https://learn.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest#az_vm_list_ip_addresses){: external}.
        ```sh
        az vm list-ip-addresses -g <resource_group> -n <vm_name>
        ```
        {: pre}

    2. Log in to your instance.
        ```sh
        ssh <username>:<public_IP_address>
        ```
        {: pre}

    3. Monitor the progress of the registration script.
        ```sh
        journalctl -f -u ibm-host-attach
        ```
        {: pre}

8. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, check that your hosts are shown in the **Hosts** tab of your location. All hosts show a **Health** status of `Ready` when a connection to the machine can be established, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} location control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.

9. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-assigning-hosts).



## Security group settings for Azure
{: #azure-reqs-firewall}

As described in the [host networking requirements](/docs/satellite?topic=satellite-reqs-host-network), your Azure hosts must have access to connect to {{site.data.keyword.satellitelong_notm}}. You might find that you need to update your security group settings in Azure, similar to the following example.
{: shortdesc}

The following example is a security group that you might create for Azure.

```json
"securityRules": [
            {
                "name": "satellite",
                "id": "/subscriptions/.../securityRules/satellite",
                "type": "Microsoft.Network/networkSecurityGroups/securityRules",
                "properties": {
                    "provisioningState": "Succeeded",
                    "protocol": "*",
                    "sourcePortRange": "*",
                    "sourceAddressPrefix": "*",
                    "destinationAddressPrefix": "*",
                    "access": "Allow",
                    "priority": 100,
                    "direction": "Inbound",
                    "sourcePortRanges": [],
                    "destinationPortRanges": [
                        "80",
                        "443",
                        "30000-32767"
                    ],
                    "sourceAddressPrefixes": [],
                    "destinationAddressPrefixes": []
                }
            },
            {
                "name": "SSH",
                "id": "/subscriptions/.../securityRules/SSH",
                "type": "Microsoft.Network/networkSecurityGroups/securityRules",
                "properties": {
                    "provisioningState": "Succeeded",
                    "protocol": "TCP",
                    "sourcePortRange": "*",
                    "destinationPortRange": "22",
                    "sourceAddressPrefix": "*",
                    "destinationAddressPrefix": "*",
                    "access": "Allow",
                    "priority": 110,
                    "direction": "Inbound",
                    "sourcePortRanges": [],
                    "destinationPortRanges": [],
                    "sourceAddressPrefixes": [],
                    "destinationAddressPrefixes": []
                }
            }
]
```
{: screen}

For more information, see [Network security groups](https://learn.microsoft.com/azure/virtual-network/network-security-groups-overview){: external} in Microsoft Azure documentation.




## I added hosts to my location, what's next?
{: #azure-whats-next-host}

Now that you added hosts to your location, you can assign them to your location control plane or to your {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Assign [hosts to the location control plane](/docs/satellite?topic=satellite-setup-control-plane) or [to your {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-assigning-hosts).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.
