---

copyright:
  years: 2022, 2022
lastupdated: "2022-09-19"

keywords: satellite, hybrid, multicloud, location, host, location control plane

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Understanding {{site.data.keyword.satelliteshort}} location and hosts
{: #location-host}

A {{site.data.keyword.satellitelong}} location is a representation of an environment in your infrastructure provider, such as an on-prem data center or cloud. Locations are made up of compute sources, called hosts, that resides in your infrastructure provider or even locally. After you attach your hosts to a {{site.data.keyword.satelliteshort}} location, assign the hosts to the location control plane or use them to power your service workloads.
{: shortdesc}

Locations can be made of hosts of any size, including as small as a local desktop computer or as large as hundreds of computers in a central office. Because locations are made up of your infrastructure, you can create a {{site.data.keyword.satelliteshort}} location anywhere that your infrastructure is located. 

## {{site.data.keyword.satelliteshort}} location overview
{: #loc-overview}

To set up a {{site.data.keyword.satelliteshort}} location, you must first create the location, add hosts to it, and then create the location control plane. If you use a quick start template (Schematics template), some of these steps are done for you automatically.  
{: shortdesc}

![High level process overview.](/images/sat_location_overview.svg "High-level overview of a Satellite location"){: caption="Figure 1. High-level overview of a Satellite location" caption-side="bottom"}


1. [Plan your environment](/docs/satellite?topic=satellite-infrastructure-plan) for Satellite by choosing your infrastructure, thinking about the services that you want to use, and setting up the hosts that you want to use. Make sure your hosts meet the [minimum requirement](/docs/satellite?topic=satellite-host-reqs) and that you consider the [size of your location](/docs/satellite?topic=satellite-location-sizing).

    ![Plan for your Satellite location.](/images/1-plan-location.svg "Planning for your Satellite location"){: caption="Figure 2. Planning for your Satellite location" caption-side="bottom"}
    
2. Create your location that runs on your host infrastructure. [Choose an installation method](/docs/satellite?topic=satellite-locations) for your location, based on what is available for your infrastructure provider.

    ![Create your location.](/images/2-create-location.svg "Create your Satellite location"){: caption="Figure 3. Creating your Satellite location" caption-side="bottom"}

3. [Attach hosts to your location](/docs/satellite?topic=satellite-locations#location-create-manual) by running the installation scripts (RHEL) or ingestion scripts (RHCOS). If you are using a Schematics template, this step is done for you. After your hosts are attached, they are in an `unassigned` state.
    ![Attach hosts to your location.](/images/3-attach-hosts-location.svg "Attach hosts to your location"){: caption="Figure 4. Attaching hosts to your location" caption-side="bottom"}
     
4. Select hosts to make up your [location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane). The hosts in your {{site.data.keyword.satelliteshort}} location do not run any workloads until you assign them as compute capacity to the {{site.data.keyword.satelliteshort}} location control plane or a service. For example, a basic setup has 3 hosts that are assigned as worker nodes to the Satellite location control plane. For more information, see [sizing your location](/docs/satellite?topic=satellite-location-sizing). After you assign a host, it enters a `provisioning` status.
    ![Create your location control plane.](/images/4-assign-hosts-location.svg "Create your location control plane"){: caption="Figure 5. Creating your location control plane" caption-side="bottom"}

5. Wait for the host to enter a `normal` state. When you assign a host to the control plane, the host is bootstrapped to become a worker node in your {{site.data.keyword.satelliteshort}} location control plan. This bootstrap process consists of three phases, all of which must complete. First, required images are downloaded to the host from {{site.data.keyword.registrylong_notm}}. Then, the host is rebooted to apply the configuration. Finally, software packages are set up on the host. After the host is successfully bootstrapped, it enters a `normal` health state with an `assigned` status. You can no longer log in to the underlying machine with SSH to troubleshoot any issues. Instead, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts-debug).
    ![Satellite Location in a normal staten.](/images/5-location-normal-state.svg "Satellite Location in a normal staten"){: caption="Figure 6. Satellite Location in a normal state" caption-side="bottom"}

6. After you set up your {{site.data.keyword.satelliteshort}} location control plane, you can assign hosts to [Satellite-enabled IBM Cloud service](/docs/satellite?topic=satellite-managed-services) such as clusters or databases.
    ![Assigning hosts to your services.](/images/6-assign-hosts-to-services.svg "Assigning hosts to your Satellite-enabled services"){: caption="Figure 7. Assigning hosts to your Satellite-enabled services" caption-side="bottom"}


## I created a {{site.data.keyword.satelliteshort}} location, what comes next?
{: #loc-host-whats-next}

Now that your {{site.data.keyword.satelliteshort}} location is set up, you are ready to start using {{site.data.keyword.cloud_notm}} services.
{: shortdesc}

1. Add compute capacity to your location by [attaching more hosts to the location](/docs/satellite?topic=satellite-attach-hosts) so that you can run [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services).
2. Create a [{{site.data.keyword.satelliteshort}}-enabled {{site.data.keyword.cloud_notm}} service](/docs/satellite?topic=satellite-managed-services), such as a [{{site.data.keyword.redhat_openshift_notm}} cluster](/docs/openshift?topic=openshift-satellite-clusters). You assign the additional hosts that you previously attached as worker nodes to provide the compute power for the cluster. You can even [register existing {{site.data.keyword.redhat_openshift_notm}} clusters to your location](/docs/satellite?topic=satellite-setup-clusters-satconfig#register-openshift-clusters) to use as deployment targets.
3. Start [deploying Kubernetes resources to these clusters](/docs/satellite?topic=satellite-satcon-create) with {{site.data.keyword.satelliteshort}} Config.
4. Create [{{site.data.keyword.satelliteshort}} cluster storage templates](/docs/satellite?topic=satellite-sat-storage-template-ov).
5. Learn more about the [{{site.data.keyword.satelliteshort}} Link component](/docs/satellite?topic=satellite-link-location-cloud) and how you can use endpoints to manage the network traffic between your location and {{site.data.keyword.cloud_notm}}.

Need help? Check out [Getting support](/docs/satellite?topic=satellite-get-help) where you can find information about cloud status, issues, and logging; contacting support; and setting your email notification preferences for {{site.data.keyword.cloud_notm}} platform-related items.


