---

copyright: 
  years:  2021, 2022
lastupdated: "2022-02-01"

keywords: high availability, regions, zones, resiliency

services: virtual-servers, vpc, loadbalancer-service

subcollection: cloud-infrastructure

---

{{site.data.keyword.attribute-definition-list}}

# Resiliency and High Availability Troubleshooting
{: #ha-troubleshooting}

This section covers error message that you may see while automating your infrastructure. The messages are shown in the logs for Terraform, Schematics, and Catalog tile automation tools. 


## Duplicate prefix name
{: #ha-trouble-dup-prefix}

During the Terrafoirm apply, you get amessage similar to this:

```

module.vpc.ibm_is_vpc.vpc: Creating...
╷
│ Error: [ERROR] Error while creating VPC {
│     "Message": "Provided Name (pp-cat-vpc) is not unique",
│     "StatusCode": 400,
│     "Result": {
│         "errors": [
│             {
│                 "code": "validation_unique_failed",
│                 "message": "Provided Name (pp-cat-vpc) is not unique",
│                 "target": {
│                     "name": "name",
│                     "type": "field",
│                     "value": "pp-cat-vpc"
│                 }
│             }
│         ],
│         "trace": "8a5c42e3-425a-492c-b972-96f3de56a65d"
│     }
│ }
│  
│ 
│   with module.vpc.ibm_is_vpc.vpc,
│   on modules/vpc/vpc.tf line 12, in resource "ibm_is_vpc" "vpc":
│   12: resource "ibm_is_vpc" "vpc" {

```

The prefix specified for the Terraform created resources is not unique.  Edit the `prefix` value and specifiy a new name.

## Invalid index
{: #ha-trouble-invalid-index}

During Terraform apply, you get an error similar to this:
```
Error: Invalid index
│ 
│   on providers.tf line 3, in provider "ibm":
│    3:   region           = local.regions[var.zone]
│     ├────────────────
│     │ local.regions is object with 27 attributes
│     │ var.zone is "us-east"

```

You specified a zone that was not vaild.  For example, you entered `us-east` for zone but you needed to enter `us-east-1`, `us-east-2`, or `us-east-3`.

## Error fetching Keys An error occurred while performing the 'authenticate' step

You get a message similar to:

```
Error: [ERROR] Error fetching Keys An error occurred while performing the 'authenticate' step: {"errorCode":"BXNIM0415E","errorMessage":"Provided API key could not be found","context":{"requestId":"aDJwNTQ-fec49a18af674b77a356764b2c633520","requestType":"incoming.Identity_Token","userAgent":"Go-http-client/1.1","url":"https://iam.cloud.ibm.com","instanceId":"iamid-7.7-13974-e612b1c-84749569f-h2p54","threadId":"37adb","host":"iamid-7.7-13974-e612b1c-84749569f-h2p54","startTime":"12.07.2022 14:47:03:629 UTC","endTime":"12.07.2022 14:47:03:699 UTC","elapsedTime":"70","locale":"en_US","clusterName":"iam-id-prod-eu-gb-lon05"}}
│ {
│     "StatusCode": 400,
│     "Headers": {
│         "Akamai-Grn": [
│             "0.be277368.1657637223.3e442e1"
│         ],
│         "Cache-Control": [
│             "no-cache, no-store, must-revalidate"
│         ],
│         "Content-Language": [
│             "en-US"
│         ],
│         "Content-Length": [
│             "534"
│         ],
│         "Content-Type": [
│             "application/json"
│         ],
│         "Date": [
│             "Tue, 12 Jul 2022 14:47:03 GMT"
│         ],
│         "Expires": [
│             "0"
│         ],
│         "Pragma": [
│             "no-cache"
│         ],
│         "Strict-Transport-Security": [
│             "max-age=31536000; includeSubDomains"
│         ],
│         "Transaction-Id": [
│             "aDJwNTQ-fec49a18af674b77a356764b2c633520"
│         ],
│         "X-Content-Type-Options": [
│             "nosniff"
│         ],
│         "X-Correlation-Id": [
│             "aDJwNTQ-fec49a18af674b77a356764b2c633520"
│         ],
│         "X-Proxy-Upstream-Service-Time": [
│             "78"
│         ],
│         "X-Request-Id": [
│             "e74ad79c-e934-40b7-9ced-d7b4b2507f42"
│         ]
│     },
│     "Result": null,
│     "RawResult": "eyJlcnJvckNvZGUiOiJCWE5JTTA0MTVFIiwiZXJyb3JNZXNzYWdlIjoiUHJvdmlkZWQgQVBJIGtleSBjb3VsZCBub3QgYmUgZm91bmQiLCJjb250ZXh0Ijp7InJlcXVlc3RJZCI6ImFESndOVFEtZmVjNDlhMThhZjY3NGI3N2EzNTY3NjRiMmM2MzM1MjAiLCJyZXF1ZXN0VHlwZSI6ImluY29taW5nLklkZW50aXR5X1Rva2VuIiwidXNlckFnZW50IjoiR28taHR0cC1jbGllbnQvMS4xIiwidXJsIjoiaHR0cHM6Ly9pYW0uY2xvdWQuaWJtLmNvbSIsImluc3RhbmNlSWQiOiJpYW1pZC03LjctMTM5NzQtZTYxMmIxYy04NDc0OTU2OWYtaDJwNTQiLCJ0aHJlYWRJZCI6IjM3YWRiIiwiaG9zdCI6ImlhbWlkLTcuNy0xMzk3NC1lNjEyYjFjLTg0NzQ5NTY5Zi1oMnA1NCIsInN0YXJ0VGltZSI6IjEyLjA3LjIwMjIgMTQ6NDc6MDM6NjI5IFVUQyIsImVuZFRpbWUiOiIxMi4wNy4yMDIyIDE0OjQ3OjAzOjY5OSBVVEMiLCJlbGFwc2VkVGltZSI6IjcwIiwibG9jYWxlIjoiZW5fVVMiLCJjbHVzdGVyTmFtZSI6ImlhbS1pZC1wcm9kLWV1LWdiLWxvbjA1In19"
│ }
│ 
│ 
│   with data.ibm_is_ssh_key.ssh_key_id[0],
│   on main.tf line 54, in data "ibm_is_ssh_key" "ssh_key_id":
│   54: data "ibm_is_ssh_key" "ssh_key_id" {
│ 
```

You enter an incorrect API key so the SSH key you are using doesn't work for authentication. Edit the `api key` parameter and provide the correct API key.

## SSH key not found
{: #ha-trouble-ssh-key-not-found}


The log shows an error similar to this:

```
Error: [ERROR] No SSH Key found with name pp-mac-key11
│ 
│   with data.ibm_is_ssh_key.ssh_key_id[0],
│   on main.tf line 54, in data "ibm_is_ssh_key" "ssh_key_id":
│   54: data "ibm_is_ssh_key" "ssh_key_id" {
```

The SSH Key that you entered is not valid.  SSH Keys are region specific.  Edit the `user_ssh_key` parameter and enter an SSH key that is valid for the region that you are using. 


##  Can't connect to Bastion server
{: #ha-trouble-bastion-ip}


The log shows an error similar to this:

```
Issue while login to the Batsion server.
ssh: connect to host 52.116.124.16 port 22: Operation timed out
```

You entered the wrong IP address for the Bastion server.  Edit the Bastion IP address.

## Invalid value for variable
{: #ha-trouble-invalid-variable}

The parameters for automation have different requiremenst for length and value.  The most common error is because the parameter is not the correct length. This table lists the various parameters and their lengths:

|Parameter    | Required length or range  |
|---------|------------|
| prefix   | less than 11 characters  |
| resource_group_name |  32 characters   |
| bastion_image  |  41 characters  |
| web_image  | 41 characters  |
| app_image  |  41 characters  |
| db_image  |  41 characters  |
| app_min_servers_count | Allowed value are between 1 and 1000 |
| web_min_servers_count  | Allowed value are between 1 and 1000 |

This table lists the valid values for various parameters: 

| Parameter  | Valid values |
|------|-----|
| web_pg_strategy | Values are host_spread and power_spread  |
| db_pg_strategy  | Values are host_spread and power_spread  |
| app_pg_strategy | Values are host_spread and power_spread  |