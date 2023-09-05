---

copyright:
  years: 2020, 2023
lastupdated: "2023-09-05"

keywords: satellite, hybrid, multicloud, microsoft azure, azure, azure host

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}



# Microsoft Azure
{: #azure}

Learn how you can set up an {{site.data.keyword.satellitelong}} location with virtual instances that you created in Microsoft Azure.
{: shortdesc}

If your hosts are running Red Hat CoreOS (RHCOS), you must manually attach them to your location.
{: note}



## Adding Azure hosts to {{site.data.keyword.satelliteshort}}
{: #azure-host-attach}

You can create your {{site.data.keyword.satellitelong_notm}} location by using hosts that you added from Microsoft Azure.
{: shortdesc}


If you want to use Red Hat CoreOS (RHCOS) hosts in your location, provide your RHCOS image file to your Azure account. For more information, see [Creating custom Linux images](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/imaging){: external}. To find RHCOS images, see the list of [available images](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/). Note that you must use at least version 4.9.
{: important}


All hosts that you want to add must meet the general host requirements, such as the RHEL 7 or 8 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

Before you begin
* [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
* [Install the Azure command line interface (`az`)](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli){: external}.
* Make sure that you have user admin credentials to your Azure account.

To add hosts from Azure to your {{site.data.keyword.satelliteshort}} location,

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to add Azure hosts.
2. Retrieve the host registration script that you must run on your hosts to make them visible to your {{site.data.keyword.satellitelong_notm}} location.
    1. From the **Hosts** tab, click **Attach host**.
    2. Optional: Add host labels that are used later to [automatically assign](/docs/satellite?topic=satellite-assigning-hosts#host-autoassign-ov) hosts to {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services in the location. Labels must be provided as key-value pairs, and must match the request from the service. By default, your hosts get a `cpu`, `os`, and `memory` label, but you might want to add more to control the auto assignment, such as `env=prod` or `service=database`.
    3. Enter a file name for your script or use the name that is generated for you.
    4. Click **Download script** to generate the host script and download the script to your local machine. Note that the token in the script is an API key, which should be treated and protected as sensitive information.
3. **RHEL 7 only** Open the registration script. After the `API_URL` line, add a section to pull the required RHEL packages with the subscription manager.
    ```sh
    # Grow the base volume group first
    echo -e "r\ne\ny\nw\ny\ny\n" | gdisk /dev/sda
    # Mark result as true as this returns a non-0 RC when syncing disks
    echo -e "n\n\n\n\n\nw\n" | fdisk /dev/sda || true
    partx -l /dev/sda || true
    partx -v -a /dev/sda || true
    pvcreate /dev/sda5
    vgextend rootvg /dev/sda5
    # Grow the TMP LV
    lvextend -L+10G /dev/rootvg/tmplv
    xfs_growfs /dev/rootvg/tmplv
    # Grow the var LV
    lvextend -L+20G /dev/rootvg/varlv
    xfs_growfs /dev/rootvg/varlv

    # Enable Azure RHEL Updates
    yum update --disablerepo=* --enablerepo="*microsoft*" -y
    yum-config-manager --enable '*'
    yum repolist all
    yum install container-selinux -y
    echo "repos enabled"
    ```
    {: codeblock}

4. From your local command line, [sign in to your Azure account](https://docs.microsoft.com/en-us/cli/azure/authenticate-azure-cli?view=azure-cli-latest){: external}.
    ```sh
    az login
    ```
    {: pre}

5. Create a network security group that meets the host networking requirements for {{site.data.keyword.satelliteshort}}.
    1. Create a network security group in your resource group. For more information, see the [Azure CLI documentation](https://docs.microsoft.com/en-us/cli/azure/network/nsg?view=azure-cli-latest#az_network_nsg_create){: external}.
        ```sh
        az network nsg create --name <network_security_group_name> --resource-group <resource_group_name>
        ```
        {: pre}

    2. Create a rule in the network security group to allow SSH into virtual machines. For more information, see the [Azure CLI documentation](https://docs.microsoft.com/en-us/cli/azure/network/nsg/rule?view=azure-cli-latest#az_network_nsg_rule_create){: external}.
        ```sh
        az network nsg rule create --name ssh --nsg-name <network_security_group_name> --priority 1000 --resource-group <resource_group_name> --destination-port-ranges 22 --access Allow --protocol Tcp
        ```
        {: pre}

    3. Create a rule in the network security group to meet the [minimum host networking requirements](/docs/satellite?topic=satellite-reqs-host-network). For more information, see the [Azure CLI documentation](https://docs.microsoft.com/en-us/cli/azure/network/nsg/rule?view=azure-cli-latest#az_network_nsg_rule_create){: external}.
        ```sh
        az network nsg rule create --name satellite --nsg-name <network_security_group_name> --priority 1010 --resource-group <resource_group_name> --destination-port-ranges 80 443 30000-32767 --access Allow
        ```
        {: pre} 

    4. Optional: Verify that your network security group meets the host networking requirements, such as in the [example settings](#azure-reqs-firewall).
    
6. Create virtual machines to serve as the hosts for your {{site.data.keyword.satelliteshort}} location resources, including the control plane and any {{site.data.keyword.redhat_openshift_notm}} clusters that you want to create. The following command creates 6 VMs at the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs) for compute, disks, and image. The VMs are created in the resource group and network security group that you previously created. For more information, see the [Azure CLI documentation](https://docs.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest#az_vm_create){: external}.
    
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

7. Wait for the instances to create. During the creation of your instance, the script runs automatically. This process takes a few minutes to complete.
8. Monitor the progress of the registration script.
    1.  Get the public IP address of one of your instances. For more information, see the [Azure CLI documentation](https://docs.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest#az_vm_list_ip_addresses){: external}.
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

9. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, check that your hosts are shown in the **Hosts** tab of your location. All hosts show a **Health** status of `Ready` when a connection to the machine can be established, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} location control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.

10. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual).



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

For more information, see [Network security groups](https://docs.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview){: external} in Microsoft Azure documentation.




## I created a {{site.data.keyword.satelliteshort}} location, what's next?
{: #aws-whats-next}

Now that your {{site.data.keyword.satelliteshort}} location is set up, you are ready to start using {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Add compute capacity to your location by [attaching more hosts to the location](/docs/satellite?topic=satellite-attach-hosts) so that you can run [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster. You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.







