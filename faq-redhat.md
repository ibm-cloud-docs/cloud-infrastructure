---

copyright:
  years: 2024
lastupdated: "2024-11-15"

subcollection: cloud-infrastructure

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQs about Red Hat and {{site.data.keyword.cloud}} infrastructure
{: #faqs-for-red-hat-ibm-cloud}

Review frequently asked questions (FAQs) for using Red Hat and {{site.data.keyword.cloud_notm}} infrastructure.

## What versions of Red Hat Enterprise Linux are available in {{site.data.keyword.cloud_notm}}?
{: #faq-versions-available-red-hat-ibm-cloud}
{: faq}

* See [Lifecycle for operating systems and add-ons - Red Hat Enterprise Linux (RHEL)](/docs/bare-metal?topic=bare-metal-product-lifecycle-classic#rhel-classic) for the Red Hat versions that {{site.data.keyword.cloud_notm}} Classic infrastructure supports.
* See [Lifecycle for guest operating systems - Red Hat Enterprise Linux (RHEL)](/docs/vpc?topic=vpc-guest-os-lifecycle#rhel) for the Red Hat versions that {{site.data.keyword.vpc_short}} supports.

## How are updates applied to Red Hat Enterprise Linux on {{site.data.keyword.cloud_notm}}?
{: #faq-updates-applied-red-hat-ibm-cloud}
{: faq}

When Red Hat releases an update, IBM stock images pull updates from Red Hat official repositories - repositories that are maintained by Red Hat.

## What support can I expect from {{site.data.keyword.cloud_notm}} for Red Hat products?
{: #faq-red-hat-ibm-support}
{: faq}

For Red Hat software that is purchased natively on {{site.data.keyword.cloud_notm}}, [support](/docs/account?topic=account-gettinghelp) is the first point of contact for any issue with a Red Hat software-related issue.

IBM provides three support plans.

* Basic
* Advanced
* Premium.

For more information, see [IBM support plans](/docs/account?topic=account-support-plans).

## How does {{site.data.keyword.cloud_notm}} and Red Hat work together to solve issues?
{: #faq-support-red-hat-and-ibm-cloud}
{: faq}

If Red Hat involvement is required to solve a potential issue, {{site.data.keyword.cloud_notm}} [support](/docs/account?topic=account-gettinghelp) engages Red Hat support. For client BYOL issues, you need to contact [Red Hat support](https://www.redhat.com/en/services/support){: external}.

## What is the level of ownership between {{site.data.keyword.cloud_notm}} and customers who use Red Hat software?
{: #faq-owner-responsibilites-ibm-cloud-red-hat}
{: faq}

{{site.data.keyword.cloud_notm}} provides the cloud infrastructure and Red Hat stock images. {{site.data.keyword.cloud_notm}} [support](/docs/account?topic=account-gettinghelp) covers any issue with the cloud infrastructure and the stock images - such as driver mismatch or licensing. Issues that are related to image licensing are the responsibility of the customer. Custom images (customer-owned images) are also the responsibility of the customer.

## Does IBM Cloud support Bring Your Own License (BYOL) for Red Hat operating systems?
{: #faq-byol-red-hat-ibm-cloud}
{: faq}

Yes. {{site.data.keyword.cloud_notm}} supports BYOL. You need to contact [Red Hat support](https://www.redhat.com/en/services/support){: external} for any issues with the image.

## Am I charged for BYOL?
{: #faq-byol-charges-red-hat-ibm-cloud}
{: faq}

If you use the BYOL option, you are charged by IBM for only the {{site.data.keyword.cloud_notm}} infrastructure.

## What is the end of service (EOS) policy for Red Hat OS in {{site.data.keyword.cloud_notm}}?
{: #faq-eos-red-hat-ibm-cloud}
{: faq}

In the lifecycle of an operating system, EOS is the last date that {{site.data.keyword.cloud_notm}} delivers standard support for a version or release of a product. {{site.data.keyword.cloud_notm}} [VPC EOS dates](/docs/vpc?topic=vpc-guest-os-lifecycle) and [Classic EOS dates](/docs/bare-metal?topic=bare-metal-product-lifecycle-classic) align with the Red Hat EOS dates.

## Can I provision virtual servers in {{site.data.keyword.cloud_notm}} by using EOS software?
{: #faq-provision-eos-red-hat-ibm-cloud}
{: faq}

No. You can't provision a virtual server with software that reached its EOS date. Customers can use the existing EOS software that was provisioned before the EOS date at their own risk.

For more information about EOS, see [VPC - End of support operating system considerations](/docs/vpc?topic=vpc-eos-os-considerations-intro) and [Classic - End of support for operating systems considerations](/docs/bare-metal?topic=bare-metal-eos-os-considerations-bm-classic-intro).

## Where can I find the EOS date for Red Hat servers that run in {{site.data.keyword.cloud_notm}}?
{: #faq-eos-dates-red-hat-ibm-cloud}
{: faq}

For more information about EOS dates for Red Hat software, see [Bare Metal Servers for Classic - Lifecycle for operating systems and add-ons](/docs/bare-metal?topic=bare-metal-product-lifecycle-classic) and [Virtual Private Cloud (VPC) - Lifecycle for guest operating systems](/docs/vpc?topic=vpc-guest-os-lifecycle).

## What if a customer wants to keep operating EOS software?
{: #faq-using-eos-red-hat-ibm-cloud}
{: faq}

If the virtual server is provisioned before the EOS date, customer can continue to use EOS software at their own risk while no bug and security fixes are available.
