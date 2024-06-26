---

copyright:
  years: 2021, 2024
lastupdated: "2024-06-16"

keywords: 

subcollection: cloud-infrastructure

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for RackWare
{: #rackware-faqs}

## What is an RMM server?
{: #rw-what-is-rmm-server}
{: faq}

RackWare Management Module (RMM) server is a software appliance that is offered by RackWare that replatforms your server from an {{site.data.keyword.cloud}} classic physical bare metal server to an {{site.data.keyword.vpc_short}} virtual server instance.

## Where can I find more information about RMM server?
{: #rw-where-to-find-more-information}
{: faq}

For more information, see [RackWare's Cloud Migration documentation](https://www.rackwareinc.com/cloud-migration){: external} and [RackWare RMM Getting Started for {{site.data.keyword.cloud_notm}}](https://www.rackwareinc.com/rackware-rmm-getting-started-for-ibm-cloud){: external}.

## How do I install the RMM server?
{: #rw-how-to-install}
{: faq}

This software is available in the {{site.data.keyword.cloud}} catalog in the **Migration Tools** category. After you complete the appropriate information, a virtual server instance is installed in a new VPC with the RMM already installed.

## Is this service without charge?
{: #rw-how-much-is-service}
{: faq}

You need to Bring Your Own License (BYOL), which you must purchase directly from RackWare. For more information or inquiries, contact [sales@rackwareinc.com](mailto:sales@rackwareinc.com). However, {{site.data.keyword.IBM_notm}} is offering promotional licensing at no cost for three months for three reusable concurrent migration licenses.

## What is the promotional license?
{: #rw-promotional-license}
{: faq}

The promotional license is valid only for per-account and first-time users of the RMM server. After the three-month period, you need to purchase the license directly from RackWare.

## How does the reusable concurrent license work?
{: #rw-reusable-concurrent-license}
{: faq}

{{site.data.keyword.IBM_notm}} and RackWare put together a special license model. Typically, a license is a one-time use for each server migration. Whereas for {{site.data.keyword.IBM_notm}} purposes, after a completed migration, the license can be reused for a different server migration. The original host that the license was assigned to just needs to be deleted from the RMM server database.

The number of concurrent migrations that can occur is limited up to the number of licenses procured.

## How do I obtain and activate my promotional license?
{: #rw-obtain-activate-promotional-license}
{: faq}

You can retrieve the promotional license through the discovery script that is part of the RMM software appliance. For more information about how to use the script, see [Bare metal to virtual server migration on a private network with RMM](/docs/cloud-infrastructure?topic=cloud-infrastructure-pv-migration-private-network).

## What are the considerations or limitations for the physical to virtual migration?
{: #rw-considerations-limitations-migration}
{: faq}

For more information about considerations and limitations of the physical to virtual migration, see [Bare metal to virtual server migration overview](/docs/cloud-infrastructure?topic=cloud-infrastructure-pv-migration-overview).

## Is the migration intrusive?
{: #rw-migration-intrusive}
{: faq}

In most cases, the migration is not intrusive. The migration can be done when the server is up and running. The source server does need some empty space to do an image capture of the server. In addition, RMM does require SSH (port 22) reachability to both server and target to perform the migration. The CPU consumption for image capture and copying to the target is minimal.

## Can RackWare RMM create the target virtual server instance?
{: #rw-create-target-vsi}
{: faq}

Yes. With the RMM **Auto-Provision** feature, you can create the target virtual server instance. For more information, see ["Bare metal to virtual server migration on a private network with RMM": option 2 of Step 1: Set up and provision VPC and virtual server instance](/docs/cloud-infrastructure?topic=cloud-infrastructure-pv-migration-private-network#set-up-provision-vpc-vsi).
