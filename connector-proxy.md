---


copyright:
  years: 2023, 2024
lastupdated: "2024-08-20"

keywords: satellite, http proxy, http, proxy, connector

subcollection: satellite

---


{{site.data.keyword.attribute-definition-list}}

# Configuring a proxy for your {{site.data.keyword.satelliteshort}} Connector
{: #config-connector-proxy}

You can use a proxy to establish tunnel connection for your {{site.data.keyword.satelliteshort}} Connector.
{: shortdesc}

There are various ways to setup a proxy. These instructions assume that you have a properly configured proxy, which is accessible from the machine running the Connector agent. The following instructions have been tested with the Connector agent machine running Ubuntu 22.04 with an explicit Squid proxy running on a different machine from the Connector agent.

1. Create a Connector.

1. On the Connector agent, ensure that your Docker runtime is using the proxy so that you can log in and pull images from `icr.io`. For more information, see [Configure the Docker daemon to use a proxy server](https://docs.docker.com/engine/daemon/proxy/#httphttps-proxy){: external}. 

1. In your environment variable file for the Connector agent (`~/agent/env-files/env.txt` from the previous example), add the following environment variables with your proxy information.
    ```txt  
    HTTP_PROXY=http://my.proxy.example.com:3128
    HTTPS_PROXY=https://my.proxy.example.com:3129
    ```
    {: codeblock} 
    
    For example, if you use an HTTP proxy, the `env.txt` file is similar to:
    
    ```txt  
    SATELLITE_CONNECTOR_ID=U2.....wZyI
    SATELLITE_CONNECTOR_IAM_APIKEY=/agent-env-files/apikey
    SATELLITE_CONNECTOR_REGION=us-south
    SATELLITE_CONNECTOR_TAGS=my tag
    HTTP_PROXY=http://192.168.3.87:3128
    HTTPS_PROXY=http://192.168.3.87:3128
    ```
    {: codeblock}







