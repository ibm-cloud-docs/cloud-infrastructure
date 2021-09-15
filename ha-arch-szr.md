---

copyright: 
  years:  2021
lastupdated: "2021-09-09"

keywords: high availability, regions, zones, resiliency

content-type: tutorial

services: virtual-servers, vpc, loadbalancer-service

account-plan: paid

completion-time: 60m

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

# Architecture for a resilient infrastructure for a single zone
{: #ref-arch-szr}

Deploying your application across the availability zone in a multiple-zone region provides the highest resiliency, 4 9's. However, there are scenarios where deploying across the zones is not wanted, such as needing the latency to be minimal or the application is deemed not critical.

## Virtual server instance Consideration
{: #ref-arch-szr-vsi}

Whatever the case might be, there are a few things to consider when you are designing for resiliency for a single zone. A single virtual server instance provides a 3 9's, but deploying multiple virtual server instances of the common workload can possibly increase it to 99.95%. It is recommended to deploy at least 3, even if only 2 are deployed. Placement groups should be used when you deploy multiple virtual server instances. While the {{site.data.keyword.cloud}} VSI scheduler attempts to place the virtual server instances on different hypervisors, the scheduler might not succeed. If they are not, then a single point of failure is introduced because the virtual server instances are placed on the same hypervisor.

By creating multiple virtual server instances that offer the same service and use placement groups, the application is better protected against hardware or software failures, and even for unplanned maintenance.

If the group of virtual server instances in the tier is less than 4, then use power spread. Anything greater than 4, use hypervisor spread.
{: note}

For more information about placement groups, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc).

Scaling should also be considered. While scaling may not directly impact the overall resiliency, it does impact the application availability where the virtual server instance resources are exhausted and are no longer able to handle the increased demand.

Scaling can be done multiple ways. For example, scaling up, where a higher profile is being provisioned from either a snapshot volume or building a new system. Another example, and probably the most effective, is using the Auto scale feature for virtual server instance. Auto scale can add or remove virtual server instances based on the current demand. This automatic scaling helps control cost, where you pay only what you need and delete any virtual server instance that is no longer required. For more information about Auto scale, see [Auto scale for VPC](/docs/vpc?topic=vpc-creating-auto-scale-instance-group&interface=ui#auto-scale-vpc).

## {{site.data.keyword.cloud_notm}} load balancer consideration
{: #ref-arch-szr-lb}

Adding a {{site.data.keyword.cloud_notm}} load balancer provides a single well-known destination and also spreads client requests or loads across 2 or more virtual server instances for better resiliency and improve performance. In addition, {{site.data.keyword.cloud_notm}} load balancers do health checks to ensure that only "healthy" virtual server instances receive the client request.

{{site.data.keyword.cloud_notm}} offers two types of load balancer-network load balancer and application load balancer. A network load balancer provides better latency and throughput. Whereas application load balancers distribute traffics based on layer 7 and layer 4 information. See [{{site.data.keyword.cloud_notm}} load balancer overview](/docs/vpc?topic=vpc-nlb-vs-elb) for side-by-side feature comparison to help you choose the right load balancer for your application.

## Backup Consideration
{: #ref-arch-backup}

Snapshots work as backups and are the first line of defense for data protection. While critical applications need a suite of HA and DR solutions, a non-mission critical and non-production system can rely on backup snapshots.

Snapshots can be taken both on bootable and secondary data volumes. If you experience accidental data deletion, you can create new data volumes from the backup snapshots and selected files can be copied over. If the snapshot was taken on a bootable volume, you can create another virtual server instance from the snapshot. For more information, see [Creating Snapshots](/docs/vpc?topic=vpc-snapshots-vpc-create).

## Additional Resources

*  [Step by step guide for single-zone](/docs/cloud-infrastructure?topic=cloud-infrastructure-deploy-n-tier-app-szr)
*  [Regional snapshots in 3-tier architecture](/docs/cloud-infrastructure?topic=cloud-infrastructure-regional-snapshots-3-tier-arch)
*  [Building a highly available infrastructure for 3-tier web applications in VPC](https://cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-ha-3-tier)
*  [Cloud reliability and disaster recovery architecture](https://www.ibm.com/cloud/architecture/architectures/resilience/)





