---

copyright:
  years: 2018, 2021
lastupdated: "2021-03-30"

keywords: cloud environment, virtual server, virtual machine, vm, understanding infrastructure, IaaS model, IT ops admin, on-premises, data center

subcollection: cloud-infrastructure

---

{{site.data.keyword.attribute-definition-list}}

# Understanding IaaS basics
{: #getting-started-tutorial}

As many organizations move to a cloud environment, either on-premises or hosted in data centers, the IT operations administrator's (IT ops admin) role is being redefined. The scope and complexity of this change increases significantly based on the type of environment that your organization wants to deploy.
{: .shortdesc}

Before you moved to the cloud, you worked with an inherently secure environment with systems that are connected to your private LAN or intranet. In a cloud environment, you're now expected to perform the following tasks:

* Procure your system components from "somewhere."
* Understand network implications and security challenges.
* Work with different technologies.  
* Integrate the new environment with tools that are not natively part of the cloud stack.
* Continue to support your internal customers as if you still have an on-premises data center.

{{site.data.keyword.cloud}} is here to support you 100% on your journey. The resources available to you include comprehensive documentation, planning tools, qualified support specialists, and an active user community. Let's get you started on your journey.

![Plan, build, and manage your cloud infrastructure with IBM](https://www.youtube.com/embed/Kmt_odiCWvU){: video output="iframe" data-script="none" id="youtubeplayer" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen}

## Video transcript
{: #video-transcript-it-ops-2}
{: notoc}

Welcome to {{site.data.keyword.cloud_notm}}! As an IT operations administrator, you had your choice of any number of cloud providers. You chose {{site.data.keyword.cloud_notm}} because it's a leader in cloud as a service. We offer superior levels of security, functionality, integration, interoperability, and usability. 

Use the {{site.data.keyword.cloud_notm}} console to get access to our catalog of over 190 unique services across several categories including Security, Compute, Network, Storage, Integration, and Data Management [Click Catalog menu item, then select from the categories listed in the transcript]. In addition to the catalog, there are a variety of tools to help you plan your hosting environment before you begin provisioning it. One tool is the [{{site.data.keyword.cloud_notm}} Design Decision Tool](https://github.com/ibm-cloud-architecture/infrastructure-design-decision-tool), which helps you compare infrastructure designs so you can build the best {{site.data.keyword.cloud_notm}} solution to suit your needs.

After you've planned and designed your infrastructure, use the cost estimator [Click Cost estimator icon then click Review summary] to see how much your infrastructure will cost before placing your order. You can access it by clicking the Cost Estimator icon in the console menu bar.

The IT ops admin journey steps you through the various stages of planning, building, and managing your environment. The journey contains links to the compute, storage, and networking options offered by {{site.data.keyword.cloud_notm}}, and information on the {{site.data.keyword.cloud_notm}} Platform as a Service and Software as a Service.

{{site.data.keyword.cloud_notm}} offers you a strong support network that includes forums and wikis [Click the Support menu option, then find the Ask the community link or Create a case link in the Need more help? section], as well as support specialists to answer your questions. As a member of the {{site.data.keyword.cloud_notm}} IT operations administrator family, you have the support of designers, developers, and other IT ops admins who share the same ideas and risks as you do.

Know that you have the support of {{site.data.keyword.cloud_notm}}, too. Once again, welcome. 

## Cloud service models
{: #cloud-svc-models-2}

Three types of cloud service models exist: Infrastructure as a Service (IaaS), Platform as a Service (PaaS), and Software as a Service (SaaS). Figure 1 explains who does what within each service model. For more information, see [IaaS, PaaS, and SaaS - IBM Cloud service models](https://www.ibm.com/cloud/learn/iaas-paas-saas){: external} .

![Cloud service models.](images/cloud-svc-models.svg "Diagram showing the cloud service models"){: caption="Figure 1. Cloud service models" caption-side="bottom"}

With the IaaS model, your provider is responsible for maintaining the underlying infrastructure only and optionally installing software, such as operating systems, applications, and databases. Your access to the underlying infrastructure is limited, and you're responsible for installing your software or have your service provider install it. You're also responsible for all other maintenance, which includes service packs, virus software, and patches.

With the PaaS model, your provider is responsible for the systems through the operating system and for all infrastructure management, which includes OS patches, hardware repairs, and network settings. You build and maintain the application, and you or your provider can install middleware, including databases or other types. This model is used to develop and test software. For more information, see [A practical guide to platform as a service: What is PaaS](https://www.ibm.com/blogs/cloud-computing/2016/08/10/practical-guide-paas/){: external}.

With the SaaS model, your provider maintains the systems through the actual application. The application is cloud-aware, and users can use different end points, depending on the software provider, to use the software. The cloud provider is responsible for all infrastructure and application management, which includes software updates, hardware repairs, and network settings. This model is often used in pay-as-you-go software licensing models. For more information, see [SaaS applications for business and IT](https://www.ibm.com/cloud/saas){: external} .

## Cloud types
{: #cloud-types-2}

There are three different types of clouds available: public, private, and hybrid. A public cloud includes a shared set of resources that are provisioned to allow access to a company's resources. It is hosted in a multi-tenant environment on a virtual server, and it can be accessed from anywhere.

A private cloud includes resources that are provisioned to allow access to a company's resources. It is hosted on dedicated hardware, such as a bare metal server, and either onsite at the company's office (or across offices) or by a cloud provider. A private cloud can be accessed from anywhere.

A hybrid cloud includes resources that combine the aspects of both public and private clouds. It is hosted both onsite at a company's office (or across offices) and by a cloud provider. A hybrid cloud can be accessed from anywhere.

## Next steps
{: #nextsteps1}

To continue, see [Planning your infrastructure](/docs/cloud-infrastructure?topic=cloud-infrastructure-planning-2).
