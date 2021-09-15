---

copyright:
  years: 2021
lastupdated: "2021-07-28"

keywords: 

subcollection: cloud-infrastructure

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:faq: data-hd-content-type='faq'}

# FAQs for RackWare
{: rackware-faqs}

## What is RMM server?
{: #what-is-rmm-server}
{: faq}

RMM server is a software appliance that is offered by RackWare that replatforms your server from an IBM Cloud classic physical bare metal server to am IBM Cloud VPC virtual server instance.

## Where can I find more information about RackWare RMM server?
{: #where-to-find-more-information}
{: faq}

For RMM server overview information, see [RackWare's Cloud Migration documentation](https://www.rackwareinc.com/cloud-migration){: external}. For RMM server usage guide information, see [RackWare RMM User's Guide for IBM Cloud](https://www.rackwareinc.com/rackware-rmm-users-guide-for-ibm-cloud){: external}.

## How do I install the RMM server?
{: #how-to-install}
{: faq}

This software is available in the IBM Cloud catalog in the **Migration Tools** category. After you fill out the appropriate information, a virtual server instance is installed in a new VPC with the RMM already installed.

## Is this service free?
{: #how-much-is-service}
{: faq}

You need to bring your own license (BYOL), which you must purchase directly from RackWare. For more information or inquiries, contact [sales@rackwareinc.com](mailto:sales@rackwareinc.com). However, IBM is offering promotional licensing at no cost for three months for three reusable concurrent migration licenses.

## What is the promotional license?
{: #promotional-license}
{: faq}

The promotional license is valid only for per-account and first-time users of RackWare RMM server. After the three-month period, you need to purchase the license directly from RackWare.

## How does the reusable concurrent license work?
{: #reusable-concurrent-license}
{: faq}

IBM and RackWare have put together a special license model. Typically, a license is a one-time use for each server migration. Whereas for IBM purposes, after a completed migration, the license can be reused for a different server migration. The original host that the license was assigned to just needs to be deleted from the RMM server database. 

The number of concurrent migrations that can occur is limited up to the number of licenses procured.

## How do I obtain and activate my promotional license?
{: #obtain-activate-promotional-license}
{: faq}

You can retrieve the promotional license through the discovery script that is part of the RMM software appliance. For more information on how to use the script, see [Physical to virtual migration on a private network using RackWare RMM](/docs/cloud-infrastructure?topic=cloud-infrastructure-pv-migration-private-network).

## What are the considerations or limitations for the physical to virtual migration?
{: #considerations-limitations-migration}
{: faq}

For more information on considerations and limitations of the physical to virtual migration, see [Physical to virtual migration overview](/docs/cloud-infrastructure?topic=cloud-infrastructure-pv-migration-overview).

## Is the migration intrusive?
{: #migration-intrusive}
{: faq}

In most cases, the migration is not intrusive. The migration can be done when the server is up and running. The source server does need some free space to do an image capture of the machine. In addition, RMM does require SSH (port 22) reachability to both server and target to perform the migration. The CPU consumption for image capture and copying to the target is minimal.