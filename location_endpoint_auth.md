---


copyright:
  years: 2024, 2024
lastupdated: "2024-10-10"

keywords: satellite, endpoints, authentication

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Authenticating Location endpoint traffic
{: #location_endpoint_auth}

All endpoint traffic is encrypted by default. However, you can choose to provide your own certificates to implement source and destination authentication. For TLS or HTTPS endpoints, there are two TLS connections in the endpoint request flow that you might need to configure. The first TLS handshake is between the source and the Satellite Link service. The second TLS handshake is between the Satellite Link service and your destination or target server. You can provide certificates for one or both of these connections.
{: shortdesc}

Review the following authentication options. If you don't configure any endpoint authentication settings, endpoint traffic is still encrypted, but Satellite Link acts as a transit service only. This applies even to endpoints using the TCP or HTTP protocol. For endpoints with protocol TLS or HTTPS, traffic through Satellite Link will attempt to use simple authentication by default, relying on its well known built-in certificates, which may or may not be sufficient.

[Simple authentication between the Satellite Link service and the destination](#mutual-auth-cli-loc)
:   The destination server authenticates itself with the Satellite Link service. To set up simple authentication with the destination, your endpoint must have a destination protocol of TLS. If the destination's server certificate is not signed by a well-known certificate authority or is not self-signed, you will need to configure a trusted CA certificate or chain to validate the destination's server certificate.

[Simple authentication with the Satellite Link service](#simple-auth-source-loc)
:   The Satellite Link service authenticates itself to the source. To set up simple authentication with the Satellite Link service, you must set the endpoint's source protocol to TLS or HTTPS. Certificate configuration then depends on the endpoint's destination type.
:   For endpoints with destination type 'cloud', you must provide a server certificate and key that the Satellite Link service will use to authenticate with the source. Note that if your server certificate is not signed by a well-known signing authority, you must also install a CA root certificate in your source environment.
:   For endpoints with destination type 'location', you do not need to configure any Satellite Link service certificates because Satellite Link uses its own certificate signed by [DigiCert](https://www.digicert.com/){: external}. DigiCert certificates are [compatible](https://knowledge.digicert.com/general-information/compatibility-of-digicert-trusted-root-certificates){: external} with all modern browsers and platforms but if your source has no root CA certificate, you will need to [download](https://www.digicert.com/kb/digicert-root-certificates.htm){: external} one from DigiCert and install it in your source environment.

[Mutual authentication between Satellite Link and the destination server](#mutual-auth-destination-loc)
:    With mutual authentication, the destination server authenticates itself to the Satellite Link service and the Satellite Link service must authenticate itself to the destination. To set up mutual authentication between the Satellite Link service and the destination server, you need to configure the Satellite Link service authentication the same way as in the simple destination authentication case. However, you will also need to provide a client certificate and key that the Satellite Link service uses to authenticate with the destination.


[Mutual authentication between the source and the Satellite Link service](#mutual-auth-source-loc)
:   With mutual authentication, the Satellite Link service authenticates itself to the source and the source must authenticate itself to the Satellite Link service. To set up mutual authentication between the source and Satellite Link service, you must configure the Satellite Link service authentication the same way as in the simple source authentication case. However, you also need to configure a CA certificate that the Satellite Link service uses to validate the source's client certificate.



[Mutual authentication between the source and the Satellite Link service as well as Satellite Link and the destination server](#mutual-auth-both-loc)
:   Requests to both the Satellite Link service and the destination server are authenticated. Two TLS handshakes must be verified before traffic is allowed. For this setup, create an endpoint with a source protocol of TLS or HTTPS and a destination protocol of TLS. Then, provide all of the certificates needed for both the source and destination mutual authentication cases, as described above.

If you choose to provide your own certificates for endpoint authentication, you are responsible for rotating the certificates and managing expiration dates. If your certificate expires, you might experience disrupted traffic.
{: important}


## Setting up authentication in the CLI
{: #mutual-auth-cli-loc}

The `source` options refer to the TLS handshake between the source and the Satellite Link service. The `dest` options refer to the TLS handshake between the Satellite Link service and your destination or target server. You can provide certificates for one or both of these connections. Unspecified settings are set to their default values.

The `ibmcloud sat endpoint authn` command is only used to set certificates for the Satellite Link service, acting as a server on the source side and as a client on the destination side. You may also need to configure certificates for the source (client) and/or destination (server) in their corresponding application environments.
{: note}

Review the following example scenarios.



### Simple authentication between the Satellite Link service and the destination
{: #simple-auth-destination-loc}

1. Create an HTTPS endpoint to an HTTPS server with destination certificate verification. 

    ```sh
    ibmcloud sat endpoint create 
      --location ID \
      --name myEndpoint \
      --dest-hostname example.com \
      --dest-port 443 \
      --source-protocol HTTPS \
      --dest-type location
    ```
    {: pre}

1. Create an HTTPS endpoint to an HTTP server with source mutual authentication.

    ```sh
    ibmcloud sat endpoint create \
      --location ID \
      --name myEndpoint \
      --dest-hostname example.com \
      --dest-port 80 \
      --dest-protocol TCP \
      --source-protocol HTTPS \
      --dest-type cloud
    ```
    {: pre}

1. Configure mutual TLS between the source and the Satellite Link service.

    This example provides the certificate of the trusted CA to validate the source's client certificate.
    ```sh
    ibmcloud sat endpoint authn set \
      --location ID \
      --endpoint myEndpoint \
      --source-tls-mode simple \
      --source-ca-cert-file /path/to/clientCACerts.pem
    ```
    {: pre}

1. Before your certificates expire, rotate them by replacing the existing authentication certificates with new ones. Only the certificates that you specify are replaced.

    ```sh
    ibmcloud sat endpoint authn rotate \
      --location ID \
      --endpoint myEndpoint \
      --source-ca-cert-file /path/to/clientCACerts.pem
    ```
    {: pre}

### Simple authentication with the Satellite Link service
{: #simple-auth-source-loc}

1. Create an HTTPS endpoint to an HTTPS server with source certificate verification. 

    ```sh
    ibmcloud sat endpoint create 
      --location ID \
      --name myEndpoint \
      --dest-hostname example.com \
      --dest-port 443 \
      --source-protocol HTTPS \
      --dest-type location
    ```
    {: pre}

1. Configure simple TLS with Satellite Link. This example provides the certificate of the trusted CA to validate the source server's certificate.

    ```sh
    ibmcloud sat endpoint authn set \
      --location ID \
      --endpoint myEndpoint \
      --dest-tls-mode simple
      --source-ca-cert-file /path/to/sourceCACerts.pem
    ```
    {: pre}

1. Before your certificates expire, rotate them by replacing the existing authentication certificates with new ones. Only the certificates that you specify are replaced.

    ```sh
    ibmcloud sat endpoint authn rotate \
      --location ID \
      --endpoint myEndpoint \
      --source-ca-cert-file /path/to/sourceCACerts.pem
    ```
    {: pre}

### Mutual authentication between the Satellite Link service and the destination
{: #mutual-auth-destination-loc}

1. Create an HTTP endpoint to an HTTPS server.

    ```sh
    ibmcloud sat endpoint create \
      --location ID \
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
    ibmcloud sat endpoint authn set \
      --location ID \
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
      --location ID \
      --endpoint myEndpoint \
      --dest-cert-file /path/to/clientCertificate.pem \
      --dest-key-file /path/to/clientKey.pem \
      --dest-ca-cert-file /path/to/serverCACerts.pem
    ```
    {: pre}



### Mutual authentication between the source and the Satellite Link service
{: #mutual-auth-source-loc}

Unlike the other examples, which can work with an endpoint `--dest-type` of either `location` or `cloud`, this one must use `--dest-type cloud` because setting source certificates for location destination endpoints is not supported.
{: exception}

1. Create an HTTPS endpoint to an HTTP server with source mutual authentication.

    ```sh
    ibmcloud sat endpoint create \
      --location ID \
      --name myEndpoint \
      --dest-hostname example.com \
      --dest-port 80 \
      --dest-protocol TCP \
      --source-protocol HTTPS \
      --dest-type cloud
    ```
    {: pre}

1. Configure mutual TLS between the source and the Satellite Link service.

    This example provides the certificate of the trusted CA to validate the source's client certificate.
    ```sh
    ibmcloud sat endpoint authn set \
      --location ID \
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
      --location ID \
      --endpoint myEndpoint \
      --source-cert-file /path/to/serverCertificate.pem \
      --source-key-file /path/to/serverKey.pem \
      --source-ca-cert-file /path/to/clientCACerts.pem
    ```
    {: pre}




### Mutual authentication at both the source and destination
{: #mutual-auth-both-loc}



1. Create an HTTPS endpoint to an HTTPS server.

    ```sh
    ibmcloud sat endpoint create \
      --location ID \
      --name myEndpoint \
      --dest-hostname example.com \
      --dest-port 443 \
      --source-protocol HTTPS \
      --dest-type location
    ```
    {: pre}

1. Configure mutual TLS authentication with both the source and the destination server.

    This example enables mutual authentication between the source and the Satellite Link service as well as between the Satellite Link service and the destination server.

    ```sh
    ibmcloud sat endpoint authn set \
      --location ID \
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
      --location ID \
      --endpoint myEndpoint \
      --source-ca-cert-file /path/to/clientCACerts.pem \
      --dest-cert-file /path/to/clientCertificate.pem \
      --dest-key-file /path/to/clientKey.pem \
      --dest-ca-cert-file /path/to/serverCACerts.pem
    ```
    {: pre}

## Removing certificates
{: #remove-certs-loc}

The `ibmcloud sat endpoint authn set` command can be used to remove one or more existing certificates. The set command removes all certificates other than the ones specified in the command's options. For example, the following command removes all currently configured certificates for endpoint `myEndpoint`.

```sh
ibmcloud sat endpoint authn set --location ID --endpoint myEndpoint
```
{: pre}

If you only want to remove some of the certificates, leaving others unchanged, make sure to pass options with the current values of the other certificates. For example, the following command can be used to remove `source` certificates that are currently set, leaving the `dest` certificates unchanged.

```sh
ibmcloud sat endpoint authn set \
  --location ID \
  --endpoint myEndpoint \
  --dest-tls-mode mutual \
  --dest-cert-file /path/to/current/clientCertificate.pem \
  --dest-key-file /path/to/current/clientKey.pem \
  --dest-ca-cert-file /path/to/current/serverCACerts.pem
```
{: pre}
