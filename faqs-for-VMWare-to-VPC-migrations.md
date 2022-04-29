---

copyright:
  years: 2021, 2022
lastupdated: "2022-04-29"

keywords: infrastructure

subcollection: cloud-infrastructure

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}

# FAQs for VMware (on-premises or classic) to IBM Cloud VPC migration
{: #faqs-vmware}

## What is the RMM server? 
{: #what-rmm-server}
{: faq}

RackWare Management Module (RMM) server is a software appliance that is offered by RackWare that replatforms your server from a VMware (on-premises or classic) to an {{site.data.keyword.vpc_short}} virtual server instance.
 
## Where can I find more information about the RMM server? 
{: #where-rmm-server-info}
{: faq}

For RMM server overview information, see [RackWare's Cloud Migration](https://www.rackwareinc.com/cloud-migration){: external} documentation. For RMM server usage guide information, see the [RackWare RMM User's Guide for {{site.data.keyword.cloud_notm}}](https://www.rackwareinc.com/rackware-rmm-users-guide-for-ibm-cloud){: external}.
 
## How do I install the RMM server? 
{: #how-rmm-server-install}
{: faq}

This software is available in the {{site.data.keyword.cloud_notm}} catalog in the Migration Tools category. After you provide the appropriate information, a virtual server instance is installed in a new VPC with the RMM already installed.
 
## What are the limitations for the VMware to VPC migration? 
{: #limitations-vmware-migration}
{: faq}

For more information about limitations of the VMware to VPC migration, see VMware to VPC migration limitation.
 
## How do I get an RMM server license?  
{: #how-procure-rmm-server-license}
{: faq}

For more information, see [Order license for VMware to VPC migration](/docs/cloud-infrastructure?topic=cloud-infrastructure-migrating-images-vmware-vpc#byol-bring-your-own-license-from-rackware). 
 
## Is it necessary to add an RMM SSH key to the source and target server? 
{: #necessary-add-rmm-ssh-key}
{: faq}

Yes, RMM uses SSH for communication between source, target, and the RMM server. So it is necessary to add RMM server’s public key to the source and target server.
 
 
## Where can I find IBM Cloud documentation about VMware VM (on-premises or classic) to IBM Cloud VPC migration? 
{: #where-vmware-doc-info}
{: faq}

For more information, see [RackWare Cloud Migration Documentation](https://www.rackwareinc.com/cloud-migration).
 
## Who do I contact for support? 
{: #where-support-info}
{: faq}

Open any issue directly with the RackWare support team. The support team is available 365x24x7.

Open a case by using the following options:

- Email: support@rackwareinc.com 
- Phone: +1 (844) 797-8776 

In all cases, add ‘RMM - {{site.data.keyword.cloud_notm}}’ in the subject line. The RackWare support is based in United States and India. 
 
## Can I migrate on-premises VMware virtual machines to IBM Cloud VPC virtual server instances? 
{: #can-i-migrate-vm}
{: faq}

Yes, as long as connectivity is established from on-premises to the RMM server and {{site.data.keyword.vpc_short}}. 
 
## Is data migration supported for IBM File or Block performance endurance storage? 
{: #is-data-migration-supported}
{: faq}

Only local storage and Block performance endurance storage is supported in {{site.data.keyword.cloud_notm}} classic. File performance and endurance volume’s data migration is not supported, for this, consider that uses a third-party tool such as `rsync`. A sample script that uses `rsync` can be found [here](https://github.com/IBM-Cloud/vpc-migration-tools){: external}.

## Is the migration intrusive?
{: #is-migration-intrusive}
{: faq}

Usually the migration is nonintrusive. It can be done while the server is up and running. However, the source server does need some unused space to do the image capture of the server. In addition, RMM does require SSH (port 22) to be open on both the server and target to run the migration. The CPU consumption for image capture and copying to the target must be minimal.
 
## What data does the discovery tool identify from VMware vCenter? 
{: #what-data-does-discovery-identify}
{: faq}

The discovery tool discovers guest VMs from VMware and it uploads into RMM as the source for migrating in a typical wave. Each wave is named by ESXi host IP address from which guest VMs are discovered.
 
## What operating systems support the migration?
{: #what-os-support-migration}
{: faq} 

Supports migrating Windows Server 2012, 2012R2, 2016, and 2019; Red Hat Enterprise Linux (RHEL), CentOS, Ubuntu, and Debian Linux operating systems.
 
## Does the migration solution support auto-provisioning of the target server?
{: #does-migration-support-auto-provisioning}
{: faq}

Yes. With RMM **Auto-Provision** feature, you can auto-provision the target server. For more informaation, see "Bare metal to virtual server migration on a private network that uses RMM": option 2 of Step 1: [Set up and provision VPC and virtual server instance](/docs/cloud-infrastructure?topic=cloud-infrastructure-migrating-images-vmware-vpc#cloud-vpc-vsi-setup).
 
 