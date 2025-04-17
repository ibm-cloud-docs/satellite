---


copyright:
  years: 2024, 2025
lastupdated: "2025-04-17"

keywords: satellite, endpoints, authentication

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Authenticating Connector endpoint traffic
{: #connector_endpoint_auth}

All endpoint traffic is encrypted by default. However, you can choose to provide your own certificates to implement source and destination authentication. For TLS or HTTPS endpoints, there are two TLS connections in the endpoint request flow that you might need to configure. The first TLS handshake is between the source and the Connector service. The second TLS handshake is between the Connector service and your destination or target server. You can provide certificates for one or both of these connections.
{: shortdesc}

If you don't configure any endpoint authentication settings, endpoint traffic is still encrypted, but Connector acts as a transit service only. This applies even to endpoints using the TCP or HTTP protocol. For endpoints with protocol TLS or HTTPS, traffic through the Connector will attempt to use simple authentication by default, relying on its well known built-in certificates, which may or may not be sufficient.

Review the following authentication options.

[Simple authentication between the Connector service and the destination](#simple-auth-destination-conn)
:   The destination server authenticates itself with the Connector service. To set up simple authentication with the destination, your endpoint must have a destination protocol of TLS. If the destination's server certificate is not signed by a well-known certificate authority or is not self-signed, you will need to configure a trusted CA certificate or chain to validate the destination's server certificate.

[Simple authentication with the Connector service](#simple-auth-source-conn)
:   The Connector server authenticates itself to the source. To set up simple authentication with the Connector service, you must set the endpoint's source protocol to TLS or HTTPS. Certificate configuration then depends on the endpoint's destination type.
:   For endpoints with destination type 'cloud', you must provide a server certificate and key that the Connector service will use to authenticate with the source. Note that if your server certificate is not signed by a well-known signing authority, you must also install a CA root certificate in your source environment.
:   For endpoints with destination type 'location', you do not need to configure any Connector server certificates because Connector uses its own certificate signed by [DigiCert](https://www.digicert.com/){: external}. DigiCert certificates are [compatible](https://knowledge.digicert.com/general-information/compatibility-of-digicert-trusted-root-certificates){: external} with all modern browsers and platforms but if your source has no root CA certificate, you will need to [download](https://www.digicert.com/kb/digicert-root-certificates.htm){: external} one from DigiCert and install it in your source environment.

[Mutual authentication between the Connector service and the destination](#mutual-auth-destination-conn)
:    With mutual authentication, the destination server authenticates itself to the Connector service and the Connector service must authenticate itself to the destination. To set up mutual authentication between the Connector service and the destination server, you need to configure the Connector service authentication the same way as in the simple destination authentication case. However, you will also need to provide a client certificate and key that the Connector service uses to authenticate with the destination.

[Mutual authentication between the source and Connector service](#mutual-auth-source-conn)
:   With mutual authentication, the Connector service authenticates itself to the source and the source must authenticate itself to the Connector service. To set up mutual authentication between the source and Connector service, you must configure the Connector server authentication the same way as in the simple source authentication case. However, you also need to configure a CA certificate that the Connector service uses to validate the source's client certificate.

[Mutual authentication between the source and the Connector service as well as Connector and the destination server](#mutual-auth-both-conn).
:   Requests to both the Connector service and the destination server are authenticated. Two TLS handshakes must be verified before traffic is allowed. For this setup, create an endpoint with a source protocol of TLS or HTTPS and a destination protocol of TLS. Then, provide the certificates needed for both the source and destination mutual authentication cases.

If you choose to provide your own certificates for endpoint authentication, you are responsible for rotating the certificates and managing expiration dates. If your certificate expires, you might experience disrupted traffic.
{: important}


## Setting up authentication in the CLI
{: #mutual-auth-cli-conn}

The `source` options refer to the TLS handshake between the source and the Connector service. The `dest` options refer to the TLS handshake between the Connector service and your destination or target server. You can provide certificates for one or both of these connections. Unspecified settings are set to their default values.

The `ibmcloud sat endpoint authn` command is only used to set certificates for the Connector service, acting as a server on the source side and as a client on the destination side. You may also need to configure certificates for the source (client) and destination (server) in their corresponding application environments.
{: note}

Review the following example scenarios.


### Simple authentication between the Connector service and the destination
{: #simple-auth-destination-conn}

1. Create an HTTPS endpoint to an HTTPS server.

    ```sh
    ibmcloud sat endpoint create 
      --connector-id ID \
      --name myEndpoint \
      --dest-hostname example.com \
      --dest-port 443 \
      --source-protocol HTTPS \
      --dest-type location
    ```
    {: pre}

1. Configure simple TLS with the destination server.

    This example provides the certificate of the trusted CA to validate the destination server's certificate.

    ```sh
    ibmcloud sat endpoint authn set \
      --connector-id ID \
      --endpoint myEndpoint \
      --dest-tls-mode simple \
      --dest-ca-cert-file /path/to/serverCACerts.pem
    ```
    {: pre}

1. Before your certificates expire, rotate them by replacing the existing authentication certificates with new ones. Only the certificates that you specify are replaced.

    ```sh
    ibmcloud sat endpoint authn rotate \
      --connector-id ID \
      --endpoint myEndpoint \
      --dest-ca-cert-file /path/to/serverCACerts.pem
    ```
    {: pre}

### Simple authentication with the Connector service
{: #simple-auth-source-conn}

Unlike most configurations, which can work with an endpoint `--dest-type` of either `location` or `cloud`, this one must use `--dest-type cloud` because setting source certificates for location destination endpoints is not supported.
{: exception}

1. Create an HTTPS endpoint to an HTTP server.

    ```sh
    ibmcloud sat endpoint create \
      --connector-id ID \
      --name myEndpoint \
      --dest-hostname example.com \
      --dest-port 80 \
      --dest-protocol TCP \
      --source-protocol HTTPS \
      --dest-type cloud
    ```
    {: pre}

1. Configure simple TLS between the source and the Connector service.

    This example provides the server certificate for the Connector service to authenticate with the source.

    ```sh
    ibmcloud sat endpoint authn set \
      --connector-id ID \
      --endpoint myEndpoint \
      --source-tls-mode simple \
      --source-cert-file /path/to/serverCertificate.pem \
      --source-key-file /path/to/serverKey.pem
    ```
    {: pre}

1. Before your certificates expire, rotate them by replacing the existing authentication certificates with new ones. Only the certificates that you specify are replaced.

    ```sh
    ibmcloud sat endpoint authn rotate \
      --connector-id ID \
      --endpoint myEndpoint \
      --source-cert-file /path/to/serverCertificate.pem \
      --source-key-file /path/to/serverKey.pem
    ```
    {: pre}

### Mutual authentication between the Connector service and the destination
{: #mutual-auth-destination-conn}

1. Create an HTTP endpoint to an HTTPS server.

    ```sh
    ibmcloud sat endpoint create \
      --connector-id ID \
      --name myEndpoint \
      --dest-hostname example.com \
      --dest-port 443 \
      --dest-protocol TLS \
      --source-protocol HTTP \
      --dest-type location
    ```
    {: pre}

1. Configure mutual TLS with the destination server.

    Similar to the simple authentication example, `--dest-ca-cert-file` validates the destination server's certificate. To perform the mutual TLS handshake, `--dest-cert-file` and `--dest-key-file` are used.

    ```sh
    ibmcloud sat endpoint authn set \
      --connector-id ID \
      --endpoint myEndpoint \
      --dest-tls-mode mutual \
      --dest-cert-file /path/to/clientCertificate.pem \
      --dest-key-file /path/to/clientKey.pem \
      --dest-ca-cert-file /path/to/serverCACerts.pem
    ```
    {: pre}

1. Before your certificates expire, rotate them by replacing the existing authentication certificates with new ones. Only the certificates that you specify are replaced.

    ```sh
    ibmcloud sat endpoint authn rotate \
      --connector-id ID \
      --endpoint myEndpoint \
      --dest-cert-file /path/to/clientCertificate.pem \
      --dest-key-file /path/to/clientKey.pem \
      --dest-ca-cert-file /path/to/serverCACerts.pem
    ```
    {: pre}

### Mutual authentication between the source and the Connector service
{: #mutual-auth-source-conn}

Unlike most configurations, which can work with an endpoint `--dest-type` of either `location` or `cloud`, this one must use `--dest-type cloud` because setting source certificates for location destination endpoints is not supported.
{: exception}

1. Create an HTTPS endpoint to an HTTP server.

    ```sh
    ibmcloud sat endpoint create \
      --connector-id ID \
      --name myEndpoint \
      --dest-hostname example.com \
      --dest-port 80 \
      --dest-protocol TCP \
      --source-protocol HTTPS \
      --dest-type cloud
    ```
    {: pre}

1. Configure mutual TLS between the source and the Connector service.

    This example provides the server certificate for the Connector service to authenticate with the source and the certificate of the trusted CA to validate the source's client certificate.

    ```sh
    ibmcloud sat endpoint authn set \
      --connector-id ID \
      --endpoint myEndpoint \
      --source-tls-mode mutual \
      --source-cert-file /path/to/serverCertificate.pem \
      --source-key-file /path/to/serverKey.pem \
      --source-ca-cert-file /path/to/clientCACerts.pem
    ```
    {: pre}

1. Before your certificates expire, rotate them by replacing the existing authentication certificates with new ones. Only the certificates that you specify are replaced.

    ```sh
    ibmcloud sat endpoint authn rotate \
      --connector-id ID \
      --endpoint myEndpoint \
      --source-cert-file /path/to/serverCertificate.pem \
      --source-key-file /path/to/serverKey.pem \
      --source-ca-cert-file /path/to/clientCACerts.pem
    ```
    {: pre}

### Mutual authentication at both the source and destination
{: #mutual-auth-both-conn}

1. Create an HTTPS endpoint to an HTTPS server.

    ```sh
    ibmcloud sat endpoint create \
      --connector-id ID \
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
    ibmcloud sat endpoint authn set \
      --connector-id ID \
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
    ibmcloud sat endpoint authn rotate \
      --connector-id ID \
      --endpoint myEndpoint \
      --source-ca-cert-file /path/to/clientCACerts.pem \
      --dest-cert-file /path/to/clientCertificate.pem \
      --dest-key-file /path/to/clientKey.pem \
      --dest-ca-cert-file /path/to/serverCACerts.pem
    ```
    {: pre}


## Displaying authentication settings
{: #get-certs-conn}

The `ibmcloud sat endpoint authn get` command can be used to display the configured authentication settings for your endpoint. For example, the following command can be used to output the currently configured TLS modes and certificates for endpoint `myEndpoint`.

```sh
ibmcloud sat endpoint authn get --connector-id ID --endpoint myEndpoint
```
{: pre}

Assuming `myEndpoint` is configured as shown in the [source and destination mutual authentication example](#mutual-auth-both-conn), this command will produce output similar to the following.

```sh
OK

Destination TLS Mode:          mutual
Destination Certificate:       /path/to/clientCertificate.pem
Destination Key:               /path/to/clientKey.pem
Destination CA Certificates:   /path/to/serverCACerts.pem
Source TLS Mode:               mutual
Source Certificate:            -
Source Key:                    -
Source CA Certificates:        /path/to/clientCACerts.pem
```
{: screen}


## Removing certificates
{: #remove-certs-conn}

The `ibmcloud sat endpoint authn set` command can be used to remove one or more existing certificates. The set command removes all certificates other than the ones specified in the command's options. For example, the following command removes all currently configured certificates for endpoint `myEndpoint`.

```sh
ibmcloud sat endpoint authn set --connector-id ID --endpoint myEndpoint
```
{: pre}

If you only want to remove some certificates, leaving others unchanged, make sure to pass options with the current values of the other certificates. For example, the following command can be used to remove `source` certificates that are currently set, leaving the `dest` certificates unchanged.

```sh
ibmcloud sat endpoint authn set \
  --connector-id ID \
  --endpoint myEndpoint \
  --dest-tls-mode mutual \
  --dest-cert-file /path/to/current/clientCertificate.pem \
  --dest-key-file /path/to/current/clientKey.pem \
  --dest-ca-cert-file /path/to/current/serverCACerts.pem
```
{: pre}
