---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Logging in to a RHEL host machine to debug
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

2. Check the various log output files from the host registration and host bootstrapping processes. Replace `<filepath>` with the following files to check in order. Depending on the issue, certain log files might or might not files be present on the host.

    1. The `nohup.out` logs from the host registration attempt.
    2. The `/var/log/firstboot.log` for the first bootstrapping attempt. If the host registration failed, you do not have this file.
    3. The `/tmp/bootstrap/bootstrap_base.log` for the base bootstrapping process, if the first boot was unsuccessful. If the host registration failed, you do not have this file.
    
        ```sh
        tail <filepath>
        ```
        {: pre}

3. Run the `journalctl` commands when you attempt to attach your host again to log the systemd service that is causing the issue.

    ```sh
    journalctl -u ibm-host-agent --no-pager
    ```
    {: pre}
    
    ```sh
    journalctl -u ibm-first-boot --no-pager
    ```
    {: pre}
    
    ```sh
    journalctl -u ibm-host-attach --no-pager
    ```
    {: pre}
   
    
4. Review the logs for errors. See the following sections for more details.


## First boot did not complete successfully
{: #ts-hosts-login-first-boot}

You receive output similar to the following messages.

```sh
export HOME=/root
HOME=/root
'[' '!' -f /var/log/firstboot.flag ']'
~
```
{: codeblock}

The first boot did not complete successfully. Check the `/tmp/bootstrap/bootstrap_base.log` file and continue looking for errors.

## Host registration script fails
{: #ts-hosts-login-host-script}

You receive output similar to the following messages. Note that the package name might be replaced with another package name.

```sh
No package matching '\''container-selinux'\'' found available, installed or updated
No package rh-python36 available. Error: Nothing to do
```
{: codeblock}

For more information about these messages, see [Host registration script fails](/docs/satellite?topic=satellite-host-registration-script-fails).

## Machine cannot be reached on the network
{: #ts-hosts-login-cannot-reach}

You receive output similar to the following messages.

```sh
curl: (6) Could not resolve host: <URL>.com; Unknown error
tar -xvf bootstrap.tar
tar: This does not look like a tar archive
tar: Exiting with failure status due to previous errors
[[ -n ‘’ ]]
echo ‘Failed to untar bootstrap.tar’
Failed to untar bootstrap.tar` \n `+ rm -rf /tmp/bootstrap
```
{: codeblock}

The machine cannot be reached on the network. Check that your machine meets the [minimum requirements for network connectivity](/docs/satellite?topic=satellite-host-reqs), [remove the host](/docs/satellite?topic=satellite-host-remove), and try to [add](/docs/satellite?topic=satellite-attach-hosts) and [assign](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual) the host again. Alternatively, the infrastructure provider network might have issues, such as a failed connection. Consult the infrastructure provider documentation for further debugging steps.

## Host is attempting to register with the location
{: #ts-hosts-login-host-register}

You receive output similar to the following message.

```sh
kubectl --kubeconfig=/tmp/bootstrap/priveledgedcertdir/privledged-kubeconfig
```
{: codeblock}

The host is attempting to register with the location.

1. Find the {{site.data.keyword.satelliteshort}} control plane endpoint.

    ```sh
    ibmcloud sat location get --location <LOCATION_ID>
    ```
    {: pre}
    
2. Find the **Public Service Endpoint URL** field, for example, `https://c103-e.containers.cloud.ibm.com:12345`.
3. Confirm your connection exists by running `nc -z -v <ENDPOINT>` from your host. For example,
    
    ```sh
    nc -z -v c103-e.containers.cloud.ibm.com 12345
    ```
    {: pre}
    
4. Repeat the previous step to verify that your host can connect to each of the required outbound [hostnames for your region](/docs/satellite?topic=satellite-reqs-host-network#reqs-host-network-firewall-outbound).


