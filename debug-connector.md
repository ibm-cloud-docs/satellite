---

copyright:
  years: 2023, 2023
lastupdated: "2023-07-05"

keywords: satellite, hybrid, multicloud, connector, create connector

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Debugging Connectors
{: #debug-connector}
  
To troubleshoot your issues quickly and efficiently, it is highly recommended that you connect your {{site.data.keyword.satelliteshort}} Connector instance to an {{site.data.keyword.la_full_notm}} instance.  
{: shortdesc}
  
Access your {{site.data.keyword.satelliteshort}} Connector instance from the console. If you don’t have an {{site.data.keyword.la_full_notm}} instance in your account for the region you created the {{site.data.keyword.satelliteshort}} Connector in, click **Connect** under the **Logging for Link** section. You will be taken to the **Catalog** page where you can create an {{site.data.keyword.la_full_notm}} instance. If you already have an {{site.data.keyword.la_full_notm}} instance, click **Configure** under the **Logging for Link** section. Then, select your existing logging instance. After you’ve connected an {{site.data.keyword.la_full_notm}} instance to your {{site.data.keyword.satelliteshort}} Connector, you can use the **Logging for Link** section to open the Logging Instance dashboard and the output will be filtered for your Connector.
  
The logging instance must have **Receive Platform Logs** enabled. To enable this option, select **Options** -> **Edit Platform** from the list of logging instances.
{: note}

Typically there are two types of errors:
1. The tunnel cannot be established. The agent does not show up in the Active Agents tab on the console.
2. The tunnel is established and you can see the agent in the list of Active Agents, but you cannot access an on-prem application from {{site.data.keyword.cloud_notm}} using an endpoint.  
  
## Tunnel cannot be established - Agent does not show up on list of Active Agents
{: #agent-not-listed}
  
The tunnel does not get established and you don’t see your Satellite Connector Agent showing up in the list on the UI under the “Active Agents” tab. 
{: shortdesc}
  
There is approximately a 2-minute delay from when the Connector Agent starts to when it shows up in the list. 
{: tip}

After 2 minutes, if the agent is still not displaying, follow these debugging steps:

1. Verify that the Connector ID and region are specified correctly.

1. Open the Logging Dashboard and review the Connector logs. Often, the issue is with the IAM API key and you see a message similar to the following example. For more information, see [Why isn't my API key working](/docs/satellite?topic=satellite-ts-connector-api).
    ```sh
    Failed to get configuration from API /v1/connectors/U2F0ZWxsaXRlQ29ubmVjdG9yOiJjaTExMGxpdzFwazluMGdybXUyMCI, region us-east, code: 401. IAM Error: "status code: 400. Provided API key could not be found.", API Error: "null", hostname: "482bddf6c60b"
    ```
    {: pre} 

  
1. Check the log on the agent container. If there are no errors in the {{site.data.keyword.la_full_notm}} Dashboard, this means there is an issue that is occuring before the agent communcates with the tunnel servers. You can get more information by looking at the log file on the agent container. The command varies by container platform. If you are using Docker, you can use the following command:
    ```sh
    docker logs <container id>
    ```
    {: pre}
  
1. You should be able to determine from the log messages what the issue is. The most common reason for errors is that your agent does not have public outbound access to talk to the IBM tunnel servers. See [Why is my Connector Agent unable to establish the tunnel with IBM Cloud](/docs/satellite?topic=satellite-ts-connector-tunnel).

  
1. Verify that you are using the correct container hardware platform. For example, you are trying to run the agent image on an arm64 platform. The connector agent will only run on linux/amd64 platforms or those platforms that can emulate amd64. If this is the case, you will see an error similar to:
    ```sh
    {"msg":"exec container process `/usr/local/bin/node`: Exec format error","level":"error","time":"2023-06-16T14:37:54.000567792Z"}
    ```
    {: screen}  
    
    Note for Apple Mac silicon users: If you are trying out the Connector on a Mac with Apple silicon which uses an ARM64 processor, the container agent will run if Rosetta2 has been installed. This is normally installed with Docker. When running the Connector Agent, you will see the following warning:
    ```sh
    icr.io/ibm/satellite-connector/satellite-connector-agent:v1.0.3 WARNING: The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested 43064456c42434f056348a32773a732d02d4a68690fc6b2b36790be8daa49bb2
    ```
    {: screen} 
       
    In this case, this is just a warning and the Connector Agent is running. If you don’t want to see the warning, you can specify the `--platform linux/amd64` option on your `docker run` command.

1. Verify that the container platform can pull the image. The image is located in the IBM Container Registry at `icr.io/ibm/satellite-connector/satellite-connector-agent:<version>`.  Make sure you have specified the image correctly. The machine running the agent has network access to `icr.io`, and that you have logged in to the IBM Container Registry. For more information, see [Pulling the agent image](/docs/satellite?topic=satellite-run-agent-locally#pull-agent-image).
  
Note to Docker Swarm users: If you see the following error about “No such image”:
```sh
icr.io/ibm/satellite-connector/satellite-connector-agent:v1.0.4   swarm-worker1   Shutdown    Rejected 5 minutes ago   "No such image: icr.io/ibm/sat…" 
```
{: screen}   
  
This means Docker Swarm was unable to pull the image. It is likely due to invalid IBM Container Registry credentials. To resolve this issue:
1. Remove the service.
2. Log in to the IBM Container Registry.
3. Restart the stack.
  
  
## Tunnel is established - Agent container is listed in the Active Agents tab on the console
{: #agent-listed} 

If your agent container is listed in the Active Agents tab on the console, follow these debugging steps:
  
1. Go to your Connector instance and open the Logging Dashboard. This automatically filters the logging output for your Connector ID. 
  
1. Review the error messages.
  
    After the tunnel is established, any errors will be located in both the {{site.data.keyword.la_full_notm}} instance and in the agent’s container platform logs.  Most errors will now be those trying to access an endpoint from within {{site.data.keyword.cloud_notm}} to an application running on-prem over the tunnel. When accessing an endpoint, at the start of the connection a `flowlog` entry is written to the logging instance. For example:
    ```sh
    flowlog: start for client 10.249.96.47:1206 connect to postgres.apps.wdc6.toddjohn.net:5432, conn_type: location
    ```
    {: screen}     
      
    After the connection is closed, another `flowlog` entry is written with some details about the connection. For example:  
    ```sh
    flowlog: end for client 10.249.96.47:1206 connect to postgres.apps.wdc6.toddjohn.net:5432, conn_type: location, duration 387 ms, BytesToCloud 2444, BytesFromCloud 168
    ```
    {: screen}    
      
    The duration is the time the connection is open, not the round trip time of requests. 
    {: note} 
      
    If there are any errors trying to connect to the endpoint, a `flowlog` entry is written containing the details of the error. For example:
    ```sh
    flowlog: error when client 10.249.96.47:1209 connecting to postgres.apps.wdc6.toddjohn.net:5433, conn_type: location, detail: connect ECONNREFUSED 192.168.3.84:5433
    ```
    {: screen}

 
1. If you don’t see any `flowlog`  entries, ensure your {{site.data.keyword.cloud_notm}} application has access to the CSE endpoint and is using the correct endpoint address and port. For example, if you are using a VPC instance or a VPC Kubernetes cluster, a security group might be blocking access. Ensure your security groups allow traffic from your VPC to the CSE endpoint IP and port.
  
1. Verify that your endpoint is configured correctly and the on-prem application is listening on the configured Destination FQDN or IP and Destination Port. If your on-prem application is using a container, its IP address might have changed. For more information, see [Why can't I reach my endpoint from IBM Cloud](/docs/satellite?topic=satellite-ts-connector-cannot-reach).
  
1. If you are running multiple agents to the same Connector, ensure all agents have network access to the endpoint. Each connection request is routed to a random agent and therefore all agents must have network connectivity to the all the on-prem endpoints. There is no mechanism to target an individual agent for a specific Connector.


  


