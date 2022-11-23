---

copyright:
  years:  2021, 2022
lastupdated: "2022-11-23"

keywords: migration, migrate, migrating, migrate infrastructure

subcollection: cloud-infrastructure

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}

# Bare metal to bare metal or virtual server migration overview
{: #pv-migration-overview}

In this migration process, migrating happens from a physical bare metal server to a bare metal or virtual server instance. The RMM tool simplifies the overall migration process of moving the OS, applications, and data from the bare metal environment to a bare metal or virtual server. You might want to move away from bare metal servers due to their hardware life cycle (qualifying new hardware, OS certification, and EOL support) or due to data center closures. Migrating from bare metal servers to bare metal or virtual servers allows you to modernize your environment and adopt virtualization.
{: shortdesc}

## Planning for your migration
{: #pv-planning}

Before you begin migrating your physical bare metal server to a virtual server, review the following requirements:

1. You need an existing VPC environment.
2. You need to install an RMM SSH key on both the source and target environments. Windows will need to install SSH Daemon provided by the RMM server.
3. Your bare metal server boot volume needs to be less than 250 GB.
4. Your bare metal server needs to be on a supported operating system. For a list of supported operating systems, see [Virtual server images](/docs/vpc?topic=vpc-about-images). 
5. Your source and target need to be able to reach other and the RMM. You can do this over the public internet with public IPs, or if you have a private-only environment, then you must set up either a VPN or transit gateway, use the following links:

  - [Direct link (2.0)](https://cloud.ibm.com/docs/dl?topic=dl-get-started-with-ibm-cloud-dl) 
  - [VPNs](https://cloud.ibm.com/docs/vpc?topic=vpc-vpn-overview)
  - [Transit Gateway](https://cloud.ibm.com/docs/transit-gateway?topic=transit-gateway-ordering-transit-gateway)

To improve data transfer rate, adjust bandwidth allocation of RMM server. To know how to change bandwidth allocation, see [Adjusting bandwidth allocation using the UI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#adjusting-bandwidth-allocation-ui).
{:note: .note}

## Migration considerations
{: #pv-migration-considerations}

### Capabilities 
{: #pv-capabilities}

* Application and environmental settings are preserved
* Data sync (data drift) before final cut-over is supported
* Data migration (secondary and block volume)  

VPC does not have support for snapshot, replication, and shared volume. You can manage these solutions through the native OS capabilities, tools, or third party of your choice.
{: note}

### Limitations
{: #pv-limitations}

* New IP address (original IP is not preserved).
* Classic add-ons are not carried over and will be lost.
* File-based storage migration is not supported.
* Hardware configuration is not brought over (RAID configuration).
* Bare metal with GPU is not supported due to no GPU support on VPC.
* Not an OS replatforming (Windows to Linux or vice versa, changing linux distribution, or major software upgrade). Software upgrades should be handled through the normal process.

## Validating your migration 
{: #pv-validating-migration}

After your server migration, you will want to validate your compute resources, such as applications and data, and update or validate the following:

* Validate or update your FQDN/Hostname.
* Update your `yum` repos, application settings, and configuration file.
* Update licensing where applicable.
* Remove RMM SSH key from target after final validation.
