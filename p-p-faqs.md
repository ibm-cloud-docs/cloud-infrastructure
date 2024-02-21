---

copyright:
  years: 2021, 2024
lastupdated: "2024-02-21"

keywords: 

subcollection: cloud-infrastructure

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for classic bare metal to classic bare metal migration
{: #bare-metal-faqs}

## What is an RMM server?
{: #bm-what-is-rmm-server}
{: faq}

RackWare Management Module (RMM) server is a software appliance that is offered by RackWare that replatforms your server from an {{site.data.keyword.cloud}} classic bare metal server to an {{site.data.keyword.cloud_notm}} classic bare metal server.

## Where can I find more information about RMM server?
{: #bm-where-to-find-more-information}
{: faq}

For RMM server overview information, see [RackWare's Cloud Migration documentation](https://www.rackwareinc.com/cloud-migration){: external}.

For RMM server usage guide information, see [RackWare RMM Getting Started for {{site.data.keyword.cloud_notm}}](https://www.rackwareinc.com/rackware-rmm-getting-started-for-ibm-cloud){: external}.

## How do I install the RMM server?
{: #bm-how-to-install-rmm-server}
{: faq}

This software is available in the {{site.data.keyword.cloud_notm}} catalog in the Migration Tools category. After you complete the appropriate information, a virtual server instance is installed in a new VPC with the RMM already installed.

## What are the limitations for the classic bare metal to classic bare metal migration?
{: #bm-bare-metal-to-bare-metal-limitations}
{: faq}

For more information about limitations of the classic bare metal to classic bare metal migration, see [{{site.data.keyword.cloud}} classic bare metal to classic bare metal migration limitations](/docs/cloud-infrastructure?topic=cloud-infrastructure-p-p-migration-bare-metal-overview#p-p-migration-bare-metal-limitations).

## What are supported operating systems for classic bare metal to classic bare metal migration?
{: #bm-bare-metal-supported-operating-systems}
{: faq}

For more information about supported operating systems, see the [{{site.data.keyword.cloud}} classic bare metal to classic bare metal migration supported operating systems](/docs/cloud-infrastructure?topic=cloud-infrastructure-p-p-migration-bare-metal-overview#p-p-migration-bare-metal-supported-os).

## How does RackWare handle bare metal servers with NIC bonding?
{: #bm-nic-bonding-supported}
{: faq}

The RMM migration tool does support migration over the bonded interface for classic to classic migration as well as on-premises to VPC migration. User intervention is not needed for migration with bonded interface.

## Is RMM a free service? 
{: #bm-is-rmm-server-license-free}
{: faq}

RMM is a Bring-Your-Own-License (BYOL) subscription-based service. Contact the RackWare sales team or see [Order a license](/docs/cloud-infrastructure?topic=cloud-infrastructure-p-p-migration-bare-metal-overview#p-p-migration-bare-metal-ordering-license) for classic bare metal to classic bare metal migration.

## Mounted file system or storage did not show up on the target server automatically?
{: #bm-mounted-filesystem-storage-no-show}
{: faq}

Make sure to have `/etc/fstab/entry` for automatic mounting of any file system at the target server.

## Is it necessary to add an RMM SSH key to the source and target server?
{: #bm-necessary-to-add-rmm-ssh-key}
{: faq}

Yes, RMM uses SSH to communicate to both the source and target servers. 

## Where can I find {{site.data.keyword.cloud}} documentation about classic bare metal to classic bare metal migration?
{: #bm-where-migration-doc}
{: faq}

For more information, see [{{site.data.keyword.cloud_notm}} classic bare metal to classic bare metal migration overview](/docs/cloud-infrastructure?topic=cloud-infrastructure-p-p-migration-bare-metal-overview).


## Who do I reach out to for support?
{: #bm-where-for-support}
{: faq}

1. Open any issues directly with RackWare support team. The support team is available 365x24x7.

2. Open a case by using the following options:

    a. Email: support@rackwareinc.com
  
    b. Phone: +1 (844) 797-8776
  
    c. In all cases, add 'RMM - {{site.data.keyword.cloud_notm}}’ in the subject line. The RackWare support is based in the United States and India.

## Is data migration supported for {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}}? 
{: #bm-is-data-migration-supported}
{: faq}

Only local storage and {{site.data.keyword.blockstorageshort}} is supported in {{site.data.keyword.cloud_notm}} classic. {{site.data.keyword.filestorage_short}} share’s data migration is not supported. To migrate data from file shares, consider the use of a third-party tool such as `rsync`. A sample script that uses `rsync` can be found [here](https://github.com/IBM-Cloud/vpc-migration-tools){: external}.









