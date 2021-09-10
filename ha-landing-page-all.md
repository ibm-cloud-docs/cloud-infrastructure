---

copyright: 
  years:  2021
lastupdated: "2021-08-18"

keywords: high availability, regions, zones, resiliency

services: virtual-servers, vpc, loadbalancer-service

account-plan: paid

subcollection: cloud-infrastructure

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}
{:new_window: target="_blank"}
{:step: data-tutorial-type='step'}

# About High Availability
{: #landing-about-ha-dr-backup}

With {{site.data.keyword.cloud}}, you can protect your critical workloads by building highly available infrastructure. In doing so, you can safeguard your classic or VPC application against a single point of failure, providing the highest level of uptime. The {{site.data.keyword.cloud_notm}} standardized user interface makes building resilient infrastructure as simple as provisioning a server. To further assist users with their deployment, Terraform scripts are provided to automate your deployment consistently across different projects or locations. Protect your critical workloads against outages by building resilient infrastructure on IBM cloud.
{: shortdesc} 

## What is resiliency
{: what-is-resiliency}

Resiliency enables your cloud solution to run at acceptable levels even if one or more areas of the infrastructure experiences a problem, for example due to power loss, planned or unplanned maintenance, or ransomware. A resilient solution can span across several key components and parts or all might apply to your business requirements, technical requirements or both:

*  High Availability - Systems that provide continuous service, often by using redundant components to protect against failure. Redundancy is using multiple identical resources in different locations to protect against a single point of failure. 
*  Back up - Continuous backup of critical data for recovery to continue to provide service if key data is lost or inaccessible.
*  Disaster Recovery - Recovering access to the infrastructure after extreme events such as natural disasters, cyberattacks, or wide range power outages.

## Regions and zones

![Levels of Resiliency](images/ha-resiliency-infographic.png){:caption="Figure 1. Levels of Resiliency" caption-side="bottom"}

The data centers are in different geographies, countries, and regions. Each region has one or more availability zones, which are specific physical locations. Whether the data centers are multi-zone (MZR) or not, all of them maintain multiple power feeds, fiber links, dedicated generators, and battery backup to avoid a single-point-of-failure (SPOF) between zones and regions. While all the data centers have multiple power feeds, several of the more mature sites such as AMS01, DAL05, 06, 08. FRA02. HKG02, MEX01, MIL01, PAR01, SJC01, SNG01, WDC01, and WDC03 have some 1U single socket server chassis that might not accommodate a dual power feed. If you have a 1U single socket server in one of these sites, you might want to consider a 2U chassis with redundant power supplies. For more information about availability zones, see [Locations for resource deployment](/docs/overview?topic=overview-locations).

The advantage of an MZR is that it provides:
*  Consistent cloud services across the different zones 
*  Better resiliency, availability, higher interconnect speed between data centers for cloud platforms
*  Infrastructure services such as IBM® Cloud Object Storage, and IBM Cloud load balancers 

These features can be critical to your applications. Deploying the application in an MZR rather than a single zone can increase the availability from 3 9’s to 4 9’s when deployed over three zones.

## High availability considerations for compute resources

Creating more than one virtual server instance helps with your high availability. However, when you layer in [placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc) to ensure that multiple virtual server instances are on different physical servers within a data center or availability zone, it further improves your overall redundancy and availability story. Furthermore, you can improve it more by spreading it across different availability zones. Your business applications continue to run and provide service to your customers even if one or more virtual server instances are lost.

As another additive, [autoscale](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=ui) helps optimize compute cost. You pay for what you need now and delete it when it is no longer needed. Auto scale allows you to scale the virtual server instance dynamically to ensure you have just enough capacity for the current demand.

## Using LBaaS to help increase resiliency

{{site.data.keyword.cloud_notm}} provides multiple network devices and connections, ensuring your servers and data storage components are always in contact with each other. When you are spreading your workload across multiple machines, considering using a load balancer. A load balancer provides a well-known destination to your application. It also provides redundancy and scaling.

IBM Cloud load balancer (LBaaS) is a network offering by IBM Cloud that can be used to increase resiliency within a region. To read more about the different types of LBaaS and capabilities, you can read it [here](/docs/vpc?topic=vpc-nlb-vs-elb). To increase resiliency across regions, then consider the global load-balancing functions of [Cloud Internet Services (CIS)](/docs/cis?topic=cis-configure-glb).

## Using snapshot to help with data resiliency

Some occurrences or events can put your business continuity at risk such as ransomware, data loss or corruption, accidental deletion, or catastrophic events. Backup should be part of your data resiliency plan to recover from these events. Backup is a point-in-time copy of your data from either your boot or data volumes. You can find more information about IBM Cloud backup capabilities [here](/docs/vpc?topic=vpc-snapshots-vpc-create#snapshots-vpc-create).

## High Availability components and tutorials

For more information about High Availability Reference Architecture solutions, see:
*  [High Availability infrastructure on IBM Classic](/docs/cloud-infrastructure?topic=cloud-infrastructure-ha-introduction)
*  [Building a highly available 3-tier web application in VPC](docs/cloud-infrastructure?topic=cloud-infrastructure-components-three-tier-architecture)
*  [Building a highly available 3-tier infrastructure in VPC](docs/cloud-infrastructure?topic=cloud-infrastructure-ha-3-tier)

For more information about High Availability how-to solutions, see: 
* [Building highly available and scale wep application in classic](/docs/cloud-infrastructure?topic=solution-tutorials-highly-available-and-scalable-web-application)
* [Enabling Auto Scale for better capacity and resiliency](/docs/cloud-infrastructure?topic=cloud-infrastructure-ha-auto-scale)
* [Building blue green (A/B) deployment in VPC](docs/cloud-infrastructure?topic=cloud-infrastructure-ha-pools-origins)
* [Deploying critical application in an MZR](docs/cloud-infrastructure?topic=cloud-infrastructure-multi-zone-resiliency)

For more information about Backup, see:
* [Backup reference architecture for 3-tier web application in VPC](docs/cloud-infrastructure?topic=cloud-infrastructure-regional-snapshots-3-tier-arch)
* [How-to guide for setting up snapshots for a 3-tier application in VPC](https://test.cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-create-three-tier-architecture)

For more information about automating your resilient High Availability solution with Terraform scripts, see:
*  [Creating a resilient three-tier highly available infrastructure VPC with Terraform](/docs/cloud-infrastructure?topic=cloud-infrastructure-create-three-tier-resilient-vpc)
*  [Creating a resilient three-tier highly available infrastructure VPC with Auto Scale by using Terraform](/docs/cloud-infrastructure?topic=cloud-infrastructure-create-three-tier-resilient-vpc-autoscale)
