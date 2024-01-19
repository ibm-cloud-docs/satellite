---


copyright:
  years: 2020, 2024
lastupdated: "2024-01-19"

keywords: satellite, hybrid, multicloud, alibaba, alibaba hosts, alibaba cloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}



# Attaching Alibaba Cloud hosts
{: #alibaba}

You can add hosts from Alibaba Cloud to {{site.data.keyword.satelliteshort}}.
{: shortdesc}

All hosts that you want to add must meet the host requirements, such as the RHEL 7 or 8 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

Before you begin, [create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations). Note that your location displays `Action required` until you add hosts and create the control plane.

To add hosts from Alibaba cloud, follow these general steps.

1. [Download the host script](/docs/satellite?topic=satellite-host-attach-download).
2. Set up your virtual machines in Alibaba Cloud.
    1. Log in to your [Alibaba account](https://us.alibabacloud.com/en){: external}.
    2. From the [VPC console](https://vpc.console.aliyun.com/vpc){: external}, create or select an existing Virtual Private Cloud. When you create a VPC, you must create a vSwitch in each zone where you want to add hosts.
    3. Select the **Resources** tab.
    4. Verify that you have a **Route table** and at least one **vSwitch**. 
    5. Create a security group by clicking **add** from the **Security groups** section. For more information about the values to set, see [Security group settings](#alibaba-reqs-secgroup).
    6. From the [Elastic Computing Service (ECS)](https://ecs.console.aliyun.com/server#/home) console, create a minimum of 3 instances that meet the {{site.data.keyword.satelliteshort}} [host requirements](/docs/satellite?topic=satellite-host-reqs).
    7. Install a [supported Red Hat Enterprise Linux](/docs/satellite?topic=satellite-infrastructure-plan#infras-plan-os) version.
3. Connect to your instances and install any [required packages](/docs/satellite?topic=satellite-host-reqs).     
4. Upload and run the host attach script on each instances. After the host script completes, your hosts are available in the **Hosts** tab of your [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}. All hosts show a **Status** of `Ready` when a connection to the machine can be established. Note that the hosts show a **Status** of `Unassigned` as the hosts are not yet assigned.
    1. Upload the host script file to your Alibaba instance.
    
        ```sh
        scp -i <PEM-FILE> <Path2HostScript> cloud-user@<PUBLIC_IP>:/tmp/attach.sh
        ```
        {: pre}
    
    2. Log in to your instance.

        ```sh
        ssh -i <path-to-pemfile> cloud-user@<PUBLIC_IP>
        ```
        {: pre}

    3. Run the host script file. For example,

        ```sh
        sudo nohup bash /tmp/attach.sh &
        ```
        {: pre}

    4. Review the status of the registration script.
        
        ```sh
        journalctl -f -u ibm-host-attach
        ```
        {: pre} 
        
5. Assign your hosts to either the [control plane](/docs/satellite?topic=satellite-setup-control-plane) or a [worker pool](/docs/satellite?topic=satellite-assigning-hosts).

## Security group settings
{: #alibaba-reqs-secgroup}

As described in the [host networking requirements](/docs/satellite?topic=satellite-reqs-host-network), your Alibaba hosts must have access to connect to {{site.data.keyword.satellitelong_notm}}. If you use hosts in a virtual private cloud (VPC), you can create a security group similar to the following example. You can get the owner, group, user, and VPC IDs from your Alibaba provider resources.
{: shortdesc}


|Action|Priority|Protocol Type|Port Range|Authorization Object|
|------|-----|------|-----|-----|
| Allow |	1 | Custom TCP | Destination `30000/32767` | Source `0.0.0.0/0` |
| Allow |	1 | Custom TCP | Destination `443/443` | Source `0.0.0.0/0` |
{: caption="Example security group for Alibaba" caption-side="bottom"}

In addition to these inbound rules, you must allow all outbound connectivity to all ports and IP addresses.
{: note}

For more information, see [Security groups](https://www.alibabacloud.com/help/ecs/user-guide/security-groups-1/){: external} in the Alibaba documentation.

## I added hosts to my location, what's next?
{: #alibaba-whats-next-host}

Now that you added hosts to your location, you can assign them to your location control plane or to your {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Assign [hosts to the location control plane](/docs/satellite?topic=satellite-setup-control-plane) or [to your {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-assigning-hosts).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.




