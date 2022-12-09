---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-09"

keywords: ip, range, firewall, network, traffic, security

subcollection: cloud-infrastructure

---

{{site.data.keyword.attribute-definition-list}}

# IBM Cloud IP ranges
{: #ibm-cloud-ip-ranges}

A frequently asked question is, "What IP ranges do I allow through the firewall?" The following tables contain the full range of IPs to use with these IBM firewalls and appliances.
{: shortdesc}

* IBM Cloud Juniper vSRX Standard
* IBM Virtual Router Appliance
* Fortigate Security Appliance 10 Gbps
* IBM Security Groups
* Hardware Firewall

To identify potential conflicts between IP ranges in your on-premises environment(s) and IP ranges used in {{site.data.keyword.cloud_notm}}, you can search in the [IP Ranges Calculator tool](https://ibm.biz/cidr-calculator). The tool allows you to also download the IP ranges below in JSON format. Disclaimer: This is a community tool that is updated based on support availability.
{: important}

## Front-end (public) network
{: #front-end-network}

Ports to allow:
- All TCP/UDP ports
- ICMP – ping (for support troubleshooting and monitoring)

|Data center|City|IP range|
|---|---|---|
|ams03|Amsterdam |159.8.198.0/23|
|che01|Chennai |169.38.118.0/23|
|dal05|Dallas |173.192.118.0/23|
|dal08|Dallas |192.255.18.0/24|
|dal09|Dallas |198.23.118.0/23|
|dal10|Dallas |169.46.118.0/23|
|dal12|Dallas |169.47.118.0/23|
|dal13|Dallas |169.48.118.0/24|
|fra02|Frankfurt |159.122.118.0/23|
|fra04|Frankfurt |161.156.118.0/24|
|fra05|Frankfurt |149.81.118.0/23|
|lon02|London |5.10.118.0/23|
|lon04|London |158.175.127.0/24|
|lon05|London |141.125.118.0/23|
|lon06|London |158.176.118.0/23|
|mil01|Milan |159.122.138.0/23|
|mon01|Montreal |169.54.118.0/23|
|osa21|Osaka |163.68.118.0/24|
|osa22|Osaka |163.69.118.0/24|
|osa23|Osaka |163.73.118.0/24|
|par01|Paris |159.8.118.0/23|
|sao01|São Paulo |169.57.138.0/23|
|sjc01|San Jose |50.23.118.0/23|
|sjc03|San Jose |169.45.118.0/23|
|sjc04|San Jose |169.62.118.0/24|
|sng01|Jurong East |174.133.118.0/23|
|syd01|Sydney |168.1.18.0/23|
|syd04|Sydney |130.198.118.0/23|
|syd05|Sydney |135.90.118.0/23|
|tok02|Tokyo |161.202.118.0/23|
|tok04|Tokyo |128.168.118.0/23|
|tok05|Tokyo |165.192.118.0/23|
|tor01|Toronto |158.85.118.0/23|
|tor04|Toronto |163.74.118.0/23|
|tor05|Toronto |163.75.118.0/23|
|wdc01|Washington D.C. |208.43.118.0/23|
|wdc03|Washington D.C. |192.255.38.0/24|
|wdc04|Washington D.C. |169.55.118.0/23|
|wdc06|Washington D.C. |169.60.118.0/23|
|wdc07|Washington D.C. |169.61.118.0/23|
{: caption="Table 1: Front-end (public) network" caption-side="bottom"}   

## Load balancer IPs
{: #load-balancer-ips}

|Data center|City|IP range|
|---|---|---|
|ams03|Amsterdam|159.8.197.0/24|
|che01|Chennai |169.38.117.0/24|
|dal05|Dallas |50.23.203.0/24  \n 108.168.157.0/24  \n 173.192.117.0/24  \n 192.155.205.0/24|
|dal09|Dallas |169.46.187.0/24  \n 198.23.117.0/24|
|dal10|Dallas |169.46.117.0/24|
|dal12|Dallas |169.47.117.0/24|
|dal13|Dallas |169.48.117.0/24|
|fra02|Frankfurt |159.122.117.0/24|
|fra04|Frankfurt |161.156.117.0/24|
|fra05|Frankfurt |149.81.117.0/24|
|lon02|London |5.10.117.0/24|
|lon04|London |158.175.117.0/24|
|lon05|London |141.125.117.0/24|
|lon06|London |158.176.117.0/24|
|mil01|Milan |159.122.137.0/24|
|mon01|Montreal |169.54.117.0/24|
|par01|Paris |159.8.117.0/24|
|sao01|São Paulo |169.57.137.0/24|
|sjc01|San Jose |50.23.117.0/24|
|sjc03|San Jose |169.45.117.0/24|
|sng01|Jurong East |174.133.117.0/24|
|syd01|Sydney |168.1.17.0/24|
|syd04|Sydney |130.198.117.0/24|
|syd05|Sydney |135.90.117.0/24|
|tok02|Tokyo |161.202.117.0/24|
|tok04|Tokyo |128.168.117.0/24|
|tok05|Tokyo |165.192.117.0/24|
|tor01|Toronto |158.85.117.0/24|
|wdc01|Washington D.C. |50.22.248.0/25  \n 169.54.27.0/24  \n 198.11.250.0/24  \n 208.43.117.0/24|
|wdc04|Washington D.C. |169.55.117.0/24|
|wdc06|Washington D.C. |169.60.117.0/24|
|wdc07|Washington D.C. |169.61.117.0/24|
{: caption="Table 2: Load balancer IPs" caption-side="bottom"}   

## Back-end (private) network
{: #back-end-network}

IP block: Your private IP block for server-to-server communications (`10.X.X.X/X`)

Ports to allow:
- All TCP/UDP ports
- ICMP – ping (for support troubleshooting and monitoring)

### Customer private network space
{: #customer-private-network-space}

| City| Data center | Pod | IP range |
|---|---|---|---|
| Amsterdam| ams03 | BCR01 | 10.1.243.0/24  \n  10.3.48.0/24  \n 10.3.139.0/24  \n 10.136.0.0/15 |
| | ams03 | BCR02 | 10.175.0.0/16  \n 10.214.0.0/16 |
| Chennai | che01 | BCR01 | 10.3.58.0/24  \n 10.162.0.0/15  \n 10.200.27.0/24 |
| Dallas| dal05 | BCR01 | 10.0.40.0/22  \n 10.1.151.0/24  \n 10.3.60.0/24  \n 10.40.0.0/14  \n 10.251.0.0/23  \n 10.251.64.0/23 |
| | dal05 | BCR03 | 10.80.0.0/14  \n 10.251.4.0/23  \n 10.251.68.0/23 |
| | dal05 | BCR04 | 10.84.0.0/16  \n 10.86.0.0/16  \n 10.251.70.0/23 |
| | dal09 | BRC01 | 10.2.123.0/24  \n 10.2.244.0/24  \n 10.3.16.0/24  \n 10.120.0.0/15 |
| | dal09 | BCR02 | 10.142.0.0/15 |
| | dal09 | BCR03 | 10.98.0.0/15  \n 10.152.0.0/15 |
| | dal09 | BCR04 | 10.154.0.0/15 |
| | dal09 | BCR05 | 10.172.0.0/16 |
| | dal09 | BCR06 | 10.173.0.0/16 |
| | dal10 | BCR01 | 10.0.192.0/26  \n 10.3.62.0/24  \n 10.3.247.0/24  \n 10.176.0.0/15  \n 10.200.91.0/24 |
| | dal10 | BCR02 | 10.0.192.64/26  \n 10.171.0.0/16 |
| | dal10 | BCR03 | 10.93.0.0/16 |
| | dal10 | BCR04 | 10.5.0.0/16  \n 10.23.0.0/16  \n 10.38.0.0/16  \n 10.94.0.0/15  \n 10.221.0.0/16 |
| | dal12 | BCR01 | 10.3.2.0/24  \n 10.184.0.0/15  \n 10.200.123.0/24  \n 10.241.0.0/16 |
| | dal12 | BCR02 | 10.48.0.0/16  \n 10.74.0.0/16 |
| | dal13 | BCR01 | 10.3.3.0/24  \n 10.186.0.0/15  \n 10.200.139.0/24  \n 10.208.0.0/16 |
| | dal13 | BCR02 | 10.36.0.0/16  \n 10.73.0.0/16  \n 10.209.0.0/16  \n 10.220.0.0/16 |
| | dal13 | BCR03 | 10.37.0.0/16 |
| Frankfurt | fra02 | BCR01 | 10.2.240.0/24  \n 10.3.38.0/24  \n 10.3.91.0/24  \n 10.134.0.0/15  \n 10.201.155.0/24 |
| | fra02 | BCR02 | 10.85.0.0/16  \n 10.194.0.0/16 |
| | fra02 | BCR03 | 10.20.0.0/16  \n 10.215.0.0/16 |
| | fra04 | BCR01 | 10.3.13.0/24  \n 10.21.0.0/16  \n 10.75.0.0/16  \n 10.201.123.0/24  \n 10.240.0.0/16 |
| | fra05 | BCR01 | 10.3.15.0/24  \n 10.13.0.0/16  \n 10.123.0.0/16  \n 10.201.139.0/24 |
| | fra05  | BCR02 | 10.174.0.0/16 |
| London| lon02 | BCR01 | 10.1.219.0/24  \n 10.3.40.8/29  \n 10.3.40.16/28  \n 10.3.40.32/27  \n 10.3.40.64/26  \n 10.3.40.128/25  \n 10.112.0.0/15  \n 10.201.107.0/24 |
| | lon02 | BCR02 | 10.164.0.0/15 |
| | lon04 | BCR01 | 10.3.7.0/24  \n 10.45.0.0/16  \n 10.201.43.0/24  \n 10.222.0.0/16 |
| | lon05 | BCR01 | 10.3.21.0/24  \n 10.196.0.0/15  \n 10.201.59.0/24 | 
| | lon06 | BCR01 | 10.3.11.0/24  \n 10.72.0.0/16  \n 10.201.75.0/24  \n 10.242.0.0/16 |
| Milan | mil01 | BCR01 | 10.1.241.0/24  \n 10.3.50.8/29  \n 10.3.50.16/28  \n 10.3.50.32/27  \n 10.3.50.64/26  \n 10.3.50.128/25  \n 10.3.155.0/24  \n 10.144.0.0/15 |
| Montreal| mon01 | BCR01 | 10.3.46.8/29  \n 10.3.46.16/28  \n 10.3.46.32/27  \n 10.3.46.64/26  \n 10.3.46.128/25  \n 10.3.123.0/24  \n 10.140.0.0/15 |
| Osaka | osa21 | BCR01 | 10.3.59.0/24  \n 10.8.0.0/16  \n 10.201.246.0/24 |
| | osa22 | BCR01 | 10.3.57.0/24  \n 10.9.0.0/16  \n 10.201.247.0/24 |
| | osa23 | BCR01 | 10.3.55.0/24  \n 10.10.0.0/16  \n 10.201.248.0/24 |
| Paris | par01 | BCR01 | 10.2.155.0/24  \n 10.2.243.0/24  \n 10.3.26.8/29  \n 10.3.26.16/28  \n 10.3.26.32/27  \n 10.3.26.64/26  \n 10.3.26.128/25  \n 10.126.0.0/15 |
| | par04 | BCR01 | 10.3.27.0/24  \n 10.217.0.0/16 |
| | par05 | BCR01 | 10.3.25.0/24  \n 10.218.0.0/16 |
| | par06 | BCR01 | 10.3.24.0/24  \n 10.219.0.0/16 |
| São Paulo | sao01 | BCR01 | 10.3.54.0/29  \n 10.3.54.8/29  \n 10.3.54.16/28  \n 10.3.54.32/27  \n 10.3.54.64/26  \n 10.3.54.128/25  \n 10.150.0.0/15  \n 10.200.11.0/24 |
| | sao04 | BCR01 | 10.3.49.0/24  \n 10.14.0.0/16  \n 10.201.251.0/24 |
| | sao05 | BCR01 | 10.3.47.0/24  \n 10.15.0.0/16  \n 10.201.252.0/24 |
| San Jose | sjc01 | BCR01 | 10.1.203.0/24  \n 10.3.14.0/24  \n 10.52.0.0/14  \n 10.251.6.0/23  \n 10.251.72.0/23 |
| | sjc01 | BCR03 | 10.122.0.0/16 |
| | sjc03 | BCR01 | 10.3.56.0/24  \n 10.3.187.0/24  \n 10.160.0.0/15 |
| | sjc03 | BCR02 | 10.168.0.0/15 |
| | sjc04 | BCR01 | 10.3.9.0/24  \n 10.87.0.0/16  \n 10.201.91.0/24 |
| Jurong East | sng01 | BCR01 | 10.2.43.0/24  \n 10.3.20.0/24  \n 10.64.0.0/16  \n 10.66.0.0/15  \n 10.251.12.0/23  \n 10.251.86.0/23 |
| | sng01| BCR02 | 10.116.0.0/15 |
| Sydney | syd01 | BCR01 | 10.3.44.0/24  \n 10.3.107.0/24  \n 10.138.0.0/15  \n 10.202.43.0/24 |
| | syd01 | BCR02 | 10.210.0.0/16 |
| | syd04 | BCR01 | 10.3.6.0/24  \n 10.63.0.0/16  \n 10.201.27.0/24 |
| | syd05 | BCR01 | 10.3.23.0/24  \n 10.195.0.0/16  \n 10.207.27.0/24 |
| Tokyo | tok02 | BCR01 | 10.2.241.0/24  \n 10.3.30.0/24  \n 10.3.75.0/24  \n 10.3.242.0/24  \n 10.129.0.0/16  \n 10.132.0.0/15  \n 10.201.171.0/24 |
| | tok02 | BCR02 | 10.212.0.0/16 |
| | tok04 | BCR01 | 10.3.17.0/24  \n 10.192.0.0/16  \n 10.201.187.0/24 |
| | tok05 | BCR01 | 10.3.19.0/24  \n 10.193.0.0/16  \n 10.201.203.0/24 |
| | tor01 | BCR01 | 10.2.59.0/24  \n 10.3.42.0/24  \n 10.114.0.0/15  \n 10.202.107.0/24 |
| | tor01 | BCR02 | 10.166.0.0/15 |
| | tor04 | BCR01 | 10.1.2.0/24  \n 10.3.53.0/24  \n 10.11.0.0/16 |
| | tor05 | BCR01 | 10.1.6.0/24  \n 10.3.51.0/24  \n 10.243.0.0/16 |
| Washington D.C. | wdc01 | BCR05 | 10.108.0.0/15 |
| | wdc01 | BCR06 | 10.124.0.0/15 |
| | wdc04 | BCR01 | 10.3.10.0/29  \n 10.3.52.0/24  \n 10.3.171.0/24  \n 10.3.240.0/24  \n 10.148.0.0/15 |
| | wdc04 | BCR02 | 10.0.192.128/26  \n 10.170.0.0/16 |
| | wdc04 | BCR03 | 10.183.0.0/16  \n 10.216.0.0/16 |
| | wdc04 | BCR04 | 10.65.0.0/16  \n 10.201.11.0/24  \n 10.211.0.0/16 |
| | wdc04 | BCR05 | 10.213.0.0/16 |
| | wdc06 | BCR01 | 10.3.4.0/24  \n 10.188.0.0/15  \n 10.200.171.0/24 |
| | wdc07 | BCR01 | 10.3.5.0/24  \n 10.39.0.0/16  \n 10.190.0.0/15  \n 10.200.187.0/24 |
{: caption="Table 3: Customer private network space" caption-side="bottom"}

## Service network (on back-end/private network)
{: #service-network}

[CHANGE LOG](#cloud-infrastructure-jul2522)

* Be sure to configure rules and verify routes for dal10, wdc04, and the location of your server. If your server is in an EU location, you must add rules allowing traffic from dal10, and wdc04 to your server.
* Traffic must be able to travel between the service networks and your server in both directions.
* By default, all servers and gateway/firewall devices are configured with a static route for the `10.0.0.0/8` network to the Back-end Customer Router (BCR). If you change that configuration such that the entire `10.0.0.0/8` network is pointed elsewhere, you must also configure static routes for the service networks to ensure they are pointed to the BCR. Failing to do so will result in the static routes being pointed to whichever IP address you replaced the original with. If you do not change the default static route for `10.0.0.0/8`, then the service networks are already routed correctly.

Ports to allow:
- All TCP/UDP ports (for access from your local workstation)
- ICMP – ping (for support troubleshooting and monitoring)

|Data center|City|IP range|
|---|---|---|
|All| - |10.0.64.0/19|
|ams03|Amsterdam|10.3.128.0/20  \n 161.26.28.0/22|
|che01|Chennai|10.200.16.0/20  \n 161.26.32.0/22  \n 166.9.60.0/24|
|dal05|Dallas|10.1.128.0/19[^fn1]  \n 161.26.12.0/24 |
|dal08|Dallas|100.100.0.0/20|
|dal09|Dallas|10.2.112.0/20  \n 10.3.192.0/24 \n 161.26.4.0/22|
|dal10|Dallas|10.200.80.0/20 \n 161.26.13.0/24 \n 161.26.96.0/22  \n 166.9.12.0/23  \n 166.9.48.0/24  \n 166.9.50.0/24  \n 166.9.228.0/24  \n 166.9.250.192/27|
|dal12|Dallas|10.200.112.0/20  \n 161.26.108.0/22  \n 166.9.14.0/23  \n 166.9.51.0/24  \n 166.9.229.0/24  \n 166.9.250.224/27|
|dal13|Dallas|10.200.128.0/20  \n 161.26.112.0/22  \n 166.9.16.0/23  \n 166.9.58.0/24  \n 166.9.230.0/24  \n 166.9.251.0/27|
|fra02|Frankfurt|10.3.80.0/20  \n 161.26.36.0/22  \n 166.9.28.0/23|
|fra04|Frankfurt|10.201.112.0/20  \n 161.26.144.0/22  \n 166.9.30.0/23|
|fra05|Frankfurt|10.201.128.0/20  \n 161.26.148.0/22  \n 166.0.32.0/23|
|lon02|London|10.1.208.0/20  \n 161.26.8.0/22|
|lon04|London|10.201.32.0/20  \n 161.26.128.0/22  \n 166.9.36.0/23|
|lon05|London|10.201.48.0/20  \n 161.26.140.0/22  \n 166.9.34.0/23|
|lon06|London|10.201.64.0/20  \n 161.26.160.0/22  \n 166.9.38.0/23|
|mil01|Milan|10.3.144.0/20   \n 161.26.52.0/22|
|mon01|Montreal|10.3.112.0/20  \n 161.26.56.0/22|
|osa21|Osaka|10.202.112.0/20  \n 161.26.184.0/22  \n 166.9.70.0/24|
|osa22|Osaka|10.202.144.0/20  \n 161.26.188.0/22  \n 166.9.71.0/24|
|osa23|Osaka|10.202.160.0/20  \n 161.26.192.0/22  \n 166.9.72.0/24|
|par01|Paris|10.2.144.0/20  \n 161.26.60.0/22  \n 166.9.79.0/24  \n 166.9.245.128/27|
|sao01|São Paulo|10.200.0.0/20  \n 161.26.64.0/22  \n 166.9.82.0/24|
|sao04|São Paulo|10.202.208.0/20  \n 161.26.204.0/22  \n 166.9.83.0/24|
|sao05|São Paulo|10.202.240.0/20  \n 161.26.208.0/22  \n 166.9.84.0/24|
|sjc01|San Jose|10.1.192.0/20  \n 10.200.32.0/20|
|sjc03|San Jose|10.3.176.0/20  \n 161.26.72.0/22|
|sjc04|San Jose|10.201.80.0/20  \n 161.26.136.0/22|
|sng01|Jurong East|10.2.32.0/20  \n 10.200.144.0/20  \n 161.26.76.0/22|
|syd01|Sydney|10.3.96.0/20  \n 10.202.32.0/20  \n 161.26.80.0/22  \n 161.26.168.0/22  \n 166.9.52.0/23|
|syd04|Sydney|10.201.16.0/20  \n 161.26.124.0/22  \n 166.9.54.0/23|
|syd05|Sydney|10.202.16.0/20  \n 161.26.164.0/22  \n 166.9.56.0/23|
|tok02|Tokyo|10.3.64.0/20  \n 10.201.160.0/20  \n 161.26.84.0/22  \n 166.9.40.0/23|
|tok04|Tokyo|10.201.176.0/20  \n 161.26.152.0/22  \n 166.9.42.0/23|
|tok05|Tokyo|10.201.192.0/20  \n 161.26.156.0/22  \n 166.9.44.0/23|
|tor01|Toronto|10.2.48.0/20  \n 10.202.96.0/20  \n 161.26.88.0/22  \n 166.9.76.0/24|
|tor04|Toronto|10.202.176.0/20  \n 161.26.196.0/22  \n 166.9.77.0/24|
|tor05|Toronto|10.202.192.0/20  \n 161.26.200.0/22  \n 166.9.78.0/24|
|wdc01|Washington D.C.|10.1.96.0/19  \n 161.26.16.0/22|
|wdc04|Washington D.C.|10.3.160.0/20  \n 10.201.0.0/20  \n 161.26.92.0/22  \n 161.26.132.0/22  \n 166.9.20.0/23  \n 166.9.231.0/24|
|wdc06|Washington D.C.|10.200.160.0/20  \n 161.26.120.0/22  \n 166.9.22.0/23  \n 166.9.232.0/24  \n 166.9.251.64/27|
|wdc07|Washington D.C.|10.200.176.0/20  \n 161.26.132.0/22  \n 166.9.24.0/23  \n 166.9.233.0/24  \n 166.9.251.96/27|
{: caption="Table 4: Service network" caption-side="bottom"}   

[^fn1]: The 10.1.129.0/24 subnet, within the 10.1.128.0/19 master subnet, is used for Global service virtual IPs, which are not located in dal05.

### Service by data center
{: #service-by-data-center}

As of 8 June 2020, all instances of the AdvMon (Nimsoft) by Data Center service are deprecated and are no longer supported. For more information, see [FAQs](/docs/virtual-servers?topic=virtual-servers-faq-monitoring#what-are-important-transition-dates).
{: deprecated}

| Data center | IP range |
|-----|-----|
| **Required Flows**:  \n * Outbound TCP 8086 and TCP 8087 from your private  \n VLANs to IP ranges documented in dal09 and dal10 only.  \n * Outbound TCP 2546 from your private VLANs to IP ranges  \n documented for each DC where you need to access your vault. | |
| ams03 | 10.3.134.0/24 |
| che01 | 10.200.22.0/24 |
| dal05 | 10.1.146.0/24 |
| dal08 | 100.100.6.0/24 |
| dal09 | 10.2.118.0/24 |
| dal09 | 10.2.126.0/24 |
| dal10 | 10.200.86.0/24 |
| dal12 | 10.200.118.0/24 |
| dal13 | 10.200.134.0/24 |
| fra02 | 10.3.86.0/24 \n 10.201.150.0/24 |
| fra04 | 10.201.118.0/24 |
| fra05 | 10.201.134.0/24 |
| lon02 | 10.1.214.0/24 \n 10.201.102.0/24 |
| lon04 | 10.201.38.0/24 |
| lon05 | 10.201.54.0/24 |
| lon06 | 10.201.70.0/24 |
| mil01 | 10.3.150.0/24 |
| mon01 | 10.3.118.0/24 |
| osa21 | 10.202.118.0/24 |
| osa22 | 10.202.150.0/24 |
| osa23 | 10.202.166.0/24 |
| par01 | 10.2.150.0/24 |
| sao01 | 10.200.6.0/24 |
| sjc01 | 10.1.198.0/24  \n 10.200.38.0/24 |
| sjc03 | 10.3.182.0/24 |
| sjc04 | 10.201.86.0/24 |
| sng01 | 10.2.38.0/24  \n 10.200.150.0/24 |
| syd01 | 10.3.102.0/24 |
| syd04 | 10.201.22.0/24 |
| tok02 | 10.201.166.0/24 |
| tok04 | 10.201.182.0/24 |
| tok05 | 10.201.198.0/24 |
| tor01 | 10.2.54.0/24 |
| wdc01 | 10.1.114.0/24 |
| wdc03 | 100.100.38.0/24 |
| wdc04 | 10.3.166.0/24  \n 10.201.6.0/24 |
| wdc06 | 10.200.166.0/24 |
| wdc07 | 10.200.182.0/24 |
{: class="simple-tab-table"}
{: caption="Table 5. eVault by Data Center" caption-side="left"}
{: #simpletabtable1}
{: tab-title="eVault"}
{: tab-group="IAM-simple"}

| Data center | IP range |
|-----|-----|
| **Required Flows**:  \n NFS File Storage:  \n * TCP & UDP 111 (sunrpc)  \n * TCP & UDP 2049 (nfs)  \n * TCP & UDP 111(portmapper)  \n * TCP & UDP 635 (nfsd)  \n * TCP & UDP 4045-4048  \n * UDP 4049  \n Block Storage:  \n * TCP & UDP 65200 (iscsi) | |
| ams03 | 10.3.142.0/24  \n 161.26.30.0/24 |
| che01 | 10.200.30.0/24  \n 161.26.34.0/24 |
| dal05 | 10.1.154.0/24  \n 10.2.110.0/24 |
| dal08 | 100.100.14.0/24 |
| dal09 | 10.2.125.0/24 \n 10.2.126.0/24 |
| dal10 | 10.200.94.0/24  \n 10.202.239.0/24  \n 161.26.13.0/24  \n 161.26.98.0/24  \n 161.26.99.0/24 |
| dal12 | 10.200.126.0/24  \n 161.26.110.0/24  \n 161.26.111.0/24 |
| dal13 | 10.200.142.0/24  \n 10.200.142.0/24  \n 10.200.143.128/25  \n 161.26.114.0/24  \n 161.26.115.0/24 |
| fra02 | 10.3.94.0/24  \n 10.201.154.0/24  \n 161.26.38.0/24  \n 161.26.39.0/24 |
| fra04 | 10.201.110.0/24  \n 161.26.146.0/24 |
| fra05 | 10.201.142.0/24  \n 161.26.150.0/24 |
| lon02 | 10.1.222.0/24  \n 10.201.110.0/24  \n 161.26.10.0/24
| lon04 | 10.201.46.0/24  \n 161.26.130.0/24 |
| lon05 | 10.201.62.0/24  \n 161.26.162.0/24 |
| lon06 | 10.201.78.0/24  \n 161.26.142.0/24 |
| mil01 | 10.3.158.0/24  \n 161.26.54.0/24 |
| mon01 | 10.3.126.0/24  \n 161.26.58.0/24 |
| osa21 | 10.202.126.0/24  \n 161.26.186.0/24 |
| osa22 | 10.202.158.0/24  \n 161.26.190.0/24 |
| osa23 | 10.202.174.0/24  \n 161.26.194.0/24 |
| par01 | 10.2.158.0/24  \n 10.2.159.0/24  \n 161.26.62.0/24 |
| par04 | 10.202.94.0/24  \n 161.26.174.0/24 |
| par05 | 10.202.78.0/24  \n 161.26.178.0/24 |
| par06 | 10.202.62.0/24  \n 161.26.182.0/24 |
| sao01 | 10.200.14.0/24  \n 10.200.15.0/25  \n 10.203.78.0/24  \n 161.26.66.0/24  \n 161.26.67.0/24 |
| sao04 | 10.202.221.0/24  \n 161.26.206.0/24 |
| sao05 | 10.202.253.0/24  \n 161.26.210.0/24 |
| sjc01 | 10.1.206.0/24 |
| sjc03 | 10.3.190.0/24  \n 161.26.74.0/24 |
| sjc04 | 10.201.94.0/24  \n 161.26.138.0/24 |
| sng01 | 10.2.46.0/24  \n 10.200.158.0/24  \n 161.26.78.0/24 |
| syd01 | 10.3.110.0/24  \n 10.202.46.0/24  \n 161.26.170.0/24  \n 161.26.82.0/24 |
| syd04 | 10.201.30.0/24  \n 130.198.64.0/18  \n 161.26.126.0/24 |
| syd05 | 10.202.30.0/24  \n 161.26.166.0/24 |
| tok02 | 10.3.78.0/24  \n 10.3.79.0/24  \n 10.201.174.0/24  \n 161.26.86.0/24  \n 161.26.87.0/24 |
| tok04 | 10.201.190.0/24  \n 161.26.154.0/24 |
| tok05 | 10.201.206.0/24  \n 161.26.158.0/24 |
| tor01 | 10.2.62.0/24  \n 10.202.110.0/24  \n 161.26.90.0/24  \n 161.26.91.0/24 |
| tor04 | 10.202.189.0/24  \n 161.26.198.0/24 |
| tor05 | 10.202.205.0/24  \n 161.26.202.0/24 |
| wdc01 | 10.1.122.0/24  \n 10.1.104.0/24 |
| wdc03 | 100.100.46.0/24 |
| wdc04 | 10.201.10.0/24  \n 10.201.14.0/24  \n 10.3.174.0/24  \n 10.3.175.0/25  \n 161.26.134.0/24  \n 161.26.94.0/24 |
| wdc06 | 10.200.174.0/24  \n 161.26.118.0/24 |
| wdc07 | 10.200.90.0/24  \n 161.26.122.0/24 |
{: caption="Table 6. File and Block by Data Center" caption-side="left"}
{: #simpletabtable2}
{: tab-title="File & Block"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

| Data center | IP range |
|-----|-----|
| **Required Flows**:  \n * Inbound: TCP and UDP, 48000.  \n * Outbound: TCP and UDP, 48000-48020. | |
| ams03 | 10.3.131.0/24 |
| che01 | 10.200.19.0/24 |
| dal05 | 10.1.143.0/24  \n 10.1.139.0/24|
| dal08 | 100.100.3.0/24 |
| dal09 | 10.2.115.0/24 |
| dal10 | 10.200.83.0/24 |
| dal12 | 10.200.115.0/24 |
| dal13 | 10.200.131.0/24 |
| fra02 | 10.3.83.0/24 \n 10.201.147.0/24 |
| fra04 | 10.201.115.0/24 |
| fra05 | 10.201.131.0/24 |
| lon02 | 10.1.211.0/24 \n 10.201.99.0/24 |
| lon04 | 10.201.35.0/24 |
| lon05 | 10.201.51.0/24 |
| lon06 | 10.201.67.0/24 |
| mil01 | 10.3.147.0/24 |
| mon01 | 10.3.115.0/24 |
| par01 | 10.2.147.0/24 |
| sao01 | 10.200.3.0/24 |
| sjc01 | 10.1.195.0/24 |
| sjc03 | 10.3.179.0/24 |
| sjc04 | 10.201.83.0/24 |
| sng01 | 10.2.35.0/24 |
| syd01 | 10.3.99.0/24 |
| syd04 | 10.201.19.0/24 |
| tok02 | 10.3.67.0/24 \n 10.201.163.0/24 |
| tok04 | 10.201.179.0/24 |
| tok05 | 10.201.195.0/24 |
| tor01 | 10.2.51.0/24 |
| wdc01 | 10.1.111.0/24 |
| wdc03 | 100.100.35.0/24 |
| wdc04 | 10.3.163.0/24 |
| wdc04 | 10.201.3.0/24 |
| wdc06 | 10.200.163.0/24 |
| wdc07 | 10.200.179.0/24 |
{: caption="Table 7. AdvMon (Nimsoft) by Data Center" caption-side="left"}
{: #simpletabtable3}
{: tab-title="AdvMon (Nimsoft)"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

| Data center | IP range |
|-----|-----|
| **Required Flows**:  \n Outbound: TCP 80, 443.[^fn2] | |
| ams03 | 10.3.130.0/24 |
| che01 | 10.200.18.0/24 |
| dal05 | 10.1.142.0/24  \n 10.1.138.0/24 |
| dal08 | 100.100.2.0/24 |
| dal09 | 10.2.114.0/24 |
| dal10 | 10.200.82.0/24 |
| dal12 | 10.200.114.0/24 |
| dal13 | 10.200.130.0/24 |
| fra02 | 10.3.82.0/24 \n 10.201.146.0/24 |
| fra04 | 10.201.114.0/24 |
| fra05 | 10.201.130.0/24 |
| lon02 | 10.1.210.0/24 \n 10.201.98.0/24 |
| lon04 | 10.201.34.0/24 |
| lon05 | 10.201.50.0/24 |
| lon06 | 10.201.66.0/24 |
| mil01 | 10.3.146.0/24 |
| mon01 | 10.3.114.0/24 |
| osa21 | 10.202.114.0/24 |
| osa22 | 10.202.146.0/24 |
| osa23 | 10.202.162.0/24 |
| par01 | 10.2.146.0/24 |
| sao01 | 10.200.2.0/24 |
| sjc01 | 10.1.194.0/24  \n 10.200.34.0/24 |
| sjc03 | 10.3.178.0/24 |
| sjc04 | 10.201.82.0/24 |
| sng01 | 10.2.34.0/24  \n 10.200.146.0/24 |
| syd01 | 10.3.98.0/24 |
| syd04 | 10.201.18.0/24 |
| tok02 | 10.3.66.0/24 \n 10.201.162.0/24 |
| tok04 | 10.201.178.0/24 |
| tok05 | 10.201.194.0/24 |
| tor01 | 10.2.50.0/24 |
| wdc01 | 10.1.110.0/24  \n 10.1.106.0/24|
| wdc03 | 100.100.34.0/24 |
| wdc04 | 10.3.162.0/24 |
| wdc04 | 10.201.2.0/24 |
| wdc06 | 10.200.162.0/24 |
| wdc07 | 10.200.178.0/24 |
{: caption="Table 8. ICOS by Data Center" caption-side="left"}
{: #simpletabtable4}
{: tab-title="ICOS"}
{: tab-group="IAM-simple"}
{: class="simple-tab-table"}

[^fn2]: Directionality is from the customer compute perspective. Outbound means leaving your account towards the service. Inbound means service reaching out to compute.
{: note}

### Change log for service network IP ranges (26 October 2022)
{: #cloud-infrastructure-oct2622}


#### Removed
* All address 166.8.0.0/14
* ams01 address 10.2.64.0/20
* ams01 address 10.2.66.0/24
* ams01 address 10.2.67.0/24
* ams01 address 10.2.70.0/24
* ams01 address 10.2.78.0/24
* ams01 address 10.200.48.0/20
* ams01 address 10.200.50.0/24
* ams01 address 10.200.54.0/24
* ams01 address 10.200.62.0/24
* dal01 address 10.0.90.0/24
* dal05 address 10.1.143/139.0/24
* dal05 address 10.1.159.0/24
* dal06 address 10.2.128.0/20
* dal06 address 10.2.130.0/24
* dal06 address 10.2.131.0/24
* dal06 address 10.2.142.0/24
* dal06 address 10.2.134.0/24
* dal07 address 10.1.176.0/20
* fra02AZ address 10.201.158.0/24
* hkg02 address 161.26.40.0/22
* hkg02 address 10.2.162.0/24
* hkg02 address 10.2.163.0/24
* hkg02 address 10.2.166.0/24
* hkg02 address 10.2.174.0/24
* hou02 address 10.1.174.0/24
* lon02AZ address 10.201.110.0/24
* mel01 address 10.2.94.0/24
* mel01 address 161.26.46.0/24
* mex01 address 10.2.176.0/20
* mex01 address 10.2.178.0/24
* mex01 address 10.2.179.0/24
* mex01 address 10.2.182.0/24
* mex01 address 10.2.190.0/24
* mex01 address 161.26.48.0/22
* mex01 address 161.26.50.0/24
* osl01 address 10.200.110.0/24
* osl01 address 161.26.106.0/24
* seo01 address 10.200.64.0/20
* seo01 address 10.200.66.0/24
* seo01 address 10.200.67.0/24
* seo01 address 10.200.78.0/24
* seo01 address 10.200.86.0/24
* seo01 address 161.26.100.0/22
* seo01 address 161.26.102.0/24
* seo01 address 166.9.46.0/23
* sjc01 address 10.200.46.0/24
* tok02AZ address 10.201.174.0/24
* wdc01 address 10.1.127.0/24
* wdc03 address 100.100.32.0/20

#### Added
* ams03 address 161.26.28.0/22
* ams03 address 161.26.30.0/24
* che01 address 161.26.32.0/22
* che01 address 161.26.34.0/24
* che01 address 166.9.60.0/24
* dal05 address 10.1.139.0/24
* dal05 address 10.1.143.0/24
* dal05 address 10.2.110.0/24
* dal05 address 161.26.12.0/24
* dal09 address 10.2.126.0/24
* dal09 address 161.26.4.0/22
* dal10 address 10.202.239.0/24
* dal10 address 161.26.13.0/24
* dal10 address 161.26.96.0/22
* dal10 address 161.26.98.0/24
* dal10 address 161.26.99.0/24
* dal10 address 166.9.12.0/23
* dal10 address 166.9.48.0/24
* dal10 address 166.9.50.0/24
* dal10 address 166.9.228.0/24
* dal10 address 166.9.250.192/27
* dal12 address 161.26.108.0/22
* dal12 address 161.26.110.0/24
* dal12 address 161.26.111.0/24
* dal12 address 166.9.14.0/23
* dal12 address 166.9.51.0/24
* dal12 address 166.9.229.0/24
* dal12 address 166.9.250.224/27
* dal13 address 161.26.112.0/22
* dal13 address 166.9.16.0/23
* dal13 address 166.9.58.0/24
* dal13 address 166.9.230.0/24
* dal13 address 166.9.251.0/27
* dal13 address 10.200.142.0/24
* dal13 address 10.200.143.128/25
* dal13 address 161.26.114.0/24
* dal13 address 161.26.115.0/24
* fra02 address 10.201.154.0/24
* fra02 address 161.26.36.0/22
* fra02 address 161.26.38.0/24
* fra02 address 161.26.39.0/24
* fra02 address 166.9.28.0/23
* fra04 address 161.26.144.0/22
* fra04 address 161.26.146.0/24
* fra04 address 166.9.30.0/23
* fra05 address 161.26.148.0/22
* fra05 address 161.26.150.0/24
* fra05 address 166.0.32.0/23
* lon02 address 161.26.8.0/22
* lon02 address 161.26.10.0/24
* lon02 address 10.201.110.0/24
* lon04 address 161.26.128.0/22
* lon04 address 161.26.130.0/24
* lon04 address 166.9.36.0/23
* lon05 address 161.26.140.0/22
* lon05 address 161.26.162.0/24
* lon05 address 166.9.34.0/23
* lon06 address 161.26.142.0/24
* lon06 address 161.26.160.0/22
* lon06 address 166.9.38.0/23
* mil01 address 161.26.52.0/22
* mil01 address 161.26.54.0/24
* mon01 address 161.26.56.0/22
* mon01 address 161.26.58.0/24
* osa21 address 161.26.184.0/22
* osa21 address 161.26.186.0/24
* osa21 address 166.9.70.0/24
* osa22 address 161.26.188.0/22
* osa22 address 161.26.190.0/24
* osa22 address 166.9.71.0/24
* osa23 address 161.26.192.0/22
* osa23 address 161.26.194.0/24
* osa23 address 166.9.72.0/24
* par01 address 10.2.159.0/24
* par01 address 161.26.62.0/24
* par01 address 161.26.60.0/22
* par01 address 166.9.79.0/24
* par01 address 166.9.245.128/27
* par04 address 10.202.94.0/24
* par04 address 161.26.174.0/24
* par05 address 10.202.78.0/24
* par05 address 161.26.178.0/24
* par06 address 10.202.62.0/24
* par06 address 161.26.182.0/24
* sao01 address 10.200.15.0/25
* sao01 address 10.203.78.0/24
* sao01 address 161.26.64.0/22
* sao01 address 161.26.66.0/24
* sao01 address 161.26.67.0/24
* sao01 address 166.9.82.0/24
* sao04 address 10.202.221.0/24
* sao04 address 161.26.204.0/22
* sao04 address 161.26.206.0/24
* sao04 address 166.9.83.0/24
* sao05 address 10.202.253.0/24
* sao05 address 161.26.208.0/22
* sao05 address 161.26.210.0/24
* sao05 address 166.9.84.0/24
* sjc01 address 10.200.32.0/20
* sjc03 address 161.26.72.0/22
* sjc03 address 161.26.74.0/24
* sjc04 address 161.26.136.0/22
* sjc04  address 161.26.138.0/24
* sng01 address 10.200.144.0/20
* sng01 address 161.26.76.0/22
* sng01 address 161.26.78.0/24
* syd01 address 10.202.46.0/24
* syd01 address 161.26.80.0/22
* syd01 address 161.26.82.0/24
* syd01 address 161.26.168.0/22
* syd01 address 161.26.170.0/24
* syd01 address 166.9.52.0/23
* syd04 address 130.198.64.0/18
* syd04 address 161.26.124.0/22
* syd04 address 161.26.126.0/24
* syd04 address 166.9.54.0/23
* syd05 address 10.202.30.0/24
* syd05 address 161.26.164.0/22
* syd05 address 161.26.166.0/24
* syd05 address 166.9.56.0/23
* tok02 address 10.201.174.0/24
* tok02 address 10.3.79.0/24
* tok02 address 161.26.84.0/22
* tok02 address 161.26.86.0/24
* tok02 address 161.26.87.0/24
* tok02 address 166.9.40.0/23
* tok04 address 161.26.152.0/22
* tok04 address 161.26.154.0/24
* tok04 address 166.9.42.0/23
* tok05 address 161.26.156.0/22
* tok05  address 161.26.158.0/24
* tok05 address 166.9.44.0/23
* tor01 address 10.202.96.0/20
* tor01 address 10.202.110.0/24
* tor01 address 161.26.88.0/22
* tor01 address 161.26.90.0/24
* tor01 address 161.26.91.0/24
* tor01 address 166.9.76.0/24
* tor04 address 10.202.189.0/24
* tor04 address 161.26.196.0/22
* tor04 address 161.26.198.0/24
* tor04 address 166.9.77.0/24
* tor05 address 10.202.205.0/24
* tor05 address 161.26.200.0/22
* tor05 address 161.26.202.0/24
* tor05 address 166.9.78.0/24
* wdc01 address 161.26.16.0/22
* wdc04 address 10.201.10.0/24
* wdc04 address 10.3.175.0/25
* wdc04 address 161.26.92.0/22
* wdc04 address 161.26.94.0/24
* wdc04 address 161.26.132.0/22
* wdc04 address 161.26.134.0/24
* wdc04 address 166.9.20.0/23
* wdc04 address 166.9.231.0/24
* wdc06 address 161.26.118.0/24
* wdc06 address 161.26.120.0/22
* wdc06 address 166.9.22.0/23
* wdc06 address 166.9.232.0/24
* wdc06 address 166.9.251.64/27
* wdc07 address 161.26.122.0/24
* wdc07 address 161.26.132.0/22
* wdc07 address 166.9.24.0/23
* wdc07 address 166.9.233.0/24
* wdc07 address 166.9.251.96/27

## SSL VPN network (on back-end/private network)
{: #ssl-vpn-network}

ICMP – ping (for support troubleshooting)

All TCP/UDP ports (for access from your local workstation)

## SSL VPN data centers
{: #ssl-vpn-data-centers}

|Data center|City|IP range|
|---|---|---|
|ams03|Amsterdam|10.3.220.0/24|
|che01|Chennai|10.200.232.0/24|
|dal05|Dallas|10.1.24.0/23|
|dal09|Dallas|10.2.232.0/24|
|dal10|Dallas|10.200.228.0/24|
|dal12|Dallas|10.200.216.0/22|
|dal13|Dallas|10.200.212.0/22|
|fra02|Frankfurt|10.2.236.0/24|
|lon02|London|10.2.220.0/24|
|lon04|London|10.200.196.0/24|
|lon05|London|10.201.208.0/24|
|lon06|London|10.3.200.0/24|
|mil01|Milan|10.3.216.0/24|
|mon01|Montreal|10.3.224.0/24|
|osa21|Osaka|10.202.128.0/24|
|osa22|Osaka|10.202.132.0/24|
|osa23|Osaka|10.202.136.0/24|
|par01|Paris|10.3.236.0/24|
|sao01|São Paulo|10.200.236.0/24|
|sjc01|San Jose|10.1.224.0/23|
|sjc03|San Jose|10.3.204.0/24|
|sjc04|San Jose|10.200.192.0/24|
|sng01|Jurong East|10.2.192.0/23|
|syd01|Sydney|10.3.228.0/24|
|syd04|Sydney|10.200.200.0/24|
|syd05|Sydney|10.201.212.0/24|
|tok02|Tokyo|10.2.224.0/24|
|tok04|Tokyo|10.201.228.0/24|
|tok05|Tokyo|10.201.224.0/24|
|tor01|Toronto|10.1.232.0/24  \n 10.1.233.0/24|
|tor04|Toronto|10.1.0.0/24|
|tor05|Toronto|10.1.4.0/24|
|wdc01|Washington D.C.|10.1.16.0/23|
|wdc04|Washington D.C.|10.3.212.0/24|
|wdc03|Washington D.C.|100.101.132.0/24|
|wdc06|Washington D.C.|10.200.208.0/24|
|wdc07|Washington D.C.|10.200.204.0/24|
{: caption="Table 9: SSL VPN data centers" caption-side="bottom"}  

## SSL VPN PoPs
{: #ssl-vpn-pops}

|PoP|City|IP range|
|---|---|---|
|atl01|Atlanta|10.1.41.0/24|
|chi01|Chicago|10.1.49.0/24|
|den01|Denver|10.1.53.0/24|
|lax01|Los Angeles|10.1.33.0/24|
|mia01|Miami|10.1.37.0/24|
|nyc01|New York|10.1.45.0/24|
{: caption="Table 10: SSL VPN PoPs" caption-side="bottom"}

## Legacy networks
{: #legacy-networks}

|IP range|
|---|
|12.96.160.0/24|
|66.98.240.192/26|
|67.18.139.0/24|
|67.19.0.0/24|
|70.84.160.0/24|
|70.85.125.0/24|
|75.125.126.8|
|209.85.4.0/26|
|216.12.193.9|
|216.40.193.0/24|
|216.234.234.0/24|
{: caption="Table 11: Legacy networks" caption-side="bottom"}

## Red Hat Enterprise Linux server requirements
{: #red-hat-enterprise-linux-server}

If your server uses a Red Hat Enterprise Linux (RHEL) license provided by {{site.data.keyword.cloud_notm}} infrastructure, provisioning is dependent on completing the following actions:

* Complete the SSL VPN requirements listed in [Service network (on back-end/private network)](/docs/cloud-infrastructure?topic=cloud-infrastructure-ibm-cloud-ip-ranges#service-network).
* Open the RHEL endpoint rhha01.updates.us-south.iaas.service.networklayer.com. This requires adding the IP 161.26.112.28 to the firewall rules. Since DNS round robin is involved, this endpoint is not a single endpoint and could be moved as needed.
* Allow access to the service network as follows. Otherwise, updates and licensing do not function properly. 

|Server location|Allow private service network for this data center|
|---|---|
|Amsterdam (ams03)|fra02|
|Chennai (che01)|tok02|
|Dallas (dal05, dal09, dal10, dal12, dal13)|dal09|
|Frankfurt (fra02, fra04, fra05)|fra02|
|London (lon02, lon04, lon05, lon06)|lon02|
|Milan (mil01)|fra02|
|Montreal (mon01)|mon01|
|Paris (par01)|fra02|
|San Jose (sjc01, sjc03, sjc04)|dal09|
|Sao Paulo (sao01)|dal09|
|Singapore (sng01)|syd01|
|Sydney (syd01, syd04, syd05)|syd01|
|Tokyo (tok02, tok04, tok05)|tok02|
|Toronto (tor01)|mon01|
|Washington DC (wdc01, wdc04, wdc06, wdc07)|mon01|
|Any data center not listed|dal09|
{: caption="Table 12: Red Hat Enterprise Linux server requirements" caption-side="bottom"}

To resolve common provisioning issues, permit access to the entire service network by allowing the IP 161.26.0.0/16. 
{: important}

## Windows VSI server requirements
{: #windows-vsi-server}

If you have a firewall, you must allow your Windows VSI to access to WSUS server IP address ranges as follows; otherwise, updates and licensing do not function properly.

* wsustok0401.service.softlayer.com is 10.201.177.200
* wsustok0201.service.softlayer.com is 10.3.65.50
* wsustok0501.service.softlayer.com is 10.201.193.200

|Data Center|City|BCR IP Range|
|---|---|---|
|tok04|Tokyo|10.3.17.0/24 /n 10.192.0.0/16 /n 10.201.187.0/24|
{: caption="Table 13: Windows VSI server requirements" caption-side="bottom"}

For more information about locating your WSUS server in the Windows registry by using a regisration key, see [Configuring Automatic Updates by editing the registry](https://learn.microsoft.com/en-us/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry){: external} or [Configure Clients in a Non–Active Directory Environment](https://learn.microsoft.com/de-de/security-updates/windowsupdateservices/21669493){: external}.