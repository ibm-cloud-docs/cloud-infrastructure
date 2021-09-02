---

copyright:
  years: 2021
lastupdated: "2021-09-02"

keywords: 

subcollection: cloud-infrastructure

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note .note}
{:important: .important}
{:faq: data-hd-content-type='faq'}

# FAQs for bare metal to bare metal 
{: bare-metal-faqs}


## What is RMM server?
{: #what-is-rmm-server}
{: faq}

RMM server is a software appliance that is offered by RackWare that replatforms your server from an IBM Cloud classic bare metal server to an IBM Cloud classic bare metal server

## Where can I find more information about RackWare RMM server?
{: #where-to-find-more-information}
{: faq}

For RMM server overview information, see [RackWare's Cloud Migration documentation](https://www.rackwareinc.com/cloud-migration).

For RMM server usage guide information, see [RackWare RMM User's Guide for IBM Cloud](https://www.rackwareinc.com/rackware-rmm-users-guide-for-ibm-cloud).

## How do I install the RMM server?
{: #how-to-install-rmm-server}
{: faq}

This software is available in the IBM Cloud catalog in the Migration Tools category. After you fill out the appropriate information, a virtual server instance is installed in a new VPC with the RMM already installed.

## What are the limitations for the Bare Metal to Bare Metal migration?
{: #bare-metal-to-bare-metal-limitations}
{: faq}

For more information on limitations of the Bare Metal to Bare Metal migration, see [Bare Metal to Bare Metal migration limitations](https://test.cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-p-p-migration-bare-metal-overview#p-p-migration-bare-metal-limitations).

## What are supported operating systems for Bare Metal to Bare Metal migration?
{: #bare-metal-supported-operating-systems}
{: faq}

For more information on supported operating systems, see [Bare Metal to Bare Metal migration supported operating systems](https://test.cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-p-p-migration-bare-metal-overview#p-p-migration-bare-metal-supported-os).

## Is NIC bonding supported?
{: #nic-bonding-supported}
{: faq}

The RMM migration tool does not support migration over the bonded interface. NIC bonding must be removed first before starting the migration. See how to [Remove NIC bonding](https://test.cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-p-p-migration-bare-metal-overview#p-p-migration-bare-metal-removing-nic-bond) for more details.


## Is RMM a free service? 
{: #is-rmm-server-license-free}
{: faq}

Rackware RMM is a bring your own license (BYOL) subscription-based service. Contact the [Rackware sales team ](sales@rackwareinc.com)or see [Order a license](https://test.cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-p-p-migration-bare-metal-overview#p-p-migration-bare-metal-ordering-license) for Bare Metal to Bare Metal migration.

## Mounted filesystem / storage did not show up on target automatically?
{: #mounted-filesystem-storage-no-show}
{: faq}

Make sure to have ``/etc/fstab/entry`` for automatic mounting of any file system at the target machine.

## Is it necessary to add RMM SSH key to source and target machine?
{: #necessary-to-add-rmm-ssh-key}
{: faq}

Yes, RMM uses SSH to communicate to both the source and target servers. 

## Where can I find IBM cloud documentation about Bare Metal to Bare Metal migration?
{: #where-migration-doc}
{: faq}

See [RackWare Cloud Migration](https://www.rackwareinc.com/rackware-rmm-users-guide-for-ibm-cloud){: external}


## Who do I reach out for support?
{: #where-for-support}
{: faq}

1. Please open any issues directly with RackWare support team. The support team is available 365x24x7.

2. Please open a case via following options:

    a. Email: support@rackwareinc.com
  
    b. Phone: +1 (844) 797-8776
  
    c. In all cases, please add 'RackWare RMM -IBM Cloud’ in the subject line. The RackWare support is based in the United States and India.
  
## Can I migrate on-prem to IBM cloud classic?
{: #can-you-migrate-on-prem}
{: faq}

No, this tool is only for classic Bare Metal to classic Bare Metal.

## Is data migration supported for File or Block performance endurance storage?
{: #is-data-migration-supported}
{: faq}

No, RMM currently only supports local storage migration. For block or file performance and endurance volumes, consider using a 3rd party tool such as ``rsync``. A sample script using ``rsync`` can be found [here](https://github.com/IBM-Cloud/vpc-migration-tools).









