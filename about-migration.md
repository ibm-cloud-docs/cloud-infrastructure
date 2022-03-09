---

copyright:
  years:  2020, 2021, 2022
lastupdated: "2022-04-08"

keywords: migration, migrate, migrating, migrate infrastructure

subcollection: cloud-infrastructure

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}

# About migration
{: #about-migration-infra}

The driver to migrate might come from business factors such as cost reduction, consolidation, or data center closure. You also might want to migrate to be more cloud native or adopt new technologies such as VPC. Regardless of the reason, migration can be as simple as migrating a single virtual server instance, or it can be as complex as migrating a piece of your application to a more complex environment where you need to migrate an entire pod or data center with all of the underlying components. 

Use the following steps as guidance to frame your migration journey. 

## Step 1. Assess
{: #step1-assess}

Do you need to migrate instances to a new data center due to data center closures? Do you want to migrate your entire classic infrastructure to VPC? Assess your situation and identify your existing infrastructure to determine what components you have, how they are configured, and what you want to migrate.

Not only do you need to assess your current environment, but you need to assess the target environment to understand the capabilities, support, and differences between the two environments, if applicable. In this assessment step, you can get a general idea of the complexity of the migration so that you can develop a migration strategy.

## Step 2. Plan
{: #step2-plan}

Spend time planning for your migration by analyzing your current infrastructure and determining whether your resources and components can be migrated or if there would be any disruptions to your current business environment. Understanding how much time is needed to migrate and whether it needs to be done in stages can help you simplify the migration journey.

## Step 3. Migrate
{: #step3-migrate}

After you've spent time assessing your existing infrastructure and planning for your migration, you can migrate your resources and components with ease and confidence. Depending on your migration use case, there might be tools available to help you with the migration process.

## Step 4. Validate
{: #step4-validate}

After you migrate your resources and components into your target infrastructure, and before you make your infrastructure live, you will want to validate and test your environment to ensure it is ready for production. This might also entail updating your DNS and global load balancers, routes, or retiring old services.

## Migration use cases
{: #migration-use-cases}

### Classic to classic 
{: #classic-to-classic}

* Virtual server instances - Your current classic data center is closing or you want to migrate to a different classic data center due to business requirements. For more information, see [Migrating a virtual server between classic data centers](/docs/cloud-infrastructure?topic=virtual-servers-migrating-vsi-new-datacenter).
* Bare metal servers - By using a custom image template, you can capture a bare metal image to replicate its configuration to order more bare metal servers with the same configurations. For more information, see [About bare metal customer image templates](/docs/cloud-infrastructure?topic=bare-metal-getting-started-bm-custom-image-templates).

### Classic to VPC
{: #classic-to-vpc}

If you want to migrate your IBM Cloud classic infrastructure to VPC, you can use {{site.data.keyword.vpc-plus-migration}}. {{site.data.keyword.vpc-plus-migration}} is a third-party, software-based migration-as-a-service solution for migrating components from classic infrastructure to your VPC. {{site.data.keyword.vpc-plus-migration}} allows you to discover and choose resources for migration, create, and set up those resources in your VPC environment. You can also run and manage your VPC environment from within the tool. For more information, see [Getting started with {{site.data.keyword.vpc-plus-migration}}](/docs/cloud-infrastructure?topic=wanclouds-vpc-plus-getting-started-tutorial).

<!--### Gen 1 to Gen 2 
{: #gen1-to-gen2}-->

<!--If you are currently using the VPC (Gen 1) environment, you need to migrate your environment to VPC (Gen 2). The fastest way to do this is to use a vendor that IBM has engaged who will work with you to provide a migration path specific to your infrastructure. Contact your IBM representative for more information.-->

<!--You can also choose to perform this migration with your own resources by following the steps in our docs. If you choose to complete this migration, refer to [Instructions for Migrating from VPC (Gen 1) to VPC (Gen 2)](/docs/cloud-infrastructure?topic=vpc-on-classic-migrating-vpc).-->
