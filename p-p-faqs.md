---

copyright:
  years: 2021, 2022
lastupdated: "2022-03-23"

keywords: 

subcollection: cloud-infrastructure

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for bare metal to bare metal migration
{: #bare-metal-faqs}

## What is RMM server?
{: #bm-what-is-rmm-server}
{: faq}

RMM server is a software appliance that is offered by RackWare that replatforms your server from an {{site.data.keyword.cloud}} classic bare metal server to an {{site.data.keyword.cloud_notm}} classic bare metal server.

## Where can I find more information about RackWare RMM server?
{: #bm-where-to-find-more-information}
{: faq}

For RMM server overview information, see [RackWare's Cloud Migration documentation](https://www.rackwareinc.com/cloud-migration){: external}.

For RMM server usage guide information, see [RackWare RMM User's Guide for {{site.data.keyword.cloud_notm}}](https://www.rackwareinc.com/rackware-rmm-users-guide-for-ibm-cloud){: external}.

## How do I install the RMM server?
{: #bm-how-to-install-rmm-server}
{: faq}

This software is available in the {{site.data.keyword.cloud_notm}} catalog in the Migration Tools category. After you complete the appropriate information, a virtual server instance is installed in a new VPC with the RMM already installed.

## What are the limitations for the bare metal to bare metal migration?
{: #bm-bare-metal-to-bare-metal-limitations}
{: faq}

For more information on limitations of the bare metal to bare metal migration, see [Bare metal to bare metal migration limitations](/docs/cloud-infrastructure?topic=cloud-infrastructure-p-p-migration-bare-metal-overview#p-p-migration-bare-metal-limitations).

## What are supported operating systems for bare metal to bare metal migration?
{: #bm-bare-metal-supported-operating-systems}
{: faq}

For more information on supported operating systems, see the [Bare metal to bare metal migration supported operating systems](/docs/cloud-infrastructure?topic=cloud-infrastructure-p-p-migration-bare-metal-overview#p-p-migration-bare-metal-supported-os).

## How does RackWare handle bare metal servers with NIC bonding?
{: #bm-nic-bonding-supported}
{: faq}

The RMM migration tool does support migration over the bonded interface for classic to classic migration as well as on-premises to VPC migration. User intervention is not needed for migration with bonded interface.

## Is RMM a free service? 
{: #bm-is-rmm-server-license-free}
{: faq}

RackWare RMM is a Bring-Your-Own-License (BYOL) subscription-based service. Contact the RackWare sales team or see [Order a license](/cloud-infrastructure?topic=cloud-infrastructure-p-p-migration-bare-metal-overview#p-p-migration-bare-metal-ordering-license) for bare metal to bare metal migration.

## Mounted file system or storage did not show up on target automatically?
{: #bm-mounted-filesystem-storage-no-show}
{: faq}

Make sure to have `/etc/fstab/entry` for automatic mounting of any file system at the target server.

## Is it necessary to add RMM SSH key to source and target server?
{: #bm-necessary-to-add-rmm-ssh-key}
{: faq}

Yes, RMM uses SSH to communicate to both the source and target servers. 

## Where can I find IBM Cloud documentation about classic bare metal to classic bare metal migration?
{: #bm-where-migration-doc}
{: faq}

For more information, see [{{site.data.keyword.cloud_notm}} classic bare metal to bare metal migration overview](/docs/cloud-infrastructure?topic=cloud-infrastructure-p-p-migration-bare-metal-overview).


## Who do I reach out for support?
{: #bm-where-for-support}
{: faq}

1. Open any issues directly with RackWare support team. The support team is available 365x24x7.

2. Open a case by using the following options:

    a. Email: support@rackwareinc.com
  
    b. Phone: +1 (844) 797-8776
  
    c. In all cases, add 'RackWare RMM - {{site.data.keyword.cloud_notm}}’ in the subject line. The RackWare support is based in the United States and India.

## Is data migration supported for File or Block performance endurance storage?
{: #bm-is-data-migration-supported}
{: faq}

Only local storage and Block performance endurance storage is supported in {{site.data.keyword.cloud_notm}} classic. File performance and endurance volume’s data migration is not supported; for this, consider using a third-party tool such as `rsync`. A sample script using `rsync` can be found [here](https://github.com/IBM-Cloud/vpc-migration-tools){: external}.









