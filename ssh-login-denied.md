---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-18"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I SSH into my host machines?
{: #ssh-login-denied}


When you try to SSH into a host machine that is assigned in {{site.data.keyword.satelliteshort}}, you see a message similar to the following.
{: tsSymptoms}

```sh
Permission denied, please try again.
```
{: screen}


After the host is successfully assigned to a {{site.data.keyword.satelliteshort}} location control plane or cluster, {{site.data.keyword.satelliteshort}} disables the ability to SSH into the host for security purposes.
{: tsCauses}

You cannot SSH into the host. If you need to modify settings on host machines in a cluster, try deploying a daemon set, such as in the [Tuning performance](/docs/containers?topic=containers-kernel) example.
{: tsResolve}

If you remove a host from your location or remove the entire location, you must reload the machine in your host infrastructure provider to SSH into the host again.
