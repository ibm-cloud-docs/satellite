<staging>---

copyright:
  years: 2020, 2021
lastupdated: "2021-12-02"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do `kubectl` or `oc` commands fail with certificate errors?
{: #kubectl-cert-errors}


When you run `kubectl` or `oc` commands against your {{site.data.keyword.satelliteshort}} cluster, you see the following error message.
{: tsSymptoms}

```sh
Unable to connect to the server: x509: certificate signed by unknown authority
```
{: screen}

During the test and development of {{site.data.keyword.satelliteshort}} in staging, unvalid Let's Encrypt certificates are used for the domain names that are assigned to your cluster.
{: tsCauses}

To use `kubectl` or `oc` commands, add the `--insecure-skip-tls-verify` flag to all of your commands.
{: tsResolve}

For example, run the following command.
```sh
kubectl get pods --insecure-skip-tls-verify
```
{: pre}

</staging>


