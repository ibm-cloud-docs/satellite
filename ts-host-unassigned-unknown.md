---

copyright:
  years: 2022, 2022
lastupdated: "2022-07-07"

keywords: satellite, hybrid, multicloud, unassigned, unresponsive

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}


# Why do my unassigned hosts have an `Unresponsive` status?
{: #ts-host-unassigned-unknown}

When you view the hosts in your location or run the `ibmcloud sat host ls` command, your `unassigned` show an `Unresponsive` status similar to the following.
{: tsSymptoms}

```sh
Name                        ID                     State        Status         Zone         Cluster                           Worker ID                                                 Worker IP
t-ca594kp10egvr0u1usv0-40   23cfa4f4349fd0104008   unassigned   Unresponsive   *            -                                 -                                                         -   
t-ca594kp10egvr0u1usv0-41   67f0bf6701a17c5a0c98   unassigned   Unresponsive   *            -                                 -                                                         -  
```
{: screen}

The host attach script for your location expires one year from the creation date. To make sure that hosts in your location don't have authentication issues, make sure to download a new copy of the host attach script at least once per year and update any unassigned hosts.
{: tsCauses}


To resolve this issue, remove the hosts from your location, download a new copy of the host attach script for your location, then reattach the hosts. Repeat the following steps for all unresponsive and unassigned hosts in your location.
{: tsResolve}

1. Remove the unresponsive hosts from your location.

    ```sh
    ibmcloud sat host rm --host HOST --location LOCATION
    ```
    {: pre}
    
2. Download a new copy of the host attach script for your location.

    ```sh
    ibmcloud sat host attach --location LOCATION [--host-label "LABEL"]  [--operating-system SYSTEM] [-q] [--reset-key]
    ```
    {: pre}

3. Reattach the hosts by running the attach script that you downloaded in the previous step. For more information, see [Attaching hosts to your location](/docs/satellite?topic=satellite-attach-hosts).


