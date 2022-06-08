---

copyright:
  years: 2022, 2022
lastupdated: "2022-06-08"

keywords: satellite, node, azure, file, disk

subcollection: satellite
content-type: troubleshoot

---
{{site.data.keyword.attribute-definition-list}}


# Why are my nodes failing to register using Azure File or Azure Disk storage templates?
{: #ts-node-register-failure}

Your worker nodes are failing to register when creating an Azure Disk or Azure File storage configuration. 
{: tsSymptoms}


If you manually assigned your Azure hosts to your location and did not use an automated deployment such as deploying from the console or a Terraform template, then you must label your worker nodes before creating your storage configuration. If your nodes are failing to register when creating a Azure Disk or Azure File {{site.data.keyword.satelliteshort}} storage configuration, verify that your worker nodes are labeled with the proper zone. Follow the steps to verify that your worker nodes have the proper labels:
{: tsResolve}

1. 1. Log in to the {{site.data.keyword.cloud_notm}} CLI.

    ```sh
    ibmcloud login
    ```
    {: pre}

1. List your {{site.data.keyword.satelliteshort}} locations and note the `Managed from` column.

    ```sh
    ibmcloud sat location ls
    ```
    {: pre}

1. Target the `Managed from` region of your {{site.data.keyword.satelliteshort}} location. For example, for `wdc` target `us-east`. For more information, see [{{site.data.keyword.satelliteshort}} regions](/docs/satellite?topic=satellite-sat-regions).

    ```sh
    ibmcloud target -r us-east
    ```
    {: pre}

1. If you use a resource group other than `default`, target it.

    ```sh
    ibmcloud target -g <resource-group>
    ```
    {: pre}
    

1. List your Azure worker nodes and make a note of the `name` of each node. 

    ```sh
    oc get nodes
    ```
    {: pre}

1. Get the details of each node and make a note of the `zone` that the node is in. For example: `eastus-1`. 

    ```sh
    oc get nodes NODE-NAME -o yaml | grep zone
    ```
    {: pre}

1. Label your Azure worker nodes with the `zone` value that you retrieved earlier. Replace `<node_name>` and `<zone>` with the node name and zone of your worker nodes. For example, if you have a worker node in zone: `eastus-1`, use the following commands to add `eastus-1` as a label to the worker node in the `eastus-1` zone.
    ```sh
    oc label node <node_name> topology.kubernetes.io/zone-
    oc label node <node_name> topology.kubernetes.io/zone=<zone> --overwrite
    ```
    {: pre}

1. Repeat the previous steps for each worker node.
