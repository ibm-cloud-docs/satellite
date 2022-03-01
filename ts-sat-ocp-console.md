---

copyright:
  years: 2021, 2022
lastupdated: "2022-03-01"

keywords: satellite, certificate, expired, console

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I access the {{site.data.keyword.redhat_openshift_notm}} web console?
{: #ts-sat-ocp-console}

When you try to access your {{site.data.keyword.satelliteshort}} cluster through the {{site.data.keyword.redhat_openshift_notm}} console, you see an error message similar to the following:
{: tsSymptoms}

```sh
2021-07-23T14:15:33Z auth: error contacting auth provider (retrying in 10s): request to OAuth issuer endpoint https://...us-east.satellite.appdomain.cloud:0000/oauth/token failed: Head https://...us-east.satellite.appdomain.cloud:0000: x509: certificate has expired or is not yet valid.
```
{: screen}

An expired or invalidated security ticket is preventing you from accessing the {{site.data.keyword.redhat_openshift_notm}} console.  
{: tsCauses}

Follow the steps to validate your certificate.
{: tsResolve}

1. [Access your {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat).

2. Run the [`master refresh`](/docs/openshift?topic=openshift-kubernetes-service-cli#cs_apiserver_refresh) command.

    ```sh
    ibmcloud oc cluster master refresh --cluster <cluster_id>
    ```
    {: pre}

3. Wait a few minutes and try to access the console again.

