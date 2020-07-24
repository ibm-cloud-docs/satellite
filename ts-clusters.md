---

copyright:
  years: 2020, 2020
lastupdated: "2020-07-24"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}


# Clusters
{: #ts-clusters}

## Debugging clusters
{: #ts-clusters-debug}

See the [{{site.data.keyword.openshiftlong_notm}} troubleshooting documentation](/docs/openshift?topic=openshift-cs_troubleshoot).

## Running `kubectl` or `oc` commands fail with certificate errors
{: #kubectl-cert-errors}

{: tsSymptoms}
When you run `kubectl` or `oc` commands against your , you see the following error message:
```
Unable to connect to the server: x509: certificate signed by unknown authority
```
{: screen}

{: tsCauses}
During the test and development of {{site.data.keyword.satelliteshort}} in staging, unvalid Let's Encrypt certificates are used for the domain names that are assigned to your cluster.

{: tsResolve}
To use `kubectl` or `oc` commands, add the `--insecure-skip-tls-verify` flag to all of your commands.

Example:
```
kubectl get pods --insecure-skip-tls-verify
```
{: pre}
