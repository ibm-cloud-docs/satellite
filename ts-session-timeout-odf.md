---


copyright:
  years: 2025, 2025
lastupdated: "2025-12-05"

keywords: satellite, hybrid, multicloud, odf, iam, session timeout

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do I see `unable to fetch header secret data` when creating a Satellite storage assignment?
{: #ts-session-timeout-odf}

When creating a Satellite storage assignment for storage services such as ODF, you see error messages similar to the following.
{: tsSymptoms}

```sh
Unable to fetch header secret data. { name: clustersubscription-111-secret, namespace: razeedeploy, key: razee-api-org-key }: secrets "clustersubscription-111-secret" is forbidden: User "IAM#111-111" cannot get resource "secrets" in API group "" in the namespace "razeedeploy"
```
{: screen}

When viewing pod logs, you see error messages similar to the following.
```sh
oc logs -f rook-ceph-osd-0-f1f1f11aa-aa1aa -c encryption-kms-get-kek
2025-11-19 03:58:11.158362 C | rookcmd: failed to get secret "ocs-deviceset-0-data-11aaaa": failed to get secret from ibm key protect: kp.Error: correlation_id='1111111f-d111-111e-bb3a-e11d1ba11111', msg='Unauthorized: Either the user does not have access to the specified resource, the resource does not exist, or the region is incorrectly set'
```
{: screen}

The session duration of your IAM dynamic access group or trusted profile session has expired. 
{: tsCauses}

The dynamic access group membership or trusted profile session expires after the number of hours that are specified in this property. For example, if the property is set to 24 hours, the user’s dynamic or trusted profile session ends one day (24 hours) after they log in.


To resolve the issue, choose from the following options:
{: tsFixes}

* Start a new log-in session.

* Update the session duration of your dynamic access group or trusted profile. For more information, see [Limiting access with time and resource attribute-based conditions](/docs/account?topic=account-iam-time-based&interface=ui).

