---


copyright:
  years: 2025, 2025
lastupdated: "2025-02-14"

keywords: satellite, hybrid, multicloud, hosts, ssh

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# Enabling SSH on Satellite hosts
{: #enabling-ssh-on-hosts}

In troubleshooting and debugging scenarios, you might need SSH access to your Satellite hosts either before or after you have assigned them to your control plane or a cluster.

For example, if you are having problems assigning hosts, you can enable a non-root user for SSH access to a host before you assign it to a cluster.

Review the following sections for steps on enabling SSH access to your hosts.

- [Enabling non-root SSH on RHCOS hosts before assignment](#non-root-ssh-before-coreos).
- [Enabling non-root SSH on RHEL hosts before assignment](#non-root-ssh-before-rhel).
- [Enabling root SSH on hosts after assignment](#root-ssh-after-assignemnt).



## Enabling non-root SSH on RHCOS hosts before assignment
{: #non-root-ssh-before-coreos}

Because root SSH access is typically disabled on CoreOS hosts and both `root` and `core` SSH access is disabled as part of the host assignment, you can grant SSH access by adding a new, non-root user that isn't the default `core` user.

These instructions describe how to enable SSH access for a new user on a Red Hat CoreOS host before assigning it to a cluster or the control plane.

Revert these steps and disable SSH access after you finish troubleshooting.
{: note}

1. Create a new `satellite` user in the host attach ignition file that you used when you created the CoreOS host. Edit `passwd` section of the ignition file as follows.

    ```json
        "passwd": {
            "users": [
                {
                    "name": "core",
                    "sshAuthorizedKeys": [
                        ""
                    ]
                },
                {
                    "name": "satellite",
                    "sshAuthorizedKeys": [
                        "ssh-rsa AAA... KEYNAME"
                    ],
                    "groups": [
                        "sudo"
                    ]
                }
            ]
        },
    ```
    {: codeblock}

1. Add an SSH public key as shown. To create an SSH key pair, you can run the `ssh-keygen`. For example:
    ```sh
    ssh-keygen -f ~/.ssh/sat-host-access -t rsa -b 4096 -C sat-host-access -P ''
    ```
    {: pre}
    

1. The `sat-host-access.pub` file has the key to add to the ignition file. Add the entire contents of the file to the ignition script by replacing `ssh-rsa AAA... KEYNAME` in the example above. The `sat-host-access` private key is what you use to SSH to the host.

Anyone with the private key (that corresponds to the public key you just added to this host) and network access to the nodes can SSH into this system.
{: note}



## Enabling non-root SSH on RHEL hosts before assignment
{: #non-root-ssh-before-rhel}

These instructions describe how to temporarily enable non-root SSH access to a RHEL 8 cluster worker node.

Revert these steps and disable SSH access after you finish troubleshooting.
{: note}

Before you begin, make sure you have root SSH access to the unassigned host.

1. If you want to use the same SSH key for this new user as the root user on this host, then you do not need another key. However, if you want to use a different SSH key pair for this new user then either have that key pair ready, or create a new SSH public/private key pair on whatever system you use to SSH to the host. Do **not** provide the private key to anyone. These instructions assume you created this new key pair.
    ```sh
    ssh-keygen -f ~/.ssh/sat-host-access -t rsa -b 4096 -C sat-host-access -P ''`
    ```
    {: pre}

Run the remaining steps on the host itself, so SSH into the host with root authority before continuing.

1. Create a user and configure it to have full sudo access without a password.
    ```sh
    useradd -U -m -s /bin/bash satellite && mkdir -p /home/satellite/.ssh && chown -R satellite:satellite /home/satellite/
    ```
    {: pre}

1. Add your SSH key to the new user.
    * If you want to use the same SSH key(s) for this user as for the root user, run the following command.
        ```sh
        cp -r /root/.ssh/authorized_keys /home/satellite/.ssh/
        ```
        {: pre}

    * If you created a new key pair, or have an existing public SSH key, then add it to the `satellite` user by running the following command.
        ```sh
        echo "<CONTENTS OF ~/.ssh/sat-host-access.pub OR YOUR OWN PUBLIC KEY>" >> /home/satellite/.ssh/authorized_keys && chmod 600 /home/satellite/.ssh/authorized_keys
        ```
        {: pre}

1. Run the following command.
    ```sh
    chown -R satellite:satellite /home/satellite/.ssh/authorized_keys
    ```
    {: pre}

1. To give this new `satellite` user full sudo access without a password, run the following command.
    ```sh
    echo "satellite ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
    ```
    {: pre}
    
    You can also configure the user to have more limited authority, configure that on the host instead. We recommend that for troubleshooting purposes you give this user full sudo access. Once you are done troubleshooting you can remove the user or limit its authority.

Now anyone with the private key that corresponds to the public key you just added to this host and network access to the nodes can SSH into this system with root authority.
{: important}

1. Log in to the node as the new user to verify it is working.
    ```sh
    ssh -i ~/.ssh/sat-host-access satellite@NODE-IP
    ```
    {: pre}

1. Then run the following command to verify you can get root authority from this user.
    ```sh
    sudo su - root
    ```
    {: pre}


## Enabling root SSH on hosts after assignment
{: #root-ssh-after-assignemnt}

Occasionally there might be a need to SSH directly into a host that is already assigned as a worker node in a cluster. For example, there might be a problem with cluster worker nodes losing connectivity to the cluster master. These instructions describe how to temporarily enable root SSH access to either a Red Hat CoreOS or RHEL 8 cluster worker node in a Satellite cluster.

Revert these steps and disable SSH access after you finish troubleshooting.
{: note}

Before you begin, make sure you have the following.
- Access to a system that has direct access to the cluster workers, to use as the SSH client system.
- Admin access to this cluster so you can run `oc debug node/NODE-NAME`.


1. Create an SSH public/private key pair on your SSH client. Don't give the private key to anyone.
    ```sh
    ssh-keygen -f ~/.ssh/sat-host-access -t rsa -b 4096 -C sat-host-access -P ''
    ```
    {: pre}

1. Verify the public key was created.
    ```sh
    cat ~/.ssh/sat-host-access.pub
    ```
    {: pre}

1. From a system that has access to the cluster master and has the admin `kubeconfig` access, run the following command.
    ```sh
    oc debug node/NODE-NAME
    ```
    {: pre}
    
1. Run the following commands from the shell on `NODE-NAME` to enable root SSH.
    ```sh
    chroot /host
    ```
    {: pre}

    ```sh
    echo "<CONTENTS OF ~/.ssh/sat-host-access.pub>" >> /root/.ssh/authorized_keys
    ```
    {: pre}

    ```sh
    chmod 600 /root/.ssh/authorized_keys
    ```
    {: pre}

1. Set an environment variable for your `SSHD_CONFIG_FILE` by using one of the following commands.
    Note that it could be in a different place depending on the exact worker version you have, and if so, set the `SSHD_CONFIG_FILE` environment variable to the full path of the `SSHD` config file so you can update it in the next few steps.
    {: note}

    Example command for CoreOS workers.
    ```sh
    export SSHD_CONFIG_FILE="/etc/ssh/sshd_config.d/40-rhcos-defaults.conf"
    ```
    {: pre}
    
    Example command for RHEL 8 workers.
    ```sh
    export SSHD_CONFIG_FILE="/etc/ssh/sshd_config"
    ```
    {: pre}

1. Check the `PermitRootLogin` setting.
    ```sh
    grep ^PermitRootLogin $SSHD_CONFIG_FILE
    ```
    {: pre}

1. Set `PermitRootLogin` to `yes`.
    ```sh
    sed -i "s/PermitRootLogin no/PermitRootLogin yes/g" $SSHD_CONFIG_FILE
    ```
    {: pre}

1. Verify the setting.
    ```sh
    grep ^PermitRootLogin $SSHD_CONFIG_FILE
    ```
    {: pre}

1. Run the following commands to restart and exit.
    ```ssh
    systemctl restart sshd
    ```
    {: pre}

    ```sh
    exit && exit
    ```
    {: pre}

At this point, root SSH access from the SSH client system is enabled by using the private key you created earlier. Note that anyone with this private key and access to the nodes can SSH into this system as root. Use the following command to SSH to the node: `ssh -i ~/.ssh/sat-host-access root@NODE-IP`.

