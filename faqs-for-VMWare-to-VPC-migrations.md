---

copyright:
  years: 2021, 2022
lastupdated: "2021-03-08"

keywords: infrastructure

subcollection: cloud-infrastructure

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:note: .note}
{:new_window: target="_blank"}

# FAQs for VMware (On-prem/Classic) to IBM VPC migrations 
{: #faqs-vmware}

## What is the RMM server? 
{: #what-rmm-server}
{: faq}

RMM server is a software appliance that is offered by RackWare that replatforms your server from a VMware (On-prem/Classic) to an IBM VPC VSI. 
 
## Where can I find more information about the RMM server? 
{: #where-rmm-server-info}
{: faq}

For RMM server overview information, see RackWare's Cloud Migration documentation. For RMM server usage guide information, see the [RackWare RMM User's Guide for IBM Cloud](https://cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-migrating-images-vmware-vpc). 
 
## How do I install the RMM server? 
{: #how-rmm-server-install}
{: faq}

This software is available in the IBM Cloud catalog under the Migration Tools category. After you provide the appropriate information, a virtual server instance is installed in a new VPC with the RMM already installed. 
 
## What are the limitations for the VMware to VPC migration? 
{: #limitations-vmware-migration}
{: faq}

For more information on limitations of the VMware to VPC migration, see VMware to VPC migration limitation. 
 
 
## How to procure an RMM server license?  
{: #how-procure-rmm-server-license}
{: faq}

See [Order License for VMware to VPC migration](https://cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-migrating-images-vmware-vpc#byol-bring-your-own-license-from-rackware). 
 
## Is it necessary to add an RMM SSH key to the source and target machine? 
{: #necessary-add-rmm-ssh-key}
{: faq}

Yes, RMM uses SSH for communication between source, target, and the RMM server machine. So it is necessary to add RMM server’s public key to the source and target server.  
 
 
## Where can I find IBM cloud documentation about VMware VM(On-Prem/Classic) to IBM VPC migration? 
{: #where-vmware-doc-info}
{: faq}

See [RackWare Cloud Migration Documentation](https://www.rackwareinc.com/cloud-migration)
 
## Who do I reach out to for support? 
{: #where-support-info}
{: faq}

Please open any issues directly with the Rackware support team. The support team is available 365x24x7.

Please open a case via following options:

- Email: support@rackwareinc.com 
- Phone: +1 (844) 797-8776 

In all cases, please add ‘RackWare RMM - IBM Cloud’ in the subject line. The RackWare support is based in United States and India. 
 
## Can I migrate on-prem VMware virtual machines to IBM cloud VPC VSI? 
{: #can-i-migrate-vm}
{: faq}

Yes, as long there is connectivity establish from the on-prem to the RMM server and IBM Cloud VPC. 
 
## Is data migration supported for IBM File or Block performance endurance storage? 
{: #is-data-migration-supported}
{: faq}

No, currently only local storage data migration is supported. If you still want to migrate data from File or Block storage, then please use a third-party tool such as `RSync`. [Here](/docs/cloud-infrastructure?topic=cloud-infrastructure-faqs-vmware#is-data-migration-supported) is a sample script that uses `RSync` to help with the data migration for the file and block volumes.
 
## Is the migration intrusive?  
{: #is-migration-intrusive}
{: faq}

 In most cases the migration is nonintrusive.  It can be done while the server is up and running.  However, the source server does need some free space to do the image capture of the machine.  In addition, RMM does require SSH (port 22) to be open on both the server and target in order to perform the migration.  The CPU consumption for image capture and copying to the target should be minimal. 
 
## What data does the Discovery tool identifies from VMware vCenter? 
{: #what-data-does-discovery-identify}
{: faq}

The discovery tool discovers guest VMs from VMware and it uploads into RMM as the source for migrating in a given wave. Each wave is named by ESXI host IP address from which guest VMs are discovered.   
 
## What operating systems does support the migration?
{: #what-os-support-migration}
{: faq} 

Supports migrating Windows Server 2012, 2012R2, 2016, and 2019; Red Hat Enterprise Linux (RHEL), CentOS, Ubuntu, and Debian Linux operating systems. 
 
## Does the migration solution support auto provisioning of the target server? 
{: #does-migration-support-auto-provisioning}
{: faq}

Yes. With Rackware's RMM **Auto Provision** feature. See "Bare metal to virtual server migration on a private network that uses RackWare RMM": option 2 of Step 1: [Set up and provision VPC and virtual server instance](/docs/cloud-infrastructure?topic=cloud-infrastructure-migrating-images-vmware-vpc#cloud-vpc-vsi-setup)
 
 