---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-24"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Logging in to a host machine to debug
{: #ts-hosts-login}

You might need to log in to your host machine to debug a host issue further.
{: shortdesc}

You can SSH into the host machine if you did not assign the host to a cluster, or if the assignment failed. Otherwise, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host by using SSH for security purposes. You can [remove the host](/docs/satellite?topic=satellite-host-remove) and reload the operating system to restore the ability to SSH into the host machine.
{: note}

1. Log in to the host machine.
    ```sh
    ssh root@<IP_address>
    ```
    {: pre}

2. Check the various log output files from the host registration and host bootstrapping processes. Replace `<filepath>` with the following files to check in order.
    ```sh
    tail <filepath>
    ```
    {: pre}

    1. The `nohup.out` logs from the host registration attempt.
    2. The `/var/log/firstboot.log` for the first bootstrapping attempt. If the host registration failed, you do not have this file.
    3. The `/tmp/bootstrap/bootstrap_base.log` for the base bootstrapping process, if the first boot was unsuccessful. If the host registration failed, you do not have this file.

3. Run the `journalctl` command when you attempt to attach your host again. For example, 
    ```sh
    journalctl -u ibm-host-attach --no-pager
    ```
    {: pre}
    
4. Review the logs for errors. Some common errors include the following messages.

| Message | Description |
| -------------- | -------------- |
| `export HOME=/root`   \n  `HOME=/root` \n `'[' '!' -f /var/log/firstboot.flag ']'`  \n  `~` | The first boot did not complete successfully. Check the `/tmp/bootstrap/bootstrap_base.log` file and continue looking for errors. |
| `No package matching '\''container-selinux'\'' found available, installed or updated`.  \n `No package rh-python36 available. Error: Nothing to do`. \n  (Note that the package name might be replaced with another package name.) |See [Host registration script fails](/docs/satellite?topic=satellite-host-registration-script-fails). |
| `curl: (6) Could not resolve host: <URL>.com; Unknown error` \n `tar -xvf bootstrap.tar` \n `tar: This does not look like a tar archive` \n `tar: Exiting with failure status due to previous errors` \n `[[ -n ‘’ ]]` \n `echo ‘Failed to untar bootstrap.tar’` \n `Failed to untar bootstrap.tar` \n `+ rm -rf /tmp/bootstrap` | The machine cannot be reached on the network. Check that your machine meets the [minimum requirements for network connectivity](/docs/satellite?topic=satellite-host-reqs), [remove the host](/docs/satellite?topic=satellite-host-remove), and try to [add](/docs/satellite?topic=satellite-attach-hosts) and [assign](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual) the host again. Alternatively, the infrastructure provider network might have issues, such as a failed connection. Consult the infrastructure provider documentation for further debugging steps. |
| `kubectl --kubeconfig=/tmp/bootstrap/priveledgedcertdir/privledged-kubeconfig` | The host is trying to register itself with the location.  \n 1. Find the {{site.data.keyword.satelliteshort}} control plane endpoint with the `ibmcloud sat location get --location <LOCATION_ID>` command.  \n 1. Find the **Public Service Endpoint URL** field, for example, `https://c103-e.containers.cloud.ibm.com:12345`.  \n 1. Confirm your connection exists by running `nc -z -v <ENDPOINT>` from your host, for example, `nc -z -v c103-e.containers.cloud.ibm.com 12345`.  \n 1. Repeat the previous step to verify that your host can connect to each of the required outbound [hostnames for your region](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-outbound). |
{: caption="Table 1. Common host machine registration and bootstrapping errors" caption-side="top"}
