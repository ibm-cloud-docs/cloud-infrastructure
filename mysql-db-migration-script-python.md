---

copyright:
  years:  2022
lastupdated: "2022-12-08"

keywords:

subcollection: cloud-infrastructure

---

{{site.data.keyword.attribute-definition-list}}

# MySQL database migration using Python
{: #mysql-python}

The MySQL database migration script allows you to migrate MySQL databases from one server to another. This is applicable to any platform:

* {{site.data.keyword.cloud}} classic infrastructure to {{site.data.keyword.vpc_short}}
* On-premises to {{site.data.keyword.vpc_short}}
* Other cloud service providers to {{site.data.keyword.vpc_short}}

## Prerequisites
{: #mysql-prerequisites}

Review the following prerequisites before you begin your migration:

1. Set up an {{site.data.keyword.cos_full_notm}} bucket.
2. Make sure that you have write access to the {{site.data.keyword.cos_short}} bucket.
3. Make sure that you have connection to the source and target server from your system.
4. Ensure that Python3 (version 3.0) and Pip3 are installed on your system.

## Migration overview diagram
{: #mysql-db-overview-diagram-python}

![Migration Overview Diagram](images/mysql_migration_script_python.svg){: caption="Figure 1. Migration overview diagram" caption-side="bottom"}

## Clone and run the database migration script
{: #clone-run-database-migration-script}

Complete the following steps to clone and run the database migration script:

1. Run the following commands to clone the public GitHub repository:

    ```sh
    git clone https://github.com/IBM-Cloud/vpc-migration-tools.git
    ```
    {: pre}

    ```sh
    cd vpc-migration-tools/db-migration/mysql
    ```
    {: pre}

2. Install the prerequisites modules for Python by running the following commands:

    ```sh
    pip3 install -U pip setuptools
    ```
    {: codeblock}

    ```sh
    pip3 install -r requirements.txt
    ```
    {: codeblock}

    `setuptools` facilitate packaging Python projects by enhancing the Python standard library distutils. The `requirements.txt` files install the Python libraries that are required in the script.
    {: note}
    
3. Run the following database migration script:

    ```sh
    python3 db_migration.py
    ```
    {: pre}

## Provide migration details
{: #provide-migration-details}

After you run the database migration script, you need to provide the details for the following parameters:
* {{site.data.keyword.cos_short}}
* Source server
* Source server database
* Target server
* Target server database

### Object Storage details
{: #cos-bucket-details}

The {{site.data.keyword.cos_short}} bucket acts as centralized storage for the source and the target server. By using the `s3fs` utility, the bucket is mounted as a file system on both the source and target server. The source uses the bucket to store the database backup, and the target uses the bucket to retrieve the database backup, which minimizes the migration duration.

Complete the following steps to mount the {{site.data.keyword.cos_short}} bucket:

1. Enter the {{site.data.keyword.cos_short}} bucket name; for example, `my-db-bucket`. This is the name of the bucket that's already been provisioned.
2. Enter the {{site.data.keyword.cos_short}} endpoint, which is the location of the bucket; for example, `https://s3.dal.us.cloud-object-storage.appdomain.cloud`. For more information, see [Endpoints and storage locations](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints).
3. Enter your {{site.data.keyword.cloud_notm}} API key. For more information, see [Creating an {{site.data.keyword.cloud_notm}} API key](https://www.ibm.com/docs/en/app-connect/containers_cd?topic=servers-creating-cloud-api-key){: external}.

### Source server details
{: #source-server-details}

After you mount your {{site.data.keyword.cos_short}} bucket, you need to provide your source server details:

1. Enter the source server IP address or hostname (this is the MySQL database that needs to be migrated).
2. Enter the source server login credentials. The script needs to authenticate with the username and password. The user privileges should be equivalent to the `root`. 

### Source server database details
{: #source-server-db-details}

1. Enter the source server MySQL connection details (username for the MySQL application, which is `root` by default).
2. Enter your password for MySQL, which authenticates you to perform actions on the MySQL database that you want to migrate.
3. Enter the database name that you want to migrate.

### Target server details
{: #target-server-details}

After you provide your source server and source server database details, you need to provide your target server details:

1. Enter the target server IP address or hostname (this is the target server where the MySQL database will be migrated).
2. Enter the target server login credentials. Similarly, for the source server, the script needs to be authenticated with the username and password. The user privilege should be equivalent to the `root`.

### Target server database details
{: #target-server-db-details}

1. Enter the target server MySQL connection details (username for the MySQL application, which is `root` by default.
2. Enter your password for MySQL, which authenticates you to perform actions to restore the database.
3. Enter the source server database name.
4. Enter the target database name. By default, this fetches the database name from the source database name. If the you want to have a different database name for migration, then you can provide the input per your requirement.
5. After a successful database migration, you are prompted with a "complete" message.
