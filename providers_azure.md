---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-03"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}



# Microsoft Azure
{: #azure}

Learn how you can set up a {{site.data.keyword.satellitelong}} location with virtual instances that you created in Microsoft Azure.
{: shortdesc}

## Adding Azure hosts to {{site.data.keyword.satelliteshort}}
{: #azure-host-attach}

You can create your {{site.data.keyword.satellitelong_notm}} location by using hosts that you added from Microsoft Azure.
{: shortdesc}

All hosts that you want to add must meet the general host requirements, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

Before you begin, [create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations#location-create).

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to add Azure hosts.
2. Retrieve the host registration script that you must run on your hosts to make them visible to your {{site.data.keyword.satellitelong_notm}} location.
   1. From the **Hosts** tab, click **Attach host**.
   2. Optional: Add host labels that are used later to [automatically assign hosts to {{site.data.keyword.satelliteshort}}-enabled services](/docs/satellite?topic=satellite-hosts#host-autoassign-ov) in the location. Labels must be provided as key-value pairs, and must match the request from the service. By default, your hosts get a `cpu` label, but you might want to add more to control the autoassignment, such as `env=prod` or `service=database`.
   3. Enter a file name for your script or use the name that is generated for you.
   4. Click **Download script** to generate the host script and download the script to your local machine.
3. Open the registration script. After the `API_URL` line, add a section to pull the required RHEL packages with the subscription manager.
   ```
   # Grow the TMP LV
   lvextend -L+10G /dev/rootvg/tmplv
   xfs_growfs /dev/rootvg/tmplv

   # Enable Azure RHEL Updates
   yum update -y
   yum-config-manager --enable '*'
   yum repolist all
   yum install container-selinux -y
   echo "repos enabled"
   ```
   {: codeblock}
4. Open the Azure [virtual machine scale set creation page](https://portal.azure.com/#create/microsoft.vmss){: external}.

   For an overview of options that you can configure for your virtual machine scale set, see the [Azure documentation](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/quick-create-portal){: external}.
   {: tip}

5. On the **Basic** tab, enter the following information:
   1. Select the resource group where you want to create your virtual machines.
   2. Enter a name for your virtual machine scale set.
   3. Select a region and the availability zones where you want to create your virtual machines. Make sure that you select 3 different availability zones to spread your virtual machines across zones for higher availability.
   4. Select a supported Red Hat Enterprise Linux 7 operating system that is enabled for cloud-init and that includes all Azure images, such as `Red Hat Enterprise Linux 7.9 - Gen1`. Other Red Hat Enterprise Linux 7 versions might not be enabled for cloud-init or do not come with all the required images to run as a host in your {{site.data.keyword.satelliteshort}} location.
   5. Select a size for your machines. Make sure that you select a machine that meets the [minimum host requirements](/docs/satellite?topic=satellite-host-reqs) for CPU and memory, such as `D4s_v3`.
   6. Choose how you want to access your virtual machines. You can enter a username and password or use an SSH key.
   7. Continue to the **Disks** tab.
6. On the **Disks** tab, add a Premium SSD disk with a size of at least 100 GB. Then, continue to the **Networking** tab.
7. On the **Networking** tab, enter the following information:
   1. Select an existing or create a virtual network that you want to connect your virtual machines to.
   2. Select an existing or create a new network interface (nic). Make sure that your network interface includes a security group that allows access to {{site.data.keyword.satelliteshort}} as described in the [Security group settings](#azure-reqs-firewall).
   3. Continue to the **Scaling** tab.
8. On the **Scaling** tab, enter the number of instances that you want to create. To create the {{site.data.keyword.satelliteshort}} location control plane, you need at least 6 virtual machines (or 3 for testing purposes). Then, continue to the **Advanced** tab.
9. On the **Advanced** tab, enter the script that you modified earlier in the **Custom data** field.
10. Click **Review + create** to start the resource validation process. Then, click **Create** to start creating your virtual machines.
11. Wait for the instances to create. During the creation of your instance, the registration script runs automatically. This process takes a few minutes to complete.
12. Monitor the progress of the registration script.
    1. From the Azure virtual machine scale set dashboard, retrieve the public IP address of one of your instances.
    2. Log in to your instance.

       **Log in with a pem file**:
       ```
       ssh -i <key>.pem <username>@<public_IP_address>
       ```
       {: pre}

       **Log in with username and password**:
       ```
       ssh <username>:<public_IP_address>
       ```
       {: pre}

    3. Monitor the progress of the registration script.
       ```
       journalctl -f -u ibm-host-attach
       ```
       {: pre}

13. Check that your hosts are shown in the **Hosts** tab of your location from the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}. All hosts show a **Health** status of `Ready` when a connection to the machine can be established, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} location control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.

14. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-hosts#host-assign).


## Security group settings
{: #azure-reqs-firewall}

As described in the [host networking requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network), your Azure hosts must have access to connect to {{site.data.keyword.satellitelong_notm}}. You might find that you need to update your security group settings in Azure, similar to the following example.
{: shortdesc}

**Example security group settings**
```
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


<br />

