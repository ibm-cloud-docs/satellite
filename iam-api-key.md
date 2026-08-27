---


copyright:
  years: 2020, 2026
lastupdated: "2026-08-27"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# API keys in {{site.data.keyword.cloud_notm}} 
{: #iam-api-key}

{{site.data.keyword.satelliteshort}} uses {{site.data.keyword.cloud_notm}} IAM [API keys](/docs/iam?topic=iam-manapikey) to authorize various requests.
{: shortdesc}


## {{site.data.keyword.satelliteshort}} API key
{: #api-key-satellite}

{{site.data.keyword.satelliteshort}} automatically creates an {{site.data.keyword.cloud_notm}} IAM API key for you, that impersonates the permissions of the user that creates the location. The API key name is formatted as `satellite-<LOCATION_NAME>`.

## Container service API key
{: #api-keys-containers}

{{site.data.keyword.satelliteshort}} uses the {{site.data.keyword.openshiftlong_notm}} API key specific to the resource group and region managing the {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

The API key name is in the format `containers-kubernetes-key`. The account owner can reset the API key by logging in to a region and resource group and running `ibmcloud ks api-key reset`.

This API key is used to authorize actions to various {{site.data.keyword.cloud_notm}} services, such as one of the following.
- {{site.data.keyword.openshiftlong_notm}} for clusters.
- {{site.data.keyword.registrylong_notm}} for images.
- Service-to-service authorization in IAM for any {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services that you add to your location.

For more information, see the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-access-creds).

## Infrastructure provider credentials
{: #api-keys-templates}

When creating a {{site.data.keyword.satelliteshort}} location from a template, {{site.data.keyword.satelliteshort}} checks an API key for [permissions to create a location](/docs/satellite?topic=satellite-iam#iam-roles-usecases), including {{site.data.keyword.bplong_notm}} access.
{: shortdesc}
