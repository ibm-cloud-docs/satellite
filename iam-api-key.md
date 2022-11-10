---

copyright:
  years: 2020, 2022
lastupdated: "2022-11-10"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# API keys in {{site.data.keyword.cloud_notm}} 
{: #iam-api-key}

{{site.data.keyword.satelliteshort}} uses [API keys](/docs/account?topic=account-manapikey) from {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) to authorize various requests.
{: shortdesc}


## {{site.data.keyword.satelliteshort}} API key
{: #api-key-satellite}

{{site.data.keyword.satelliteshort}} automatically creates an {{site.data.keyword.cloud_notm}} IAM API key for you, that impersonates the permissions of the user that creates the location. The API key name is formatted as `satellite-<LOCATION_NAME>`.

## Container service API key
{: #api-keys-containers}

{{site.data.keyword.satelliteshort}} uses the API key that is set for the container service, {{site.data.keyword.openshiftlong_notm}}, which is specific to the resource group and region that the {{site.data.keyword.satelliteshort}} location is managed from.
{: shortdesc}

The API key name is in the format `containers-kubernetes-key`. The account owner can reset the API key by logging in to a region and resource group and running `ibmcloud ks api-key reset`.

This API key is used to authorize actions to various {{site.data.keyword.cloud_notm}} services, such as one of the following.
- {{site.data.keyword.openshiftlong_notm}} for clusters.
- {{site.data.keyword.registrylong_notm}} for images.
- Service-to-service authorization in IAM for any {{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} services that you add to your location.

For more information, see the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-access-creds#api_key_about).

## Infrastructure provider credentials
{: #api-keys-templates}

If you create a {{site.data.keyword.satelliteshort}} location from a template, such as an {{site.data.keyword.bplong_notm}} template for AWS, {{site.data.keyword.satelliteshort}} checks for permissions with an API key. The API key must have the [required permissions to create a location](/docs/satellite?topic=satellite-iam#iam-roles-usecases), including to {{site.data.keyword.bplong_notm}}, which is used to automate the infrastructure creation from the template cloud provider.
{: shortdesc}



