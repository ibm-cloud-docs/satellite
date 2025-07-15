---


copyright:
  years: 2023, 2025
lastupdated: "2025-07-15"

keywords: satellite, connector

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Running your Connector agent as a service in Docker Swarm Mode for high availability
{: #run-agent-swarm}

In addition to running the container stand-alone as shown in the previous example, you can use Docker Swarm mode to run a cluster of up to 3 containers on multiple machines to provide high availability of the {{site.data.keyword.satelliteshort}} Connector agent.
{: shortdesc}

To configure {{site.data.keyword.satelliteshort}} Connectors, you must have Administrator access to the **Satellite** service in IAM access policies.
{: note}

- Satellite Connector treats all the agents connected to a Connector as equal.
- The TCP sessions are round robin through all the agents.
- It is recommended to use a minimum of 3 agents for redundancy. This makes Satellite Connector service more reliable and there is no additonal charge for additional agents. On windows, this means using multiple Windows hosts.
- If you are using containers make sure your containers aren't all on the same VM or hardware as that's not redundant.
- If you're running a high bandwidth workload it's recommended to use at least 6 agents because that brings more hosts and more bandwidth.
- The maximum agents per Connector is 9, but this setup is not recommend because containers and virtual machines are ephemeral and there might be a delay in noticing when one container disappeared and a new one is trying to connect. Instead, it is recommend to leave at least 3 open slots to allow for containers and VMs to stop and start.

1. Create a {{site.data.keyword.satelliteshort}} Connector agent compose file called `connector-agent.yaml` on your Swarm manager system and copy the following content to it.
    If you have multiple environments, you create multiple config files and name them by environment. For example, create a `connector-agent-prod.yaml` for production and a `connector-agent-staging.yaml` for staging. Then, include the Connector ID that you want to use for each environment in the respective file.
    {: tip}

    ```yaml
    version: '3.9'
    services:
      agent:
        image: icr.io/ibm/satellite-connector/satellite-connector-agent:latest
        environment:
        - SATELLITE_CONNECTOR_ID=/satellite-connector-id
        - SATELLITE_CONNECTOR_IAM_APIKEY=/run/secrets/satellite-connector-iam-apikey
        - SATELLITE_CONNECTOR_TAGS={{.Node.Hostname}} # You also can add other details in this field. For example, SATELLITE_CONNECTOR_TAGS="Some text"
        deploy:
          replicas: 3
          restart_policy:
            condition: any
          update_config:
            parallelism: 2
            delay: 20s
            failure_action: rollback
            order: start-first
          resources:
            limits:
              cpus: '0.40'
              memory: 500M
            reservations:
              cpus: '0.04'
              memory: 200M 
        configs: 
          - source: satellite-connector-id
            uid: '1000'
            gid: '1000'
            mode: 0400
          - source: satellite-connector-region
            uid: '1000'     
            gid: '1000'     
            mode: 0400      
        secrets:
          - source: satellite-connector-iam-apikey
            uid: '1000'
            gid: '1000'
            mode: 0400
        logging:
          driver: "json-file"
          options:
            max-size: ${JSON_FILE_MAX_SIZE:-1m}
            max-file: ${JSON_FILE_MAX_FILE:-10}

    configs:
      satellite-connector-id:
        external: true
      satellite-connector-region:
        external: true

    secrets:
      satellite-connector-iam-apikey:
        external: true

    networks:
      default:
        external: true
        name: bridge
    ```
    {: codeblock}
  
1. Create a Docker config with the {{site.data.keyword.satelliteshort}} Connector ID.
    ```sh
    printf <satellite connector id value> | docker config create satellite-connector-id -
    ```
    {: pre}
  
1. Create a Docker config with the {{site.data.keyword.satelliteshort}} Connector region.
    ```sh
    printf <satellite connector region value> | docker config create satellite-connector-region -
    ```
    {: pre}
  
1. Create a Docker secret with your IAM API key.
    ```sh
    printf <Your IAM API key> | docker secret create satellite-connector-iam-apikey -
    ```
    {: pre}

By default, the Swarm compose file sets the `SATELLITE_CONNECTOR_TAGS` environment variable to the Swarm node's hostname. You can adjust this value to suit your needs within the compose file. In addition, the number of replicas is set to 3 by default. You can adjust this number lower, but only a maximum of 3 replicas are supported.

1. Deploy the {{site.data.keyword.satelliteshort}} Connector agent service.
      1. Make sure you are logged in to the {{site.data.keyword.registrylong}} either using the `ibmcloud cr login` CLI or with your API key `docker login -u iamapikey -p <your apikey> icr.io`.
      1. Deploy the stack.  
          ```sh
          docker stack deploy --compose-file satellite-connector-agent.yaml --with-registry-auth satellite_connector
          ```
          {: pre}  

1. Verify the {{site.data.keyword.satelliteshort}} Connector agent service is running.
    ```sh
    docker service ps satellite_connector_agent
    ```
    {: pre} 
    
    
    
