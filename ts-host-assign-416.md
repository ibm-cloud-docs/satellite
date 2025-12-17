---


copyright:
  years: 2025, 2025
lastupdated: "2025-12-17"

keywords: satellite, rhel, rhel 9, openshift 4.16 troubleshoot, host assign fail, 4.16 cluster

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I assign a RHEL 9 host to a 4.16 cluster?
{: #host-assign-416}

When you try to assign RHEL 9 host to a version 4.16 cluster, you see a message similar to the following error in the host logs.
{: tsSymptoms}

```sh
Nov 26 14:10:11 sat-e2e-d4jfo5u20j-4 ibm-host-agent.sh[13722]: Error: RHEL 9 is NOT supported on 4.16 Satellite clusters
```
{: screen}


RHEL 9 is available only for cluster versions 4.17 and later. You can assign RHEL 9 hosts to your control plane, but if you want to use a RHEL 9 host as cluster worker node. Your cluster must be version 4.17 and later.
{: tsCauses}


Complete the following steps to resovle the issue.
{: tsResolve}

1. Update your cluster to version 4.17.

1. Retry assigning the host to your cluster.

1. If the issue persists, open a [support case](/docs/account?topic=account-using-avatar). In the case details, be sure to include any relevant log files, error messages, or command outputs.

