---

copyright:
  years:  2021
lastupdated: "2022-02-24"

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

# Bare metal to virtual server migration overview
{: #pv-migration-overview}

Bare metal to virtual server migration is the process of migrating from a physical bare metal server to a virtual server instance. The RMM tool simplifies the overall migration process of moving the OS, applications, and data from the bare metal environment to a virtual server. You might want to move away from bare metal servers due to their hardware life cycle (qualifying new hardware, OS certification, and EOL support) or due to data center closures. Migrating from bare metal servers to virtual servers allows you to modernize your environment and adopt virtualization. Virtual servers also offer portability and resiliency. For example, if the host (underlying hardware fails), the virtual server can then be moved to a different host with ease. 
{: shortdesc}

## Planning for your migration
{: #pv-planning}

Before you begin migrating your physical bare metal server to a virtual server, review the following requirements:

1. You need an existing VPC environment.
2. You need to install an RMM SSH key on both the source and target environments. Windows will need to install SSH Daemon provided by the RMM server.
3. Your bare metal server boot volume needs to be less than 250 GB.
4. Your bare metal server needs to be on a supported operating system. For a list of supported operating systems, see [Virtual server images](/docs/vpc?topic=vpc-about-images). 
5. Your source and target need to be able to reach other and the RMM. You can do this over the public internet with public IPs, or if you have a private-only environment, then you must set up either a VPN or transit gateway (links to solution guides).

To improve data transfer rate, adjust bandwidth allocation of RMM server. To know how to change bandwidth allocation, see [Adjusting bandwidth allocation using the UI](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=ui#adjusting-bandwidth-allocation-ui).
{:note: .note}

## Migration considerations
{: #pv-migration-considerations}

### Capabilities 
{: #pv-capabilities}

* Application and environmental settings are preserved
* Data sync (data drift) prior to final cut-over is supported
* Data migration (secondary and block volume)  

VPC does not have support for snapshot, replication, and shared volume. You can manage these solutions through the native OS capabilities, tools, or third party of your choice.
{: note}

### Limitations
{: #pv-limitations}

* New IP address (original IP is not preserved)
* Classic add-ons are not carried over and will be lost
* File-based storage migration is not supported
* Hardware configuration is not brought over (RAID configuration)
* Bare metal with GPU is not supported due to no GPU support on VPC
* Not an OS re-platforming (Windows to Linux or vice versa, changing linux distribution, or major software upgrade). Software upgrades should be handled through the normal process.

## Validating your migration 
{: #pv-validating-migration}

After your server migration, you will want to validate your compute resources, such as applications and data, and update or validate the following:

* Validate or update your FQDN/Hostname.
* Update your `yum` repos, application settings, and configuration file
* Update licensing where applicable.
* Remove RMM SSH key from target after final validation.
