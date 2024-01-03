---


copyright:
  years: 2020, 2024
lastupdated: "2024-01-03"

keywords: satellite, hybrid, attaching hosts, hosts, attach hosts, attach hosts to location

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Attaching on-prem hosts to your location
{: #attach-hosts}

After you create the location in {{site.data.keyword.satellitelong}}, add compute capacity to your location so that you can set up {{site.data.keyword.redhat_openshift_notm}} clusters or interact with other [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-managed-services).
{: shortdesc}

To attach Red Hat CoreOS (RHCOS) hosts, your location must be enabled for Red Hat CoreOS. For more information, see [Is my location enabled for Red Hat CoreOS?](/docs/satellite?topic=satellite-locations#verify-coreos-location). Note that you can still attach Red Hat Enterprise Linux hosts to a location that is enabled for Red Hat CoreOS.

Before you begin, make sure that you create host machines that meet the [minimum hardware requirements](/docs/satellite?topic=satellite-host-reqs) in your on-prem data center, in {{site.data.keyword.cloud_notm}}, or in public cloud providers.

After you attach a host to your location, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host as root with SSH for security purposes. You might see error messages if you try to SSH as root into a host that is attached successfully to a location. To restore the ability to SSH into the machine, you can [remove the host](/docs/satellite?topic=satellite-host-remove) and reload the operating system.

Not sure how many hosts to attach to your location? See [Sizing your {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-location-sizing).
{: tip}


## Attaching on-premises RHEL hosts to your location
{: #attach-rhel-hosts}

To attach RHEL hosts that reside in your on-premises data center to your location, follow these general steps to run the host attachment script.

1. [Download the host script](#attach-hosts) for your location. To attach a host with a RHEL operating system, the attachment script is a Shell script.
2. Retrieve the public IP address of your host, or if your host has only a private network interface, the private IP address of your host.      
3. Copy the script from your local machine to your host.
    ```sh
    scp -i <filepath_to_key_file> <filepath_to_script> <username>@<IP_address>:/tmp/attach.sh
    ```
    {: pre}        

4. Log in to your host.
    ```sh
    ssh -i <filepath_to_key_file> <username>@<IP_address>
    ```
    {: pre}

5. Update your host to have the required RHEL packages. For more information about how to install these package, see the [Red Hat documentation](https://access.redhat.com/solutions/253273){: external}.
6. Run the script.
    ```sh
    sudo nohup bash /tmp/attach.sh &
    ```
    {: pre}

7. Check the progress of the registration script.
    ```sh
    journalctl -f -u ibm-host-attach
    ```
    {: pre}

8. As you run the scripts on each machine, check that your hosts are shown in the **Hosts** tab of your location dashboard. This process might take a few minutes to complete. All hosts show a **Health** status of `Ready` when a heartbeat for the machine can be detected, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.redhat_openshift_notm}} cluster.

9. After you have attached your hosts, assign them to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-setup-control-plane) or use them to create a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters).

If your host is not attaching to your location, you can log in to the host to debug it. For more information, see [Logging in to a RHEL host machine to debug](/docs/satellite?topic=satellite-ts-hosts-login).
{: tip}

After you attach a host to your location, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host as root with SSH for security purposes. You might see error messages if you try to SSH as root into a host that is attached successfully to a location. To restore the ability to SSH into the machine, you can [remove the host](/docs/satellite?topic=satellite-host-remove) and reload the operating system.
{: note}

## Attaching on-premises Red Hat CoreOS hosts to your location
{: #attach-rhcos-hosts}

To attach Red Hat CoreOS (RHCOS) hosts that reside in your on-premises data center to your location, follow these general steps to run the host attachment script.

1. [Download the host script](#attach-hosts) for your location. To attach a host with a Red Hat CoreOS (RHCOS) operating system, the attachment script is a Red Hat CoreOS ignition (`.ign`) file.
2. Boot your RHCOS host and include the file path to the ignition script as the `--user-data`. This command varies, depending on the type of host that you are adding. For example, if your hosts are Amazon Web Services (AWS) cloud hosts, then you add `--user-data file:///tmp/attach_hypershift.ign` to your [launch template](https://docs.aws.amazon.com/cli/latest/reference/ec2/run-instances.html){: external}. Consult your provider documentation for more information about how to boot your host and include a file path to the ignition script.
3. As you run the scripts on each machine, check that your hosts are shown in the **Hosts** tab of your location dashboard. This process might take a few minutes to complete. All hosts show a **Health** status of `Ready` when a heartbeat for the machine can be detected, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} control plane or a {{site.data.keyword.redhat_openshift_notm}} cluster.
4. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters).


If your host is not attaching to your location, you can log in to the host to debug it. For more information, see [Logging in to a RHCOS host machine to debug](/docs/satellite?topic=satellite-ts-hosts-login-rhcos).
{: tip}

After you attach a host to your location, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host as root with SSH for security purposes. You might see error messages if you try to SSH as root into a host that is attached successfully to a location. To restore the ability to SSH into the machine, you can [remove the host](/docs/satellite?topic=satellite-host-remove) and reload the operating system.
{: note}

## I added hosts to my location, what's next?
{: #on-prem-whats-next-host}

Now that you added hosts to your location, you can assign them to your location control plane or to your {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Assign [hosts to the location control plane](/docs/satellite?topic=satellite-setup-control-plane) or [to your {{site.data.keyword.cloud_notm}} services](/docs/satellite?topic=satellite-assigning-hosts).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-register-openshift-clusters) to use as deployment targets.
3. [Manage your applications](/docs/satellite?topic=satellite-cluster-config) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.


