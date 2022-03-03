---

copyright:
  years:  2020, 2021
lastupdated: "2021-04-02"

keywords: monitoring plan, monitoring no driver, monitoring plans, basic monitoring, monitoring agent, monitoring tiers

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
{:external: .external}

# {{site.data.keyword.mon_full_notm}} agents and plans
{: #monitoring-agents-and-plans}

Before you provision an {{site.data.keyword.mon_full_notm}} instance, you need to understand your monitoring options. 
{: shortdesc}

## Monitoring plans
{: #monitoring-plans}

{{site.data.keyword.cloud_notm}} monitoring offers different plans. The following plans offer different solutions at different prices.

1. **Basic monitoring** is used to initiate service and slow pings to make sure that a device is online and responsive. <!--If an echo isn't received in the allotted time (1 second for service pings, 5 seconds for slow pings), an alert is sent to the email address on the account. A status of `Up` in the status field indicates that an echo was received, while `Down` indicates that the echo wasn't received.-->

2. **Full agent graduated tier** offers the full {{site.data.keyword.mon_full_notm}} experience without limiting what metrics are available and runs your agents in a non resource-restrictive environment.

3. **Full agent 'no driver mode'** offers a limited set of metrics and runs your agents in a non resource-restrictive environment.

4. **Light agent graduated tier** <!--(free trial?)--> offers the full {{site.data.keyword.mon_full_notm}} experience and runs your agents in a resource-restrictive environment.

5. **Light agent 'no driver mode'** <!--(free trial?)--> offers a limited set of metrics and runs your agents in a resource-restrictive environment.

{{site.data.keyword.mon_full_notm}} 'no driver mode' is offered at a lower cost per month. This price adjustment comes into effect after you enable the 'no driver mode'. For more information about enabling 'no driver mode', see [Enabling IBM Cloud Monitoring 'no driver mode'](/docs/cloud-infrastructure?topic=cloud-infrastructure-enabling-monitoring-light-no-driver).
{: note}

Graduated tiers offer a tiered pricing structure that is based on your time-series usage and billed either hourly or monthly. For information about the graduated tier plan prices, see [{{site.data.keyword.mon_full_notm}} pricing](/docs/monitoring?topic=monitoring-pricing_plans#pricing_plans)
{: note}
