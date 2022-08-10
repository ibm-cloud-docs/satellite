---

copyright:
  years: 2022, 2022
lastupdated: "2022-08-10"

keywords: satellite, hybrid, multicloud, location, location removed, location deleted

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why was my location deleted?
{: #ts-location-gone}


Help! My location is gone.
{: tsSymptoms}

If you create a location, but do not assign any hosts to it, then it is deleted after 2 weeks. If you create a location and a location control plane, but all of the attached hosts become unavailable or unassigned for more than 2 weeks, the location is tagged for abandonment evaluation. If you do not change the state of your location within 45 days after it is tagged, your location is deleted.
{: tsCauses}

Create a new location. To prevent your location from being deleted, keep it in a `Normal` state by attaching and assigning your hosts in the location and location control plane.
{: tsResolve}

You can determine if your location is tagged by first looking for hosts that are associated with the location and then looking for when that host was last modified.

Run the **`sat hosts`** command to display a list of hosts for a location.

```sh
ibmcloud sat hosts ls --location LOCATION_ID
```
{: pre}

In this example output, all of the hosts for the location are in an `unassigned` state and assigned a `Reload required` status. This location is in the 45 day evaluation period.

```sh
Retrieving hosts...
OK
Name                             ID                     State        Status            Zone    Cluster   Worker ID   Worker IP   
sat-e2e-abcdl12345oi5kfg50fg-1   1abcde2f1e37c0adb123   unassigned   Reload Required   dal10   -         -           -   
sat-e2e-abcdl12345oi5kfg50fg-2   1abcde2fd14f1c1fb456   unassigned   Reload Required   dal10   -         -           -   
sat-e2e-abcdl12345oi5kfg50fg-3   1abcde2fe3bb455cc789   unassigned   Reload Required   dal10   -         -           -   
```
{: screen}

To find out when the evaluation period started for your location, run the **`host get`** command for one of your hosts and look for the `modifiedDate` field. This date indicates when the `Reload Required` status was set and the evaluation period started.

```sh
ibmcloud sathost get --location LOCATION --host HOST --output json
```
{: pre}

In this example output, the `modifiedDate` field shows a date of `2022-07-22T05:20:14+0000`. In 45 days from this date, your location is scheduled to be deleted.

```sh
    {
        "id": "344aec73e3bb455cca94",
        "name": "sat-e2e-cboul5110noi5kfg50fg-3",
        ....
        "state": "unassigned",
         ...
        "health": {
            "status": "reload-required",
            "message": "Reload Required",
            "modifiedDate": "2022-07-22T05:20:14+0000"
        }
    }
```
{: screen}







