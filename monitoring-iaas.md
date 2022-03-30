---

copyright:
  years: 2020, 2022
lastupdated: "2022-03-30"

subcollection: cloud-infrastructure

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:note: .note}
{:new_window: target="_blank"}

# {{site.data.keyword.mon_full_notm}}
{: #monitoring-iaas}

{{site.data.keyword.mon_full_notm}} collects basic Classic infrastructure and VPC virtual server instance metrics such as CPU usage, disk usage, network traffic, and memory. These metrics are stored in {{site.data.keyword.mon_full_notm}}. <!--If you have a Sysdig account, then metrics are displayed for that {{site.data.keyword.mon_full_notm}} instance. -->You can access metrics through the prebuilt dashboard.
{: shortdesc}

{{site.data.keyword.mon_full_notm}} metrics are available only if you use the monitoring full agent. If you provisioned a 'no driver mode' instance, see [{{site.data.keyword.mon_full_notm}} 'no driver mode' metrics](/docs/cloud-infrastructure?topic=cloud-infrastructure-enabling-monitoring-light-no-driver#monitoring-light-metrics).
{: important} 

## Platform metrics overview
{: #platform-metrics-overview}

You can view platform metrics when you enable monitoring services on your {{site.data.keyword.cloud_notm}} platform. A monitoring instance must be configured in a region to monitor these metrics. For more information about enabling Platform metrics, see [Enabling platform metrics](/docs/monitoring?topic=monitoring-platform_metrics_enabling).

Before you enable {{site.data.keyword.mon_full_notm}} on your platform, keep the following information in mind:

* You can configure only one instance of the {{site.data.keyword.mon_full_notm}} service per region to collect platform metrics.

* Platform metrics are regional. Metrics are monitored only from {{site.data.keyword.mon_full_notm}} services that are in the same region of the instance that you want to monitor. 

* Metrics are collected automatically and are available for monitoring through the {{site.data.keyword.mon_full_notm}}-enabled instance. 

### Metrics available by virtual server generation
{: #metrics-by-server-generation}

* To view Classic virtual server monitoring metrics, see [Classic virtual server instance metrics definitions](/docs/virtual-servers?topic=virtual-servers-monitoring-classic-metrics).
* To view VPC virtual server monitoring metrics, see [VPC virtual server instances metrics definitions](/docs/vpc?topic=vpc-vpc-monitoring-metrics)

## Basic monitoring
{: #basic-monitoring}

You use basic monitoring to initiate service and slow pings to make sure that the device is online and responsive.

| Basic monitoring service types | Description |
| ----- | ----- |
| SERVICE PING | Test ping to address |
| SLOW PING | Test ping to address - doesn't fail on slow server response due to high latency or high server load |
{: caption="Table 1. Basic monitoring service types" caption-side="bottom"} 	

If an echo isn't received in the allotted timeframe (1 second for service pings, 5 seconds for slow pings), an alert is sent to the email address on the account. A status of **Up** in the **Status** field indicates that an echo was received, while **Down** indicates that the echo wasn't received.
{: tip}

### Host ping and TCP service monitoring
{: #basic-monitoring-addons}

**Host ping** is the default basic monitoring option. If you want more basic monitoring options, you need to select **Host ping and TCP service monitoring**. This selection gives you access to the following add-on monitoring services.

| Add-on monitoring service types | Description |
| ----- | ----- |
| DNS | Test generic `nslookup` on address |
| DNS CUSTOM | Test `nslookup` for specified domain on address |
| FTP | Test for FTP connection to port 21 on address |
| HTTP | Test for HTTP connection to port 80 on address |
| HTTP CUSTOM | Test for HTTP connection to port 80 on address, checks for given response text | 
| HTTPS | Test for HTTP connection to port 443 on address |
| IMAP | Test for IMAP connection on address |
| LDAP | Test for LDAP connection on address |
| NNTP | Test for NNTP connection on address |
| POP | Test for POP connection on address |
| SMTP | Test for SMTP connection on address |
| SSH | Test for SSH connection to port 22 on address |
| TCP CUSTOM | Test for TCP connection to specified port on address |
| TELNET | Test for telnet connection to port 23 on address |
| UDP SIP | Test for UDP connection to specified port on address |
{: caption="Table 2. Add-on monitoring service types" caption-side="bottom"}

## Viewing configured monitors
{: #view-configured-monitors}

To view configured monitors, follow these steps:
1. Go to your console's device menu. For more information, see [Navigating to devices](/docs/virtual-servers?topic=virtual-servers-navigating-devices).
2. From the **Devices** menu, select **Device List** and select the device.
3. Click the **Monitoring** tab. All current pings are viewable on the landing page.

The **Monitoring** tab is only visible if at least one monitor is configured.
{: note}

## Next steps
{: #monitoring-next-steps}

For more information about monitoring your infrastructure and environments, see the following information.

* [Getting started with {{site.data.keyword.mon_full_notm}}](/docs/monitoring?topic=monitoring-getting-started).
* If you're ready to provision a monitoring instance, see [Provisioning an instance](/docs/monitoring?topic=monitoring-provision).
* If you're interested in monitoring plans and pricing, see [Pricing](/docs/monitoring?topic=monitoring-pricing_plans).
