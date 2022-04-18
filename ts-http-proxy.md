---

copyright:
  years: 2022, 2022
lastupdated: "2022-04-18"

keywords: satellite, troubleshoot, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Why can't my HTTP proxy connect to the Red Hat servers?
{: #ts-http-proxy}

If the `REDHAT_PACKAGE_MIRROR_LOCATION` value is misconfigured, then you receive an error similar to the following message.
{: tsSymptoms}

```sh
[root@user-c55ppnc10v3kufi8lth0-good-proxy-oc-4 ~]# yum install curl
Loaded plugins: product-id, search-disabled-repos, subscription-manager

https://rhncapdal1001.service.networklayer.com/pulp/repos/customer/Library/content/dist/rhel/server/7/7Server/x86_64/devtools/1/os/repodata/repomd.xml: [Errno 14] curl#60 - "Peer's Certificate issuer is not recognized."
Trying other mirror.
It was impossible to connect to the Red Hat servers.
```
{: screen}

This message indicates that the system is unable to find the package mirrors.
{: tsCauses}

Double-check your configuration. For more information, see [Configuring an HTTP proxy for your Satellite hosts](/docs/satellite?topic=satellite-config-http-proxy).
{: tsResolve}


