---

copyright:
  years:  2020
lastupdated: "2020-09-15"

keywords: Sysdig, IBM Cloud monitoring, platform metrics, metrics, Sysdig metrics, monitoring

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

# {{site.data.keyword.mon_full_notm}} monitoring metrics
{: #sysdig-monitoring-metrics}

{{site.data.keyword.mon_full_notm}} is operated by Sysdig in partnership with {{site.data.keyword.IBM_notm}} and collects basic Classic (Gen 1) and VPC (Gen 2) virtual server instance metrics such as CPU usage, disk usage, network traffic, and memory. These metrics are stored in Sysdig. If you have a Sysdig account, then metrics are displayed for that Sysdig instance. You can access metrics through the prebuilt dashboard.
{:shortdesc}

Sysdig metrics are available only if you use the {{site.data.keyword.mon_full_notm}} full agent. If you provisioned a 'no driver mode' instance, see [{{site.data.keyword.mon_full_notm}} 'no driver mode' metrics](/docs/cloud-infrastructure?topic=cloud-infrastructure-enabling-sysdig-light-no-driver#sysdig-light-metrics).
{:important} 

## Platform metrics overview
{: #platform-metrics-overview}

You can view platform metrics when you enable Sysdig services on your {{site.data.keyword.cloud_notm}} platform. A Sysdig instance must be configured in a region to monitor these metrics. For more information about enabling Platform metrics, see [Enabling platform metrics](https://test.cloud.ibm.com/docs/Monitoring-with-Sysdig?topic=Sysdig-platform_metrics_enabling).

Before you enable Sysdig on your platform, keep the following information in mind:

* You can configure only one instance of the {{site.data.keyword.mon_full_notm}} service per region to collect platform metrics.

* Platform metrics are regional. Metrics are monitored only from Sysdig services that are in the same region of the instance that you want to monitor. 

* Metrics are collected automatically and are available for monitoring through the Sysdig-enabled instance. 

## Metrics available by virtual server generation
{: #metrics-by-server-generation}

* To view Classic virtual server Sysdig metrics, see [Classic virtual server instance metrics definitions](/docs/cloud-infrastructure?topic=cloud-infrastructure-classic-sysdig-metrics).
* To view VPC virtual server Sysdig metrics, see [VPC virtual server instances metrics definitions](/docs/cloud-infrastructure?topic=cloud-infrastructure-vpc-sysdig-metrics)
