---

copyright:
  years: 2021
lastupdated: "2021-08-27"

keywords: high availability, regions, zones, resiliency

subcollection: cloud-infrastructure

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:external: target="_blank" .external}

# Taking snapshot backups for your 3-Tier applications for a single zone

For an overview of Regional snapshots in 3-tier architecture, refer this [link](/docs/cloud-infrastructure?topic=cloud-infrastructure-regional-snapshots-3-tier-arch). 
The following section provides an overview of scheduling snapshots of a 3-tier application volumes, either bootable or secondary attached to a running web, app, and database tiers.

Snapshots can be scheduled by running an API script through cron job, by first creating a service ID in IAM, followed by retrieving the access token and running the script.

## Step 1: Create Service ID in IAM

1.  Log in to the [{{site.data.keyword.cloud}} console](https://cloud.ibm.com/login){: external}.
2.  Select **Manage > Access (IAM)**.
3.	Select **Service IDs**. 
4.  Select **Create**.

## Step 2: Create Access policies for the Service IDs that you created

1.	Select the ***Service ID***.
2.  Select **Access policies**.
3.  Select **Assign access**.
4.	Select **Assign Service ID additional access**.
5.	Make sure that **IAM Services** is selected
6.	Click **Which Service do you want to assign access to** and select ***VPC Infrastructure Services***.
7.	Select **Resources based on selected attributes**.
8.	Under **Add Attributes**, select:
    1. Your resource group.
    2. Resource type ***Block Storage for VPC***.
9.	Under **Platform access**, select ***Administrator***
10.	Use the default for all others as default.
11. Select **Add**.
12.	Repeat steps 2-11 for resource types: 
    1.	Block Storage Snapshots for VPC
    2.	Virtual Server for VPC
13.	Check the Access Summary in the right panel and select **Assign**. 

## Step 3: Create API keys 

1.	Click the Service ID and then API Keys. Provide a name and description, create the API key and then download or copy the API key created. This key is used to retrieve the token later.

This Service ID API key has authorization to create snapshots on block storage volumes based on your selection of Resource Types. A further fine-grained policy can be set up to limit access only to selected virtual servers or storage volumes by providing the instance ID or volume ID under resource types if wanted.
{: note}
 
## Step 4: Setting up the Cron Script:

### Identify the VPC end point. 

The end point depends on the region you are running VSIs. For example, if you are running the API from Sydney region, it is https://au-syd.iaas.cloud.ibm.com

### Identify the source volume IDs. The source volume IDs can be retrieved either through CLI or UI. 

1.  Log in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/login){: external}.
2.	Select **VPC Infrastructure > Virtual Server Instances**. 
3.  Select your VSI Instance.
4.	Scroll down to storage volumes and select the boot or data volume that you want to take a snapshot of.
5.	Make a note of the Volume ID on the page.

### Identify your resource group ID.

1.  Log in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/login){: external}.
2.	Select **Resource group**s and identify your resource group ID.

### Copy the script

Copy the script and schedule it through a cron. Modify the sample script for multiple volumes, boot or data, web, app or database VSIs and run them through your bastion host. Use your Source Volume ID, Resource Group ID, and API Key where indicated.

```
#!/bin/bash
when=`date +%m%d-%H%M`;
vpc_api_endpoint="https://au-syd.iaas.cloud.ibm.com"
sourcevol=<Your Source Volume ID>
resgrp=<Your resource Group ID>

#Pass the service id apikey to below curl command

curl -X POST     "https://iam.cloud.ibm.com/identity/token"     -H "content-type: application/x-www-form-urlencoded"     -H "accept: application/json"     -d 'grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<Your API key>' > /tmp/token.json

iam_token=`cat /tmp/token.json | awk -F "\"" '{print $4}'`

curl -s --output /tmp/snapcron.out -X POST \
  "$vpc_api_endpoint/v1/snapshots?version=2021-05-06&generation=2" \
  -H "Authorization: Bearer $iam_token" \
  -d '{
      "name": "'"my-snapshot-bootvol-$when"'",
      "source_volume": { "id": "'"$sourcevol"'" },
      "resource_group": { "id": "'"$resgrp"'" }
    }'
```

## References
1.	https://cloud.ibm.com/docs/vpc?topic=vpc-snapshots-vpc-create&interface=api#snapshots-vpc-create
2.	https://cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-create-three-tier-architecture

