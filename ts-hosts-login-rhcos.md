---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: satellite, hybrid, multicloud, rhcos

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Logging in to a RHCOS host machine to debug
{: #ts-hosts-login-rhcos}

You might need to log in to your host machine to debug issues.
{: shortdesc}

You can SSH into the host machine if you did not assign the host to a cluster, or if the assignment failed. Otherwise, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host by using SSH for security purposes. You can [remove the host](/docs/satellite?topic=satellite-host-remove) and reload the operating system to restore the ability to SSH into the host machine.
{: note}

1. Download the host attach script and add your public SSH key.

    ```json
    {
      "ignition": {
        "version": "3.1.0"
      },
      "passwd": {
        "users": [
          {
            "name": "core",
            "sshAuthorizedKeys": [ "PUBLIC-SSH-KEY" ]
          }
        ]
      },
    ...
    }
    ```
    {: codeblock}

1. Log in to your host as `core`.
    ```sh
    ssh core@<IP_address>
    ```
    {: pre}

1. Run the following `journalctl` commands when you attempt to attach your host again to log the systemd service that is causing the issue.

    ```sh
    journalctl -u ibm-host-attach --no-pager
    ```
    {: pre}

    ```sh
    journalctl -u ibm-host-agent --no-pager
    ```
    {: pre}
    
    ```sh
    journalctl -u ibm-firstboot-ignition --no-pager
    ```
    {: pre}
    

    
1. Review the logs for errors. See the following section for more details.

## Machine cannot be reached on the network
{: #ts-hosts-login-cannot-reach-rhcos}

You receive output similar to the following messages.

```sh
curl: (6) Could not resolve host
```
{: codeblock}

The machine cannot be reached on the network. Check that your machine meets the [minimum requirements for network connectivity](/docs/satellite?topic=satellite-host-reqs), [remove the host](/docs/satellite?topic=satellite-host-remove), and try to [add](/docs/satellite?topic=satellite-attach-hosts) and [assign](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual) the host again. 

Alternatively, the infrastructure provider network might have issues, such as a failed connection. Consult the infrastructure provider documentation for further debugging steps.




