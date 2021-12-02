---

copyright:
  years: 2020, 2021
lastupdated: "2021-12-02"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Microsoft Azure
{: #azure}

Learn how you can set up an {{site.data.keyword.satellitelong}} location with virtual instances that you created in Microsoft Azure.
{: shortdesc}

## Automating your Azure location setup with a {{site.data.keyword.bpshort}} template
{: #azure-template}

Automate your Azure setup with templates that use [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-about-schematics) to create a {{site.data.keyword.satelliteshort}} location, provision hosts in your Azure account, and set up the {{site.data.keyword.satelliteshort}} location control plane for you. 
{: shortdesc}

For more configuration options, you can [manually attach Azure hosts to a {{site.data.keyword.satelliteshort}} location](#azure-host-attach).
{: tip}

Before you begin, make sure that you have the correct [{{site.data.keyword.cloud_notm}} permissions](/docs/satellite?topic=satellite-iam#iam-roles-usecases) to create locations, including to {{site.data.keyword.satelliteshort}} and {{site.data.keyword.bpshort}}. To create the template and manage its resources, {{site.data.keyword.satelliteshort}} automatically creates an {{site.data.keyword.cloud_notm}} IAM [API key](/docs/account?topic=account-manapikey). You can optionally provide the value of an existing API key that has the correct permissions in the same account.

1. In your Azure cloud provider, [set up your account credentials](/docs/satellite?topic=satellite-infrastructure-plan#infra-creds-azure).
2. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click **Create location**.
3. In the **Setup** section, click **Azure**.
4. In the **Azure credentials** section, enter the **Azure client ID (app ID)**, **Azure tenant ID**, and **Azure secret key (password)** values that you previously created for the service principal.
5. Click **Fetch options from Azure**.
6. Review the **Azure environment** details that are prepopulated. By default, enough VMs are created to provide hosts for 1 small location that can run about 2 demo clusters. To change the subscription, region, instance type, or number of VMs for the hosts, click the **Edit** pencil icon.
7. Review the **Satellite location** details. If you edited the Azure environment details, you might want to click the **Edit** pencil icon to change details such as the description, API key, or {{site.data.keyword.cloud_notm}} multizone region that the location is managed from.
8. In the **Summary** pane, review the cost estimate.
9. Click **Create location**. Your location might take about 30 minutes to finish provisioning.
10. Optional: To review the provisioning progress, review the logs in the {{site.data.keyword.bpshort}} workspace that is automatically created for you.
    1. Click **Manage in Schematics**. If you see an error, navigate to the [{{site.data.keyword.bpshort}} workspaces console](https://cloud.ibm.com/schematics/workspaces){: external} and click the name of your workspace, such as `us.east.cartOrder...`.
    2. From the **Activity** tab, find the current activity row and click **View log** to review the log details.
    3. Wait for the {{site.data.keyword.bpshort}} action to finish and the workspace to enter an **Active** state.

Well done, your {{site.data.keyword.satelliteshort}} location is creating! You can review the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external} to see when your location is in a **Normal** state and ready to use.


The following resources are created by the template in the resource group of your Azure cloud subscription.

- 1 virtual network that spans the region.
- 1 network security group to meet the host networking requirements for {{site.data.keyword.satelliteshort}}.
- 1 virtual machine for each host that you specified, spread evenly across the region. By default, 6 virtual machines are created.
- 1 network interface for each virtual machine.
- 1 disk for each virtual machine.

The following resources are created by the template in your {{site.data.keyword.cloud_notm}} account.

- 1 {{site.data.keyword.satelliteshort}} location.
- 3 {{site.data.keyword.satelliteshort}} hosts that represent the virtual machines in Azure, attached to the location and assigned to the {{site.data.keyword.satelliteshort}} location control plane.
- 3 {{site.data.keyword.satelliteshort}} hosts that represent the virtual machines in Azure, attached to the location, unassigned, and available to use for services like an {{site.data.keyword.openshiftshort}} cluster. If you added more than 6 hosts, the number of hosts equals the number that you specified minus the 3 that are assigned to the control plane.

What's next?

The {{site.data.keyword.bpshort}} template helped with the initial creation, but you are in control for subsequent location management actions, such as [attaching more hosts](/docs/satellite?topic=satellite-hosts#attach-hosts), [creating {{site.data.keyword.satelliteshort}} clusters](/docs/satellite?topic=openshift-satellite-clusters), or [scaling the {{site.data.keyword.satelliteshort}} location control plane](/docs/satellite?topic=satellite-locations#control-plane-scale). If you [remove](/docs/satellite?topic=satellite-locations#location-remove) your {{site.data.keyword.satelliteshort}} location, make sure to [remove your workspace in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-workspace-setup#del-workspace), too.


## Adding Azure hosts to {{site.data.keyword.satelliteshort}}
{: #azure-host-attach}

You can create your {{site.data.keyword.satellitelong_notm}} location by using hosts that you added from Microsoft Azure.
{: shortdesc}

All hosts that you want to add must meet the general host requirements, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

Before you begin
* [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations#location-create).
* [Install the Azure command line interface (`az`)](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli){: external}.
* Make sure that you have user admin credentials to your Azure account.

To add hosts from Azure to your {{site.data.keyword.satelliteshort}} location,

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to add Azure hosts.
2. Retrieve the host registration script that you must run on your hosts to make them visible to your {{site.data.keyword.satellitelong_notm}} location.
    1. From the **Hosts** tab, click **Attach host**.
    2. Optional: Add host labels that are used later to [automatically assign hosts to {{site.data.keyword.satelliteshort}}-enabled services](/docs/satellite?topic=satellite-hosts#host-autoassign-ov) in the location. Labels must be provided as key-value pairs, and must match the request from the service. By default, your hosts get a `cpu` label, but you might want to add more to control the autoassignment, such as `env=prod` or `service=database`.
    3. Enter a file name for your script or use the name that is generated for you.
    4. Click **Download script** to generate the host script and download the script to your local machine.
3. Open the registration script. After the `API_URL` line, add a section to pull the required RHEL packages with the subscription manager.
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

    3. Create a rule in the network security group to meet the [minimum host networking requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network). For more information, see the [Azure CLI documentation](https://docs.microsoft.com/en-us/cli/azure/network/nsg/rule?view=azure-cli-latest#az_network_nsg_rule_create){: external}.
        ```sh
        az network nsg rule create --name satellite --nsg-name <network_security_group_name> --priority 1010 --resource-group <resource_group_name> --destination-port-ranges 80 443 30000-32767 --access Allow
        ```
        {: pre} 

    4. Optional: Verify that your network security group meets the host networking requirements, such as in the [example settings](#azure-reqs-firewall).
    
6. Create virtual machines to serve as the hosts for your {{site.data.keyword.satelliteshort}} location resources, including the control plane and any {{site.data.keyword.openshiftshort}} clusters that you want to create. The following command creates 6 VMs at the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs) for compute, disks, and image. The VMs are created in the resource group and network security group that you previously created. For more information, see the [Azure CLI documentation](https://docs.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest#az_vm_create){: external}.
    ```sh
    az vm create --name <vm_name> --resource-group <resource_group> --admin-user <username> --admin-password <password> --image RedHat:RHEL:7-LVM:latest --nsg <network_security_group> --os-disk-name <disk_name> --os-disk-size-gb 128 --size Standard_D4s_v3 --count 6 --custom-data <filepath_to_host_registration_script>
    ```
    {: pre}
    
    If you don't want to pass the `--custom-data` command option during VM creation, you can run the host registration script on each VM after provisioning.
    {: tip}

7. Wait for the instances to create. During the creation of your instance, the registration script runs automatically. This process takes a few minutes to complete.
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

10. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-hosts#host-assign).


## Security group settings
{: #azure-reqs-firewall}

As described in the [host networking requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network), your Azure hosts must have access to connect to {{site.data.keyword.satellitelong_notm}}. You might find that you need to update your security group settings in Azure, similar to the following example.
{: shortdesc}

The following example is a security group that you might create for Azure.

```sh
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
```
{: screen}

## Access to {{site.data.keyword.satelliteshort}} clusters and the {{site.data.keyword.openshiftshort}} web console
{: #azure-reqs-console-access}

The private IP addresses of your instances are automatically registered and added to your location's DNS record and the cluster's subdomain. This setup prevents users that are not connected to your hosts' private network from accessing the cluster from a local machine or opening the {{site.data.keyword.openshiftshort}} web console. You must be connected to your hosts' private network, such as through VPN access, to [connect to your cluster and access the {{site.data.keyword.openshiftshort}} web console](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat_se). Alternatively, if your hosts have public network connectivity, you can test access to your cluster by changing your cluster's and location's DNS records to [use your hosts' public IP addresses](/docs/openshift?topic=openshift-access_cluster#sat_public_access).

