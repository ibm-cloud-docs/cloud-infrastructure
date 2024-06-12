---

copyright:
  years:  2022, 2024
lastupdated: "2024-02-20"

subcollection: cloud-infrastructure

---

{{site.data.keyword.attribute-definition-list}}

# MySQL database migration
{: #mysql-db-overview}

MySQL database is a relational database management system (RDMS). Migrating such RDMS requires an efficient strategy that helps to migrate the data in a secure and consistent model. {{site.data.keyword.cloud}} helps to migrate the MySQL database from any infrastructure to {{site.data.keyword.vpc_short}}.
{: shortdesc}

## Why migrate?
{: #mysql-why-migrate}

1. Moving to an entirely new server.
2. Restoring databases from a backup.
3. Creating a development server or going live to a production server.
4. Versioning - A specific database or its version has features that offer more benefits than their existing database or its version.

## Migration overview diagram
{: #mysql-db-overview-diagram}

![Migration Overview Diagram](images/mysql_db_migration.svg){: caption="Figure 1. Migration overview diagram" caption-side="bottom"}

## Use cases
{: #mysql-use-cases}

1. On-premises to {{site.data.keyword.vpc_short}}
2. {{site.data.keyword.cloud_notm}} classic bare metal or virtual server instance to {{site.data.keyword.vpc_short}}
3. Other cloud service providers to {{site.data.keyword.vpc_short}}

## Prerequisites
{: #mysql-db-prerequisites}

Before you begin your MySQL database migration, review and complete the following prerequisites:

1. Back up all the data.
2. Select the appropriate compute capacity for your target server.
3. Ensure that enough disk space is available for the target server.
4. Ensure that the user has appropriate rights or permissions to perform migration activity.
5. Define a plan for the migration.

## Migration considerations
{: #mysql-db-migration-consideration}

* Migrating a server during nonpeak times.
* Choosing the right migration strategy that accommodates the RTO and RPO.
* Reducing complexity by migrating the database in a phased approach instead of a single step.

## Migration methods
{: #mysql-db-migration-methods}

| Migration tools and solutions | Use cases |
| ----------------- | -------- |
| RackWare Management Module (RMM)| Database on single server or multiple servers |
| Database Migration script| Small data migration or schema-only migration. |
{: caption="Table 1. MySQL migration methods" caption-side="bottom"}

### RackWare Management Module (RMM)
{: #mysql-rackware-management-module}

RackWare Management Module (RMM) is a simple workload migration solution that is provided by IBM’s partner RackWare. RMM provides an automated and convenient process to migrate existing compute workloads to {{site.data.keyword.cloud_notm}}. It tracks the data changes on the source server until cutover, and performs delta syncs to the target server in {{site.data.keyword.cloud_notm}}. This tool migrates a server with everything on it including the operating system, along with its installed database application and data (lift and shift migration). RMM can do database migration if it's a platform-based database (for example, it can access database workloads with a public IP address). For more information, see [On-premises VMware VM to {{site.data.keyword.vpc_short}} migration with RMM](/docs/cloud-infrastructure?topic=cloud-infrastructure-migrating-images-vmware-vpc).

### Database Migration Script (Python)
{: #mysql-database-migration-script-python}

By using Python as a source, you can migrate the database from any server to {{site.data.keyword.vpc_short}}. The migration script uses Python libraries that can back up the database from the source server and restore it on the new server.

The new server runs in the latest versions of MySQL, since this migration focuses on the database alone. The migration process is consistent and secure and can be performed from any system.

For more information, see [MySQL database migration by using Python](/docs/cloud-infrastructure?topic=cloud-infrastructure-mysql-python).

