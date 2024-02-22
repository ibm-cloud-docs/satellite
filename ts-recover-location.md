---

copyright:
  years: 2024, 2024
lastupdated: "2024-02-22"

keywords: satellite, recover, disaster recovery, troubleshooting

subcollection: satellite

---
{{site.data.keyword.attribute-definition-list}}


# Recovering your location
{: #ts-recover-location}

The following steps outline the general flow of recovering from a disaster event within a Satellite location.
{: shortdesc}

1. Replace any unhealthy infrastructure in the Location control plane. Remove unhealthy hosts, then attach new hosts and assign them to the control plane.

1. After your control plane is healthy and has sufficient capacity for the services running in the Satellite location, the automated restoration process is executed in the Satellite platform.

    Typically at this state, the location shows the `R0025: The Satellite location has OpenShift clusters in critical health` warning.
    {: note}
  
1. Open a support case to track the status of the automated recovery. In the case details, provide the following information.

    ```txt
    Satellite Location: LOCATION-ID had a disaster event across the infrastructure associated with the Satellite location. We have proceeded to recover/replace the unhealthy infrastructure within the location control plane and have sufficient capacity to run all cluster control planes. These are the following OpenShift clusters within the location:
       CLUSTER-ID
       CLUSTER-ID
    ```
    {: screen}

1. After the automated restoration process completes, the `R0025` message is removed and the location is ready for deployments.
1. Recover or replace any unhealthy infrastructure in the data plane of each OpenShift cluster within the location. Remove unhealthy worker nodes. Attach new hosts and assign them as worker nodes. Repeat this process until all worker nodes in the cluster are healthy and running.
1. Begin application and persistent storage DR. Consult the appropriate application specific documentation or storage solution documentation for more details.

