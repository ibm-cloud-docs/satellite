---

copyright:
  years: 2020, 2020
lastupdated: "2020-10-19"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Logging in to a host machine to debug
{: #ts-hosts-login}

You might need to log in to your host machine to debug a host issue further.
{: shortdesc}

You can only SSH into the machine if you did not assign the host to a cluster, or if the assignment failed. Otherwise, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host via SSH for security purposes. You can [remove the host](/docs/satellite?topic=satellite-hosts#host-remove) and reload the operating system to restore the ability to SSH into the machine.
{: note}

1.  Log in to the machine.
    ```
    ssh root@<IP_address>
    ```
    {: pre}
2.  Check the various log output files from the host registration and host bootstrapping processes. Replace `<filepath>` with the following files to check in order.
    ```
    tail <filepath>
    ```
    {: pre}
    1.  The `nohup.out` logs from the host registration attempt.
    2.  The `/var/log/firstboot.log` for the first bootstrapping attempt. If the host registration failed, you do not have this file.
    3.  The `/tmp/bootstrap/bootstrap_base.log` for the base bootstrapping process, if the first boot was unsuccessful. If the host registration failed, you do not have this file.
3.  Review the logs for errors. Some common errors include the following.
    <table summary="This table is read from left to right. The first column has the error message. The second column has the description of the how to resolve the error.">
    <caption>Common host machine registration and bootstrapping errors</caption>
    <thead>
    <th>Message</th>
    <th>Description</th>
    </thead>
    <tbody>
    <tr>
    <td><pre class="screen"><code>export HOME=/root
    HOME=/root
    '[' '!' -f /var/log/firstboot.flag ']'
    ~</code></pre></td>
    <td>The first boot did not complete successfully. Check the <code>/tmp/bootstrap/bootstrap_base.log</code> file and continue looking for errors.</td>
    </tr>
    <tr>
    <td><code>No package matching '\''container-selinux'\'' found available, installed or updated</code>.<br><br><code>No package rh-python36 available. Error: Nothing to do</code>.<br><br>(Note that the package name might be replaced with another package name.)</td>
    <td>See [Host registration script fails](/docs/satellite?topic=satellite-host-registration-script-fails).</td>
    </tr>
    <tr>
    <td><pre class="screen"><code>curl: (6) Could not resolve host: <URL>.com; Unknown error
    tar -xvf bootstrap.tar
    tar: This does not look like a tar archive
    tar: Exiting with failure status due to previous errors
    [[ -n ‘’ ]]
    echo ‘Failed to untar bootstrap.tar’
    Failed to untar bootstrap.tar
    + rm -rf /tmp/bootstrap</code></pre></td>
    <td>The machine cannot be reached on the network. Check that your machine meets the [minimum requirements for network connectivity](/docs/satellite?topic=satellite-host-reqs), [remove the host](/docs/satellite?topic=satellite-hosts#host-remove), and try to [add](/docs/satellite?topic=satellite-hosts#attach-hosts) and [assign](/docs/satellite?topic=satellite-hosts#host-assign) the host again. Alternatively, the infrastructure provider network might have issues, such as a failed connection. Consult the infrastructure provider documentation for further debugging steps.</td>
    </tr>
    </tbody>
    </table>
    