---


copyright:
  years: 2020, 2026
lastupdated: "2026-08-27"

keywords: satellite, hybrid, multicloud, storage error messages, error message, cloud storage, cluster storage, debug storage

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting storage error messages in {{site.data.keyword.satellitelong_notm}}
{: #debug-storage}

Debug storage issues in {{site.data.keyword.satellitelong_notm}} by reviewing health information and resolving common storage error messages.
{: shortdesc}

## Reviewing error messages and logs
{: #review-messages-logs-storage}

### ST0001 error message
{: #st0001}

The cluster ID is not specified in the body of your request. To list cluster IDs, run `ibmcloud ks cluster ls`.
{: shortdesc}

Response code: `400`


### ST0002 error message
{: #st0002}

The request body parameters are incomplete or incorrectly formatted. Include all required parameters in JSON format.
{: shortdesc}


Response code: `404`

### ST0003 error message
{: #st0003}

Internal server error occurred.
{: shortdesc}

Error type: General

Response code: `500`

### ST0004 error message
{: #st0004}

The specified volume ID could not be found.
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0005 error message
{: #st0005}

The specified worker node could not be found.
{: shortdesc}

Error type: Provisioning

How to fix it: To list all worker nodes in the cluster run `ibmcloud ks worker ls`.

Response code: `404`

### ST0006 error message
{: #st0006}

The service is currently unavailable. Wait a few minutes and try again. MS Backend Error is: `error-message`
{: shortdesc}

Error type: Services

Response code: `500`

### ST0007 error message
{: #st0007}

Volume attachment ID not found. Run `ibmcloud is volume VOLUME_ID` and note the `Vdisk ID` in the Volume Attachment Instance Reference.
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0008 error message
{: #st0008}

User is not authorized to create service subscription on CLUSTER-ID cluster ID.
{: shortdesc}

Error type: Authorization

Response code: `401`

Verify that storage configuration and service cluster both must be on same {{site.data.keyword.satelliteshort}} location, if not create a configuration and then retry.



### ST0009 error message
{: #st0009}

CLUSTER-ID cluster is not a service {{site.data.keyword.satelliteshort}} cluster.
{: shortdesc}

Error type: Bad request

Response code: `400`

How to fix it: Verify your service clusters by running `ibmcloud sat services --location <location-ID>` command.



### ST0010 error message
{: #st0010}

User doesn't have access to perform this operation.
{: shortdesc}

Error type: Authorization,

How to fix it:  login as location or service admin and then perform this operation,

Response code: `400`

### ST0013 error message
{: #st0013}

The cluster ID is not specified in the query. To list cluster IDs, run `ibmcloud ks cluster ls`. 
{: shortdesc}

Error type: General,

Response code: `400`

### ST0014 error message
{: #st0014}

Required input parameter PARAMETER is missing or unsupported. The specified value is PARAMETER; expected value(s): VALUE.
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0015 error message
{: #st0015}

The required input parameter PARAMETER-NAME in the request body is missing.
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0016 error message
{: #st0016}

Failed to update the tags for the volume with CRN VOLUMECRN. `BackendError: BACKEND-ERROR`
{: shortdesc}

Error type: Services

Response code: `400`

### ST0017 error message
{: #st0017}

Failed to retrieve resource details.
{: shortdesc}

Error type: Services

Response code: `400`

### ST0018 error message
{: #st0018}

Result limit reached.
{: shortdesc}

Error type: Services

Response code: `404`


### ST0019 error message
{: #st0019}

OBJECT-TYPE not found with the identifier: IDENTIFIER.
{: shortdesc}

Error type: Services

Response code: `404`

### ST0020 error message
{: #st0020}

The request parameter with name PARAMETER-NAME is not supported.
{: shortdesc}

Error type: Services

Response code: `404`

### ST0021 error message
{: #st0021}

OBJECT-TYPE with the identifier OBJECT-IDENTIFIER already exists.
{: shortdesc}

Error type: Services

Response code: `404`

### ST0022 error message
{: #st0022}

Failed to send credential audit event.
{: shortdesc}

Error type: Services

Response code: `500`

### ST0023 error message
{: #st0023}

Cannot update configuration CONFIGURATION-NAME. Remove the NUMBER dependent assignments first, or create a new configuration.
{: shortdesc}

Error type: Services

Response code: `500`

### ST0024 error message
{: #st0024}

The given parameter value VALUE does not match with regular expression REG-EX.
{: shortdesc}

Error type: Services

Response code: `400`

### ST0025 error message
{: #st0025}

The length of given parameter value VALUE is less than minimum allowed length MININUM-LENGTH.
{: shortdesc}

Error type: Services

Response code: `400`

### ST0026 error message
{: #st0026}

The length of given parameter value VALUE is greater than maximum allowed length MAXIMUM-LENGTH.
{: shortdesc}

Error type: Services

Response code: `400`

### ST0027 error message
{: #st0027}

Unable to update OBJECT-TYPE with the identifier: IDENTIFIER. No OBJECT-TYPE parameter found in the request body to update.
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0028 error message
{: #st0028}

Unable to add or update storage class as storage class template is not registered for TEMPLATE and VERSION.
{: shortdesc}

Error type: Services

Response code: `404`

### ST0030 error message
{: #st0030}

Cluster not found corresponding to the given cluster ID: CLUSTER-ID
{: shortdesc}

Error type: Bad request

How to fix it: To list all the Satellite clusters that you have access to, run `ibmcloud oc cluster ls --provider=satellite`.

Response code: `404`

### ST0031 error message
{: #st0031}

The required input query parameter PARAMETER-NAME is missing.
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0032 error message
{: #st0032}

The PARAMETER and PARAMETER parameters are exclusive. Provide only one of these parameters.
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0033 error message
{: #st0033}

Updating a cluster assignment to cluster groups is unsupported. Remove it with `ibmcloud sat storage assignment rm` and re-create it with cluster groups.
{: shortdesc}

Error type: Bad request

Response code: `400`

### ST0034 error message
{: #st0034}

Failed to remove old temporary storage configuration with name: OBJECT-NAME.
{: shortdesc}

Error type: Services

How to fix it: To remove the storage configuration manually, run `ibmcloud sat storage config rm --config=<config-name>`.

Response code: `500`

### ST0035 error message
{: #st0035}

`ERROR-MESSAGE. INCIDENT-ID`
{: shortdesc}

Error type: Services

Response code: `500`

### ST0036 error message
{: #st0036}

Parameter is an invalid UUID.
{: shortdesc}

Error type: Bad request

Response code: `400`

### ST0037 error message
{: #st0037}

Failed to fetch OBJECT IDENTIFIER. 
{: shortdesc}

Error type: Services

Response code: `500`

### ST0038 error message
{: #st0038}

{{site.data.keyword.satelliteshort}} Location LOCATION not found. Run `ibmcloud sat locations` to list available locations.
{: shortdesc}

Error type: Bad request

Response code: `404`

### ST0039 error message
{: #st0039}

{{site.data.keyword.satelliteshort}} is not available in this region. Use `ibmcloud target -r REGION` to select a supported region. See `http://ibm.biz/satloc-mzr` for options.
{: shortdesc}

Error type: {{site.data.keyword.satelliteshort}}

Response code: `404`

### ST0040 error message
{: #st0040}

Format error in template file. TEMPLATE-ERROR
{: shortdesc}

Error type: Bad request

Response code: `400`

### ST0041 error message
{: #st0041}

Maximum allowed storage requests limit reached. Existing count:EXISTING-COUNT allowed count:ALLOWED-COUNT.
{: shortdesc}

Error type: Bad request

Response code: `400`

### ST0042 error message
{: #st0042}

A storage request for volume-type:`<volumeType>` already exists with ID REQUEST-ID.
{: shortdesc}

Error type: Bad request

Response code: `400`

### ST0043 error message
{: #st0043}

Backend service access denied.
{: shortdesc}

Error type: Services
Response code: 403

### ST0044 error message
{: #st0044}

Provided total-capacity must be greater than current total-capacity.
{: shortdesc}

Error type: Bad request

Response code: `400`

### ST0045 error message
{: #st0045}

Total-capacity must be a positive integer and the unit of total-capacity must be in the form of `E`, `T`, `G`, `M`, `K`, `B`.
{: shortdesc}

Error type: Bad request


Response code: `400`

### ST0046 error message
{: #st0046}

This storage configuration is already up to date with the latest revision. No newer revision available for TEMPLATE-NAME at version TEMPLATE-VERSION. Current revision: CURRENT-VERSION and latest revision.

Error type: Bad request

Response code: 406 

### ST0047 error message
{: #st0047}

The PARAMETER1 value PARAMETER2 must contain only alphabets, numbers, underscore, and hyphen.

Error type: Bad request

Response code: `400`

### ST0048 error message
{: #st0048}

Failed to update wanted storage configuration. Assignment creation failed.

Error type: Services

Response code: `500`


### ST0049 error message
{: #st0049}

Configuration is created in CONFIG-LOCATIOIN location but the cluster is in CLUSTER-LOCATION location. Re-create the configuration in CLUSTER-LOCATION location and retry.

Error type: Bad request

Response code: `400`

### ST0050 error message
{: #st0050}

The controller ID is not specified in the request.

Error type: Bad request

Response code: `400`

### ST0051 error message
{: #st0051}

Failed to retrieve OBJECT-TYPE. `BackendError: BACKEND-ERROR`

Error type: Services

Response code: `500`

### ST0052 error message
{: #st0052}

Unable to create or update storage configuration. Multiple storage classes defined with the name STORAGE-CLASS-NAME.

Error type: Bad request

Response code: `400`
