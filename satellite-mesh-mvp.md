---

copyright:
  years: 2021, 2022
lastupdated: "2022-01-20"

keywords: satellite, mesh, istio, microservices

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}} 

# Setting up a service mesh with {{site.data.keyword.satelliteshort}} Mesh
{: #sat-mesh}

{{site.data.keyword.Bluemix_notm}} {{site.data.keyword.satelliteshort}} Mesh provides an easy way to create, configure, connect and monitor your microservices network. 
{: shortdesc}

{{site.data.keyword.satelliteshort}} Mesh is in a limited beta release and is not available to all customers. It is supported for single Satellite clusters only. Do not use this service for production workloads. 
{: beta}

## What is a service mesh?
{: #what-is-service-mesh}

A service mesh is a configurable, language-independent infrastructure layer that coordinates communication between microservices. Distributed microservice architecture simplifies the rapid deployment of small independent services, but also creates new challenges such as controlling and securing traffic between services and enforcing consistent access policies throughout the network. The service mesh reduces these complexities by using network proxies to intercept and facilitate communication between microservices.

## What is {{site.data.keyword.satelliteshort}} Mesh?
{: #sat-mesh-description}

{{site.data.keyword.satelliteshort}} Mesh is built on the Istio-based Red Hat OpenShift Service Mesh and includes additional automation for configuring and monitoring services in your Satellite clusters. For more information on the Red Hat Openshift Service Mesh platform, see the [Red Hat documentation](https://docs.openshift.com/container-platform/4.8/service_mesh/v2x/ossm-about.html){: external}.
{: shortdesc} 

### What are the benefits of {{site.data.keyword.satelliteshort}} Mesh? 
{: #sat-mesh-benefits}

{{site.data.keyword.satelliteshort}} Mesh makes it easy to integrate an Istio-based service mesh into your existing {{site.data.keyword.satelliteshort}} infrastructure. The benefits of {{site.data.keyword.satelliteshort}} Mesh include:

- **Life-cycle management**: Services that use {{site.data.keyword.satelliteshort}} Mesh automatically receive security patch updates.
- **High availability**: By integrating your service mesh in your {{site.data.keyword.satelliteshort}} control plane, you can take advantage of {{site.data.keyword.satelliteshort}}'s multi-zone resiliency.
- **Integrated CLI**: With a single command, you can install and configure {{site.data.keyword.satelliteshort}} Mesh onto your cluster. 

### Which Istio and Red Hat OpenShift Service Mesh features are supported on {{site.data.keyword.satelliteshort}} Mesh?
{: #sat-mesh-istio-redhat}

**Supported Istio features**
The following Istio features are supported on {{site.data.keyword.satelliteshort}} Mesh. For more information, see the [Istio documentation](https://istio.io/v1.9/docs/tasks/){: external}.

- Dynamic request routing
- Fault injection
- Traffic shifting
- TCP traffic shifting
- Request timeouts
- Circuit breaking
- Traffic mirroring
- Ingress gateways
- Secure gateways
- Ingress gateways without TLS termination
- External service access
- Egress TLS origination
- Egress gateways
- Egress gateways with TLS origination
- Egress with wildcard hosts
- Security authentication and authorization

**Supported Red Hat Openshift Service Mesh features**
For information on what Red Hat OpenShift Service Mesh features are supported on {{site.data.keyword.satelliteshort}} Mesh, see:

- [Adding services to a service mesh](https://docs.openshift.com/container-platform/4.8/service_mesh/v2x/ossm-create-mesh.html){: external}.
- [Enabling sidecar injection](https://docs.openshift.com/container-platform/4.9/service_mesh/v2x/prepare-to-deploy-applications-ossm.html){: external}.

## Architecture
{: #sat-mesh-architecture}

Service mesh configurations consist of a data plane and a control plane. The data plane includes the proxies that route traffic between services and the control plane configures the proxies that run within the data plane. 
{: shortdesc}

The control plane, which is managed by {{site.data.keyword.IBM_notm}}, is deployed on a cluster that is separate from the mesh workload, while the data plane, which is managed by the user, is deployed on the cluster worker nodes. 



### What comes with {{site.data.keyword.satelliteshort}} Mesh?
{: #sat-mesh-components}

{{site.data.keyword.satelliteshort}} Mesh installs the core components of Istio. For more information about any of the following control plane components, see the Istio documentation.

- `Envoy` proxies intercept inbound and outbound traffic for all services in the mesh. Proxies are deployed as sidecar containers in the same pods as your app containers.
- `istiod` is the control plane component that is managed by {{site.data.keyword.IBM_notm}} outside of the {{site.data.keyword.satelliteshort}} cluster. It is a consolidation of the components that are responsible for traffic-management, security, and mesh configurations. 

## Installing {{site.data.keyword.satelliteshort}} Mesh
{: #sat-mesh-install}

Follow the steps to install {{site.data.keyword.satelliteshort}} Mesh using the CLI. For more information about the {{site.data.keyword.satelliteshort}} Mesh commands, [see the CLI reference](/docs/satellite?topic=satellite-satellite-cli-reference#sat-mesh-commands). 
{: shortdesc}

The {{site.data.keyword.satelliteshort}} Mesh beta release is available only in the `us-south` region. Before you begin, you must target `us-south` by running `ibmcloud target -r us-south`.
{: note}

{{site.data.keyword.satelliteshort}} Mesh is designed to be the only service mesh installed on a cluster and might conflict with other service mesh providers. Do not use {{site.data.keyword.satelliteshort}} Mesh alongside other other service mesh providers, such as Istio, Maistra, or RedHat Service Mesh. 
{: important}

Before you begin:
[Install the CLI plug-in for {{site.data.keyword.satelliteshort}} commands](/docs/satellite?topic=satellite-setup-cli).

Installing {{site.data.keyword.satelliteshort}} Mesh:

1. [Access your {{site.data.keyword.satelliteshort}} cluster](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat).

2. Target the `us-south` region.

    ```sh
    ibmcloud target -r us-south
    ```
    {: pre}

3. Run the command to create a {{site.data.keyword.satelliteshort}} Mesh instance.

    ```sh
    ibmcloud sat mesh create --name <mesh_name> --cluster <cluster_id>
    ```
    {: pre}

4. Verify the installation by viewing the details of your {{site.data.keyword.satelliteshort}} Mesh instance.

    ```sh
    ibmcloud sat mesh get --mesh <mesh_name>
    ```
    {: pre}

    Example output
    ```sh
    Mesh name:                 test_mesh
    Mesh ID:                   a0612m7djht6mqewwjw5
    Data plane cluster name:   test_cluster
    Data plane cluster ID:     c620bfn20tqwe453j860
    Conditions:
    Type    Status   Last transition time            Message
    Ready   True     2021-11-18 16:33:50 +0100 CET   All component are Available
    OK
    ```
    {: screen}

## Removing a {{site.data.keyword.satelliteshort}} Mesh instance
{: #sat-mesh-remove}

Follow the steps to remove a {{site.data.keyword.satelliteshort}} Mesh instance. For more information about the {{site.data.keyword.satelliteshort}} Mesh commands, [see the CLI reference](/docs/satellite?topic=satellite-satellite-cli-reference#sat-mesh-commands). 
{: shortdesc}

1. Run the command to remove the {{site.data.keyword.satelliteshort}} Mesh instance.
    ```sh
    ibmcloud sat mesh rm --mesh <mesh_name>
    ```
    {: pre}

2. List your mesh instances and verify that the specified instance is removed.
    ```sh
    ibmcloud sat mesh ls
    ```
    {: pre}


  
