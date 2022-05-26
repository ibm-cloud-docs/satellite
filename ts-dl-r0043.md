---

copyright:
  years: 2022, 2022
lastupdated: "2022-05-26"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I get an R0043 error after setting up {{site.data.keyword.dl_short}}?
{: #ts-dl-r0043}


After you set up {{site.data.keyword.dl_short}}, you see a message similar to one of the following messages.
{: tsSymptoms}

```sh
R0043: The location does not meet the following requirement: Hosts must have TCP/UDP/ICMP Layer 3 connectivity for all ports across hosts. If you still have issues, contact IBM Cloud Support and include your Satellite location ID.
```
{: screen}

```sh
Oct 7 12:02:09 satellite-link-tunnel-74b648978b-jw4mr satellite-link-tunnel-container 50 flowlog: error when client 10.211.100.161:57124 connecting to a96ff1d0c68e3e5e087b3-6b64a6ccc9c596bf59a86625d8fa2202-ce00.us-east.satellite.appdomain.cloud:30000, conn_type: location, detail: connect ECONNREFUSED 10.0.2.80:30000
```
{: screen}

Your host doesn't meet the requirements or it needs to be rebooted.
{: tsCauses}

Follow these steps to resolve your issue
{: tsResolve}

1. Verify that your host meets the requirements. For more information see [Host network requirements](/docs/satellite?topic=satellite-reqs-host-network).

2. If your host meets the requirements, perform an `nslookup` on your location subdomain to retrieve the IP address of the host that is registered with this subdomain.  
 
3. Find the host in your infrastructure provider and reboot the host. 

4. If the reboot does not resolve the issue, replace the host.
    
5. If you are still unable to resolve the issue, [open a support case](/docs/satellite?topic=satellite-get-help#help-support). In the support case description, include all debugging steps that you followed and the output from these steps.
