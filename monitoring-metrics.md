---

copyright:
  years:  2020, 2021
lastupdated: "2021-04-01"

keywords: IBM Cloud monitoring, platform metrics, metrics, monitoring metrics, monitoring

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
{: #monitoring-metrics}

{{site.data.keyword.mon_full_notm}} collects basic Classic (Gen 1) and VPC (Gen 2) virtual server instance metrics such as CPU usage, disk usage, network traffic, and memory. You can access metrics through the prebuilt {{site.data.keyword.mon_full_notm}} dashboard.
{:shortdesc}

Monitoring metrics are available only if you use the monitoring full agent. If you provisioned a 'no driver mode' instance, see [Monitoring 'no driver mode' metrics](/docs/cloud-infrastructure?topic=cloud-infrastructure-enabling-monitoring-light-no-driver#monitoring-light-metrics).
{:important} 

## Platform metrics overview
{: #platform-metrics-overview}

You can view platform metrics when you enable monitoring services on your {{site.data.keyword.mon_full_notm}} platform. A monitoring instance must be configured in a region to monitor these metrics. For more information about enabling Platform metrics, see [Enabling platform metrics](https://test.cloud.ibm.com/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-platform_metrics_enabling).

Before you enable monitoring on your platform, keep the following information in mind:

* You can configure only one instance of the monitoring service per region to collect platform metrics.

* Platform metrics are regional. Metrics are monitored only from monitoring services that are in the same region of the instance that you want to monitor. 

* Metrics are collected automatically and are available for monitoring through the {{site.data.keyword.mon_full_notm}}-enabled instance. 

## Metrics available by virtual server generation
{: #metrics-by-server-generation}

* To view Classic virtual server monitoring metrics, see [Classic virtual server instance metrics definitions](/docs/cloud-infrastructure?topic=cloud-infrastructure-classic-monitoring-metrics).
* To view VPC virtual server monitoring metrics, see [VPC virtual server instances metrics definitions](/docs/cloud-infrastructure?topic=cloud-infrastructure-vpc-monitoring-metrics)
