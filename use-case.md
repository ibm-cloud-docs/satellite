---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-07"

keywords: satellite, hybrid, multicloud, use case

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.satelliteshort}} use cases
{: #use-case}

The {{site.data.keyword.satellitelong_notm}} uses cases outlined here are hypotheses designed to help explore the solution’s applicability for areas within various industries. These hypotheses are based on the following cross-industry capabilities of {{site.data.keyword.satelliteshort}}.
{: shortdesc}

- Use {{site.data.keyword.cloud_notm}} services wherever they are needed.
    - Scenarios where data must be processed close to the source, such as IoT, data lakes, and analytics (to meet latency requirements).
    - Scenarios where restrictions are placed on cross-border data movement (countries without public cloud data centers). 
- Use {{site.data.keyword.cloud_notm}} services to assist operations on-premises or at remote locations.
    - Remotely delivered {{site.data.keyword.cloud_notm}} managed services is especially useful in supporting frequent upgrades and function modifications that require Kubernetes and {{site.data.keyword.redhat_openshift_notm}} skills. 

## Benefits of using {{site.data.keyword.satelliteshort}}
{: #benefits}

Review some key benefits for using {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

|Benefits|Description|
|----------|----------------------------------|
|Bring your own infrastructure to {{site.data.keyword.cloud_notm}}.|With {{site.data.keyword.satellitelong_notm}}, you can bring your own infrastructure that is in your on-premises data center, in public cloud providers, or in edge environments to {{site.data.keyword.cloud_notm}} by creating a {{site.data.keyword.satelliteshort}} location. With this setup, you can run your workloads in the physical location of your choice to meet legal requirements, compliance standards, data speeds, and network latency requirements while securely using {{site.data.keyword.cloud_notm}} services. For more information, see [Setting up {{site.data.keyword.satelliteshort}} locations](/docs/satellite?topic=satellite-locations). |
|Securely access {{site.data.keyword.cloud_notm}} services.|Every {{site.data.keyword.satelliteshort}} location is securely connected to {{site.data.keyword.cloud_notm}} through an encrypted TLS tunnel that is provided with the {{site.data.keyword.satelliteshort}} Link component. By using this TLS tunnel, you can securely access {{site.data.keyword.cloud_notm}} services in your location and use them with the same security and compliance standards as in {{site.data.keyword.cloud_notm}}.  For more information, see [Connecting {{site.data.keyword.satelliteshort}} locations with services outside of locations by using endpoints](/docs/satellite?topic=satellite-link-location-cloud).  |
|Control and monitor network traffic to services.|With {{site.data.keyword.satelliteshort}} Link, you can control network access to apps, services, and servers that run in {{site.data.keyword.cloud_notm}}, public cloud providers, or in your on-premises data center. Configure these apps, services, and servers as sources for your {{site.data.keyword.satelliteshort}} endpoints. All network traffic that is sent through an endpoint is automatically captured and can be reviewed by the user.  |
|Consistently deploy Kubernetes resources across multiple locations.|Use a single dashboard to manage the deployment of Kubernetes resources across cloud, on-premises, and edge environments with {{site.data.keyword.satelliteshort}} Config and gain global visibility into your apps and Kubernetes operations. For more information, see [Deploying Kubernetes resources across clusters](/docs/satellite?topic=satellite-setup-clusters-satconfig). |
|Centralize your monitoring and logging.|{{site.data.keyword.satellitelong_notm}} is integrated with {{site.data.keyword.mon_full_notm}}, {{site.data.keyword.la_full_notm}}, and {{site.data.keyword.at_full_notm}}. You can view the metrics and logs for the apps that run in your location, the {{site.data.keyword.cloud_notm}} services that you use, or the events that happen in your location from a single location. |
{: caption="Reasons to use Satellite." caption-side="top"}

For more information about {{site.data.keyword.satelliteshort}}, how it works and the service benefits, see the [{{site.data.keyword.satelliteshort}} product page](https://www.ibm.com/cloud/satellite){: external}.

## Common
{: #use-case-common}

This use case is applicable to all industries.
{: shortdesc}

### Deploying {{site.data.keyword.redhat_openshift_notm}} environment as a managed service wherever it is needed
{: #use-case-common1}

Frequent version upgrades and function modifications that require Kubernetes and {{site.data.keyword.redhat_openshift_notm}} skills are performed remotely through {{site.data.keyword.cloud_notm}} managed services. The same services can be deployed wherever they are needed to process data close to its source to reduce latency.
{: shortdesc}

Pain points:
:   It is hard to secure and maintain skills for running {{site.data.keyword.redhat_openshift_notm}} internally. There are limitations with where the services can be used as data must be loaded to cloud to use managed services.

{{site.data.keyword.satelliteshort}} enables:
:   Using remotely delivered IBM’s managed services eliminates the need to secure {{site.data.keyword.redhat_openshift_notm}} skills internally. You can deploy services wherever they are needed, on-premises or in remote locations.

## Finance
{: #use-case-finance}

These use cases are applicable to the finance industry.
{: shortdesc}

### Leveraging mainframe data for customers restricted from moving data to cloud
{: #use-case-finance1}

The on-premises platform is delivered remotely by {{site.data.keyword.cloud_notm}} as a managed service. {{site.data.keyword.cpd_short}} is run on this platform for analyzing and leveraging the mainframe data as is, on-premises. 
{: shortdesc}

Pain points:
:   Certain data cannot be placed on cloud for security reasons. Thus it is a challenge to maintain and manage data analytics systems.

{{site.data.keyword.satelliteshort}} enables:
:   Analytics platform that was previously only available on cloud to be deployed on-premises at a customer site to bring mainframe data to life.

### Providing a unified environment for developing, verifying, and running containers for customer on third-party clouds
{: #use-case-finance2}

{{site.data.keyword.cloud_notm}} Managed Services that are required for developing, testing, and running containers are deployed on third-party clouds for a unified container platform. {{site.data.keyword.redhat_openshift_notm}} and container application versions can be managed from one single location. 
{: shortdesc}

Pain points:
:   It is hard to operate {{site.data.keyword.redhat_openshift_notm}} across locations and environments. Middleware and application versions and releases are inconsistent across environments. 

{{site.data.keyword.satelliteshort}} enables: 
:   Central management of {{site.data.keyword.redhat_openshift_notm}} services that are deployed at various locations, ensuring that the services are run using consistent versions and releases.

### Extending {{site.data.keyword.cloud_notm}} security and compliance for customers maintaining cloud security
{: #use-case-finance3}

The security and compliance delivered by {{site.data.keyword.cloud_notm}} for Financial Services is extended to on-premises environments and third-party clouds to run multiple {{site.data.keyword.redhat_openshift_notm}} clusters securely. Adherence to security policies is automatically scanned and reported. 
{: shortdesc}

Pain points:
:   {{site.data.keyword.redhat_openshift_notm}} is built and run at various locations, making maintenance and management of security a challenge. It is difficult to apply the same security policies across various {{site.data.keyword.redhat_openshift_notm}} environments. 

{{site.data.keyword.satelliteshort}} enables: 
:   Central management of {{site.data.keyword.redhat_openshift_notm}} services deployed at various locations, ensuring that the same security policies are applied.

## Insurance
{: #use-case-insurance}

This use case is applicable to the insurance industry.
{: shortdesc}

### Analytics platform on-premises
{: #use-case-insurance1}

Streamline and enhance functions related to AI-driven analytics as well as data storage and updates through local data processing enabled by {{site.data.keyword.satelliteshort}}. These functions include knowledge searches on insurance sales terminals, customer data management, AI optical character recognition (OCR), product recommendations, quote creation, and maintenance efficiency improvement.
{: shortdesc}

Pain points
:   Staff use terminals at the insurance sales offices to deliver timely and suitable advice and to provide the right services to help address customer issues. They need a more sophisticated search system with appropriate up-to-date information to be able to discover relevant answers from the diverse range of content.

{{site.data.keyword.satelliteshort}} enables:
:   Sophisticated AI-driven knowledge search system at the customer site and data processing where the data resides, eliminating the need to move data to cloud. Data is processed where the data resides, eliminating the need to move data to cloud.

## Manufacturing
{: #use-case-manufacture}

These use cases are applicable to the manufacturing industry.
{: shortdesc}

### Internet of things (IoT) service platform 
{: #use-case-manufacture1}

An IoT service platform can be delivered on cloud or on-premises to meet the user’s (manufacturing plants) requirements. The IoT service platform can be distributed, managed, and controlled remotely for users (manufacturing plants) that are restricted from connecting to and using cloud. 
{: shortdesc}

Pain points:
:   For plants with stringent security network requirements, it is impossible to deliver IoT services in the same way as cloud. For these plants, services must be built separately.

{{site.data.keyword.satelliteshort}} enables:
:   IoT service platform that was previously only available on cloud to be deployed within the customer’s plant, benefiting from use of cloud-like services.

### Enabling the digital transformation of on-premises legacy systems at steel mills and other manufacturing plants 
{: #use-case-manufacture2}

Use of {{site.data.keyword.satelliteshort}} enables legacy host systems at each site to run on digital transformation service platforms built on containers through integration, with the added benefit of driving digital transformation at the isolated sites that have so far missed out on the advantages of cloud and container technology.
{: shortdesc}

Pain points:
:   Systems with strict on-premises requirements, such as z/HOST and sensitive data, prevent the move to cloud. Many applications cannot tolerate network latency. Lack of container skills in on-premises environment hampers digital transformation of site applications. 

{{site.data.keyword.satelliteshort}} enables:
:   Digital transformation service platform built on containers that was previously only available on cloud can now be deployed within the plant. It also enabled the delivery of cloud-like quality control, such as image analysis, and data lake related services at the plant.

### Establishing a service platform for point of sale (POS) or multi function printer (MFP) systems 
{: #use-case-manufacture3}

When providing platform-based management services and data services to store POS systems nationwide as well as MFPs in corporate offices, it can be difficult to integrate with these customer systems. Using {{site.data.keyword.satelliteshort}} enables platform services to be delivered to a site that meets customer requirements. 
{: shortdesc}

Pain points:
:   It is hard to build a service platform that can provide a function for collecting the latest data and other added value to deliver services leveraging data from POS and MFP equipment. Customers have unique requirements for their system environments, preventing cost effective rollout of the same services.

{{site.data.keyword.satelliteshort}} enables: 
:   It enables delivery of services to both cloud and on-premises environments according to customer requirements, making it easier to integrate with customer servers. Similar experience regardless of environment enables customers to distribute and manage applications using the latest microservices anytime.
 
### Accelerating the establishment of new global sites and plants 
{: #use-case-manufacture4}

When establishing a new global site, production line management systems and Supply Chain Management (SCM) systems used in existing plants can be distributed from cloud, thereby only needing servers at the new site. This has the benefit of accelerating system build and operations, while addressing the skill gap and security concerns.
{: shortdesc}

Pain points:
:   When establishing new global sites, a significant amount of time is spent on building server environments for each site, deploying applications and developing an operations structure. Lack of skills due to inability to secure staff in the new region delays going live. The common application cannot be moved to cloud due to latency and security issues. 

{{site.data.keyword.satelliteshort}} enables: 
:   It helps eliminate issues including skill shortage and acceleration of deployment and go-live when delivering applications to various global sites. Applications can be deployed and run on-premises, addressing security and latency concerns. With unified management, the same applications can be easily rolled out and operated.

### Using cloud for cost and speed when starting new services during development phase and move on-premises once established 
{: #use-case-manufacture5}

Using {{site.data.keyword.satelliteshort}} enables applications to be deployed in accordance with which phase the server or application is in. For example, platform applications can be easily deployed on-premises, following a growth in business or in response to heightened security risks. 
{: shortdesc}

Pain points:
:   Many businesses want to start small on cloud and move to on-premises after the business has grown to a certain scale. They do not want to be locked into a single vendor, as cloud applications cannot be moved on-premises. They need the flexibility to deploy to distributed environments including on-premises in response to the scale of business. 

{{site.data.keyword.satelliteshort}} enables: 
: Applications initially developed and delivered on cloud can be moved as-is on-premises. This enables distributed deployment in accordance with the scale of business and acceleration in business speed and mitigation of various risks.

### Deploying applications on-premises, only scaling out to cloud during peak times
{: #use-case-manufacture6}

Using {{site.data.keyword.satelliteshort}} enables applications to be deployed anywhere, on-premises or on cloud. This capability can be used to scale-out (autoscale) to cloud during peak times such as new product releases to optimize cost.
{: shortdesc}

Pain points:
:   Clients prefer to run on-premises from cost reduction perspective and only distribute resources to cloud during peak times. The application deployment experience is inconsistent between cloud and on-premises. Clients also want to use scalable containers on-premises.

{{site.data.keyword.satelliteshort}} enables: 
:   It enables delivery of container platforms that support DevOps and can be distributed anywhere. Same applications can be instantly moved to cloud only during peak times, optimizing costs.

## Distribution
{: #use-case-distribution}

This use case is applicable to the distribution industry.
{: shortdesc}

### Delivering and integrating store or warehouse digitization solutions for the distribution or retail sectors
{: #use-case-distribution1}

Use {{site.data.keyword.satelliteshort}} to deliver digitization solutions and integrate with store systems such as store computers and POS, warehouse systems. Leverage AI where applicable. 
{: shortdesc}

Pain points:
:  It is difficult to deliver and integrate store digitization solution. It is also hard to collect and leverage edge data. Data transfer and privacy concerns hamper the use of AI. 

{{site.data.keyword.satelliteshort}} enables: 
:   It enables easy delivery of store digitization solution as well as integration with POS data and store computers. IBM Edge Application Manager and Watson Machine Learning can be run on {{site.data.keyword.satelliteshort}}, allowing analytics at the source of data. Additionally, it allows use and management of container-based applications for retail stores and warehouses.

## Public sector or health 
{: #use-case-health}

This use case is applicable to the public sector or health industry.
{: shortdesc}

### On-premises service platform
{: #use-case-health1}

For hospitals that cannot readily connect to or use cloud, {{site.data.keyword.redhat_openshift_notm}} and {{site.data.keyword.cloud_notm}} services such as IBM Watson are deployed as managed services within the customer site. These services then become available to use within the hospital environment, while the platform is distributed, managed and controlled remotely. 
{: shortdesc}

Pain points:
:   It is difficult to use cloud services as hospital medical and patient records cannot be taken offsite from a security standpoint. It is also a challenge to meet data processing needs that will emerge with increased use of wireless technology and local 5G in medical equipment.

{{site.data.keyword.satelliteshort}} enables: 
:   Cloud platform services that were previously only available on cloud can be deployed and made available for use within the hospital environment.

## Communications 
{: #use-case-communications}

This use case is applicable to the communications industry.
{: shortdesc}

### Multi-access edge computing (MEC) platform support service 
{: #use-case-communications1}

Configure edge cloud distributed nationwide and use a single control point to remotely run and manage the platform for containerized applications and data.
{: shortdesc}

Pain points:
:   With the widespread adoption of 5G, increased edge cloud needs can be expected in the near future, where data must be processed close to users and devices. 

{{site.data.keyword.satelliteshort}} enables: 
:   Edge cloud can be deployed to where it is needed to deliver platforms for running innovative new services using AI. Managed services can remotely support the edge cloud platform.

## Utilities
{: #use-case-utilities}

This use case is applicable to the utilities industry.
{: shortdesc}

### Expanding the use of smart meters with {{site.data.keyword.satelliteshort}}
{: #use-case-utilities1}

Deploy customer applications and {{site.data.keyword.cloud_notm}} analytics services as managed services on the edge server that manages smart meters to not only manage electricity supply and demand but also deliver services tailored to specific regions and households. 
{: shortdesc}

Pain points:
:   With the deregulation of the electricity industry, power companies must focus on providing added value and not simply supplying power. Power companies want to effectively use data collected from smart meters and use the data as a basis for delivering a diverse range of services.

{{site.data.keyword.satelliteshort}} enables: 
:   The various data collected from smart meters can be used in real time by applications. Use of management-free {{site.data.keyword.cloud_notm}} services enables easier data processing. Applications distributed across multiple sites can be centrally managed.


