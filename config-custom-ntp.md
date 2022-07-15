---

copyright:
  years: 2022, 2022
lastupdated: "2022-07-15"

keywords: satellite, hybrid, multicloud, ntp, custom ntp, network time protocol

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}


# Specifying a custom Network Time Protocol (NTP) server
{: #config-custom-ntp}

Keeping time is an integral part of any system. You can choose to allow access to the [Red Hat network time protocol (NTP) servers](/docs/satellite?topic=satellite-reqs-host-network-outbound#host-out-ntp) or you can configure access to a custom Network Time Protocol (NTP) server.
{: shortdesc}

You can specify a custom NTP server for Red Hat CoreOS hosts only.
{: note}


## Specifying a custom NTP server for an unattached host
{: #custom-ntp-config}

To configure your hosts to use a custom NTP server before you attach them to a location, edit the host script to include your custom NTP server information. This example uses a `chrony` NTP server.

1. Create a `chrony.conf` file similar to the following example.
    
    ```txt
    pool time.adn.networklayer.com iburst

    server time.adn.networklayer.com iburst minpoll 3 maxpoll 4

    # Record the rate at which the system clock gains/losses time.
    driftfile /var/lib/chrony/drift

    # Allow the system clock to be stepped in the first three updates
    # if its offset is larger than 1 second.
    makestep 1.0 3

    # Enable kernel synchronization of the real-time clock (RTC).
    rtcsync

    # Specify file containing keys for NTP authentication.
    keyfile /etc/chrony.keys

    # Get TAI-UTC offset and leap seconds from the system tz database.
    leapsectz right/UTC

    # Specify directory for log files.
    logdir /var/log/chrony
    ```
    {: codeblock}
    
2. Convert the content to base64.

    ```sh
    cat chrony.conf | base64
    ```
    {: pre}

   
3. Download the attach script for your location.  For RHCOS hosts, the attach script is an ignition (.ign) script.

    ```sh
    ibmcloud sat host attach --location LOCATION --operating-system RHCOS
    ```
    {: pre}
    
4. Edit the script and add an entry to the `storage.files` array, where `BASE64_ENCODED_CHRONY_FILE_DATA` is the base 64 string from step 2.

    ```txt
    {
      "overwrite": true,
      "path": "/etc/chrony.conf",
      "contents": {
        "source":"data:text/plain;base64,BASE64_ENCODED_CHRONY_FILE_DATA"
        },
        "mode": 420
     }
     ```
     {: codeblock}
     
5. Save the script.
6. Validate the script.

    ```sh
    cat IGNITION_FILE_PATH | jq -r
    ```
    {: pre}
    
After your script is validated, you can [attach the hosts](/docs/satellite?topic=satellite-attach-hosts) to your location.

## Specifying a custom NTP for an attached host
{: #custom-ntp-attached-host}

You can configure a custom NTP server for your hosts after they are attached to the location.
{: shortdesc}



1. Create a `chrony.conf` file similar to the following example.
    
    ```txt
    pool time.adn.networklayer.com iburst

    server time.adn.networklayer.com iburst minpoll 3 maxpoll 4

    # Record the rate at which the system clock gains/losses time.
    driftfile /var/lib/chrony/drift

    # Allow the system clock to be stepped in the first three updates
    # if its offset is larger than 1 second.
    makestep 1.0 3

    # Enable kernel synchronization of the real-time clock (RTC).
    rtcsync

    # Specify file containing keys for NTP authentication.
    keyfile /etc/chrony.keys

    # Get TAI-UTC offset and leap seconds from the system tz database.
    leapsectz right/UTC

    # Specify directory for log files.
    logdir /var/log/chrony
    ```
    {: codeblock}
    
2. Log in to the host system.
3. Update the `chrony.conf` file for that host, where `CHRONY_FILE_DATA` is the name of the file you created in step 1. Do not convert this file into base64.

    ```sh
    cat >"/etc/chrony.conf" <<EOF
    CHRONY_FILE_DATA
    EOF
    systemctl restart chronyd
    ```
    {: pre}
      
After you update the `chrony.conf` file, your host uses your custom NTP server.
