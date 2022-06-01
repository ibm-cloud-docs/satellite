---

copyright:
  years: 2020, 2022
lastupdated: "2022-05-31"

keywords: satellite, hybrid, multicloud, troubleshoot

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I log in to my worker nodes or debug them with `oc debug` command?
{: #ts-cluster-ocdebug}

When you log in to the terminal of worker node or run the `oc debug node` command from the CLI, you receive the following error.
{: tsSymptoms}


```sh
oc debug node/10.200.0.00
Starting pod/10200000-debug ...

To use host binaries, run chroot /host
Pod IP: 10.200.0.10
If you don't see a command prompt, try pressing enter.

Removing debug pod ...
Error from server: error dialing backend: dial tcp 10.200.0.00:12345: connect: connection timed out
```
{: screen}

There is a faulty node that is causing DNS issues to the cluster. 
{: tsCauses}

To resolve this issue, determine which node is causing the error. Then, stop that node and try running `oc debug node`. See [Debugging clusters](/docs/satellite?topic=satellite-ts-clusters-debug).
{: tsResolve}

If you cannot determine which node is causing the issue, [reboot your {{site.data.keyword.satelliteshort}} location control plane hosts](/docs/satellite?topic=satellite-host-update-location) one at a time and try running `oc debug node`. If you are still receiving the error, then [reboot one worker node at a time](/docs/satellite?topic=satellite-host-update-workers).

If you can determine that a particular worker node is causing the problem and the `oc debug node` command is successful on other worker nodes, then replace the host where the `oc debug node` command fails.
