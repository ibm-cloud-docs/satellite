---

copyright:
  years: 2022
lastupdated: "2022-02-22"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why aren't my hosts attaching to my location?
{: #host-not-attaching}


When you run your host attach script, some hosts are not attaching to your location, but you do not receive an error message. Your host machines are virtual machine clones.
{: tsSymptoms}

Network identities of virtual machines that are attached to the same location must be unique. If you are using cloned virtual machines, these clones might not have a unique network identity and Satellite cannot attach them all to the location. To verify that the network identities are not unique, SSH into the host and run the following commands.
{: tsCauses}

```sh
cat /etc/machine-id
cat /var/lib/dbus/machine-id
```
{: pre}

If the IDs match, then you must take steps to resolve this issue.

To resolve this issue, follow these steps.
{: tsResolve}

1. Remove all hosts from the location. 
2. Reload your operating systems on each host.
3. SSH into each host and run the following commands.

    ```sh
    rm -f /etc/machine-id
    dbus-uuidgen --ensure=/etc/machine-id
    rm -f /var/lib/dbus/machine-id
    dbus-uuidgen --ensure
    ```
    {: pre}

4. Reattach your hosts.
