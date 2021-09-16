---
copyright:
  years: 2019, 2021
lastupdated: "2021-04-20"

keywords: understanding infrastructure, vpc, classic infrastructure, cloud environment

subcollection: cloud-infrastructure

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Comparing {{site.data.keyword.cloud_notm}} Classic and VPC infrastructure environments
{: #compare-infrastructure}

Compare the key differences between {{site.data.keyword.cloud}} infrastructure environments to decide which one is best for your workloads and applications.
{: shortdesc}

If you aren't familiar with the environment types, review the following descriptions.

* Classic infrastructure is our existing IaaS platform. This environment is best for lift and shift workloads so you can move applications quickly and keep the same architecture.
* VPC infrastructure is our new IaaS platform, based on software-defined networking and ideal for cloud-native applications.

Classic infrastructure and VPC infrastructure are cost neutral, so you can focus on what environment best meets your needs.
{: note}

## Compute differentiators
{: #compare-compute}

See the following table for the compute differences between classic and VPC.

| Category   |  Classic Infrastructure   | VPC Infrastructure |
| ---------- | ------------------------- | ------------------ |
|  **Services**  |Full catalog of services, such as {{site.data.keyword.baremetal_short}}, {{site.data.keyword.BluVirtServers_short}} instances, VMware, SAP | {{site.data.keyword.BluVirtServers_short}} instances only |
| **Performance and availability** | | Better availability achievable through zone architecture |
| **Pricing** | Hourly and monthly billing, plus suspend billing features | Hourly, suspend billing, and sustained usage discount |
| **Virtual server families** | Public, dedicated, transient, reserved | Public, dedicated |
| **Profiles** | All profiles, including the GPU profiles | Balanced, compute, memory profiles with higher RAM and vCPU options |
| **Supported images** | Full set of pre-stock images, plus custom images | Limited set of pre-stock images, plus the ability to import a custom image |
| **Platform integration** | | IAM and resource group integration for a unified experience |
{: caption="Table 1. Compute comparison" caption-side="top"}
{: summary="This table has row and column headers. The row headers identify possible features. The column headers identify the differentiators between classic infrastructure and VPC infrastructure. To understand the differences between environments, go to the row and find the details for the feature that you're interested in."}

## Network differentiators
{: #compare-network}

See the following table for the networking differences between classic and VPC.

| Category   |  Classic infrastructure   | VPC infrastructure |
| ---------- | ------------------------- | ------------------ |
| **Location construct**    | Data centers and PODs <br>(Might require VLAN spanning to connect two different pods or data centers, and purchasing gateways to control and route traffic) | Regional model that abstracts infrastructure so you don't need to worry about pod locations.|
| **Network functions and services** |Physical and virtual appliances from multiple vendors | Cloud-native network functions (VPNs, LBaaS)<br>(VPC isolation, dedicated resources carved out of public cloud, with more options for VPNs, LBaaS, multiple vNIC instances, and larger subnet sizes) |
| **IP addresses** | IPv6 addresses supported | IPv4 addresses only |
| **Gateway routin**g | Use a virtual or physical network appliance (Virtual Router Appliance, Vyatta, Juniper vSRX, Fortinet FSA) | Traffic routing is handled by public gateway and floating IP services |
| **Network address translation (NAT)** | Use a virtual or physical network appliance (Virtual Router Appliance, Vyatta, Juniper vSRX, Fortinet FSA) | Supported by the Bring-your-own-IP (BYOIP) functionality  |
| **IPsec Virtual private network (VPN)** | Use a virtual or physical network appliance (Virtual Router Appliance, Vyatta, Juniper vSRX, Fortinet FSA) | Supported by the VPN-as-a-service offering |
|  **Elastic load balancing** | Cloud Load Balancer  | Load Balancer for VPC |
| **Global load balancing**| Cloud Internet Services, Citrix Netscaler MPX | Cloud Internet Services |
|**Hybrid connectivity** | NAT solution to bridge between IBM Cloud and your IT environment | Bring your own private IP address without NAT or IPSec tunnels <br>Note: You can enable your VPC to access classic infrastructure resources. |
{: caption="Table 2. Network comparison" caption-side="top"}
{: summary="This table has row and column headers. The row headers identify possible features. The column headers identify the differentiators between classic infrastructure and VPC infrastructure. To understand the differences between environments, navigate to the row and find the details for the feature that you're interested in."}

## Storage differentiators
{: #compare-storage}

See the following table for the storage differences between classic and VPC.

|  Classic infrastructure   | VPC infrastructure |
| ------------------------- | ------------------ |
|Robust set of storage services, block storage (iSCSI), and file storage (NFS-based) offerings| Block storage as primary boot disk (with basic lifecycle management), and secondary data volumes  <br> Note: Volume encryption is available during provisioning.|
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

To review all our VPC infrastructure capabilities, see [About virtual private cloud](/docs/vpc?topic=vpc-about-vpc). To start exploring infrastructure overall, see [Building your infrastructure](/docs/overview?topic=overview-first-steps-it-ops).
