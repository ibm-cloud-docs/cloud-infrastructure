---

copyright:
  years: 2020
lastupdated: "2020-11-03"

keywords: infrastructure, monitoring

subcollection: cloud-infrastructure

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:note: .note}
{:new_window: target="_blank"}

# IBM Cloud monitoring services
{: #monitoring}

After you have your infrastructure and environments up and running, you are ready to set up your monitoring service. You can choose an {{site.data.keyword.mon_full_notm}} plan that has different pricing options.
{: shortdesc}

<!--## Basic monitoring with initiate service and slow pings
{: #basic-monitoring}-->

<!--You use basic monitoring to initiate service and slow pings to make sure that the device is online and responsive.
If an echo isn't received in the allotted time frame (1 second for service pings, 5 seconds for slow pings), an alert is sent to the email address on the account. A status of **Up** in the **Status** field indicates that an echo was received, while **Down** indicates that the echo wasn't received.
To view configured monitors, follow these steps:
1. Go to your console's device menu. For more information, see [Navigating to devices](/docs/virtual-servers?topic=virtual-servers-navigating-devices).
2. From the **Devices** menu, select **Device List** and select the device.
3. Click the **Monitoring** tab. All current pings are viewable on the landing page.-->

## Getting started with Sysdig
{: #sysdig-monitoring}

{{site.data.keyword.cloud}} offers {{site.data.keyword.mon_full_notm}} to keep you aware of any issues with your devices. 
{{site.data.keyword.mon_full_notm}} gives you insight into the performance and health of your applications, services, and 
platforms. Your administrators, DevOps teams, and developers have full stack telemetry with advanced features to help them 
monitor and troubleshoot, define alerts, and design custom dashboards.

For information about setting up an {{site.data.keyword.mon_full_notm}} monitoring agent, see [Getting started with {{site.data.keyword.mon_full_notm}}](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-getting-started).

The **Monitoring** tab is only visible if at least one monitor is configured.
{:note}

## Next steps
{: #monitoring-next-steps}

For more information about monitoring your infrastructure and environments, see the following information.

* If you're ready to provision a Sysdig instance, see [Provisioning an instance](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-provision).
* If you're interested in {{site.data.keyword.mon_full_notm}} plans, see [{{site.data.keyword.mon_full_notm}} agents and plans](/docs/cloud-infrastructure?topic=cloud-infrastructure-sysdig-agents-and-plans).
* If you're interested in {{site.data.keyword.mon_full_notm}} pricing information, see [Pricing](/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-pricing_plans).
