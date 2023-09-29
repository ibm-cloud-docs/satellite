---

copyright:
  years: 2023, 2023
lastupdated: "2023-09-29"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why is my Connector Agent unable to establish the tunnel with {{site.data.keyword.cloud_notm}}?
{: #ts-connector-tunnel}


Your Connector Agent cannot establish the tunnel with {{site.data.keyword.cloud_notm}}. When you check your Connector Agent Docker logs, you see error messages similar to the following example.
{: tsSymptoms}

```sh
{"level":30,"time":1685976939200,"pid":13,"hostname":"2f07087dc304","name":"connector-agent","msgid":"LA2","msg":"Satellite Connector Agent starting up for U2F0ZWxsaXRlQ29ubmVjdG9yOiJjaHV1NTJpMjFuNzlxZm5sNmFwZyI in region us-south, release info: 20230428-7ec78c1a8051847b20af1d0202362da4af1abe68_A."}
{"level":30,"time":1685976939202,"pid":13,"hostname":"2f07087dc304","name":"tOps","msg":"retrive from https://c-01-ws.us-south.link.satellite.test.cloud.ibm.com/iam"}
{"level":30,"time":1685976939593,"pid":13,"hostname":"2f07087dc304","name":"tunneldns","msgid":"D04","msg":"doTunnelDNSLookup DNS resolve c-01-ws.us-south.link.satellite.test.cloud.ibm.com to 169.46.14.242"}
{"level":60,"time":1685977059299,"pid":13,"hostname":"2f07087dc304","name":"tOps","msgid":"O03-703","msg":"getToken error","err":{"type":"TimeoutError","message":"Timeout awaiting 'request' for 120000ms","stack":"RequestError: Timeout awaiting 'request' for 120000ms\n    at ClientRequest.<anonymous> (/connector_agent/node_modules/got/dist/source/core/index.js:970:65)\n    at Object.onceWrapper (node:events:628:26)\n    at ClientRequest.emit (node:events:525:35)\n    at origin.emit (/connector_agent/node_modules/@szmarczak/http-timer/dist/source/index.js:43:20)\n    at TLSSocket.socketErrorListener (node:_http_client:502:9)\n    at TLSSocket.emit (node:events:513:28)\n    at emitErrorNT (node:internal/streams/destroy:151:8)\n    at emitErrorCloseNT (node:internal/streams/destroy:116:3)\n    at process.processTicksAndRejections (node:internal/process/task_queues:82:21)\n    at Timeout.timeoutHandler [as _onTimeout] (/connector_agent/node_modules/got/dist/source/core/utils/timed-out.js:36:25)\n    at listOnTimeout (node:internal/timers:571:11)\n    at process.processTimers (node:internal/timers:512:7)","name":"TimeoutError","code":"ETIMEDOUT","timings":{"start":1685976939285,"socket":1685976939291,"lookup":1685976939595,"error":1685977059291,"phases":{"wait":6,"dns":304,"total":120006}},"event":"request"},"msg":"Timeout awaiting 'request' for 120000ms"}
{"level":50,"time":1685977059302,"pid":13,"hostname":"2f07087dc304","name":"utilities","msg":"makeLinkAPICall GET /v1/connectors/U2F0ZWxsaXRlQ29ubmVjdG9yOiJjaHV1NTJpMjFuNzlxZm5sNmFwZyI failed to get token to run"}
{"level":60,"time":1685977059303,"pid":13,"hostname":"2f07087dc304","name":"agent_tunnel","msgid":"LAT06","msg":"Failed to get configuration from API, will exit in 200s"}
```
{: screen}  

Your Connector Agent Docker container was unable to connect to the {{site.data.keyword.cloud_notm}} Connector servers, because of a firewall, gateway, or proxy configuration that is blocking public outbound access to {{site.data.keyword.cloud_notm}} in your environment.
{: tsCauses}

Make sure the the machine that is running the Connector Agent has outbound connectivity to {{site.data.keyword.cloud_notm}} and can resolve those hostnames. For more information, see the network connectivity requirements documented in [Minimum requirements](/docs/satellite?topic=satellite-understand-connectors#min-requirements).
{: tsResolve}




