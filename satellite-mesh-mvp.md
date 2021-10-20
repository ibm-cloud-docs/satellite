---

copyright:
  years: 2021, 2021
lastupdated: "2021-10-20"

keywords: satellite, mesh, istio, microservices

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}} 

# {{site.data.keyword.satelliteshort}} mesh
{: #sat-mesh}

{{site.data.keyword.Bluemix_notm}} {{site.data.keyword.satelliteshort}} mesh provides an easy way to create, configure, connect and monitor your microservices network. 
{: shortdesc}

## What is a service mesh?
{: #what-is-service-mesh}

A service mesh is a configurable, language-independent infrastructure layer that coordinates communication between microservices. Distributed microservice architecture simplifies the rapid deployment of small independent services, but also creates new challenges such as controlling and securing traffic between services and enforcing consistent access policies throughout the network. The service mesh reduces these complexities by using network proxies to intercept and facilitate communication between microservices.

## What is {{site.data.keyword.satelliteshort}} mesh?
{: #sat-mesh-description}

{{site.data.keyword.satelliteshort}} mesh is built on the Istio-based Red Hat OpenShift Service Mesh and includes additional automation for configuring and monitoring services in your Satellite clusters. For more information on the Red Hat Openshift Service Mesh platform, see the [Red Hat documentation](https://docs.openshift.com/container-platform/4.8/service_mesh/v2x/ossm-about.html){: external}.
{: shortdesc}

{{site.data.keyword.satelliteshort}} mesh is available in beta and is currently supported for single {{site.data.keyword.satelliteshort}} clusters only. Don't use this service for production workloads.
{: beta}

### What's unique about {{site.data.keyword.satelliteshort}} mesh?
{: #sat-mesh-unique}


{{site.data.keyword.satelliteshort}} mesh utilizes a multi-tenant, external control plane model where the control plane and the data plane are deployed separately. The control plane, which is managed by {{site.data.keyword.IBM_notm}}, is deployed on your {{site.data.keyword.satelliteshort}} location control plane and kept separate from the mesh workload, while the data plane, which is managed by the user, is deployed on the cluster worker nodes. This configuration provides more capability for vendor-management and automation. 

### What are the benefits of {{site.data.keyword.satelliteshort}} mesh? 
{: #sat-mesh-benefits}

{{site.data.keyword.satelliteshort}} mesh makes it easy to integrate an Istio-based service mesh into your existing {{site.data.keyword.satelliteshort}} infrastructure. The benefits of {{site.data.keyword.satelliteshort}} mesh include:

- **Life-cycle management**: Services that use {{site.data.keyword.satelliteshort}} mesh automatically receive security patch updates.
- **High availability**: By integrating your service mesh in your {{site.data.keyword.satelliteshort}} control plane, you can take advantage of {{site.data.keyword.satelliteshort}}'s multi-zone resiliency.
- **Integrated CLI**: With a single command, you can install and configure {{site.data.keyword.satelliteshort}} mesh onto your cluster. 

## Architecture
{: #sat-mesh-architecture}

Service mesh configurations consist of a data plane and a control plane. The data plane includes the proxies that route traffic between services and the control plane configures the proxies that run within the data plane. 
{: shortdesc}

The control plane, which is managed by {{site.data.keyword.IBM_notm}}, is deployed on a cluster that is separate from the mesh workload, while the data plane, which is managed by the user, is deployed on the cluster worker nodes. 



### What comes with {{site.data.keyword.satelliteshort}} mesh?
{: #sat-mesh-components}

{{site.data.keyword.satelliteshort}} mesh installs the core components of Istio. For more information about any of the following control plane components, see the Istio documentation.

- `Envoy` proxies intercept inbound and outbound traffic for all services in the mesh. Proxies are deployed as sidecar containers in the same pods as your app containers.
- `istiod` is the control plane component that is managed by {{site.data.keyword.IBM_notm}} outside of the {{site.data.keyword.satelliteshort}} cluster. It is a consolidation of the components that are responsible for traffic-management, security, and mesh configurations. 

## Installing {{site.data.keyword.satelliteshort}} mesh
{: #sat-mesh-install}

Follow the steps to install {{site.data.keyword.satelliteshort}} mesh using the CLI.
{: shortdesc}

Before you begin:
[Install the CLI plug-in for {{site.data.keyword.satelliteshort}} commands](/docs/satellite?topic=satellite-setup-cli).

Installing {{site.data.keyword.satelliteshort}} mesh:
1. [Access your {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat).

2. Run the command to create a {{site.data.keyword.satelliteshort}} mesh instance.
    ```sh
    ibmcloud sat mesh create --name <instance_name> --cluster <cluster_name>
    ```
    {: pre}

    **Parameter list**
    - `name`: The name of the {{site.data.keyword.satelliteshort}} mesh instance
    - `cluster`: The name or ID of the {{site.data.keyword.satelliteshort}} cluster to use as a data plane for the new instance.
    - `version`: The {{site.data.keyword.satelliteshort}} mesh version for the new instance.

3. Verify the installation by viewing the details of your {{site.data.keyword.satelliteshort}} mesh instance.
    ```sh
    ibmcloud sat mesh get
    ```
    {: pre}

## Removing a {{site.data.keyword.satelliteshort}} mesh instance
{: #sat-mesh-remove}

Follow the steps to remove a {{site.data.keyword.satelliteshort}} mesh instance.
{: shortdesc}

1. Run the command to remove the {{site.data.keyword.satelliteshort}} mesh instance.
    ```sh
    ibmcloud sat mesh rm --name <instance_name>
    ```
    {: pre}

2. List your mesh instances and verify that the specified instance is removed.
    ```sh
    ibmcloud sat mesh ls
    ```
    {: pre}

  