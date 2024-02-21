---

copyright:
  years: 2021, 2024
lastupdated: "2024-02-21"

keywords: infrastructure

subcollection: cloud-infrastructure

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for VMware (on-premises or classic) to {{site.data.keyword.vpc_short}} migration
{: #faqs-vmware}

## What is the RMM server? 
{: #vmware-what-rmm-server}
{: faq}

RackWare Management Module (RMM) server is a software appliance that is offered by RackWare that replatforms your server from a VMware (on-premises or classic) to an {{site.data.keyword.vpc_short}} virtual server instance.
 
## Where can I find more information about the RMM server? 
{: #vmware-where-rmm-server-info}
{: faq}

For RMM server overview information, see [RackWare's Cloud Migration](https://www.rackwareinc.com/cloud-migration){: external} documentation. For RMM server usage guide information, see the [RackWare RMM Getting Started for {{site.data.keyword.cloud_notm}}](https://www.rackwareinc.com/rackware-rmm-getting-started-for-ibm-cloud){: external}.
 
## How do I install the RMM server? 
{: #vmware-how-rmm-server-install}
{: faq}

This software is available in the {{site.data.keyword.cloud_notm}} catalog in the Migration Tools category. After you provide the appropriate information, a virtual server instance is installed in a new VPC with the RMM already installed.
 
## How do I get an RMM server license?  
{: #vmware-how-procure-rmm-server-license}
{: faq}

For more information, see [Order license for VMware to VPC migration](/docs/cloud-infrastructure?topic=cloud-infrastructure-migrating-images-vmware-vpc-classic#license-rackware-bring-classic). 
 
## Is it necessary to add an RMM SSH key to the source and target server? 
{: #vmware-necessary-add-rmm-ssh-key}
{: faq}

Yes, RMM uses SSH for communication between source, target, and the RMM server. So it is necessary to add the RMM server’s public key to the source and target server.
 
 
## Where can I find {{site.data.keyword.cloud}} documentation about VMware VM (on-premises or classic) to {{site.data.keyword.vpc_short}} migration? 
{: #vmware-where-vmware-doc-info}
{: faq}

For On-premises VMware VM to {{site.data.keyword.vpc_short}} migration with RMM documentation, click [here](/docs/cloud-infrastructure?topic=cloud-infrastructure-migrating-images-vmware-vpc).

For Classic VMware VM to {{site.data.keyword.vpc_short}} migration with RMM documentation, click [here](/docs/cloud-infrastructure?topic=cloud-infrastructure-migrating-images-vmware-vpc-classic)
 
## Who do I contact for support? 
{: #vmware-where-support-info}
{: faq}

Open any issue directly with the RackWare support team. The support team is available 365x24x7.

Open a case by using the following options:

- Email: support@rackwareinc.com 
- Phone: +1 (844) 797-8776 

In all cases, add ‘RMM - {{site.data.keyword.cloud_notm}}’ in the subject line. The RackWare support is based in the United States and India. 
 
## Can I migrate on-premises VMware virtual machines to {{site.data.keyword.vpc_short}} virtual server instances? 
{: #vmware-can-i-migrate-vm}
{: faq}

Yes, as long as connectivity is established from on-premises to the RMM server and {{site.data.keyword.vpc_short}}. 
 
## Is data migration supported for {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}}? 
{: #vmware-is-data-migration-supported}
{: faq}

Only local storage and {{site.data.keyword.blockstorageshort}} is supported in {{site.data.keyword.cloud_notm}} classic. {{site.data.keyword.filestorage_short}} share’s data migration is not supported. To migrate data from file shares, consider the use of a third-party tool such as `rsync`. A sample script that uses `rsync` can be found [here](https://github.com/IBM-Cloud/vpc-migration-tools){: external}.

## Is the migration intrusive?
{: #vmware-is-migration-intrusive}
{: faq}

Usually the migration is nonintrusive. It can be done while the server is up and running. However, the source server does need some unused space to do the image capture of the server. In addition, RMM does require SSH (port 22) to be open on both the server and target to run the migration. The CPU consumption for image capture and copying to the target must be minimal.
 
## What data does the discovery tool identify from VMware vCenter? 
{: #vmware-what-data-does-discovery-identify}
{: faq}

The discovery tool discovers guest VMs from VMware and it uploads into RMM as the source for migrating in a typical wave. Each wave is named by the ESXi host IP address from which guest VMs are discovered.
 
## Does the migration solution support auto-provisioning of the target server?
{: #vmware-does-migration-support-auto-provisioning}
{: faq}

Yes. With the RMM **Auto-Provision** feature, you can auto-provision the target server. For more information, see "Bare metal to virtual server migration on a private network that uses RMM": option 2 of Step 1: [Set up and provision VPC and virtual server instance](/docs/cloud-infrastructure?topic=cloud-infrastructure-migrating-images-vmware-vpc-classic#cloud-vpc-vsi-setup).
 
 