---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-17"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Storage error messages
{: #debug-storage}

You can debug storage by reviewing the provided health information.
{: shortdesc}

## Reviewing error messages and logs
{: #review-messages-logs-storage}

### ST0001
{: #st0001}

The cluster ID is not specified in the body of your request. To list cluster IDs, run `ibmcloud ks cluster ls`.
{: shortdesc}

Response code: `400`


### ST0002
{: #st0002}

The input parameters in the request body are either incomplete or in the wrong format. Be sure to include all required parameters in your request in JSON format.
{: shortdesc}


Response code: `404`

### ST0003
{: #st0003}

Internal server error occurred.
{: shortdesc}

Error type: General

Response code: `500`

### ST0004
{: #st0004}

The specified volume ID could not be found.
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0005
{: #st0005}

The specified worker node could not be found.
{: shortdesc}

Error type: Provisioning

How to fix it: To list all worker nodes in the cluster run `ibmcloud ks worker ls`.

Response code: `404`

### ST0006
{: #st0006}

The service is currently unavailable. Wait a few minutes and try again. MS Backend Error is : `error-message`
{: shortdesc}

Error type: Services

Response code: `500`

### ST0007
{: #st0007}

The specified volume attachment ID could not be found. To retrieve the volume attachment ID, run `ibmcloud is volume VOLUME_ID` and note the `Vdisk ID` in the Volume Attachment Instance Reference.
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0008
{: #st0008}

User is not authorized to create service subscription on `<cluster-id>` cluster ID
{: shortdesc}

Error type: Authorization

Response code: `401`

Verify that storage configuration and service cluster both should be on same {{site.data.keyword.satelliteshort}} location, if not create a configuration and then re-try.



### ST0009
{: #st0009}

`<cluster-id>` cluster is not a service {{site.data.keyword.satelliteshort}} cluster
{: shortdesc}

Error type: Bad request

Response code: `400`

How to fix it: Verify your service clusters by running `ibmcloud sat services --location <location-ID>` command.



### ST0010
{: #st0010}

User don't have access to perform this operation
{: shortdesc}

Error type: Authorization,

How to fix it:  login as location or service admin and then perform this operation,

Response code: `400`

### ST0013
{: #st0013}

The cluster ID is not specified in the query. To list cluster IDs, run `ibmcloud ks cluster ls`. 
{: shortdesc}

Error type: General,

Response code: `400`

### ST0014
{: #st0014}

The required input parameter `<parameter>` in the request body is  either missing or unsupported. The specified value is `<parameter>` . The expected value(s) is(are) `<value>`
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0015
{: #st0015}

The required input parameter `<<parameter-name>` in the request body is missing.
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0016
{: #st0016}

Failed to update the tags for the volume with CRN `<VolumeCRN>`. BackendError: {{.BackendError}}
{: shortdesc}

Error type: Services

Response code: `400`

### ST0017
{: #st0017}

Failed to retrieve resource details.
{: shortdesc}

Error type: Services

Response code: `400`

### ST0018
{: #st0018}

Result limit reached.
{: shortdesc}

Error type: Services

Response code: `404`


### ST0019
{: #st0019}

`<object-type>` not found with the identifier: `<identifier>>`
{: shortdesc}

Error type: Services

Response code: `404`

### ST0020
{: #st0020}

The request parameter with name `<<parameter-name>` is not supported
{: shortdesc}

Error type: Services

Response code: `404`

### ST0021
{: #st0021}

`<object-type>` with the identifier: `{{.ObjetIdentifier>` already exists
{: shortdesc}

Error type: Services

Response code: `404`

### ST0022
{: #st0022}

Failed to send credential audit event.
{: shortdesc}

Error type: Services

Response code: `500`

### ST0023
{: #st0023}

Unable to update configuration with name `<ConfigurationName>`. `<number>` assignments depend on this configuration.  remove them before updating this configuration or create a new configuration
{: shortdesc}

Error type: Services

Response code: `500`

### ST0024
{: #st0024}

The given parameter value `<value>` does not match with regular expression `{{.RegEx>`
{: shortdesc}

Error type: Services

Response code: `400`

### ST0025
{: #st0025}

The length of given parameter value `<value>` is less than minimum allowed length `<minimum-length>`
{: shortdesc}

Error type: Services

Response code: `400`

### ST0026
{: #st0026}

The length of given parameter value `<value>` is greater than maximum allowed length `<maximum-length`
{: shortdesc}

Error type: Services

Response code: `400`

### ST0027
{: #st0027}

Unable to update `<object-type>` with the identifier: `<identifier>`. No `<object-type>` parameter found in the request body to update.
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0028
{: #st0028}

Unable to add or update storage class as storage class template is not registered for `<template>` and `<version>`
{: shortdesc}

Error type: Services

Response code: `404`

### ST0030
{: #st0030}

Cluster not found corresponding to the given cluster ID: `<cluster-id>`
{: shortdesc}

Error type: Bad request

How to fix it: To list all the {{site.data.keyword.satelliteshort}} clusters you have access to, run `ibmcloud oc cluster ls --provider=satellite`

Response code: `404`

### ST0031
{: #st0031}

The required input query parameter `<<parameter-name>` is missing.
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0032
{: #st0032}

The `<parameter>` and `<parameter>` parameters are exclusive. Provide only one of these parameters.
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0033
{: #st0033}

Updating an assignment, created for a cluster, to cluster group(s) is not supported. Run `ibmcloud sat storage assignment rm` to remove the assignment and create new assignment with cluster group(s).
{: shortdesc}

Error type: Bad request

Response code: `400`

### ST0034
{: #st0034}

Failed to remove old temporary storage configuration with name: `<ObjectName>`.
{: shortdesc}

Error type: Services

How to fix it: To remove the storage configuration manually, run `ibmcloud sat storage config rm --config=<config-name>`

Response code: `500`

### ST0035
{: #st0035}

{{.ErrorMessage}}. {{.IncidentID}}
{: shortdesc}

Error type: Services

Response code: `500`

### ST0036
{: #st0036}

Parameter is an invalid UUID
{: shortdesc}

Error type: Bad request

Response code: `400`

### ST0037
{: #st0037}

Failed to fetch `<object>` `<identifier>`. 
{: shortdesc}

Error type: Services

Response code: `500`

### ST0038
{: #st0038}

{{site.data.keyword.satelliteshort}} Location `<location>` not found. To list all the {{site.data.keyword.satelliteshort}} locations you have access to, run `ibmcloud sat locations`.
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0039
{: #st0039}

{{site.data.keyword.cloud_notm}} {{site.data.keyword.satelliteshort}} is not available in this region. Choose a supported {{site.data.keyword.cloud_notm}} region with `ibmcloud target -r <region>` to manage your {{site.data.keyword.satelliteshort}} location from, and try again. For supported multizone regions, see `http://ibm.biz/satloc-mzr`
{: shortdesc}

Error type: {{site.data.keyword.satelliteshort}}

Response code: `404`

### ST0040
{: #st0040}

Format error in template file. {{.TemplateError}}
{: shortdesc}

Error type: Bad request

Response code: `400`

### ST0041
{: #st0041}

Maximum allowed storage requests limit reached. Existing count:`<ExistingCount}}` allowed count:`{{.AllowedCount>`
{: shortdesc}

Error type: Bad request

Response code: `400`

### ST0042
{: #st0042}

A storage request for volume-type:`<volumeType>` already exists with ID `<requestID>`
{: shortdesc}

Error type: Bad request

Response code: `400`

### ST0043
{: #st0043}

Backend service access denied.
{: shortdesc}

Error type: Services
Response code: 403

### ST0044
{: #st0044}

Provided total-capacity should be greater than current total-capacity
{: shortdesc}

Error type: Bad request

Response code: `400`

### ST0045
{: #st0045}

Total-capacity should be a positive integer and the unit of total-capacity should be in the form of E, T, G, M, K, B
{: shortdesc}

Error type: Bad request


Response code: `400`

### ST0046
{: #st0046}

This storage configuration is already up to date with the latest revision. No newer revision available for '{{.TemplateName}}' at version '{{.TemplateVersion}}'. Current revision: '{{.CurrentRevision}}' and latest revision.

Error type: Bad request

Response code: 406 

### ST0047
{: #st0047}

The {{.Param1}} value '{{.Param2}}' should only contain alphabets, numbers, underscore and hyphen.

Error type: Bad request

Response code: `400`

### ST0048
{: #st0048}

Failed to update desired storage configuration. Assignment creation failed.

Error type: Services

Response code: `500`


### ST0049
{: #st0049}

Configuration is created in '{{.ConfigLocation}}' location but the cluster is in '{{.ClusterLocation}}' location. Please recreate the configuration in '{{.ClusterLocation}}' location and retry.

Error type: Bad request

Response code: `400`

### ST0050
{: #st0050}

The controller ID is not specified in the request.

Error type: Bad request

Response code: `400`

### ST0051
{: #st0051}

Failed to retrieve '{{.ObjectType}}'. BackendError: {{.BackendError}}

Error type: Services

Response code: `500`

### ST0052
{: #st0052}

Unable to create or update storage configuration. Multiple storage classes defined with the name '{{.StorageClassName}}'.

Error type: Bad request

Response code: `400`




