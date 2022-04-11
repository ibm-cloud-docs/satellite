---

copyright:
  years: 2022, 2022
lastupdated: "2022-04-11"

keywords: satellite, networking, connectivity, hosts

subcollection: satellite, host check, setup

---

{{site.data.keyword.attribute-definition-list}}


# Checking your host setup
{: #host-network-check}

After you create hosts that meet the [system requirements](/docs/satellite?topic=satellite-host-reqs), [network requirements](/docs/satellite?topic=satellite-reqs-host-network), and the [outbound connectivity requirements](/docs/satellite?topic=satellite-reqs-host-network-outbound), you can verify your host setup by running the following `sat-host-check` script.

1. Log in to your host.
    ```sh
    ssh root@<IP_address>
    ```
    {: pre}
    
    1. For RHEL hosts, SSH as root.
        ```sh
        ssh root@<IP_address>
        ```
        {: pre}
        
    1. For RHCOS hosts, copy your public SSH key into your ignition script and log in as core.
        1. Download the host attach script and add your public SSH key.

            ```json
            {
              "ignition": {
                "version": "3.1.0"
              },
              "passwd": {
                "users": [
                  {
                    "name": "core",
                    "sshAuthorizedKeys": [ "PUBLIC-SSH-KEY" ]
                  }
                ]
              },
            ...
            }
            ```
            {: codeblock}

        1. Log in to your host as `core`.
            ```sh
            ssh core@<IP_address>
            ```
            {: pre}


1. Download the script and make it executable.
    ```sh
    curl https://origin.<region>.containers.cloud.ibm.com/satellite-health/sat-host-check -o sat-host-check && chmod +x sat-host-check
    ```
    {: pre}
    
    Example command for `us-south`.
    ```sh
    curl https://origin.us-south.containers.cloud.ibm.com/satellite-health/sat-host-check -o sat-host-check && chmod +x sat-host-check
    ```
    {: pre}
    
1. Run the script and specify the region where you created your location.
    ```sh
    ./sat-host-check --region $REGION
    ```
    {: pre}
    
    Example command for `us-south`.
    
    ```sh
    ./sat-host-check --region us-south
    ```
    {: pre}
    
    Example output for checking a host in `us-south`.
    ```sh
    ===========================
      TEST PHASE: redhatOS
    ===========================
    PASS:  custom Grub configuration (/boot/grub/grub.conf) not present
    PASS:  custom Network configuration (/sbin/ifup-local) not present
    PASS:  custom Network configuration (/sbin/ifdown-pre-local) not present
    PASS:  custom Network configuration (/sbin/ifdown-local) not present
    PASS:  custom NTP configuration (/etc/ntp.conf) not present
    === subscription-manager  ===
    STDOUT:
    Usage: subscription-manager MODULE-NAME [MODULE-OPTIONS] [--help]
    PASS:  subscription-manager configured
    === yum info rh-python36 ===
    ...
    ===========================
      TEST PHASE: network
    ===========================
    === ip addr ===
    ...
    FAIL:  wrong number of network interfaces (expected 2, found: 3: [1: lo, 2: eth0, 3: eth1])
    === ip route ===
    ...
    info:  no checks performed on 'ip route' output
    === Check connectivity to google.com:80 ===
    PASS:  got response back from google.com:80
    === Check connectivity to our region us-south ===
    === Check connectivity to hosted control plane ===
    === Check connectivity to 169.63.123.154:30000 ===
    PASS:  got response back from 169.63.123.154:30000
    === Check connectivity to 169.60.123.162:30000 ===
    PASS:  got response back from 169.60.123.162:30000
    === Check connectivity to 52.117.93.26:30000 ===
    PASS:  got response back from 52.117.93.26:30000
    === Check connectivity to 52.117.88.42:30000 ===
    PASS:  got response back from 52.117.88.42:30000
    === Check connectivity to 169.47.174.106:30000 ===
    PASS:  got response back from 169.47.174.106:30000
    === Check connectivity to 169.60.92.50:30000 ===
    PASS:  got response back from 169.60.92.50:30000
    === Check connectivity to 169.61.74.210:30000 ===
    PASS:  got response back from 169.61.74.210:30000
    === Check connectivity to 169.62.9.250:30000 ===
    PASS:  got response back from 169.62.9.250:30000
    === Check connectivity to 169.62.10.162:30000 ===
    PASS:  got response back from 169.62.10.162:30000
    === Check 443 ports ===
    === Check connectivity to 169.60.73.142:443 ===
    PASS:  got response back from 169.60.73.142:443
    === Check connectivity to 169.60.101.42:443 ===
    PASS:  got response back from 169.60.101.42:443
    === Check connectivity to 169.61.83.62:443 ===
    PASS:  got response back from 169.61.83.62:443
    === Check connectivity to 169.61.109.34:443 ===
    PASS:  got response back from 169.61.109.34:443
    === Check connectivity to 169.62.10.162:443 ===
    FAIL:  Could not create request: dial tcp 169.62.10.162:443: connect: connection refused
    === Check connectivity to 169.63.75.82:443 ===
    FAIL:  Could not create request: dial tcp 169.63.75.82:443: connect: connection refused
    === Check connectivity to 169.63.88.178:443 ===
    PASS:  got response back from 169.63.88.178:443
    === Check connectivity to 169.63.88.186:443 ===
    PASS:  got response back from 169.63.88.186:443
    === Check connectivity to 169.63.94.210:443 ===
    FAIL:  Could not create request: dial tcp 169.63.94.210:443: connect: connection refused
    === Check connectivity to 169.63.111.82:443 ===
    FAIL:  Could not create request: dial tcp 169.63.111.82:443: connect: connection refused
    === Check connectivity to 169.63.149.122:443 ===
    FAIL:  Could not create request: dial tcp 169.63.149.122:443: connect: connection refused
    === Check connectivity to 169.63.158.82:443 ===
    FAIL:  Could not create request: dial tcp 169.63.158.82:443: connect: connection refused
    === Check connectivity to 169.63.160.130:443 ===
    FAIL:  Could not create request: dial tcp 169.63.160.130:443: connect: connection refused
    === Check connectivity to link control plane ===
    === Test Failure Summary: ===
    redhatOS - error checking RHN configuration with `subscription-manager`: exit status 1
    network - wrong number of network interfaces (expected 2, found: 3: [1: lo, 2: eth0, 3: eth1])
    network - Could not create request: dial tcp 169.62.10.162:443: connect: connection refused
    network - Could not create request: dial tcp 169.63.75.82:443: connect: connection refused
    network - Could not create request: dial tcp 169.63.94.210:443: connect: connection refused
    network - Could not create request: dial tcp 169.63.111.82:443: connect: connection refused
    network - Could not create request: dial tcp 169.63.149.122:443: connect: connection refused
    network - Could not create request: dial tcp 169.63.158.82:443: connect: connection refused
    network - Could not create request: dial tcp 169.63.160.130:443: connect: connection refused
    cleaned up temp dir: `/tmp/sathostcheck-3139302841`
    ```
    {: screen}
    
1. Review the test failure summary. Depending on the failure, review the requirements and update your hosts.
    * [Host system requirements](/docs/satellite?topic=satellite-host-reqs)
    * [Host storage requirements](/docs/satellite?topic=satellite-reqs-host-storage)
    * [Host network requirements](/docs/satellite?topic=satellite-reqs-host-network)
    * [Host outbound connectivity requirements](/docs/satellite?topic=satellite-reqs-host-network-outbound) and the region-specific outbound connectivity requirements for the region where you created your location.

1. If the host check succeeds, you can continue attaching the host to your location.



