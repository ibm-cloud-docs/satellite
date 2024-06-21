---


copyright:
  years: 2024, 2024
lastupdated: "2024-06-21"

keywords: satellite, endpoints, authentication

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Authenticating Connector endpoint traffic
{: #connector_endpoint_auth}

All endpoint traffic is encrypted by default. However, you can choose to provide your own certificates to implement source and destination authentication. There are two TLS connections in the endpoint request flow. The first TLS handshake is between the source and the Connector service. The second TLS handshake is between the Connector service and your destination or target server. You can provide certificates for one or both of these connections.
{: shortdesc}

This is an experimental feature that is available for evaluation and testing purposes and might change without notice.
{: experimental}

Review the following authentication options.

Default authentication settings
:   If you don't provide your own certificates for mutual authentication, endpoint traffic is still encrypted, but Connector acts as a transit service only.

Simple authentication with the Connector service.
:   To set up simple authentication with the Connector service, you must set the endpoint's source protocol to TLS or HTTPS. You can optionally provide a server certificate and key that the Connector service can use to authenticate with the source.

Simple authentication with the destination server.
:   To set up simple authentication on the destination, create an endpoint with a destination protocol of TLS. You can then optionally provide a trusted CA certificate or chain to validate the destination server's certificate.

Mutual authentication between the source and Connector service.
:   Requests from the source to the Connector service are authenticated using the certificates you provide. A TLS handshake verifies the request before traffic is allowed. To set up mutual authentication between the source and Connector service, create an endpoint with a source protocol of TLS or HTTPS, then optionally provide a server certificate, a server private key, and a trusted CA certificate.

Mutual authentication between Connector and the destination server.
:   Requests to the destination are authenticated using the certificates you provide. A TLS handshake verifies the request before traffic is allowed. To set up mutual authentication between Connector and the destination server, create an endpoint with a destination protocol of TLS, then optionally provide a client certificate, a client private key, and trusted CA certificate.

Mutual authentication between the source and the Connector service as well as Connector and the destination server.
:   Requests to both the Connector service and the destination server are authenticated. Two TLS handshakes must be verified before traffic is allowed. For this setup, create an endpoint with a source protocol of TLS or HTTPS. Then, to set up authentication with the Connector service, provide a source certificate, a source key to encrypt the source certificate, and trusted CA certificate. For authentication with the destination server, provide a client certificate, a client private key to encrypt the client certificate, and trusted CA certificate.

If you choose to provide your own certificates for endpoint authentication, you are responsible for rotating the certificates and managing expiration dates. If your certificate expires, you might experience disrupted traffic.
{: note}


## Setting up authentication in the CLI
{: #mutual-auth-cli-loc}

The `source` options refer to the TLS handshake between the source and the Connector service. The `dest` options refer to the TLS handshake between the Connector service and your destination or target server. You can provide certificates for one or both of these connections. Unspecified settings are set to their default values.


Review the following example scenarios.


### Simple authentication between the Connector service and the destination
{: #simple-auth-source-loc}

1. Create an HTTPS endpoint to an HTTPS server with destination certificate verification. 

    ```sh
    ibmcloud sat endpoint create 
      --connector-id myConnectorID \
      --name myEndpoint \
      --dest-hostname example.com \
      --dest-port 443 \
      --source-protocol HTTPS \
      --dest-type location
    ```
    {: pre}

1. Configure simple TLS with the destination server. This example provides the certificate of the trusted CA to validate the destination server's certificate.

    ```sh
    ibmcloud sat experimental endpoint authn set \
      --connector-id myConnectorID \
      --endpoint myEndpoint \
      --dest-tls-mode simple
      --dest-ca-cert-file /path/to/serverCACerts.pem
    ```
    {: pre}

1. Before your certificates expire, rotate them by replacing the existing authentication certificates with new ones. Only the certificates that you specify are replaced.

    ```sh
    ibmcloud sat experimental endpoint authn rotate \
      --dest-ca-cert-file /path/to/serverCACerts.pem
    ```
    {: pre}

### Mutual authentication between the Connector service and the destination
{: #mutual-auth-destination-loc}

1. Create an HTTP endpoint to an HTTPS server.

    ```sh
    ibmcloud sat endpoint create \
      --connector-id myConnectorID \
      --name myEndpoint \
      --dest-hostname example.com \
      --dest-port 443 \
      --dest-protocol TLS\
      --source-protocol HTTP \
      --dest-type location
    ```
    {: pre}

1. Configure mutual TLS with the destination server.

    Similar to the previous use case, the `--dest-ca-cert-file` validates the destination server's certificate. The `dest-cert-file` and `dest-key-file` are used to perform the TLS handshake.

    ```sh
    ibmcloud sat experimental endpoint authn set \
      --connector-id myConnectorID \
      --endpoint myEndpoint \
      --dest-tls-mode mutual \
      --dest-cert-file /path/to/clientCertificate.pem \
      --dest-key-file /path/to/clientKey.pem \
      --dest-ca-cert-file /path/to/serverCACerts.pem
    ```
    {: pre}

1. Before your certificates expire, rotate them by replacing the existing authentication certificates with new ones. Only the certificates that you specify are replaced.

    ```sh
    ibmcloud sat experimental endpoint authn rotate \
      --dest-cert-file /path/to/clientCertificate.pem \
      --dest-key-file /path/to/clientKey.pem \
      --dest-ca-cert-file /path/to/serverCACerts.pem
    ```
    {: pre}



### Mutual authentication between the source and the Connector service
{: #mutual-auth-source-loc}

1. Create an HTTPS endpoint to an HTTP server with source mutual authentication.

    ```sh
    ibmcloud sat endpoint create \
      --connector-id myConnectorID \
      --name myEndpoint \
      --dest-hostname example.com \
      --dest-port 80 \
      --dest-protocol TCP \
      --source-protocol HTTPS \
      --dest-type location
    ```
    {: pre}

1. Configure mutual TLS between the source and the Connector service.

    This example provides the certificate of the trusted CA to validate the source's client certificate.
    ```sh
    ibmcloud sat experimental endpoint authn set \
      --connector-id myConnectorID \
      --endpoint myEndpoint \
      --source-tls-mode mutual \
      --source-cert-file /path/to/serverCertificate.pem \
      --source-key-file /path/to/serverKey.pem \
      --source-ca-cert-file /path/to/clientCACerts.pem
    ```
    {: pre}

1. Before your certificates expire, rotate them by replacing the existing authentication certificates with new ones. Only the certificates that you specify are replaced.

    ```sh
    ibmcloud sat experimental endpoint authn rotate \
      --source-ca-cert-file /path/to/clientCACerts.pem
    ```
    {: pre}




### Mutual authentication at both the source and destination
{: #mutual-auth-both-loc}



1. Create an HTTPS endpoint to an HTTPS server.

    ```sh
    ibmcloud sat endpoint create \
      --connector-id myConnectorID \
      --name myEndpoint \
      --dest-hostname example.com \
      --dest-port 443 \
      --source-protocol HTTPS \
      --dest-type location
    ```
    {: pre}

1. Configure mutual TLS authentication with both the source and the destination server.

    This example enables mutual authentication between the source and the Connector service as well as between the Connector service and the destination server.

    ```sh
    ibmcloud sat experimental endpoint authn set \
      --connector-id myConnectorID \
      --endpoint myEndpoint \
      --source-tls-mode mutual \
      --source-ca-cert-file /path/to/clientCACerts.pem \
      --dest-tls-mode mutual \
      --dest-cert-file /path/to/clientCertificate.pem \
      --dest-key-file /path/to/clientKey.pem \
      --dest-ca-cert-file /path/to/serverCACerts.pem
    ```
    {: pre}

1. Before your certificates expire, rotate them by replacing the existing authentication certificates with new ones. Only the certificates that you specify are replaced.

    ```sh
    ibmcloud sat experimental endpoint authn rotate \
    --source-ca-cert-file /path/to/clientCACerts.pem \
    --dest-cert-file /path/to/clientCertificate.pem \
    --dest-key-file /path/to/clientKey.pem \
    --dest-ca-cert-file /path/to/serverCACerts.pem
    ```
    {: pre}


