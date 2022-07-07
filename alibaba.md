---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: satellite, hybrid, multicloud, alibaba

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}



# Alibaba Cloud
{: #alibaba}

You can create your {{site.data.keyword.satellitelong_notm}} location by using hosts that you added from Alibaba Cloud. Review the following host requirements that are specific to hosts that are in the Alibaba Cloud. For required access in Alibaba Cloud, see [Alibaba permissions](/docs/satellite?topic=satellite-iam#permissions-alibaba).
{: shortdesc}

## Adding Alibaba hosts to {{site.data.keyword.satelliteshort}}
{: #alibaba-host-attach}

Add your Alibaba Cloud hosts manually to {{site.data.keyword.satelliteshort}}.
{: shortdesc}

All hosts that you want to add must meet the host requirements, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

Before you begin, [create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations). Note that your location displays `Action required` until you add hosts and create the control plane.

### 1. Download the host script
{: #alibaba-host-script}

Before you begin, [create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to add Alibaba hosts.
2. Retrieve the host registration script that you must run on your hosts to make them visible to your {{site.data.keyword.satellitelong_notm}} location.
    1. From the **Hosts** tab, click **Attach host**.
    2. Optional: Enter any host labels that are used later to [automatically assign](/docs/satellite?topic=satellite-assigning-hosts#host-autoassign-ov) hosts to {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services in the location. Labels must be provided as key-value pairs, and must match the request from the service. For example, you might have host labels such as `env=prod` or `service=database`. By default, your hosts get a `cpu` label, but you might want to add more to control the auto assignment, such as `env=prod` or `service=database`.
    3. Enter a file name for your script or use the name that is generated for you.
    4. Click **Download script** to generate the host script and download the script to your local system. Note that the token in the script is an API key, which should be treated and protected as sensitive information.

### 2. Set up your virtual machines
{: #alibaba-host-vm}

You need at least 3 virtual machines to use as hosts that meet the [host requirements](/docs/satellite?topic=satellite-host-reqs). If you do not have 3 virtual machines that meet these requirements, create them.

1. Log in to your [Alibaba account](https://us.alibabacloud.com/en){: external}.
2. From the [VPC console](https://vpc.console.aliyun.com/vpc){: external}, create or select an existing Virtual Private Cloud. When you create a VPC, you must create a vSwitch in each zone where you want to add hosts.
3. Select the **Resources** tab.
4. Verify that you have a **Route table** and at least one **vSwitch**. 
5. Create a security group by clicking **add** from the **Security groups** section. For more information about the values to set, see [Security group settings](#alibaba-reqs-secgroup).
6. From the [Elastic Computing Service (ECS)](https://ecs.console.aliyun.com/server#/home) console, create a minimum of 3 instances that meet the Satellite [host requirements](/docs/satellite?topic=satellite-host-reqs).

    1. Install [Red Hat Enterprise Linux 7.9 64bit](https://marketplace.alibabacloud.com/products/56732001/Red_Hat_Enterprise_Linux_7_9_64bit-sgcmjj00025569.html){: external} from the Alibaba Marketplace.
    2. Select the VPC, vSwitch, and security group that you created earlier.
    3. Create or use an existing SSH key file to log in to the machine. 
    4. Optionally assign a public IP address to your instance to log in to your host without using a VPN.
    
7. If you created a new key-pair when you created your instances,

    1. Download your SSH certification file (`.pem`) for your Alibaba VPC instance. 
    2. Change the permissions of your `.pem` file by running,
        ```sh
        chmod 600 <filename>
        ```
        {: codeblock}  

### 3. Connect to your instance and install packages
{: #alibaba-host-install-packages}

Before you can attach your hosts, you must install the required Red Hat Enterprise Linux packages to connect to {{site.data.keyword.satelliteshort}}.

1. From your Alibaba instance page, find the public IP address for each instance that you want to add.
2. Connect to the first Alibaba instance.

    ```sh
    ssh -i <path-to-pemfile> cloud-user@<PUBLIC_IP>
    ```
    {: pre}
    


3. Register your system with Red Hat and then install the required packages for {{site.data.keyword.satelliteshort}}. 

    1. Note that Alibaba Cloud does not support the YUM package manager or subscription manager by default, which means that to install the required packages and updates for {{site.data.keyword.satelliteshort}} on your hosts, you must provide your Red Hat account credentials in the host attach script. Run the following command on your hosts and replace `<username>` and `<password>` with your Red Hat account credentials to register your host.
        ```sh
        sudo bash
        subscription-manager register --username=<username> --password=<password>
        ```
        {: pre}
    
    2. After you register with subscription manager, install the required packages.
        ```sh
        subscription-manager refresh
        subscription-manager attach --auto
        subscription-manager status
        subscription-manager repos --enable rhel-7-server-optional-rpms --enable rhel-server-rhscl-7-rpms
        subscription-manager repos --enable rhel-server-rhscl-7-rpms
        subscription-manager repos --enable rhel-7-server-optional-rpms
        subscription-manager repos --enable rhel-7-server-rh-common-rpms
        subscription-manager repos --enable rhel-7-server-supplementary-rpms
        subscription-manager repos --enable rhel-7-server-extras-rpms
        yum install rh-python36 -y
        yum install container-selinux -y
        ```
        {: codeblock}
        
        Depending on your custom image, these packages might already be installed.
        {: note}

### 4. Upload and run the host attach script
{: #alibaba-host-script-run}

1. Locate your host script file on your system that you downloaded in step [1. Download the host script](#alibaba-host-script).
2. Upload the host script file to your Alibaba instance.
    
    ```sh
    scp -i <PEM-FILE> <Path2HostScript> cloud-user@<PUBLIC_IP>:/tmp/attach.sh
    ```
    {: pre}
    
3. Log in to your instance.

    ```sh
    ssh -i <path-to-pemfile> cloud-user@<PUBLIC_IP>
    ```
    {: pre}

4. Run the host script file. For example,

    ```sh
    sudo nohup bash /tmp/attach.sh &
    ```
    {: pre}

5. Review the status of the registration script.
        
    ```sh
    journalctl -f -u ibm-host-attach
    ```
    {: pre}  

6. Check that your hosts are shown in the **Hosts** tab of your [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}. All hosts show a **Status** of `Ready` when a connection to the machine can be established. Note that the hosts show a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} location control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.   

7. Repeat [step 3](#alibaba-host-install-packages) and [step 4](#alibaba-host-install-packages) for each instance that you want to add to your location.

### 5. Configure the control plane
{: #alibaba-host-control-plane}

The {{site.data.keyword.satelliteshort}} control plane manages the clusters and {{site.data.keyword.cloud_notm}} services that run in your location and provides a secure connection from your location to {{site.data.keyword.cloud_notm}}. You must add at least 3 hosts for high availability.

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to create a {{site.data.keyword.satelliteshort}} control plane. This location must be in a `Action required` state.
2. From the **Getting started** page, select **Configure the control plane**.
3. Select the hosts that you want to use for your control plane. These hosts must be a `Ready` state and `Unassigned` availability.
4. Click **Next**.
5. Wait for {{site.data.keyword.satelliteshort}} to set up your control plane.

## Security group settings
{: #alibaba-reqs-secgroup}

As described in the [host networking requirements](/docs/satellite?topic=satellite-reqs-host-network), your Alibaba hosts must have access to connect to {{site.data.keyword.satellitelong_notm}}. If you use hosts in a virtual private cloud (VPC), you can create a security group similar to the following example. You can get the owner, group, user, and VPC IDs from your Alibaba provider resources.
{: shortdesc}

Example security group for Alibaba

|Action|Priority|Protocol Type|Port Range|Authorization Object|
|------|-----|------|-----|-----|
| Allow |	1 | Custom TCP | Destination `30000/32767` | Source `0.0.0.0/0` |
| Allow |	1 | Custom TCP | Destination `443/443` | Source `0.0.0.0/0` |
{: summary="The table rows are read from left to right. The first column contains the event for the action. The second column describes the action."}
{: caption="Cluster add-on events" caption-side="top"}

In addition to these inbound rules, you must allow all outbound connectivity to all ports and IP addresses.
{: note}

For more information, see [Security groups](https://www.alibabacloud.com/help/en/elastic-compute-service/latest/security-security-groups){: external} in the Alibaba documentation.




## I created a {{site.data.keyword.satelliteshort}} location, what's next?
{: #alibaba-whats-next}

Now that your {{site.data.keyword.satelliteshort}} location is set up, you are ready to start using {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Add compute capacity to your location by [attaching more hosts to the location](/docs/satellite?topic=satellite-attach-hosts) so that you can run [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster. You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-satcon-existing) to use as deployment targets.
3. Start [deploying Kubernetes resources to these clusters](/docs/satellite?topic=satellite-satcon-create) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-sat-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.



