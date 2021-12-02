<staging-uc>---

copyright:
  years: 2020, 2020
lastupdated: "2020-09-04"

keywords: satellite, hybrid, multicloud, distributed cloud, use cases, scenarios, case study

subcollection: satellite

---

{{site.data.keyword.attribute-definition-list}}

# WIP - Use cases for {{site.data.keyword.satellitelong_notm}}
{: #uc_intro}

{{site.data.keyword.satellitelong}} is for your on-premises and cloud native workloads, and it can be tailored to meet your industry's best practices and your organization's use cases. Even though each use case is presented through the lens of a particular industry, these workloads represent business needs that are common across various industries. 
{: shortdesc}

You see workload themes, such as:
- Data latency and sovereignty
- DevOps and continuous delivery
- Security and resiliency


## Financial services: Payment systems in multiple countries
{: #financial}

Context
:   A common use case comes from the financial services and banking industry. This industry is incredibly competitive and utilizes technology as a differentiator. A CTO for a global bank has hundreds of applications that are critical to the operations of the bank. And, her developers need to invent new ways to meet the bank clients' needs. The public cloud has allowed her team to focus on differentiated capabilities for the bank instead of on deploying and operating software.  However, as a highly regulated and security conscious industry with a long IT history, not everything the CTO has can be deployed to the public cloud. What she really need is to be able to have the benefits and agility of cloud services available consistently across her public cloud and in her data centers. That way her developers can move fast no matter where they need to deploy.

Concerns for the CTO
:   - Demand for new features requires the agility and speed of public cloud.
    - Regulations, policies, and latency keep backend systems on-prem.
    - Fragmentation on multiple clouds isolates workloads, locking up value of under-used resources.

The solution

Unlike some other credit card companies, the bank's credit business model includes making their payment systems available as a platform for partners around the world. In the major markets, that payment platform can be deployed and operated on the public cloud quickly and scalably. The challenge CTO faces is that some of their partners are not in major markets. In places like Russia, Vietnam, and smaller countries, there is a not a public cloud footprint for her to leverage. And because of regulation, those countries often require that the financial data their company is processing must remain within the country. 

OpenShift
:   One part of the solution is to build and deploy their payment applications with Red Hat OpenShift. Leveraging the benefits of Linux and Kubernetes, OpenShift abstracts the application from the underlying infrastructure and enables the payment application to be developed in a common way. 

Satellite locations
:   But this solution still leaves how to operate the application in each location. The bank can find infrastructure in each country. But finding the resources necessary to deploy and manage OpenShift and their application is a challenge. With {{site.data.keyword.satellitelong_notm}}, the bank can establish a Satellite location in each of the countries they're partnering within. That location becomes an extension of {{site.data.keyword.IBM_notm}} public cloud and enables the bank to have an {{site.data.keyword.openshiftshort}} footprint in country along with the databases and other services the application needs. That environment is fully managed as a cloud service. 

Single dashboard across environments
:   The bank's payments team can deploy their application themselves, manage it from a common dashboard on {{site.data.keyword.cloud_notm}}. {{site.data.keyword.IBM_notm}} handles operation of all of the services from the cloud, reducing the need for in-country skilled resource. {{site.data.keyword.satelliteshort}} configuration management enables the bank to keep the applications up to date and properly configured across hundreds of instances easily. And {{site.data.keyword.satelliteshort}}'s connection to {{site.data.keyword.cloud_notm}} security, including encryption, makes the data is safe and completely controlled by the bank's team. 


## Telecommunications: Low latency for local data processing
{: #telecom}

Context
:   The Head of App Development for a telecommunications company is building and running applications with mobile broadband, URLLC, and mMTC requirements. Thus, some of her applications need to remain near the edge for latency and data residency. That complexity means she has technology partners across the globe, who expect familiar tools. Her 5G networks projects are already being developed with containerized network functions (CNF) and can represent workloads such as enhanced mobile broadband (eMBB), ultra-reliable low latency communications (URLLC) and massive machine type communications (mMTC). 

The solution

Low latency
:   Communications service providers (CSPs) and telecoms use Cloud Satellite because it lets them place workloads with direct access to the low-latency of 5G for business-to-business applications, such as faster connectivity of devices in manufacturing and healthcare, in addition to consumer applications. Examples of latency solutions:

    - Instant customization: Customers expect instant, customized reports but complex financial modeling creates latency
    - Data processing happens close to the data, alleviating latency even when using predictive AI analytics 

Familiar {{site.data.keyword.cloud_notm}} services
:   The CSPs have additional cloud-native needs because they partner with network function vendors, network compute and storage providers, and IT providers like {{site.data.keyword.IBM_notm}}. That complex partner set means that they need a common set of cloud-native tools, ones rooted in open-source for portability and consistency. To enforce consistency, {{site.data.keyword.satelliteshort}} delivers the common cloud-based platform to bring together various partners who deliver the telecom its network solution. 

## Banking: New sites for tellerless banks
{: #bank}

Context
:   A VP of Application Development at a global bank is facing the new workloads of next-gen banking: “tellerless” banks. These workloads must be operated at bank data centers, while the main next-gen back office system is run and deployed in a public cloud that is built for financial services. 

:   You might have heard about “robo-branches” -- ones with limited if any human staff onsite. The VP's development teams are creating robo-branch apps that are architected with microservices and containers, giving them better compute usage than before.  The microservices approach also gives them better up-time with the simplification of app maintenance and evolution due to atomizing of functions. Examples of what must run in the branch include scanning check deposits, PIN authentication (no driver’s license needed), and on-the-spot replacement of lost ATM cards. 

Concerns for the VP
:   - Multiple tools with multiple logins
    - No push-button way to deploy across clouds and on-prem
    - Automation is hand coded and fragile due to environmental dependencies

The solution

Consistency across teams and clouds
:   In this banking use case, the application teams back in London have to deal with development environments at the sites that are different than the cloud they use for their digital platform. What the company needs is the services they use in {{site.data.keyword.IBM_notm}} public cloud to also run as-a-service in the sites. Then London team start to build a similar operations and user experience across geographies and across on-prem and clouds. 

Less operational chores
:   When the VP established a Satellite location in each site, by using the infrastructure already present in the site, the App Development team can spend less time with life cycle chores like upgrades and security patching. 

Auditable inventory
:   Satellite Config enables her teams to gain visibility into the version of their apps they are running in each location and ensure they are updated and configured consistently. 


## Retail: Accelerating direct-to-consumer locations
{: #retail}

Context
:   A VP of Engineering for a multinational retail chain has a team that is spending too much time managing software in each geography, especially emerging and smaller markets. As a multinational retail chain, they have regional retail outlets and distribution centers all over the world, including pop-up shops. Due to latency and the velocity of the retail industry, they need some applications to run locally in each geography. 

:   The retail chain's consumers want modern, seamless experience, online-to-offline, so they're accelerating opening additional smaller format stores that are meant for customers to do things such as pick up their online orders. And thus, availability of the company's digital platform and continuity of operations is paramount. To ensure their Digital platform is available 100% of the time, the company relies on a skilled team of engineers in their Tokyo IT headquarters and the {{site.data.keyword.IBM_notm}} Public cloud. The challenge they face is how to accomplish the same thing within their physical sites. The sites are spread across the world. Due to the critical need for continuity of operations in the face of any issue, the IT systems that support site operations need to reside in the site itself. The site needs to be able to be self-sustaining even if its networks are disconnected from the rest of the world. This has resulted in the company needing to run IT infrastructure and applications in the sites. But that presents them with a problem. It is challenging to maintain those applications in all the locations. They struggle to attract and retain talent that can operate the site-specific apps given the site locations and work environment.

Concerns for the VP
:   - Availability of the company's digital platform 
    - Self-sustaining platform even if its networks are temporarily disconnected from the rest of the world
    - Struggle to attract and retain talent that can operate the site-specific apps

The solution

Cloud services everywhere
:   With {{site.data.keyword.satelliteshort}}, the VP's team offloads running the PaaS services to {{site.data.keyword.IBM_notm}} , so they can focus on their core business logic. Now the engineering team doesn't need as much skilled staff in the site itself. And their developers can build and operate their solutions the same way they do the digital platform. 

Locations where needed
:   Their {{site.data.keyword.satelliteshort}} locations in each site use pre-existing infrastructure, giving the VP choices for compute size, network connectivity, resource allocation. 

Availability with visibility
:   Integrated monitoring and logging tools are easily accessed from the {{site.data.keyword.cloud_notm}} console. The DevOps team can use built-in automation and centralized tools to ensure resiliency. At the same time, {{site.data.keyword.satelliteshort}} Config enables the team to see which versions of their apps are running in each location and ensure they are updated and configured consistently. In end, {{site.data.keyword.satelliteshort}} enables the company to achieve their business continuity goals with consistent operations and as-a-service SLAs. 

</staging-uc>


