---

copyright:
  years: 2020
lastupdated: "2020-06-03"

keywords: infrastructure

subcollection: cloud-infrastructure

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:note: .note}
{:new_window: target="_blank"}

# Storage services
{: #storage}

{{site.data.keyword.baremetal_short}} and {{site.data.keyword.BluVirtServers_short}} are provisioned with default storage. {{site.data.keyword.baremetal_short}} have a minimum of 1 TB SATA disk space, and {{site.data.keyword.BluVirtServers_short}} have a minimum of 25 GB SAN storage, with the exception of {{site.data.keyword.cloud_notm}} SAP-Certified {{site.data.keyword.baremetal_short}}. For more information on the default storage available with these servers, see [{{site.data.keyword.cloud_notm}} SAP-Certified Infrastructure](/docs/bare-metal?topic=bare-metal-sap-cert-infrastructure#sap-cert-infrastructure).

You can buy extra storage based on your needs. See the following table for a summary of your storage options.

## VPC infrastructure
{: #storage-vpc}

| Option | Description |
|--------|---------------|
| [{{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-block-storage-about) | Persistent, high-performance data storage for virtual server instances in the IBM Cloud Virtual Private Cloud (VPC). The VPC infrastructure provides rapid scaling across multiple regions and zones, and extra performance and security.  |
| [{{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage) | Distributed, multi-tenant Cloud object storage for data encrypted and dispersed across multiple geographic locations, accessed over HTTP by using a REST API. This service uses the distributed storage technologies that are provided by the IBM Cloud Object Storage System (formerly Cleversafe). |
{: caption="Table 1. Storage options - VPC" caption-side="top"}

## Classic infrastructure
{: #storage-classic}

| Option | Description |
|--------|---------------|
| [{{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-getting-started) | Persistent, high-performance iSCSI storage that is provisioned and managed independently of compute instances. iSCSI-based Block Storage LUNs are connected to authorized devices through redundant multi-path I/O (MPIO) connections. |
| [{{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-getting-started) | Persistent, fast, and flexible network-attached, NFS-based File Storage. In this network-attached storage (NAS) environment, you have total control over your file shares function and performance. File Storage shares can be connected to up to 64 authorized devices over routed TCP/IP connections for resiliency. |
| [{{site.data.keyword.cos_full_notm}} - Classic](/docs/cloud-object-storage-infrastructure?topic=cloud-object-storage-infrastructure-about-ibm-cloud-object-storage-classic-) | Information that is stored with {{site.data.keyword.cloud_notm}} Object Storage is encrypted and dispersed across multiple geographic locations, and accessed over HTTP by using a REST API. This service uses the distributed storage technologies that are provided by the IBM Cloud Object Storage System (formerly Cleversafe). |
| [{{site.data.keyword.cloud_notm}} Master Data Management on Cloud](/docs/services/MDMOnCloud?topic=MDMOnCloud-mdmoc_getting_started#mdmoc_getting_started) | Orchestrates your enterprise data throughout the complete information lifecycle with a highly configurable framework that supports hybrid cloud environments. Offers the rich features of an on-premises InfoSphere® Master Data Management deployment without the cost, complexity, and risk of managing your own infrastructure. |
| [{{site.data.keyword.backup_full}}](/docs/Backup?topic=Backup-getting-started) | An automated agent-based backup system that is managed through a browser-based management utility. You can back up data between servers in one or more data centers on the IBM Cloud network. |
{: caption="Table 2. Storage options - Classic" caption-side="top"}

## Next steps
{: nextsteps4}

To continue, see [Managing your infrastructure](/docs/cloud-infrastructure?topic=cloud-infrastructure-managing).