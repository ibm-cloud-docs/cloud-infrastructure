---

copyright:
  years: 2020, 2024
lastupdated: "2024-09-25"

keywords: infrastructure

subcollection: cloud-infrastructure

---

{{site.data.keyword.attribute-definition-list}}

# Storage services
{: #storage}

Our cloud storage services offer a scalable, security-rich, and cost-effective home for your data, and support for traditional and cloud-native workloads. Provision and deploy services such as Object, Block, and File Storage. Adjust capacity and optimize performance as requirements change. Pay only for the cloud storage that you need.

## Current Infrastructure
{: #storage-vpc}

| Option | Description |
|--------|---------------|
| [{{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-block-storage-about) | Persistent, high-performance data storage for virtual server instances in the {{site.data.keyword.cloud}} Virtual Private Cloud (VPC). The VPC infrastructure provides rapid scaling across multiple regions and zones, and extra performance and security.  |
| [{{site.data.keyword.filestorage_vpc_full}}](/docs/vpc?topic=vpc-file-storage-vpc-about) | A zonal file storage offering that provides NFS-based file storage services. You can create file shares in an availability zone within a region. You can share them with multiple virtual server instances within the same zone or other zones in your region, across multiple VPCs. You can share your NFS-based file storage across accounts and external services, such as [IBM watsonX](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/welcome-main.html?context=wx){: external}. You can also limit access to a file share to a specific virtual server instance within a VPC by using Security groups, and encrypt the data in transit. Compatible with {{site.data.keyword.vsi_is_short}} and {{site.data.keyword.bm_is_short}}. |
| [{{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage) | Distributed, multi-tenant Cloud Object Storage for data encrypted and dispersed across multiple geographic locations, which are accessed over HTTP by using a REST API. This service uses the distributed storage technologies that are provided by the {{site.data.keyword.cos_full_notm}} System. |
{: caption="Table 1. Storage options" caption-side="bottom"}

## Classic infrastructure
{: #storage-classic}

| Option | Description |
|--------|---------------|
| [{{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-getting-started) | Persistent, high-performance iSCSI storage that is provisioned and managed independently of Compute instances. iSCSI-based {{site.data.keyword.blockstorageshort}} LUNs are connected to authorized devices through redundant multi-path I/O (MPIO) connections. |
| [{{site.data.keyword.filestorage_short}}](/docs/FileStorage?topic=FileStorage-getting-started) | Persistent, fast, and flexible network-attached, NFS-based {{site.data.keyword.filestorage_short}}. In this network-attached storage (NAS) environment, you have total control over your file shares function and performance. {{site.data.keyword.filestorage_short}} shares can be connected to up to 64 authorized devices over routed TCP/IP connections for resiliency. |
| [{{site.data.keyword.backup_full}}](/docs/Backup?topic=Backup-getting-started) | An automated agent-based backup system that is managed through a browser-based management utility. You can back up data between servers in one or more data centers on the {{site.data.keyword.cloud}} network. |
{: caption="Table 2. Storage options - Classic" caption-side="bottom"}

## Next steps
{: #nextsteps4}

To continue, see [Managing your infrastructure](/docs/cloud-infrastructure?topic=cloud-infrastructure-managing).
