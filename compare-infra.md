---
copyright:
  years: 2019, 2023

lastupdated: "2023-11-15"

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
|  **Services**  |Full catalog of services, such as {{site.data.keyword.baremetal_short}}, {{site.data.keyword.BluVirtServers_short}} instances, VMware, SAP | {{site.data.keyword.BluVirtServers_short}} and Bare Metal Servers |
| **Performance and availability** | | Better availability is achievable through zone architecture |
| **Pricing** | Hourly and monthly billing, plus suspend billing features | Hourly, suspend billing, and sustained usage discount |
| **Virtual server families** | Public, dedicated, transient, reserved | Public, dedicated |
| **Profiles** | All profiles, including the GPU profiles | Balanced, compute, memory profiles with higher RAM and vCPU options |
| **Supported images** | Full set of pre-stock images, plus custom images | Limited set of pre-stock images, plus the ability to import a custom image |
| **Platform integration** | | IAM and resource group integration for a unified experience |
{: caption="Table 1. Compute comparison" caption-side="top"}
{: summary="This table has row and column headers. The row headers identify possible features. The column headers identify the differentiators between classic infrastructure and VPC infrastructure. To understand the differences between environments, go to the row and find the details for the feature that you're interested in."}

Suspend billing supports only hourly, SAN instances that are provisioned with a public profile from one of the Balanced, Compute, Memory, or Variable compute families.
{: note}

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
{: caption="Table 2. Network comparison" caption-side="top"}
{: summary="This table has row and column headers. The row headers identify possible features. The column headers identify the differentiators between classic infrastructure and VPC infrastructure. To understand the differences between environments, go to the row and find the details for the feature that you're interested in."}

## Storage differentiators
{: #compare-storage}

See the following table for the storage differences between classic and VPC.

|  Classic infrastructure   | VPC infrastructure |
| ------------------------- | ------------------ |
|Robust set of storage services, {{site.data.keyword.blockstorageshort}} (iSCSI), and {{site.data.keyword.filestorage_short}} (NFS-based) offerings. Server-side agent based Backup service with dedicated vault. \n - Snapshot support for both offerings.  \n - Cross-regional replication. \n - Adjustable IOPS and increasable capacity. \n - Volume duplication and data refresh from parent volume. \n - Encryption at rest with provide- or customer-managed encryption. | {{site.data.keyword.block_storage_is_short}} provides primary boot disks (with basic lifecycle management), and secondary data volumes. {{site.data.keyword.filestorage_vpc_short}} provides NFS-based file shares. \n - Snapshot and backup support for block volumes. \n - Zonal replication for file shares. \n - Adjustable IOPS and increasable capacity. \n - Encryption-in-transit for file shares. \ - Encryption at rest with provide- or customer-managed encryption. |
{: caption="Table 3. Storage comparison" caption-side="top"}

## Security differentiators
{: #compare-security}

See the following table for the security differences between classic and VPC.

|  Classic infrastructure   | VPC infrastructure |
| ---------- | ------------------------- |
|Vyatta, Fortigate, Juniper vSRX, Security Groups for virtual servers| Security groups, Network access control lists (ACLs)|
{: caption="Table 4. Security comparison" caption-side="top"}

## API differentiators
{: #compare-apis}

See the following table for the API differences between classic and VPC.

|  Classic infrastructure   | VPC infrastructure |
| ------------------------- | ------------------ |
|Existing {{site.data.keyword.slapi_short}} (SLAPI)| New developer-friendly, REST-based API |
{: caption="Table 5. API comparison" caption-side="top"}

## Next steps
{: #compare-nextsteps}

To review all the VPC infrastructure capabilities, see [About virtual private cloud](/docs/vpc?topic=vpc-about-vpc). To start exploring infrastructure overall, see [Building your infrastructure](/docs/overview?topic=overview-first-steps-it-ops).
