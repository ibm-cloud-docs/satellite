---

copyright:
  years: 2023, 2023
lastupdated: "2023-10-11"

keywords: satellite, connector

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Running a Connector agent
{: #run-agent-locally}

After you create your Connector, follow the steps to create a Connector agent to complete your setup.   
{: shortdesc}

To configure {{site.data.keyword.satelliteshort}} Connectors, you must have Administrator access to the **Satellite** service in IAM access policies.
{: note}
  
## Reviewing the agent image parameters
{: #review-parameters}
{: step}  
  
Configuration information is provided to the agent through the following environment variables. Any of these environment variables can either be set directly to a value or set to a path of a file that contains the value. The files are mounted to the Docker container with the standard Docker mechanisms. The path value must be accessible from within the container and is therefore based on the mount point and not a local path on the host. See the following table for an example.

| Environment variable | Description |
| -------------------- | ----------- |
| SATELLITE_CONNECTOR_ID |  **Required:** Identifier of the Satellite Connector that the agent is to bind to. |
| SATELLITE_CONNECTOR_IAM_APIKEY | **Required:** Your IAM API key. For security purposes, consider storing your IAM API key in a file and then providing the file for this value. |
| SATELLITE_CONNECTOR_REGION | **Optional:** The managed from region of the {{site.data.keyword.satelliteshort}} Connector. This value must be the short name of the region such as `us-east`. You can find the mapping from the multizone metro name that is shown in the UI to the region name in [Supported IBM Cloud regions](/docs/satellite?topic=satellite-sat-regions). |
| SATELLITE_CONNECTOR_TAGS | **Optional:** A user defined string that can be used to identify the instance of the Docker container. This string can be any value that you find useful. The value must be less than or equal to 256 characters and is truncated if over 256 characters. The following characters are removed: `<>/{}%[]?,;@$&`. |
{: caption="Table 1. Environment variables for configuration" caption-side="bottom"}
  
## Creating the local configuration files
{: #create-config-file}
{: step}

There are several ways to pass agent configuration environment variable information to the container. This example uses files. However, you can use the `docker run --env` command to specify the values. Be aware that if you use `--env` with your API key, the API key is exposed to the container environment and is visible on the output of `docker inspect` command.  You can secure your API key in a file and then use the file name in the environment variable. If you choose to use the file name, you must make sure that the file path that you specify in the environment variable is mounted to a file path in the container, as shown in the following example. The file names shown in the following steps are examples and can be tailored for your environment.

To create a Connector, you need **Administrator** Platform role for {{site.data.keyword.satelliteshort}} in IAM. To connect an Agent to an existing Connector, you need **Viewer** Platform role or **Reader** Service role for {{site.data.keyword.satelliteshort}} in IAM.
{: note}
  
1. Create a directory for the configuration files, in this example `~/agent/env-files`.
1. Create a file in the `~/agent/env-files` directory called `apikey` with a single line value of your IBM Cloud API Key that can access the {{site.data.keyword.satelliteshort}} Connector.
1. Create a file in the `~/agent/env-files` directory called `env.txt` with the following values. Modify the `SATELLITE_CONNECTOR_ID` variable with your {{site.data.keyword.satelliteshort}} Connector ID and the `SATELLITE_CONNECTOR_REGION` with the region of the {{site.data.keyword.satelliteshort}} Connector you created previously. 

    ```sh
    SATELLITE_CONNECTOR_ID=<Your Satellite Connector ID>
    SATELLITE_CONNECTOR_IAM_APIKEY=/agent-env-files/apikey
    SATELLITE_CONNECTOR_REGION=<Your Satellite Connector Region>
    SATELLITE_CONNECTOR_TAGS=sample tag
    ```
    {: codeblock}
  
1. At this point, your directory contains 2 files and looks similar to the following example:
    ```sh
    env-files$ ls
    apikey  env.txt
    ```
    {: pre}

## Pulling the agent image
{: #pull-agent-image}
{: step}


1. Log in to {{site.data.keyword.registrylong}}. Or log in to the repository directly from Docker with your API key.

    ```sh
    ibmcloud cr region-set icr.io
    ```
    {: pre}
    
    ```sh
    docker login -u iamapikey -p <your apikey> icr.io
    ```
    {: pre}

1. Pull the latest version of the published image. You can find the list of published versions from [IBM Satellite Connector Agent Release History](/docs/satellite?topic=satellite-cl-connector-agent-image).
    ```sh
    docker pull icr.io/ibm/satellite-connector/satellite-connector-agent:latest
    ```
    {: pre}


## Running the agent image
{: #run-agent-image}
{: step}  

Make sure your computing environment meets the [Minimum requirements](/docs/satellite?topic=satellite-understand-connectors&interface=ui#min-requirements) for running the agent image. Then follow these steps.

1. To view available versions of agent image, run the following command.

    ```sh
    ibmcloud cr images --include-ibm|grep connector
    ```
    {: pre}
  
    Example output:
    ```sh
    icr.io/ibm/satellite-connector/satellite-connector-agent    latest    5f4e42c8d53e   ibm         1 day ago       124 MB   -
    icr.io/ibm/satellite-connector/satellite-connector-agent    v1.0.5    6eadd91be5c0   ibm         1 week ago      124 MB   -
    icr.io/ibm/satellite-connector/satellite-connector-agent    v1.1.0    5f4e42c8d53e   ibm         1 day ago       124 MB   -
    ```
    {: screen}

1. Mount your `env-files` directory to the container's `/agent-env-files` directory by using the `-v` option. You can use the latest version or a specific version of the published image.  
    
    If an environment variable is using a path to a file, that path must be a file path inside the container. To retrieve the file path, use the `-v` option on the `docker run` command. The `-v` option is specified by the local environment variable directory path, followed by the mounted path in the container and separated by `:`. For example, `-v ~/agent/env-files:/agent-env-files`, where `~/agent/env-files` is your local path and `/agent-env-files` is a path in your container. 
    {: tip}
    
    ```sh
    docker run -d --env-file ~/agent/env-files/env.txt -v ~/agent/env-files:/agent-env-files icr.io/ibm/satellite-connector/satellite-connector-agent:latest
    ```
    {: pre}   

  
    Example command using version 1.0.3 of the image, run the following command.

    ```sh
    docker run -d --env-file ~/agent/env-files/env.txt -v ~/agent/env-files:/agent-env-files icr.io/ibm/satellite-connector/satellite-connector-agent:v1.1.0
    ```
    {: pre}


1. You can verify the tunnel gets established to your Connector by looking at the logs of the container.
    ```sh
    docker logs CONTAINER-ID
    ```
    {: pre}
 
    Near the beginning of the log, you can find entries similar to the following examples.

    ```sh
    {"level":30,"time":"2023-06-20T16:12:20.133Z","pid":8,"hostname":"6b793f671c79","name":"agentOps","msgid":"A02","msg":"Load SATELLITE_CONNECTOR_ID value from SATELLITE_CONNECTOR_ID environment variable."}
    {"level":30,"time":"2023-06-20T16:12:20.138Z","pid":8,"hostname":"6b793f671c79","name":"agentOps","msgid":"A01","msg":"Load SATELLITE_CONNECTOR_IAM_APIKEY value from file /agent-env-files/apikey."}
    {"level":30,"time":"2023-06-20T16:12:20.140Z","pid":8,"hostname":"6b793f671c79","name":"agentOps","msgid":"A02","msg":"Load SATELLITE_CONNECTOR_TAGS value from SATELLITE_CONNECTOR_TAGS environment variable."}
    {"level":30,"time":"2023-06-20T16:12:20.141Z","pid":8,"hostname":"6b793f671c79","name":"agentOps","msgid":"A02","msg":"Load SATELLITE_CONNECTOR_REGION value from SATELLITE_CONNECTOR_REGION environment variable."}
    {"level":30,"time":"2023-06-20T16:12:20.142Z","pid":8,"hostname":"6b793f671c79","name":"connector-agent","msgid":"LA2","msg":"Connector id: U2F0ZWxsaXRlQ29ubmVjdG9yOiJjaThzdWd1ZDFwZ2RrZmUxa3UxZyI, region: us-south, release info: 20230610-dd48822928d35a84b31029a996fa9abc9d29fc93_A."}
    {"level":30,"time":"2023-06-20T16:12:20.392Z","pid":8,"hostname":"6b793f671c79","name":"tunneldns","msgid":"D04","msg":"DoTunnelDNSLookup DNS resolve c-01-ws.us-south.link.satellite.cloud.ibm.com to 169.61.31.178"}
    {"level":30,"time":"2023-06-20T16:12:21.560Z","pid":8,"hostname":"6b793f671c79","name":"utilities","msg":"MakeLinkAPICall GET /v1/connectors/U2F0ZWxsaXRlQ29ubmVjdG9yOiJjaThzdWd1ZDFwZ2RrZmUxa3UxZyI status code 200"}
    {"level":30,"time":"2023-06-20T16:12:21.563Z","pid":8,"hostname":"6b793f671c79","name":"agent_tunnel","msgid":"LAT03","msg":"Got configuration"}
    {"level":30,"time":"2023-06-20T16:12:21.565Z","pid":8,"hostname":"6b793f671c79","name":"agent_tunnel","msgid":"LAT04-wss://c-01-ws.us-south.link.satellite.cloud.ibm.com/ws","msg":"Connecting to wss://c-01-ws.us-south.link.satellite.cloud.ibm.com/ws"}
    {"level":30,"time":"2023-06-20T16:12:21.922Z","pid":8,"hostname":"6b793f671c79","name":"tunneldns","msgid":"D04","msg":"DoTunnelDNSLookup DNS resolve c-01-ws.us-south.link.satellite.cloud.ibm.com to 169.61.31.178"}
    {"level":30,"time":"2023-06-20T16:12:22.294Z","pid":8,"hostname":"6b793f671c79","name":"TunnelCore","msgid":"TC24","msg":"Tunnel open","connector_id":"U2F0ZWxsaXRlQ29ubmVjdG9yOiJjaThzdWd1ZDFwZ2RrZmUxa3UxZyI"}
    {"level":30,"time":"2023-06-20T16:12:22.299Z","pid":8,"hostname":"6b793f671c79","name":"connector_tunnel_base","msgid":"CTB26-U2F0ZWxsaXRlQ29ubmVjdG9yOiJjaThzdWd1ZDFwZ2RrZmUxa3UxZyI","msg":"Send connector information to tunnel server"}
    {"level":30,"time":"2023-06-20T16:12:22.307Z","pid":8,"hostname":"6b793f671c79","name":"connector_tunnel_base","msgid":"CTB27","msg":"Tunnel connected","connector_id":"U2F0ZWxsaXRlQ29ubmVjdG9yOiJjaThzdWd1ZDFwZ2RrZmUxa3UxZyI","cipher":{"name":"TLS_AES_256_GCM_SHA384","standardName":"TLS_AES_256_GCM_SHA384","version":"TLSv1.3"}}
    ```
    {: screen}


## Next steps
{: #agent-next-steps}

After creating a Connector agent, you can create endpoints to connect from the IBM Cloud private network to a resource running on your Location. You can also control access your endpoints by creating access control list rules. For more information, see [Creating and managing Connector endpoints](/docs/satellite?topic=satellite-connector-create-endpoints).
  

