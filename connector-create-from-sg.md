---

copyright:
  years: 2024, 2024
lastupdated: "2024-05-07"

keywords: satellite, connector, migration, endpoints, destinations

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Migrating resources from {{site.data.keyword.SecureGateway}} to {{site.data.keyword.satelliteshort}} Connector
{: #connector-create-from-sg}

You can use the {{site.data.keyword.SecureGateway}} API to migrate your Gateways and Destinations to {{site.data.keyword.satelliteshort}} Connectors and Connector endpoints
{: shortdesc}

## Prerequisites
{: #sg-migrate-connector-prereqs}

[Get the details of your Gateways and Destinations](/docs/SecureGateway?topic=SecureGateway-dep-gather-sg-details).

## Migrating a Gateway to a Connector
{: #migrate-gateway-to-endpoint}

1. Migrate your Gateway to a Connector by using the `migrate2connector` API. For more information, see the [Migrate a Gateway to Satellite Connector](/apidocs/secure-gateway-v2#migratetoconnector) API reference.
    ```sh
    curl -X PUT -H 'Authorization: Bearer <IAM token>' -H 'Content-Type: application/json' -d '{ "connector_id": "A2FbRFtwNfatanQRLjrujBKmVmfOk7NjXYZIWAoVLNfd1PTXJ93aH3J", "token" : "iam_token" }' 'https://sgmanager.us-south.securegateway.cloud.ibm.com/v1/sgconfig/{gateway_id}/migrate2connector'
    ```
    {: pre}

1. Verify your Gateway was migrated by reviewing your Connector in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}.


## Migrating {{site.data.keyword.SecureGateway}} Destinations to Connector endpoints
{: #migrate-destination-to-endpoint}

You can migrate all the destinations under a Gateway to multiple Connector endpoints or an individual destination to a Connector endpoint.

If your Secure Gateway Destination name doesn't comply to the Connector endpoint naming policy, you can force rename the destination during migration, by using the `force=true` option. You can check the endpoint name used by running the API without the `force=true` option.

1. Migrate your Gateways to Connectors or [Create a Connector](/docs/satellite?topic=satellite-create-connector&interface=ui). Make a note of your Connector ID and IAM token. For more information, see the [Migrate Destinations to Satellite Connector endpoints](/apidocs/secure-gateway-v2#migratedestinationtoconnector) API reference.

1. Migrate your Destinations to endpoints by using the `migratedestinationtoconnector` API. Review the following examples to determine your use case.

    * Migrate all Destinations to Connector endpoints.
        ```sh
        curl --request PUT \
        --header 'Content-Type: application/json' \
        --data '{"token":"<connector_iam_token>","connector_id":"<connector_id>"}' \
        --header 'Authorization: Bearer <sg_gateway_token>' \
        https://<sg_server>/v1/sgconfig/<sg_gateway_id>/migrate2connector
        ```
        {: pre}


    * Migrate a single Destination to a Connector endpoint.
        ```sh
        curl --request PUT \
        --header 'Content-Type: application/json' \
        --data '{"token":"<connector_iam_token>","connector_id":"<connector_id>"}' \
        --header 'Authorization: Bearer <sg_gateway_token>' \
        https://<sg_server>/v1/sgconfig/<sg_gateway_id>/destinations/<sg_destination_id>/migrate2connector
        ```
        {: pre}

1. Verify that your Gateways and Destinations were migrated. View your Connectors and endpoints in the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}.

## Example migration requests
{: #migrate-examples}

Example request to migrate all destinations to Connector endpoints and include the `force=true` option.
```sh
$ curl --silent --request PUT --header "Content-Type: application/json" --data "{\"region\":\"stage-south\",\"token\":\"${CONNECTOR_TOKEN}\",\"connector_id\":\"${CONNECTOR_ID}\"}" --header "Authorization: Bearer ${SG_GATEWAY_TOKEN}" "https://sgmanager.us-south.securegateway.test.cloud.ibm.com/v1/sgconfig/${SG_GATEWAY_ID}/migrate2connector?force=true" | jq
{
  "migrated": [
    {
      "id": "5B9oNcWGY67_deY76",
      "description": "endpoint2"
    },
    {
      "id": "5B9oNcWGY67_nfTP5",
      "description": "endpoint 1",
      "new_endpoint_name": "endpoint-1"
    }
  ],
  "failed": [
    {
      "id": "5B9oNcWGY67_TBGie",
      "description": " cloud endpoint",
      "reason": "Connector does not support cloud type endpoint."
    }
  ]
}
```
{: screen}

Example request to migrate a Destination that doesn't comply to the Connector endpoint naming convention. Note that this results in a naming error. You can retry the request and use the `force=true` option to force renaming.
```sh
curl --silent --request PUT --header "Content-Type: application/json" --data "{\"region\":\"stage-south\",\"token\":\"${CONNECTOR_TOKEN}\",\"connector_id\":\"${CONNECTOR_ID}\"}" --header "Authorization: Bearer ${SG_GATEWAY_TOKEN}" "https://sgmanager.us-south.securegateway.test.cloud.ibm.com/v1/sgconfig/${SG_GATEWAY_ID}/destinations/5B9oNcWGY67_nfTP5/migrate2connector" | jq
{
  "migrated": [],
  "failed": [
    {
      "id": "5B9oNcWGY67_nfTP5",
      "description": "endpoint 1",
      "reason": "Endpoint name does not satisfy Connector Endpoint naming policy. Endpoint names must start with a letter and end with an alphanumeric character, can contain letters, numbers, and hyphen (-), and must be 63 characters or fewer.",
      "remediation": "Run PUT /v1/sgconfig/{gateway_id}/destinations/{destination_id}/migrate2connector?force=true API to rename the endpoint to suggested name 'endpoint-1' during migration"
    }
  ]
}
```
{: screen}

