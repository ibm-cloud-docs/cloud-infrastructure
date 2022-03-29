---

copyright: 
  years:  2022
lastupdated: "2022-03-23"

keywords: high availability, regions, zones, resiliency, disaster recovery

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

# {{site.data.keyword.cloud_notm}} Disaster Recovery (DR) and Backup and Restore 
{: #ha-dr-backup-restore}

Local service interruption can be caused by many things: hardware failures, bugs, power failures, planned and unplanned maintenance. To minimize a single point of failure, {{site.data.keyword.cloud}} architects create resilient designs that have multiple virtual server instances to provide the highest availability. 
{: shortdesc}
 
However, extreme failure events, such as data center wide failures due to network outages, natural disasters, or ransomware are harder to anticipate and plan for. Your Business Continuity and Disaster Recovery (BCDR) plan becomes a critical part of recovering from these types of events. Business continuity focuses on the processes and procedures that you implement to help your people minimize interruptions to business operations. Disaster recovery focuses on the IT components and restoring systems after a disaster.
 
## Disaster recovery components 
{: #ha-dr-components}

The DR model has two main components that deal with what can be tolerated:   

* Recovery Time Objective (RTO) - the maximum time that the service can be down. 
* Recovery Point Objective (RPO) - the maximum amount of data loss.  

![DR backup diagram.](images/ha-DR-backup-diagram.svg){: caption="Figure 1. DR backup diagram" caption-side="bottom"}

These values are directly correlated to both cost and complexity. A lower amount of time and data loss equates to higher cost and complexity. 

## DR Strategies
{: #ha-dr-strategies}

You can implement your DR strategy in one of two ways:
 
|Strategy	|Description	|Benefits|
|-----|-----|-----|
|Cold (Active/Standby)	|With a Cold DR strategy, your solution is running in one environment and duplicate  resources are available on standby. When an event occurs, you react and build up the standby site.	|A cold strategy is a reactive approach, where the upfront cost is minimal, other than storage costs for snapshots and backups. The cold approach typically entails higher RPO/RTO times and is simple to implement.|
|Warm (Active/Active)	|With a warm DR strategy, you have a pre-built environment that is already built up at a different site. The VPC, with the appropriate subnets, VSIs, load balancers, securities policies are already up and running. At a minimum, data or DBs are regularly being backed up and ready to be exported over to the backup DR site. When an event happens, you switch over to the backup DR.	|The warm approach has a shorter RPO/RTO and a higher-cost point with all the pre-provisioned services up and active.|


## IBM Cloud VPC DR Considerations
{: #ha-vpc-dr-considerations}

Things you should consider when you are implementing your zonal DR:

|Situation   |Option   |Description   |
|------------|-------|--------|
|Restore and create resources with images   |Take regular Snapshots	|Use point-of-time snapshots to back up data on the boot and data volumes. The first snapshot is complete, and subsequent snapshots are incremental. The volume snapshots are stored in the regional IBM Cloud Object Storage buckets and are available only in the region where they were created. You can create new instances from a snapshot of a boot volume. You can attach data volumes to either a new or existing instance. For more information, see the Snapshot Overview.|
||Create a Custom Image	|If you are not taking snapshots of the boot volumes, create a custom image to reduce the provisioning time for instances. New virtual instances are built from a golden virtual instance, with preinstalled applications and configurations. |
|Streamline your recovery |Use hostnames for Subnets	|Subnets are zone-specific. Subnets do not extend across the three zones. This limitation can pose a challenge especially for zonal DR. New IP addresses are assigned to the new instances, which can break any existing configurations such as security rules, application configuration files. Use hostnames and DNS instead of IP addresses because the IP addresses must be updated as part of the post-provisioning process. Using hostnames and DNS can minimize the number of changes required.
||Use Terraform to create standby sites when needed	|Terraform is an open source infrastructure as a code tool that can help lessen the upfront cost and speed up the standing of the DR site. Create a script to monitor your production site. Then, based on specific events, the script can trigger the terraform script to start the automation and build out the new DR site. using scripts this way can eliminate the need of a warm standby site. Although the RPO/RTO is not as low, it is better than the cold approach with the added benefit of avoiding the cost of unutilized virtual instances

## Next Steps
{: #ha-dr-next-steps}

For more information, see [Regional snapshots in 3-tier architecture](/docs/cloud-infrastructure?topic=cloud-infrastructure-regional-snapshots-3-tier-arch)

