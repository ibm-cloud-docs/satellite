---

copyright:
  years: 2022, 2022
lastupdated: "2022-12-01"

keywords: satellite, hybrid, multicloud, bare metal, coreos, rhcos, virtualization

subcollection: satellite

content-type: tutorial
services: satellite
account-plan: paid
completion-time: 2hr

---

{{site.data.keyword.attribute-definition-list}}


# Setting up virtualization on {{site.data.keyword.cloud_notm}} Bare Metal Servers for Classic
{: #assign-bare-metal}
{: toc-content-type="tutorial"}
{: toc-services="satellite"}
{: toc-completion-time="2hr"}

You can set up your {{site.data.keyword.baremetal_short}} to use {{site.data.keyword.redhat_openshift_notm}} virtualization in your {{site.data.keyword.satelliteshort}} location. By using virtualization, you can provision Windows or other virtual machines on your {{site.data.keyword.baremetal_short}} in a managed {{site.data.keyword.redhat_openshift_notm}} space. 
{: shortdesc}

Supported host operating systems
:   Red Hat CoreOS (RHCOS)

The following steps use {{site.data.keyword.baremetal_long}} for Classic. However, you can adapt these steps for your own bare metal servers.
{: note}

## Prerequisites
{: #assign-bare-metal-prereq}

- Create a RHCOS-enabled location. To check whether your location is RHCOS-enabled, see [Is my location enabled for Red Hat CoreOS?](/docs/satellite?topic=satellite-locations#verify-coreos-location). If your location is not enabled, [create a new one with RHCOS](/docs/satellite?topic=satellite-locations).
- Attach hosts to your location and set up your [location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).
- Find and record your bare metal host name. For this {{site.data.keyword.baremetal_short_sing}}, this information is found in the **Name** field on the **Overview** page for your specific {{site.data.keyword.baremetal_short}}.
- Find your bare metal server network information. For this {{site.data.keyword.baremetal_short_sing}}, this information is found in the **Network details** section on the **Overview** page. Record the CIDR and gateway information for the public and private interfaces for your system.
- Create or identify an {{site.data.keyword.cos_full_notm}} bucket to store your ignition file.
- Create or identify a cluster within the {{site.data.keyword.satelliteshort}} location that runs a supported operating system; for example, this tutorial uses a {{site.data.keyword.redhat_openshift_notm}} cluster that is running 4.11.


In addition, the {{site.data.keyword.baremetal_short}} used in this example required the following prerequisites.

- If you plan to have multiple VLANs for your cluster, multiple subnets on the same VLAN, or are planning for a multizone classic cluster, [enable VRF in your account](/docs/account?topic=account-vrf-service-endpoint).
- [Create two VLAN pairs](/docs/cli?topic=cli-manage-classic-vlans#sl_vlan_create) (public and private) in the same {{site.data.keyword.cloud_notm}} data center pod for each zone for your bare metal host.
- Later in this tutorial, you deploy [OpenShift Data Foundation for local disks](/docs/satellite?topic=satellite-config-storage-odf-local&interface=ui). This solution requires additional storage devices on the worker nodes. 

## {{site.data.keyword.baremetal_short_sing}} requirements
{: #setup-bare-metal}

To set up virtualization, your {{site.data.keyword.baremetal_short_sing}} must meet the following requirements. 

- Must support virtualization technology.
    - For Intel CPUs, support for virtualization is referred to as `Intel VT` or `VT-x`.
    - For AMD CPUs, support for virtualization is referred to as `AMD Virtualization` or `AMD-V`.
- Must have a minimum of minimum of 8 cores and 32 GB RAM, plus any additional cores that you need for your vCPU overhead. For more information, see [CPU overhead](https://docs.openshift.com/container-platform/4.11/virt/install/preparing-cluster-for-virt.html#CPU-overhead_preparing-cluster-for-virt){: external} in the {{site.data.keyword.redhat_openshift_notm}} docs.
- Must include enough memory for your workload needs. For example: `360 MiB + (1.002 * requested memory) + 146 MiB + 8 MiB * (number of vCPUs) + 16 MiB * (number of graphics devices)`. For more information, see [Memory overhead](https://docs.openshift.com/container-platform/4.11/virt/install/preparing-cluster-for-virt.html#memory-overhead_preparing-cluster-for-virt){: external} in the {{site.data.keyword.redhat_openshift_notm}} docs.
- Must not have an operating system installed. The Red Hat CoreOS operating system is installed later in this process.
- If you want to use OpenShift Data Foundation as your storage solution, add 2 storage disks to each of your {{site.data.keyword.baremetal_short}} when you provision them.

If your servers do not meet these requirements, follow the steps to [create a {{site.data.keyword.baremetal_short_sing}}](/docs/bare-metal?topic=bare-metal-ordering-baremetal-server). For a list of bare metal options, see [Available options for a bare metal server](/docs/bare-metal?topic=bare-metal-about-bm#options-for-bare-metal-servers).
{: tip}

## Booting up your {{site.data.keyword.baremetal_short_sing}}
{: #boot-bare-metal}
{: step}

For this specific {{site.data.keyword.baremetal_short_sing}}, you must use a browser that supports Java for classic. You can use the Safari browser on your local system or [download a Java version](https://www.java.com/en/download/){: external} that supports `javaws`.
{: note}

1. [Download a Red Hat CoreOS ISO](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/){: external}. Find the corresponding ISO version that matches the {{site.data.keyword.redhat_openshift_notm}} version that you want to use. For example, if you want to use version 4.11, [download a version of RHCOS for 4.11](https://mirror.openshift.com/pub/openshift-v4/x86_64/dependencies/rhcos/4.11/){: external} like `rhcos-4.11.9-x86_64-live.x86_64.iso`.
1. Log in to your VPN to access to your host network. For more information, see [VPN access on {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/vpn-access){: external}.
1. From the [Device list in the console](https://cloud.ibm.com/gen1/infrastructure/devices){: external}, select your bare metal server.
1. From the **Overview** page, note the networking values for your server. Find and verify the  CIDR and gateway information.
1. Click **Remote management** and make note of the `User` and `Password` in the **Management details** section. You use this username and password in later steps.
1. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions icon") > **KVM Console** to open your {{site.data.keyword.baremetal_short_sing}} console. Your browser might display a warning of an insecure self-signed certificate. Add the certificate to your browser truststore as trusted CA certificate to continue.
1. Log in to your server with the `User` and `Password` that you retrieved earlier.
1. On the **System** tab, in the **Remote console preview**, click **Settings**.
1. Select **Java** to change the interface to use Java instead of HTML5.
1. Click the console preview to download a `launch.jnlp` file.
1. Open a terminal window and run the `.jnlp` file that you downloaded earlier with the `javaws <path_to_.jnlp>` command. Note that you might be prompted to update Java. If so, follow the prompts to update Java and then retry the command. If you are prompted to allow input monitoring for the Terminal app or if the Java console window crashes, you must update your system preference setting for Input monitoring. You can update this setting by going to **System preferences > Security & Privacy > Privacy > Input monitoring**. Then, relaunch the Java console with the `javaws <path_to_.jnlp>` command.
1. After the console loads, select **Virtual Media > Virtual Storage**. 
1. In the **Logical Drive Type** section, select **ISO File**.
1. Click **Open Image** and select the RHCOS ISO file that you downloaded.
1. Click **Plug in**.
1. Enter `exit` to access BIOS log in.
1. Enter your Softlayer password at BIOS prompt. 
1. On the **Advanced** tab, look for virtualization settings and enable them. 
    1. Select **CPU Configuration**.
    2. Look for `VTD` or `Intel Virtualization Technology` and make sure to enable it. For Intel CPUs, support is referred to as `Intel VT` or `VT-x`. For AMD CPUs, supported is referred to as `AMD Virtualization` or `AMD-V`. For more information, consult your hardware manufacturer documentation.
    3. Enable `VTD` if not enabled. For more information, consult your hardware provider documentation.
1. Configure the boot order. For example, to install this {{site.data.keyword.baremetal_short_sing}}, you can use a virtual ISO file. Your specific case might use a different external boot device.
    1. Select **Boot**.
    2. Select the option to Boot from Virtual ISO. 
    3. Ensure that `Hard Drive` is also an option in boot order.
1. Save your choices and exit. For example, for this {{site.data.keyword.baremetal_short_sing}}, click **Save and Exit**.
1. Save your changes and start the installation process. For example, for this {{site.data.keyword.baremetal_short_sing}}, click **Save Changes and Reset**.

When you save and exit, RHCOS begins installing. The next time your system reboots, it boots RHCOS into memory. 

Proceed with the following sections immediately to prevent memory overwriting and corruption.
{: important}

## Setting up public networking
{: #public-network-bare-metal}
{: step}

You must set up a public network connections for your bare metal machine to download your host attach script (RHCOS ignition file). Identify your private and public interfaces. RHCOS translates network interface names to `eno` or `ens`. For this {{site.data.keyword.baremetal_short_sing}}, the public interface is `eth1` and the private interface is `eth0`.

After RHCOS is booted into memory, the `core@localhost` prompt is available. Follow these steps to set up your network connectivity for your bare metal server.

1. At the `core@localhost` prompt, run `ifconfig` to determine which interface is your public interface. Depending on the flavor of the bare-metal, the setup of the wired connections may be different. In this set up, the public interfaces were wired connections 3 and 5 and the private interfaces were wired connections 2 and 4. You can find this information in the output of the **`ifconfig`** command.
1. At the `core@localhost` prompt, enter `sudo nmtui`.
1. Click **Edit a Connection**. 
1. For each wired connection that you want to activate, click **IPv4 Configuration > Manual**.
1. Select **Show**.
1. Enter your CIDR for **Addresses**. The CIDR is the IP address and the subnet mask for that subnet.
1. Enter your gateway for **Gateway**.
1. Add DNS server information. For example, for this {{site.data.keyword.baremetal_short_sing}}, these values are `8.8.8.8` and `4.4.4.4` for public interfaces; and `10.0.80.11` and `10.0.80.12` for private interfaces.
1. Repeat these steps for each of the wired connections that you want to activate.
1. When you are finished, select **OK**, **Quit**.
1. Test that you have connectivity by pinging your server network. For example, to test the public network for this specific {{site.data.keyword.baremetal_short_sing}}, `ping 8.8.8.8`.


## Configuring your ignition file
{: #config-ignition}
{: step}

Complete the following steps to configure your ignition file. The ignition file is used to attach your bare metal server as a host in your {{site.data.keyword.satelliteshort}} location.

You must configure a separate ignition file for each bare metal host that you are attaching to the location.
{: note}

1. [Download the host attach script for your location](/docs/satellite?topic=satellite-attach-hosts#host-attach-download){: external}. Make sure to specify `RHCOS` for the host operating system.
1. Get the host name for your bare metal system; for example, `mybaremetalserver`.
1. Convert your host name to `base64` by running the following command, substituting `<hostname>` with your bare metal server host name:
    ```sh
    echo <hostname> | base64
    ```
    {: pre}

1.  Open your ignition file and add the host name information to the `"storage":{"files":[` section. Replace `<hostname>` with the base64 encoded output from the previous step.

    ```sh
      {"overwrite": true,"path": "/etc/hostname","mode": 600,"contents": {"source": "data:text/plain;base64,<hostname>"},},],}
    ```
    {: codeblock}
    
    The first block of the ignition file with the host name `mybaremetalserver` is shown in the following example:
    
    ```sh
    {"ignition":{"version":"3.1.0"},"passwd":{"users":[{"name":"core","sshAuthorizedKeys":[""]}]},"storage":{"files":[{"overwrite": true,"path": "/etc/hostname","mode": 600,"contents": {"source": "data:text/plain;base64,bXliYXJlbWV0YWxzZXJ2ZXIK"}},{"overwrite":true,"path":"/usr/local/bin/ibm-host-attach.sh","contents":{"source":"data:text/plain;base64,
    ...
    ```
    {: codeblock}

1. Convert your private networking information to `base64` by running the following command. Replace `<private_interface>` with the name of your private interface name, for example, `en01`. Replace `<privateIPCIDR>` and `<gateway>` with your private IP CIDR and gateway. These values are what you used to set up your connection in the previous section.

    ```sh
    echo '[connection]
    id=<private-interface-name>
    type=ethernet
    interface-name=<private-interface-name>
    [ipv4]
    never-default=true
    address1=<private-ip-cidr>,<gateway> 
    dns=10.0.80.11;10.0.80.12;
    route1=10.0.0.0/8,<gateway>
    route2=161.26.0.0/16,<gateway>
    route3=166.8.0.0/14,<gateway>
    dns-search=
    may-fail=false
    method=manual' | base64
    ```
    {: codeblock}
    
    For example:
    
      ```sh
      echo '[connection]
      id=eno1
      type=ethernet
      interface-name=eno1
      [ipv4]
      never-default=true
      address1=10.190.196.9/26,10.190.196.129
      dns=10.0.80.11;10.0.80.12;
      route1=10.0.0.0/8,10.190.96.129
      route2=161.26.0.0/16,10.190.96.129
      route3=166.8.0.0/14,10.190.96.129
      dns-search=
      may-fail=false
      method=manual' | base64
      ```
      {: screen}

1. Add these connection details to your ignition file. Replace `<private_interface>` with your private interface name, for example `eno1`. Replace `<private_connection_details>` with the base64 encoded output from the previous step.

    ```sh
      {"overwrite": true,"path": "/etc/NetworkManager/system-connections/<private_interface>.nmconnection","mode": 256,"contents": {"source": "data:text/plain;base64,<private_connection_details>"}}
    ```
    {: codeblock}
    
    For example, to add the networking information from the previous step, enter the following code sample.
    
    ```sh
    { "overwrite": true,"path": "/etc/NetworkManager/system-connections/en01.nmconnection","mode": 256,"contents": {     "source": "data:text/plain;base64,W2Nvbm5lY3Rpb25dCiAgICAgIGlkPWVubzEKICAgICAgdHlwZT1ldGhlcm5ldAogICAgICBpbnRlcmZhY2UtbmFtZT1lbm8xCiAgICAgIFtpcHY0XQogICAgICBuZXZlci1kZWZhdWx0PXRydWUKICAgICAgYWRkcmVzczE9MTAuMTkwLjE5Ni45LzI2LDEwLjE5MC4xOTYuMTI5CiAgICAgIGRucz0xMC4wLjgwLjExOzEwLjAuODAuMTI7CiAgICAgIHJvdXRlMT0xMC4wLjAuMC84LDEwLjE5MC45Ni4xMjkKICAgICAgcm91dGUyPTE2MS4yNi4wLjAvMTYsMTAuMTkwLjk2LjEyOQogICAgICByb3V0ZTM9MTY2LjguMC4wLzE0LDEwLjE5MC45Ni4xMjkKICAgICAgZG5zLXNlYXJjaD0KICAgICAgbWF5LWZhaWw9ZmFsc2UKICAgICAgbWV0aG9kPW1hbnVhbAo="}},
    ```
    {: screen}

1. Convert your public networking information to `base64` by running the following command. Replace `<public_interface>` with the name of your public interface name, for example, `eno2`.  Replace `<publicIPCIDR>` and `<gateway>` with your public IP CIDR and public IP gateway. These values are what you used to set up your connection in the previous section.

    ```sh
    echo '[connection]
    id=<public_interface>
    type=ethernet
    interface-name=<public_interface>
    [ipv4]
    address1=<publicIPCIDR>,<gateway>
    dns=8.8.8.8;4.4.4.4;
    dns-search=
    may-fail=false
    method=manual' |base64
    ```
    {: codeblock}
    
    
    Example command to base64 encode your public interface details.
    ```sh
    echo '[connection]
    id=eno2
    type=ethernet
    interface-name=eno2
    [ipv4]
    address1=52.117.108.24/28,52.117.108.17
    dns=8.8.8.8;4.4.4.4;
    dns-search=
    may-fail=false
    method=manual' |base64
    ```
    {: screen}

1. Add these connection details to your ignition file. Replace `<public_interface>` with your public interface name, for example `eno2`. Replace `<public_connection_details>` with the base64 encoded output from the previous step.

    ```sh
      {"path": "/etc/NetworkManager/system-connections/<public_interface>.nmconnection","mode": 256,"contents": {"source": "data:text/plain;base64,<public_connection_details>"}}
    ```
    {: codeblock}
    
    For example, to add the networking information from the previous step, enter the following code sample, immediately following the private interface code sample.
    
    ```sh
    {"path": "/etc/NetworkManager/system-connections/en02.nmconnection","mode": 256,"contents": {"source": "data:text/plain;base64,W2Nvbm5lY3Rpb25dCiAgICBpZD1lbm8yCiAgICB0eXBlPWV0aGVybmV0CiAgICBpbnRlcmZhY2UtbmFtZT1lbm8yCiAgICBbaXB2NF0KICAgIGFkZHJlc3MxPTUyLjExNy4xMDguMjQvMjgsNTIuMTE3LjEwOC4xNwogICAgZG5zPTguOC44Ljg7NC40LjQuNDsKICAgIGRucy1zZWFyY2g9CiAgICBtYXktZmFpbD1mYWxzZQogICAgbWV0aG9kPW1hbnVhbAo="}},
    
    ```
    {: screen}

1. Save your ignition file. Your file must be flat and not contain any returns. The following example ignition file shows the additions made with the previous steps for `mybasemetalserver`.

    ```sh
    {
      "ignition": {
        "version": "3.1.0"
      },
      "storage": {
        "files": [
          {
            "overwrite": true,
            "path": "/etc/hostname",
            "mode": 600,
            "contents": {
              "source": "data:text/plain;base64,ay1jY3BtYmhldzA0ZGp2ZXNvYmw4Zy1ibS0xCg=="
            }
          },
          {
            "overwrite": true,
            "path": "/etc/NetworkManager/system-connections/en01.nmconnection",
            "mode": 256,
            "contents": {
              "source": "data:text/plain;base64,W2Nvbm5lY3Rpb25dCmlkPWVubzEKdHlwZT1ldGhlcm5ldAppbnRlcmZhY2UtbmFtZT1lbm8xCltpcHY0XQpuZXZlci1kZWZhdWx0PXRydWUKYWRkcmVzczE9IDEwLjE3MC4xNS42NS8yNiwxMC4xNzAuMTUuNjUKZG5zPTEwLjAuODAuMTE7MTAuMC44MC4xMjsKcm91dGUxPTEwLjAuMC4wLzgsMTAuMTcwLjE1LjY1CnJvdXRlMj0xNjEuMjYuMC4wLzE2LDEwLjE3MC4xNS42NQpyb3V0ZTM9MTY2LjguMC4wLzE0LDEwLjE3MC4xNS42NQpkbnMtc2VhcmNoPQptYXktZmFpbD1mYWxzZQptZXRob2Q9bWFudWFsCg=="
            }
          },
          {
            "path": "/etc/NetworkManager/system-connections/en02.nmconnection",
            "mode": 256,
            "contents": {
              "source": "data:text/plain;base64,W2Nvbm5lY3Rpb25dICAgICAgICAgICAgICAgICAgICAgICAgIAppZD1lbm8yCnR5cGU9ZXRoZXJuZXQKaW50ZXJmYWNlLW5hbWU9ZW5vMgpbaXB2NF0KYWRkcmVzczE9MTY5LjQ1LjIzNS4yMTQvMjgsMTY5LjQ1LjIzNS4yMDkKZG5zPTguOC44Ljg7NC40LjQuNDsKZG5zLXNlYXJjaD0KbWF5LWZhaWw9ZmFsc2UKbWV0aG9kPW1hbnVhbAo="
            }
          },
          {
            "overwrite": true,
            "path": "/usr/local/bin/ibm-host-attach.sh",
            "contents": {
              "source": "data:text/plain;base64,<omitted>"
            },
            "mode": 493
          }
        ]
      },
      "systemd": {
        "units": [
          {
            "contents": "[Unit]\nDescription=IBM Host Attach Service\nWants=network-online.target\nAfter=network-online.target\n[Service]\nEnvironment=\"PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\"\nExecStart=/usr/local/bin/ibm-host-attach.sh\nRestart=on-failure\nRestartSec=5\n[Install]\nWantedBy=multi-user.target",
            "enabled": true,
            "name": "ibm-host-attach.service"
          }
        ]
      }
    }
        ...
        "},"mode":493}]},"systemd":{"units":[{"contents":"[Unit]\nDescription=IBM Host Attach Service\nWants=network-online.target\nAfter=network-online.target\n[Service]\nEnvironment=\"PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin\"\nExecStart=/usr/local/bin/ibm-host-attach.sh\nRestart=on-failure\nRestartSec=5\n[Install]\nWantedBy=multi-user.target","enabled":true,"name":"ibm-host-attach.service"}]}}
    ```
    {: screen}
    
1. Upload your ignition file to the {{site.data.keyword.cos_full_notm}} bucket that you identified earlier. From your **Bucket** page in the console, click **Add object**. Drag your ignition file in the bucket. 
  
## Attaching your bare metal host to the location
{: #load-ignition-file-bare-metal}
{: step}


Download the ignition file to your bare metal host, then run it to attach the bare metal host to your {{site.data.keyword.satelliteshort}} location.


1. Run the following command to download the file from your bucket to your host machine. Replace `<bucketname>` with the name of your {{site.data.keyword.cos_full_notm}} bucket, `<cos_region>` with the region of your bucket (for example, `us.east`), and `<filename>` with the name of your ignition file that is in the bucket.

    You can run the `curl` command without creating a local file on the bare metal server to ensure your bare metal server can reach the bucket by removing `> ignition.ign` from the following example.
    {: tip}

    ```sh
    curl https://<bucketName>.s3.<cos_region>.cloud-object-storage.appdomain.cloud/<filename> > ignition.ign
    ```
    {: pre}
    
    For example:
    
    ```sh
    curl https://mybucket.s3.us-east.cloud-object-storage.appdomain.cloud/attachHostmysatloc.ign > ignition.ign
    ```
    {: codeblock}

1. Identify the disk that you want to install the CoreOS operating system to. To identify disks, run `lsblk`

    ```sh
    lsblk
    ```
    {: pre}
    
    The following example is possible output.
    
    ```sh
    NAME  MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
    loop0   7:0    0  15.5G  0 loop  /run/ephemeral
    loop1   7:1    0 979.1M  1 loop  /sysroot
    sda     8:0    0   1.8T  0 disk  
    sr0    11:0    1   1.1G  0 rom.  /run/media/iso  
    ```
    {: screen}
    
    From this output, choose to install on the `sda` disk. 

1. Run the following `install` command to start the ignition file. Replace `<diskName>` with the full path of the disk that you retrieved from `lsblk` and replace `<filename>` with the name of the file you downloaded from {{site.data.keyword.cos_full_notm}}.

    ```sh
    sudo coreos-installer install <diskName> --ignition-file <filename>
    ```
    {: pre}

    The following example shows the commmand to install on`/dev/sda` with an ignition file called `./ignition.ign`.
    
    ```sh
    sudo coreos-installer install /dev/sda --ignition-file ./ignition.ign
    ```
    {: pre}

    The installation process can take an hour or two to complete. 
    {: note}

1. After the installation completes, unplug your RHCOS ISO file and reboot from your hard disk. 
    1. Enter `exit` to access BIOS log in.
    2. Enter your Softlayer password at BIOS prompt. 
    3. Select **Boot**.
    4. Select to boot from hard drive.
    5. Click **Save and Exit**.
    6. Click **Save Changes and Reset**.
1. Check your {{site.data.keyword.satelliteshort}} location to confirm that your bare metal server is attached. 

Congratulations! Your {{site.data.keyword.baremetal_short_sing}} is now attached to your location. 
 
## Assigning a {{site.data.keyword.baremetal_short_sing}} host to your {{site.data.keyword.redhat_openshift_notm}} cluster
{: #assign-bare-metal-cluster}
{: step}

After your {{site.data.keyword.baremetal_short_sing}} is attached to your location, you can assign it to a {{site.data.keyword.redhat_openshift_notm}} cluster worker pool. 

1. Find the hosts to add to your {{site.data.keyword.redhat_openshift_notm}} cluster worker pool.

    ```sh
    ibmcloud sat hosts --location <locationID>
    ```
    {: pre}

2. Assign the {{site.data.keyword.baremetal_short_sing}} to the {{site.data.keyword.redhat_openshift_notm}} cluster worker pool

    ```sh
    ibmcloud sat host assign --location <locationID> --cluster <clusterID> --host <hostID> --worker-pool default --zone <zone>
    ```
    {: pre}

Repeat this tutorial to attach more {{site.data.keyword.baremetal_short}} to your location and cluster.
{: tip}

Now that your {{site.data.keyword.baremetal_short_sing}} is assigned to a worker pool, you can set up {{site.data.keyword.redhat_openshift_notm}} virtualization.


## Setting up storage for your cluster
{: #virt-cluster-storage}
{: step}

In this example scenario, you deploy OpenShift Data Foundation across 3 nodes in the cluster by automatically discovering the available storage disks on your {{site.data.keyword.baremetal_short}}.

After you have attached at least 3 {{site.data.keyword.baremetal_short}} to your location and assigned them as worker nodes in your cluster, you can deploy OpenShift Data Foundation by using the `odf-local` {{site.data.keyword.satellite_short}} storage template.

1. From the [{{site.data.keyword.satelliteshort}} locations console](https://cloud.ibm.com/satellite/locations){: external}, click your location, then click **Storage > Create storage configuration**.
1. Give your configuration a name.
1. Select **OpenShift Data Foundation for local devices** and select version **4.10**
1. For this example, leave the rest of the default settings and click **Next**.
1. Wait for ODF to deploy. Then, verify the pods are ready by listing the pods in the `openshift-storage` namespace.
    ```sh
    oc get pods -n openshift-storage
    ```
    {: pre}
    
    Example output
    ```sh
    NAME                                                              READY   STATUS      RESTARTS   AGE
    ocs-metrics-exporter-5b85d48d66-lwzfn                             1/1     Running     0          2d1h
    ocs-operator-86498bf74c-qcgvh                                     1/1     Running     0          2d1h
    odf-console-68bcd54c7c-5fvkq                                      1/1     Running     0          2d1h
    rook-ceph-mgr-a-758845d77c-xjqkg                                  2/2     Running     0          2d1h
    rook-ceph-mon-a-85d65d9f66-crrhb                                  2/2     Running     0          2d1h
    rook-ceph-mon-b-74fd78856d-s2pdf                                  2/2     Running     0          2d1h
    rook-ceph-mon-c-76f9b8b5f9-gqcm4                                  2/2     Running     0          2d1h
    rook-ceph-operator-5d659cb494-ctkx6                               1/1     Running     0          2d1h
    rook-ceph-osd-0-846cf86f79-z97mc                                  2/2     Running     0          2d1h
    rook-ceph-osd-1-7f79ccf77d-8g4cn                                  2/2     Running     0          2d1h
    rook-ceph-osd-2-549cc486b4-7wf5k                                  2/2     Running     0          2d1h
    rook-ceph-osd-prepare-ocs-deviceset-0-data-0z6pn9-6fwqr           0/1     Completed   0          10d
    rook-ceph-osd-prepare-ocs-deviceset-1-data-0kkxrw-cppk9           0/1     Completed   0          10d
    rook-ceph-osd-prepare-ocs-deviceset-2-data-0pxktc-xm2rc           0/1     Completed   0          10d
    rook-ceph-rgw-ocs-storagecluster-cephobjectstore-a-54c58859nc8j   2/2     Running     0          2d1h
    ...
    ...
    ```
    {: screen}
    
    
## Installing the virtualization operator
{: #virt-operator-install}
{: step}

Follow the steps to [Install {{site.data.keyword.redhat_openshift_short}} Virtualization using the CLI](https://docs.openshift.com/container-platform/4.11/virt/install/installing-virt-cli.html){: external}.

## Downloading the `virtctl` CLI
{: #virt-tools}
{: step}

Follow the steps to [download the `virtctl` CLI tool](https://docs.openshift.com/container-platform/4.11/virt/install/virt-enabling-virtctl.html){: external}.

## Creating a data volume for your virtual machine
{: #virt-vm-storage}
{: step}

After you deploy OpenShift Data Foundation, you can use the `sat-ocs-ceprbd-gold` storage class to create a data volume to use storage for your VM.

1. Copy the following example data volume and save it to a file called `datavol.yaml`.
    ```yaml
    apiVersion: cdi.kubevirt.io/v1beta1
    kind: DataVolume
    metadata:
      name: fedora-1
      namespace: openshift-cnv
    spec:
      source:
        registry:
          pullMethod: node
          url: docker://quay.io/containerdisks/fedora@sha256:29b80ef738f9b09c19efc245aac3921deab9acd542c886cf5295c94ab847dfb5
      pvc:
        accessModes:
          - ReadWriteMany
        resources:
          requests:
            storage: 10Gi
        volumeMode: Block
        storageClassName: sat-ocs-cephrbd-gold
    ```
    {: codeblock}
    
1. Create the data volume.
    ```sh
    oc apply -f datavol.yaml
    ```
    {: pre}
    
1. Verify the data volume and corresponding PVC were created.
    ```sh
    oc get dv,pvc -n openshift-cnv
    ```
    {: pre}
    
    Example output.
    ```sh
    NAME                                  PHASE       PROGRESS   RESTARTS   AGE
    datavolume.cdi.kubevirt.io/fedora-1   Succeeded   100.0%                16h
    NAME                             STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS           AGE
    persistentvolumeclaim/fedora-1   Bound    pvc-fd8b1a5b-cc32-42bd-95d0-4ccf2e40bca7   10Gi       RWX            sat-ocs-cephrbd-gold   16h
    ```
    {: screen}

## Creating a virtual machine
{: #virt-vm-create}
{: step}

1. Copy the following VirtualMachine configuration and save it to a file called `vm.yaml`. Note that you can also create virtual machines through the OpenShift web console.
    ```sh
    apiVersion: kubevirt.io/v1
    kind: VirtualMachine
    metadata:
      labels:
        app: fedora-1
      name: fedora-1
      namespace: openshift-cnv
    spec:
      running: false
      template:
        metadata:
          labels:
            kubevirt.io/domain: fedora-1
        spec:
          domain:
            cpu:
              cores: 1
              sockets: 2
              threads: 1
            devices:
              disks:
              - disk:
                  bus: virtio
                name: rootdisk
              - disk:
                  bus: virtio
                name: cloudinitdisk
              interfaces:
              - masquerade: {}
                name: default
              rng: {}
            features:
              smm:
                enabled: true
            firmware:
              bootloader:
                efi: {}
            resources:
              requests:
                memory: 8Gi
          evictionStrategy: LiveMigrate
          networks:
          - name: default
            pod: {}
          volumes:
          - dataVolume:
              name: fedora-1
            name: rootdisk
          - cloudInitNoCloud:
              userData: |-
                #cloud-config
                user: cloud-user
                password: 'fedora-1-password' 
                chpasswd: { expire: False }
            name: cloudinitdisk
    ```
    {: codeblock}

1. Create the virtual machine in your cluster.
    ```sh
    oc apply -f vm.yaml
    ```
    {: pre}
    
1. Start the virtual machine.
    ```sh
    virtctl start fedora-1 -n openshift-cnv
    ```
    {: pre}
    
1. Verify the virtual machine is running.
    ```sh
    oc get vm -n openshift-cnv
    ```
    {: pre}
    
    Example output.
    ```sh
    NAME                                  AGE   STATUS    READY
    virtualmachine.kubevirt.io/fedora-1   16h   Running   True
    ```
    {: screen}
    
    
1. From the OpenShift web console, log in to your VM by using the username and password you specified in the VirtualMachine config. For example, `user: cloud-user` and `password: 'fedora-1-password'`.


Congratulations! You just deployed a Fedora virtual machine on your {{site.data.keyword.satelliteshort}} cluster.

For more information about accessing and managing VMs in your cluster, see [Additional resources](#sat-virt-additional).
    


## Additional resources
{: #sat-virt-additional}

- [Running a Windows 2019 Server VM in IBM Cloud Satellite with OpenShift Virtualization](https://lisowski0925.medium.com/running-a-windows-2019-server-vm-in-ibm-cloud-satellite-with-openshift-virtualization-234aa9a01def){: external}.
- [Using the virtualization CLI tools](https://docs.openshift.com/container-platform/4.11/virt/virt-using-the-cli-tools.html){: external}
- [Creating VMs](https://docs.openshift.com/container-platform/4.11/virt/virtual_machines/virt-create-vms.html){: external}
- [Creating a VM using the OpenShift Web Console](https://cloud.redhat.com/blog/creating-a-vm-using-the-openshift-web-console){: external}
- [Editing VMs](https://docs.openshift.com/container-platform/4.11/virt/virtual_machines/virt-edit-vms.html){: external}
- [Deleting VMs](https://docs.openshift.com/container-platform/4.11/virt/virtual_machines/virt-delete-vms.html){: external}
- [Accessing VM consoles](https://docs.openshift.com/container-platform/4.11/virt/virtual_machines/virt-accessing-vm-consoles.html){: external}


