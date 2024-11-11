---
copyright:
  years: 2019, 2024

lastupdated: "2024-11-11"

keywords: understanding infrastructure, vpc, classic infrastructure, cloud environment, infrastructure comparison, comparing infrastructures

subcollection: cloud-infrastructure

---

{{site.data.keyword.attribute-definition-list}}

# Comparing {{site.data.keyword.cloud_notm}} classic and VPC infrastructure environments
{: #compare-infrastructure}

Compare the key differences between {{site.data.keyword.cloud}} infrastructure environments to decide which one is best for your workloads and applications. Check out this [video](https://mediacenter.ibm.com/media/IBM%20Bare%20Metal%20Servers%20-%20Classic%20vs.%20VPC%20Infrastructure%20Explainer%20Video/1_hn1d69nn){: external} to learn more about the differences between the classic and VPC infrastructures.
{: shortdesc}

If you aren't familiar with the environment types, review the following descriptions.

* The classic infrastructure is the existing IaaS platform. This environment is best for lift and shift workloads so you can move applications quickly and keep the same architecture.
* VPC infrastructure is the new IaaS platform, based on software-defined networking and ideal for cloud-native applications.

## Compute differentiators
{: #compare-compute}

See the following table for the Compute differences between classic and VPC.

| Category   |  Classic infrastructure   | VPC infrastructure |
| ---------- | ------------------------- | ------------------ |
|  **Services**  | * {{site.data.keyword.BluVirtServers_short}} \n * {{site.data.keyword.baremetal_short}} \n * Dedicated hosts \n * Reserved virtual servers  | * {{site.data.keyword.BluVirtServers_short}} \n * {{site.data.keyword.baremetal_short}} \n * Dedicated hosts \n * Reservations for virtual servers and bare metal servers \n * Instance Metadata service \n * Instance templates \n * Instance groups \n * Auto Scale for VPC |
| **Performance and availability** | * Networking speeds up to 25 Gbps | * High speed networking up to 200 Gbps \n * Better availability achievable through zone architecture  |
| **Pricing** | * Hourly and monthly billing \n * Suspend billing feature for supported configurations | * Hourly and monthly billing \n * Suspend billing feature \n * Cost savings for reservations |
| **Bare Metal servers** |  Customizable servers featuring advanced Intel® Xeon® CPUs, or AMD CPUs, and NVIDIA GPUs | Profile-based servers featuring advanced Intel® Xeon® CPUs, DPU technology, rapid provisioning, and hourly billing  |
| **Virtual server profile families** | * Balanced \n * Balanced local storage \n * Variable compute \n * Compute \n * Memory \n * Transient option for supported profiles \n * GPU | * Balanced \n * Compute \n * Memory \n * Very High Memory \n * Ultra High Memory \n * GPU \n * AI Optimized \n * Storage Optimized \n * Confidential Compute \n * Generations of CPU profiles \n * Profiles for Intel, AMD, and s390x processor architectures  |
| **Supported images** | * Stock images \n * Instance add-ons: OS add-ons, control panel software, database software \n * Custom images | * Stock images \n * Custom images (includes image from volume) \n * Private catalog images |
| **Platform integration** | | IAM and resource group integration for a unified experience |
{: caption="Compute comparison" caption-side="bottom"}
{: summary="This table has row and column headers. The row headers identify possible features. The column headers identify the differentiators between classic infrastructure and VPC infrastructure. To understand the differences between environments, go to the row and find the details for the feature that you're interested in."}


## Network differentiators
{: #compare-network}

See the following table for the Networking differences between classic and VPC.

| Category   |  Classic infrastructure   | VPC infrastructure |
| ---------- | ------------------------- | ------------------ |
| **Location construct**    | Data centers and PODs \n (Might require VLAN spanning to connect two different pods or data centers, and purchasing gateways to control and route traffic) | Regional model that abstracts infrastructure so you don't need to worry about pod locations.|
| **Network functions and services** |Physical and virtual appliances from multiple vendors | Cloud-native network functions (VPNs, LBaaS) \n (VPC isolation, dedicated resources carved out of public cloud, with more options for VPNs, LBaaS, multiple vNIC instances, and larger subnet sizes) |
| **IP addresses** | IPv6 addresses supported | IPv4 addresses only |
| **Gateway routing** | Use a virtual or physical network appliance (Virtual Router Appliance, Vyatta, Juniper vSRX, Fortinet FSA) | Traffic routing is handled by public gateway and floating IP services |
| **Network address translation (NAT)** | Use a virtual or physical network appliance (Virtual Router Appliance, Vyatta, Juniper vSRX, Fortinet FSA) | Supported by the Bring-your-own-IP (BYOIP) functionality  |
| **IPsec Virtual Private Network (VPN)** | Use a virtual or physical network appliance (Virtual Router Appliance, Vyatta, Juniper vSRX, Fortinet FSA) | Supported by the VPN-as-a-service offering |
|  **Elastic load balancing** | Cloud Load Balancer  | Load Balancer for VPC |
| **Global load balancing**| Cloud Internet Services, Citrix Netscaler MPX | Cloud Internet Services |
|**Hybrid connectivity** | NAT solution to bridge between {{site.data.keyword.cloud}} and your IT environment | Bring your own private IP address without NAT or IPsec tunnels \n Note: You can enable your VPC to access classic infrastructure resources. |
{: caption="Network comparison" caption-side="top"}
{: summary="This table has row and column headers. The row headers identify possible features. The column headers identify the differentiators between classic infrastructure and VPC infrastructure. To understand the differences between environments, go to the row and find the details for the feature that you're interested in."}

## Storage differentiators
{: #compare-storage}

See the following table for the storage differences between classic and VPC.

|  Classic infrastructure   | VPC infrastructure |
| ------------------------- | ------------------ |
|Robust set of storage services, {{site.data.keyword.blockstorageshort}} (iSCSI), and {{site.data.keyword.filestorage_short}} (NFS-based) offerings. Server-side agent based Backup service with dedicated vault. \n - Snapshot support for both offerings.  \n - Cross-regional replication. \n - Adjustable IOPS and increasable capacity. \n - Encryption at rest with provider- or customer-managed encryption. \n - Volume duplication and data refresh from parent volume.| {{site.data.keyword.block_storage_is_short}} provides primary boot disks (with basic lifecycle management), and secondary data volumes. {{site.data.keyword.filestorage_vpc_short}} provides NFS-based file shares. \n - Snapshot and backup support for block volumes. \n - Zonal replication for file shares. \n - Adjustable IOPS and increasable capacity.  \n - Encryption at rest with provider- or customer-managed encryption. \n - Optional encryption in transit for block volumes and file shares. \n - Optional cross-account authorizations for encryption keys. |
{: caption="Storage comparison" caption-side="top"}

## Security differentiators
{: #compare-security}

See the following table for the security differences between classic and VPC.

|  Classic infrastructure   | VPC infrastructure |
| ---------- | ------------------------- |
|Vyatta, Fortigate, Juniper vSRX, Security Groups for virtual servers| Security groups, Network access control lists (ACLs)|
{: caption="Security comparison" caption-side="top"}

## API differentiators
{: #compare-apis}

See the following table for the API differences between classic and VPC.

|  Classic infrastructure   | VPC infrastructure |
| ------------------------- | ------------------ |
|Existing {{site.data.keyword.slapi_short}} (SLAPI)| New developer-friendly, REST-based API |
{: caption="API comparison" caption-side="top"}

## Next steps
{: #compare-nextsteps}

To review all the VPC infrastructure capabilities, see [About virtual private cloud](/docs/vpc?topic=vpc-about-vpc). To start exploring infrastructure overall, see [Building your infrastructure](/docs/overview?topic=overview-get-started-checklist).
