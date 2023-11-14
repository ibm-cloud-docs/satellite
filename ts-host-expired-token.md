---

copyright:
  years: 2020, 2023
lastupdated: "2023-11-14"

keywords: satellite, hybrid, multicloud, registration script, registration script fails

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my host attach failing with error message `A0029` `Access denied to specified controller`?
{: #ts-host-expired-token}


When you run the attach host script, your hosts are not attaching and you get a error message similar to the following example.
{: tsSymptoms}

```sh
Nov 07 18:19:56 tl2vdu ibm-host-attach.sh[5562]: + echo '{"incidentID":"a11babcd-202d-4c97-111a-1111abdcdf11","code":"A0029","description":"Access denied to specified controller.","type":"Authentication"}'
Nov 07 18:19:56 tl2vdu ibm-host-attach.sh[5562]: {"incidentID":"f97fabec-602d-4c97-946a-8699ccdcdf75","code":"A0029","description":"Access denied to specified controller.","type":"Authentication"}
Nov 07 18:19:56 tl2vdu ibm-host-attach.sh[5562]: + echo 401
Nov 07 18:19:56 tl2vdu ibm-host-attach.sh[5562]: 401
Nov 07 18:19:56 tl2vdu ibm-host-attach.sh[5562]: + [[ 401 -ne 201 ]]
Nov 07 18:19:56 tl2vdu ibm-host-attach.sh[5562]: + echo 'Error [HTTP status: 401]'
Nov 07 18:19:56 tl2vdu ibm-host-attach.sh[5562]: Error [HTTP status: 401]
Nov 07 18:19:56 tl2vdu ibm-host-attach.sh[5562]: + exit 1
Nov 07 18:19:56 tl2vdu systemd[1]: ibm-host-attach.service: Main process exited, code=exited, status=1/FAILURE
Nov 07 18:19:56 tl2vdu systemd[1]: ibm-host-attach.service: Failed with result 'exit-code'.
```
{: screen}


Your host attach script is older than one year and the `HOST_QUEUE_TOKEN` value expired.
{: tsCauses}


[Download a new copy](/docs/satellite?topic=satellite-host-attach-download) of your host attach script or ignition file from your location.
{: tsResolve}

When your host attach script expires, your unassigned hosts enter a `Unresponsive` status. For more information, see [Why do my unassigned hosts have an `Unresponsive` status](/docs/satellite?topic=satellite-ts-host-unassigned-unknown)?
{: note}

