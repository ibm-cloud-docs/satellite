---


copyright:
  years: 2024, 2024
lastupdated: "2024-04-11"

keywords: satellite, api, iam, tokens, refresh

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Setting up the API
{: #api_setup}

{{site.data.keyword.satellitelong_notm}} shares the same application programming interface (API) as {{site.data.keyword.containerlong_notm}} and {{site.data.keyword.openshiftlong_notm}}, so that you can use the same methods to consistently create and manage your {{site.data.keyword.satelliteshort}} resources.


## About the API
{: #api_about}

The {{site.data.keyword.satelliteshort}} API automates the provisioning and management of {{site.data.keyword.cloud_notm}} infrastructure resources for your clusters so that your apps have the compute, networking, and storage resources that they need to serve your users.
{: shortdesc}

The API supports the different infrastructure providers that are available for you to create clusters and resources. The `v2` API is designed to avoid breaking existing functionality when possible. However, make sure that you review the following differences between the `v1` and `v2` API.    

API endpoint prefix
:    v1 API: `https://containers.cloud.ibm.com/global/v1`
:    v2 API: `https://containers.cloud.ibm.com/global/v2`

API reference docs
:    v1 API: [`https://containers.cloud.ibm.com/global/swagger-global-api/`](https://containers.cloud.ibm.com/global/swagger-global-api/#/){: external}
:    v2 API: [`https://containers.cloud.ibm.com/global/swagger-global-api/`](https://containers.cloud.ibm.com/global/swagger-global-api/#/){: external}

API architectural style
:    v1 API: Representational state transfer (REST) that focuses on resources that you interact with through HTTP methods such as `GET`, `POST`, `PUT`, `PATCH`, and `DELETE`.
:    v2 API: Remote procedure calls (RPC) that focus on actions through only `GET` and `POST` HTTP methods.

`GET` responses
:    v1 API: The `GET` method for a collection of resources (such as `GET v1/clusters`) returns the same details for each resource in the list as a `GET` method for an individual resource (such as `GET v1/clusters/{idOrName}`).
:    v2 API: To return responses faster, the v2 `GET` method for a collection of resources (such as `GET v2/clusters`) returns only a subset of information that is detailed in a `GET` method for an individual resource (such as `GET v2/clusters/{idOrName}`).
     Some list responses include a providers property to identify whether the returned item applies to classic or VPC infrastructure. For example, the `GET zones` list returns some results such as `mon01` that are available only in the classic infrastructure provider, while other results such as `us-south-01` are available only in the VPC infrastructure provider.

Cluster, worker node, and worker-pool responses
:    v1 API: Responses include only information that is specific to the classic infrastructure provider, such as the VLANs in `GET` cluster and worker responses.
:    v2 API: The information that is returned varies depending on the infrastructure provider. For such provider-specific responses, you can specify the provider in your request. For example, VPC clusters don't return VLAN information since they don't have VLANs. Instead, they return subnet and CIDR network information.


{{../account/iam-apikeys.md#work-with-apikeys}} 



{{../account/iam-apikey_iamtoken.md#iamtoken}}



{{../account/iam-apikeys_services.md#token_auth}}



{{../account/iam-apikeys_services.md#apikey_auth}}

