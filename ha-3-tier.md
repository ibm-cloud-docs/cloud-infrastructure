---

copyright:
  years: 2021
lastupdated: "2021-09-03"

keywords: high availability, regions, zones, resiliency

subcollection: cloud-infrastructure

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}

# Building a highly available infrastructure for 3-tier web application in VPC
{: #ha-3-tier}

This reference architecture describes three methods of deploying a 3-tier web application on {{site.data.keyword.vpc_full}}. This information applies to most 3-tier web applications that need to be resilient and highly available applications

|Tier|Description|
|---|---|
|The front-end web tier|The 1st- tier is usually the user interaction that takes in user input and parses the data to be handed off to the next tier.
|The application tier|The 2nd-tier takes the data from the web tier to process and apply logic. This action can include writing or pulling data from the data tier or providing results back to the web tier.
|The data tier|The 3rd-tier stores the application data that can be in written form or query form. The storage varies across different storage technologies, such as block volumes or object storage, depending on the application requirements.

## Deployment Considerations
{{site.data.keyword.vpc_short}} is only offered in multizone regions (MZR) which are made up of three or more zones. Each zone is isolated from each other to contain failures within a zone. The zones are also interconnected with a high-speed connection for higher bandwidth and lower latency for intra-zone communications.

## Component Considerations

### Virtual Servers

Servers in each of the 3-tiers have different requirements. When you provision your virtual servers, you have multiple options for virtual server profiles: Balanced, Compute, and Memory. The vCPU-to-memory ratio varies across the different profiles, but within the profile the ratio is constant. Within a profile, the number of vCPUs and memory increases along with the bandwidth performance. Use the following information when you are provisioning your servers.

|Tier|Server requirements|
|---|---|
|Front-end web tier|This tier tends to be higher in traffic workload. These workloads are more CPU-intensive than memory-intensive. So, you want to select a Compute profile that has a vCPU ratio to memory of 1:2. Within the Compute profile, select the appropriate type based on your vCPU and memory requirements.|
|Application tier|This tier tends to be more memory-intensive and involves more caching. For this workload, select a Memory profile where the ratio of vCPU-to-memory is more weighted to the memory, 1:8.|
|Data tier|If you are using a virtual instance as the database, select a Balance profile that offers more balance between vCPU and memory, such as 1:4.|

### Load balancers
You can order two types of load balancers from {{site.data.keyword.cloud}}. While both load balancers can distribute both HTTP/HTTPS and TCP traffic, each provides different capabilities that might be more suited for different deployment types.

1. You can deploy {{site.data.keyword.loadbalancer_full}} either in public or private scenarios. For a public scenario, the load balancer has a public interface to take incoming traffic from the internet and distribute the traffic through its private interface connecting to the virtual servers as shown in [Scenario 1 - deploying in single zone](#single-zone). For a private scenario, the incoming and distributed outgoing traffic goes through private subnets, such as the traffic between the application tier and the data tier.

  {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.loadbalancer_short}} also provides horizontal scaling to better handle increased traffic load. 

  You can configure the {{site.data.keyword.loadbalancer_short}} back-end listener across multiple zones. When you use this configuration, a secondary load balancer is automatically provisioned and deployed in an active-active fashion for better availability. See [Scenario 2 - deploying in multiple zones with {{site.data.keyword.cis_full_notm}}](#multi-zone-cis).

2. {{site.data.keyword.cis_full}} is a global load balancer that acts as a public load balancer only. Typically, it is deployed for global applications across multiple regions. {{site.data.keyword.cis_short}} is geo-aware and steers the traffic to the zone that is closest to the requester.

3. {{site.data.keyword.dns_full}} is a global load balancer that acts as a private load balancer within {{site.data.keyword.cloud_notm}}. {{site.data.keyword.dns_full}} provides Virtual Private Cloud (VPC) users with private DNS zones that are resolvable only on {{site.data.keyword.cloud_notm}}. Only explicitly permitted networks can access the private DNS zones. 

In [Scenario 2 - deploying in multiple zones with {{site.data.keyword.cis_full_notm}}](#multi-zone-cis), {{site.data.keyword.cis_short}} distributes traffic between zones. {{site.data.keyword.cis_short}} allows you to create multiple server pools and origins to allow a 1:N type of deployment – one application to multiple server pools and origins. One application that is deployed across multiple server pools is helpful for DevOps management.

### Storage
Storage can be a virtual server with block volumes or {{site.data.keyword.cos_full}}. If your application requires lower latency and higher IOPs, use a virtual server with block volumes. You need a 3rd-party tool to provide replication or snapshots. For unstructured data and high data volumes, use {{site.data.keyword.cos_short}}. The {{site.data.keyword.cos_full_notm}} service has different tiers where replication varies such as within a zone, across multiple zones, or across regions. The replication is not instantaneous, but occurs over time.

### Security

{{site.data.keyword.Bluemix_notm}} provides both security groups and firewall access control lists (ACL). For host-based security, consider using security groups to preserve the firewall ACLs. Firewall ACLs are best used when they are applying the same security rule at a subnet level rather than at the host level.

## VSI Considerations 
{: #vsi-considerations}

### Placement groups
{: #placement-groups}

Creating multiple instances of the same type gives you 2 things:

1.  Removes a single point of failure. If one instance goes down, then other instances are available to pick up the work and continue the service.
2.  Increases capacity. Parallelism allows multiple workers for higher output.

When you create multiple instances, the VSI scheduler makes every effort to place the VSIs on different hypervisors. And all hypervisors are installed on highly available servers (for example, redundant power supply, redundant network connections).  Even with all of these guard rails in place, there is still a possibility for a single point of failure if somehow all your VSIs are deployed on the same hypervisor.  A system crash will take down all your VSIs. So, creating multiple VSIs may not improve your resiliency as much as you would like or hope.

Spreading the VSIs across the zones does address this risk but what about multiple VSIs that are in the same zone? For example, suppose you have 5 VSIs that are dedicated to your web tier and are spread across three zones; 2 of the 3 zones have 2 VSIs. You want to ensure that the zones that have 2 VSIs are set up for the best availability. Granted you have other zones to protect you against failure in a zone, but it would be better to ensure the 2 VSIs are created on different hypervisors. That way, if the system or hypervisor goes down, the impact is smaller than if the 2 VSIs that were placed on the same hypervisor.

To avoid placing VSIs on the same hypervisor, placement groups come into play. Placement groups guarantee a group of VSIs are not placed on the same hypervisor.  

2 different placement algorithms are available for you to choose from:

*  Hypervisor spread - Ensures the VSIs are distributed across different hypervisors. There can be up to 12 VSIs in the placement group.
*  Power spread - Ensures the VSIs are placed not only different hypervisors but also the hypervisors are placed on different power and network, essentially on different racks. There can be up to 4 VSIs in the placement group.

Of the 2 options, the second option provides a better resiliency over the first option. 

![Placement Groups.](images/placement_groups.svg){: caption="Figure 1. Placement group algorithms " caption-side="bottom"}


If you have several VSIs, such as the web or applications, then hypervisor spread is sufficient. If you have a smaller set of VSIs, such as a database, where there are multiple masters or read replicas in the same zone, then power spread a better choice.

For more informationon Placement group, see [About Placement groups ](/docs/vpc?topic=vpc-about-placement-groups-for-vpc&interface=ui).

### Auto scale
{: #capacity-considerations}

When it comes to availability, customers tend to focus on failed or maintenance events because their services need to be available during any resource loss, whether the event is expected or unexpected. An often overlooked factor of resource availability is capacity. Capacity tends to be hard to plan for, as deployed resources need to meet the current demands while also handling spikes or seasonal load increases. The capacity must cover a wide range of load conditions while also balancing the economics for the workload.

If the current resources cannot handle the increased workload demands, then incoming transaction requests are at risk of being ignored or dropped, which in turn creates an availability problem.

Auto scale through instance group helps address these challenges by scaling resources up and down based on different load conditions. Not only that, Auto scale can be a cost optimization. With scaling policy, you save money by only paying for virtual servers when you need them and deleting them when you don’t.

Placement group can be used with auto scale. If at any point the (min or max) number of VSI is greater than 3, then placement group is recommended with auto scale.
{: note}

Auto scale adjusts resources in an automated and orchestrated fashion and is available in both compute and cloud load balancer configurations. 

* Compute – Scaling the virtual server instances is determined by the average target load utilization across all of the virtual server instances in the Auto scale group. Target load utilization can be based on CPU, memory, network, or a combination of these resources. If the virtual server instances load is not at the wanted target load, then instances automatically scale - up or down - to meet the average target load utilization. The number of virtual server instances that are being added or deleted will exceed the minimum or maximum virtual server instances for an Auto scale group. More information on Auto scale can be found [here](/docs/vpc?topic=vpc-creating-auto-scale-instance-group). 

![Autoscale.](images/autoscale-compute.svg){: caption="Figure 1. Auto scale Compute adds virtual servers based on load" caption-side="bottom"}

* Cloud load balancers – Auto scale for the cloud load balancer has two parts. The first part is the horizontal scaling of the cloud load balancer. The cloud load balancer scales depending on load so that it does not become the bottleneck. This process is managed by the IBM cloud controller. The second part is making the cloud load balancer aware of membership changes to the backend server pool. In a non-Auto scale environment, this adjustment is made by the customer manually; the customer must add or remove virtual instances from the backend server pool. With Auto scale, this adjustment is an automated process and removes the need for customer intervention. 

![Autoscale.](images/autoscale-cloud-load-balancer.svg){: caption="Figure 2. Auto scale Cloud Load Balancer distributes load to Auto scaled virtual servers" caption-side="bottom"}

## Scenario 1 - deploying in single zone
{: #single-zone}

Deploying your web application in only one of the three MZR zones of a VPC provides the best low latency solution because all of the services are in close proximity and contained within a single zone.

When deploying in a single zone, deploy multiple virtual instances within a tier for better availability and capacity. No load balancer is required.

This deployment type has some risk. While you deploy multiple virtual instances, the application is not protected against a failure within a data center. This becomes a single point of failure for the application. 

![Single-zone deployment.](images/single-tier.svg){: caption="Figure 3. Deploying in single zone" caption-side="bottom"}

## Scenario 2 - deploying in multiple zones with {{site.data.keyword.cis_full_notm}}
{: #multi-zone-cis}

You can deploy your application as active-active across multiple zones by using a combination of {{site.data.keyword.cis_full_notm}} and {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.loadbalancer_short}}. This method enables replication across multiple zones, increasing the availability of the application. Because of the active-active deployment, more devices are available for parallel processing, improving resiliency and performance. The likelihood of simultaneous failure across multiple zones is much less likely in comparison to a single zone. If a failure occurs in one of the zones, the failure is contained in that zone. Even though the capacity is reduced during a failure, the application is still active in the other zone.

![Multiple-zone deployment.](images/multi-zone-cis.svg){: caption="Figure 4. Deploying in multiple zones with IBM Cloud Internet Services and IBM Cloud Load Balancer" caption-side="bottom"}

By deploying across multiple zones you can also take advantage of server pools and origins, which are part of {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.cis_short}}. allow DevOps to integrate, validate, and deploy new software features. **More information is covered in a later section of this document.**

## Scenario 3 - deploying in multiple zones with {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.loadbalancer_short}}

You can also deploy your application in multiple zones by using only {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.loadbalancer_short}} rather than a combination of {{site.data.keyword.cis_full_notm}} and {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.loadbalancer_short}}. This deployment scenario is less ideal, but is more cost-effective. You should consider this method only for less critical and less sensitive applications, such as internal applications.

![Multiple-zone deployment.](images/multi-zone-lb.svg){: caption="Figure 5. Deploying in multiple zones with {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.loadbalancer_short}} " caption-side="bottom"}

## Reference
For more {{site.data.keyword.Bluemix_notm}} high availability content, see:
* [Cloud reliability and disaster recovery architecture](https://www.ibm.com/garage/method/practices/run/cloud-platform-for-ha).
* [Application-level high availability](https://www.ibm.com/cloud/architecture/architectures/resilience/overview).
* [Deploying server pools and origins in a single region](/docs/cloud-infrastructure?topic=cloud-infrastructure-ha-pools-origins)
* [Enabling Auto scale for better capacity and resiliency](/docs/cloud-infrastructure?topic=cloud-infrastructure-ha-auto-scale).
* [Deploying critical applications with IBM Cloud MZR](/docs/cloud-infrastructure?topic=cloud-infrastructure-multi-zone-resiliency) using the {{site.data.keyword.Bluemix_notm}} console.
*  Configuration and script files for 3-tier applications are available on the [vpc-ha-iac GitHub repository](https://github.com/IBM-Cloud/vpc-ha-iac).
*  [Creating a resilient three-tier highly available infrastructure VPC with Terraform](/docs/cloud-infrastructure?topic=cloud-infrastructure-create-three-tier-resilient-vpc).
*  [Creating a resilient three-tier highly available infrastructure VPC with Autoscale by using Terraform](/docs/cloud-infrastructure?topic=cloud-infrastructure-create-three-tier-resilient-vpc-autoscale).
