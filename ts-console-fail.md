---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-15"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I access the {{site.data.keyword.redhat_openshift_notm}} console without a VPN on the VPC?
{: #ts-console-fail}


When you create a {{site.data.keyword.redhat_openshift_notm}} cluster in your {{site.data.keyword.satelliteshort}} location, you cannot access the {{site.data.keyword.redhat_openshift_notm}} web console, or access to the console is intermittent.
{: tsSymptoms}

You might see an error message similar to the following:
```sh
OpenShift web console unavailable
Wait until your ingress subdomain is available before you access the OpenShift web console.
```
{: screen}


Review the following reasons why you are unable to reach the {{site.data.keyword.redhat_openshift_notm}} web console.
{: tsCauses}

- The {{site.data.keyword.redhat_openshift_notm}} web console can be accessed only after your cluster's Ingress subdomain is created. For the Ingress subdomain to be created, hosts must be assigned as worker nodes in each zone of the default worker pool in your cluster. For example, if your default worker pool spans three zones, but hosts are assigned as worker nodes to the default worker pool in only two zones, the Ingress subdomain in your cluster cannot be created, and you cannot access the web console.
- The private IP addresses of your instances were automatically registered and added to your location's DNS record and the cluster's Ingress subdomain. This setup prevents users that are not connected to your hosts' private network from accessing the cluster from a local machine or opening the {{site.data.keyword.redhat_openshift_notm}} web console.

Choose from the following options.
{: tsResolve}

Worker nodes do not exist in each zone of the default worker pool
:    If you use host auto assignment, [attach hosts](/docs/satellite?topic=satellite-attach-hosts) to your {{site.data.keyword.satelliteshort}} location in the zone where you do not have worker nodes so that hosts can be assigned to the default worker pool. If the hosts are not automatically assigned, you can also manually [assign {{site.data.keyword.satelliteshort}} hosts in that zone to your cluster](/docs/satellite?topic=satellite-assigning-hosts#host-assign-manual).

Host private IP addresses are used for Ingress subdomain
:    Connect to your hosts' private network to access to your cluster and open the {{site.data.keyword.redhat_openshift_notm}} web console. For example, you might connect to your on-premises local network, or use a VPN such as [WireGuard](/docs/openshift?topic=openshift-access_cluster#access_vpn_openshift) to connect to your cloud provider's private network. 

Alternatively, if your hosts have public network connectivity, you can test access to your cluster by changing your cluster's service URL and your location's DNS record to [use your hosts' public IP addresses](/docs/openshift?topic=openshift-access_cluster#sat_public_access). Note that making your location and cluster subdomains available outside of your hosts' private network to your authorized cluster users is not recommended for production-level workloads.

If you are still unable to access the {{site.data.keyword.redhat_openshift_notm}} web console after completing these steps, see [Debugging the OpenShift web console](/docs/openshift?topic=openshift-ocp-debug) in the {{site.data.keyword.openshiftlong_notm}} troubleshooting documentation.
{: note}


