---

copyright: 
  years:  2020, 2021
lastupdated: "2021-06-25"

keywords: high availability, regions, zones, resiliency

content-type: tutorial

services: virtual-servers, vpc, loadbalancer-service

account-plan: paid

completion-time: 60m

subcollection: cloud-infrastructure

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}
{:new_window: target="_blank"}
{:step: data-tutorial-type='step'}

# Creating a three-tier highly available architecture on an IBM Cloud VPC
{: #create-three-tier-architecture} 
{: toc-content-type="tutorial"} 
{: toc-services="virtual-servers, vpc, loadbalancer"} 
{: toc-completion-time="60m"}

This tutorial describes the three-tiered architecture of an application that is rehosted on IBM cloud. The tutorial outlines the steps to implement a Highly Available solution in the IBM Cloud VPC infrastructure that spans multiple availability zones in one region. The strategies can differ and vary depending upon your application (web, app, and DB) and the redundancy necessary at each level. The tutorial provides a few guidelines for setting up a three tier OLTP application. Wherever possible, guidelines are provided on infrastructure, security, and operations. You might need to refer to your vendors’ supported methods of replication for the tiers, depending on the software components in your architecture.

## Architecture diagram
{: #three-tier-arch-diag-create}

This diagram shows the 3-tier architecture that you create with the tutorial. 

![Two different types of bare metal deployment.](images/3_tier_tut.png){:caption="Architecture Diagram"}

You must have full administrative privileges for managing the VPC resources.
For more information about user permissions in a VPC, see [[Getting user permissions for VPC resources](https://cloud.ibm.com/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources).

## Create VPC
{: #create-VPC-mzr}
{: step}

Create a VPC named vpcname-region1. For more information, see [Deploying critical applications with IBM Cloud MZR - Step 1: Create a VPC](https://test.cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-multi-zone-resiliency#create-vpc-mzr)

## Create Subnets
{: #create-Subnets-mzr}
{: step}

Create the following subnets:

1. vpcname-zone1web-subnet
2. vpcname-zone2web-subnet
3. vpcname-zone1mt-subnet
4. vpcname-zone2mt-subnet
5. vpcname-zone1db-subnet
6. vpcname-zone2db-dubnet

Make note of the CIDR blocks for the subnets you created.
{: note}

For detailed steps, see [Deploying critical applications with IBM Cloud MZR - Step 2 Create Subnets](https://test.cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-multi-zone-resiliency#create-subnets-mzr)

## Create SSH Keys
{: #create-SSH-Keys-mzr}
{: step}

Create SSH keys for the bastion host. You need to be able to log in to the bastion host with the private and public keys (asymmetric pair) from your client computer. For more information, see [Getting started with Virtual Private Cloud](https://cloud.ibm.com/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-ssh-keys) for details on how to generate your ssh keys on your Mac or Linux&reg; clients. If you are using Windows&reg;, generate the keys with [PuTTYgen](https://www.ssh.com/academy/ssh/putty/windows/puttygen#running-puttygen). Use the private and public keys for generating the ssh keys for your region.

1.  Log in to the [{{site.data.keyword.cloud}} console](https://cloud.ibm.com/login){: external}.
2.  Select **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > Menu > VPC Infrastructure > SSH keys**.
3.  Select **Create a new key**.
4.  Select your region and paste the public key that you generated earlier on your client.
5.  Make a note of your SSH key name. You use the SSH key when you create the bastion host.

## Create Security Groups
{: #create-Security-Groups-mzr}
{: step}

### Security Groups for Bastion Host
{: #security-groups}

Create a security group (bastion-sg) for the bastion host. You attach the bastion-sg to the bastion Virtual Server Instance that you create later.

Inbound rules for bastion-sg

| Protocol | Source type | Value |
|-----------|-----------|-----------|
| TCP | Any | Ports 22-22 |
| ICMP | Any | Type: 8 Code:empty |


Outbound rules for bastion-sg

| Protocol | Source type | Value |
| ----------- | ----------- | ----------- |
| All | Any |n/a|


These rules allow ssh access to the bastion host from the outside world. You can also set up a VPN to the bastion host.

For more information, see [Deploying critical applications with IBM Cloud MZR - Step 3 Create two security groups](https://test.cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-multi-zone-resiliency#create-subnets-mzr).

### Maintenance security groups for virtual server instances

Because you need to log in to the web, app, and database tiers from the bastion host you must create a maintenance security group. The maintenance security groups allow SSH access from the Bastion host to all of the Virtual Server Instances. The maintenance security group is used for administrative tasks only. It can be enabled on the Virtual Server Instances for routine maintenance work such as patching, viewing logs, and troubleshooting, and can be disabled when these tasks are complete.

For detailed steps, see [Deploying critical applications with IBM Cloud MZR - Step 3 Create two security groups](https://test.cloud.ibm.com/docs/cloud-infrastructure?topic=cloud-infrastructure-multi-zone-resiliency#create-subnets-mzr).
    
Create the inbound and outbound rules for vpc-secure-bastion-sg with these attributes:

Inbound rules for vpc-secure-bastion-sg

| Protocol | Source type | Source | Value |
|-------|-------|-------|-------|
| TCP | Security group | bastion-sg | Ports 22-22 |
| ICMP | Security group  | bastion-sg | Type:8 Code:empty |




Outbound rules for vpc-secure-bastion-sg

| Protocol | Source type | Value |
| ----------- | ----------- | ----------- |
| All | Any | n/a |


When this security group is attached to a virtual server instance, all outbound connections that originate from the virtual server instance are allowed. However, the incoming connections (SSH and ICMP) are restricted only from the bastion host.

## Security Groups for Virtual Server Instances
{: #security-groups-VSI-mzr}

Create a third Security Group that is specific for each of the Virtual Server Instances, such as on the web, app, and database tiers. These Security Groups are always attached to the Virtual Server Instances and permit the HTTP and TCP traffic, to and from the Virtual Server Instances.

Create three Security Groups, one for each tier with inbound and outbound rules as follows:

|Security Group|Direction|Protocol|Source type|Source|Value|
|-----|------|----|----|------|------|
|nginx-web-sg|Inbound|TCP|any|--|ports 80-80|
|||TCP|any|--|ports 443-443|
||Outbound|any|any|--|--|
|tomcat-mt-sg|Inbound|TCP|CIDR Block|vpcname-zone1web-subnet CIDR Block|ports 8080-8080|
|||TCP|CIDR Block|vpcname-zone2web-subnet|ports 8080-8080|
||outgoing|any|any|--|--|
|mysql-db-sg|Inbound|TCP|CIDR Block|vpcname-zone1mt-subnet CIDR Block|ports 3306-3306|
|||TCP|CIDR Block|vpcname-zone2mt-subnet CIDR Block|ports 3306-3306|
|||TCP|CIDR Block|vpcname-zone2db-subnet CIDR Block|ports 3306-3306|
|||TCP|CIDR Block|vpcname-zone2db-subnet CIDR Block|ports 3306-3306|
||outgoing|any|any|--|--|


### Note the following information:

*   Each security group accepts incoming requests from its preceding subnet and on the appropriate port. Security groups are stateful.
*   No restrictions are imposed on outgoing traffic.
*   The database security group is opened not only to the middle tier incoming traffic but also from the database subnets. This arrangement takes care of database replication between the MySQL nodes.
*   Opening the subnets instead of IP addresses, allows incoming traffic from any newly created Virtual Server Instances (either manually or through Auto Scaling groups) that might be added later.
*   When you create Virtual Server Instances, you attach the Security Groups to the Virtual Server Instances
*   Only the maintenance Security Group, vpc-secure-bastion-sg, provide SSH access to the web, Middle Tier, and DB servers. The other Security Groups for the web, mt, and db are needed in the production system and are always enabled.



## Create Bastion Virtual Server Instance
{: #create-Bastion-VSI-mzr}
{: step}

### Create Bastion Host
{: #create-Bastion-host-VSI-mzr}

1.  Log in to the [{{site.data.keyword.cloud}} console](https://cloud.ibm.com/login){: external}.
2.  Select **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > Menu VPC infrastructure > Virtual server instances**.
3.  Click **Create**.
4.  Enter a name for the virtual server instance and:
    1.  Select your resource group,
    2.  Select your region and zone
    3.  Select the Virtual Server Instance as your public virtual server.
    4.  Choose the OS, for example Ubuntu Linux&reg;.
    5.  Select a balanced profile like bx2-2x8.
    6.  Select the SSH keys that you created earlier for the bastion host.
    7. Edit the boot volume and update the resource group if necessary.
    8.  Choose the VPC that you created.
    9.  Select the interface name and edit the interface details. You can select either the default subnet or any one of the subnets you created earlier.
    10.  Attach the Security group _bastion-sg_. Save the details.
5.  Click **Create**.

Post Install steps on Bastion host

*   Log in to the bastion host as **ssh -i ~/.ssh/id\_rsa root@ip-address** from your Mac.
*   Create user and sudo access to the new user if you want to avoid root user for operations.
*   Run **apt upgrade –y** to bring the most recent patches to your kernel. Make sure that the public gateway is enabled on the subnet-hosting bastion server that is acting as NAT GW to download the necessary packages. Restart the host as soobnn as the upgrade is complete.
*   Create private and public keys on the bastion host.
Refer to this [link](https://cloud.ibm.com/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-ssh-keys) for details on how to generate your ssh keys on your Mac or Linux&reg; clients. If you are using Windows&reg;, generate the keys with [PuTTYgen](https://www.ssh.com/academy/ssh/putty/windows/puttygen#running-puttygen). Use the private and public keys for generating the ssh keys for your region.

*   Create a set of SSH keys for access to Virtual Server Instances from the bastion host. Follow the same steps that you used earlier to **Create SSH Keys**. You can name the new keys as _infrakeys_. The public key that was generated on the bastion host in bullet(4) is used in creating these keys. You use this SSH key (infrakeys) while you are creating the Virtual Server Instances for web, app, and database tiers. Creating the ssh key allows logging in to these Virtual Server Instances from the bastion host.

You can create two different bastion hosts one in each Availability zone to have an HA against zone failures and can even shut down one of these hosts. Follow the notes on the [Securely access remote instances with a bastion host](https://cloud.ibm.com/docs/solution-tutorials?topic=solution-tutorials-vpc-secure-management-bastion-server) page for more details on bastion hosts.
{: note}

## Creating Virtual Server Instances for the application
{: #create-VSI-for-App-mzr}
{: step}

Create 6 Virtual Server Instances, for the web tier, middle tier and database tiers, one in each zone, and in the respective subnet.

| Virtual Server Instance | Subnet name | Zone | Attach Security Group |
| ----------- | ----------- | ----------- | ----------- |
| nginx-web1-zone1 | vpcname-zone1web-subnet | Zone 1 | •	nginx-web-sg
||||•	vpc-secure-bastion-sg
| nginx-web2-zone2 | vpcname-zone2web-subnet | Zone 2 | •	nginx-web-sg
||||•	vpc-secure-bastion-sg
 | tomcat-mt1-zone1 | vpcname-zone1mt-subnet | Zone 1 | •	tomcat-mt-sg
||||•	vpc-secure-bastion-sg
 | tomcat-mt2-zone2 | vpcname-zone2mt-subnet | Zone 2 | •	tomcat-mt-sg
||||•	vpc-secure-bastion-sg
| mysql-master1-zone1 | vpcname-zone1db-subnet | Zone 1 | •	mysql-db-sg
||||•	vpc-secure-bastion-sg
 | mysql-master2-zone2  | vpcname-zone2db-subnet | Zone 2 | •	mysql-db-sg
||||•	vpc-secure-bastion-sg

Create each Virtual Server Instance.

1.  Log in to the [{{site.data.keyword.cloud}} console](https://cloud.ibm.com/login){: external}.
2.  Select **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > Menu VPC infrastructure > Virtual server instances**.
3.  Click **Create**.
4.  Enter the name of the Virtual Server Instance
5.  Select the:
    1.  Resource groups and tags, if any.
    2.  Region and the availability zone.
    3.  CPU and RAM profiles for your web, middle tier, and database servers.
    4.  Infrakeys as the SSH key that you created earlier.
    5.  Your operating system. For this example, choose Ubuntu 20.04.
    6.  Your VPC and Network interface.
    7.  A boot-volume for the Virtual Server Instances.
    8.  For MySql nodes, create and attach a secondary data volume of your wanted size.

6.  Repeat steps 1-5 for each of the remaining virtual server instances.


After the Virtual Server Instances are created, follow these steps:

1.  Make sure that the subnet is attached with public gateway to upgrade the servers.
2.  Make sure that _vpc-secure-bastion-sg_ security group is attached to all the Virtual Server Instance’s.
3.  Log in to the bastion host and from there to each of these servers.
4.  Run **apt-upgrade -y** on each server to update the kernel to the most recent patch levels and restart.
5.  Open up firewall ports that use this procedure:
    1.  For web servers ufw, allow from any to any port 80 proto tcp.
    2.  For Middle tier servers ufw, allow from any to any port 8080 proto tcp
    3.  For Database servers ufw, allow from any to any port 3306 proto tcp
    4. For Database servers ufw, port 22 for SSH access on all these servers. 
    
    Change the port numbers for web, Middle tier, and Database servers if you plan to run the services on different ports. While firewall ports are opened as any, they are still bounded by the inbound and outbound rules created earlier with security groups.

6. Validate that the ports are open from one tier to the other and also between availability zones.
 
    From web1 and web2 nc -w5 -z -v  < ipaddress of mt1/mt2 > 8080.
     
    From mt1 and mt2 nc -w5 -z -v -w5 -z -v < ipaddress of mysql1/mysql2 > 3306.
   
    From mysql1 to mysql2 nc -w5 -z -v < ipaddress of mysql1/mysql2 > 3306.

## Install Software on Virtual Server Instances
{: #install-Software-on-VSI-mzr}
{: step}

### Install MySQL software on DB nodes:
{: #install-MySql-software-on-DB-mzr}

1.  Log in to DB nodes through Bastion server.
2.  Make sure that the name resolution works from each of the nodes as update `/etc/hosts` on each mysql node with each other's address.
3.  Install mysql server:

        apt-get install mysql-server mysql-client.
    This command installs the sql server in the default location.

4. Move the sql files to the data volume that you created earlier.
    1. Stop the mysql service.
    
            service mysql stop
    2. Check the new volume on the mysql servers with **lsblk**. For instance, if it is named as vdb as follows:
    
             vdb     254:16   0   100G  0 disk
    3. Partition this volume with parted or fdisk (as an example, with fdisk, choose n, to create a new partition, first sector as 2048, size as +100G, save the changes with w). Verify that the partitions are created with lsblk or fdisk.
    4. Create a Linux&reg; file system on the vdb1 partition.  

            mkfs.xfs /dev/vdb1
    5. Identify the UUID for secondary volume (vdb1) partition.

           blkid | grep /dev/vdb1 

            /dev/vdb1: UUID="a65690bf-0c5e-467b-bb27-7c1558f1ed65" TYPE="xfs" PARTUUID="c4022fcb-01"           
    6. Enter this UUID command in fstab to make the mounts persistent after it restarts. 

            UUID=  a65690bf-0c5e-467b-bb27-7c1558f1ed65 /data xfs defaults 1 2
    7. Create a mount point and mount the created file system.

            mkdir /data

            mount /data
    8. Make sure that mysql is not running and copy the contents from default location to /data.

            service mysql stop
            rsync -av /var/lib/mysql /data
        This command creates directory mysql under /data and copies the contents.
    9. Update the configuration files. 

            a. Update /etc/mysql/mysql.conf.d/mysqld.cn

                1. datadir=/data/mysql,
    
                2. server-id=1, 

                3. log_bin=/data/mysql/mysql-bin.log.

                4. Comment out all bind-addresses.

            b. Update /etc/apparmor.d/tunables/alias

                1. /var/lib/mysql -> /data/mysql/
    10. Restart services.

            systemctl restart apparmor 
            systemctl restart mysql.service 
            systemctl enable mysql.service
5. Repeat all the steps from 3 and 4 in the second mysql node in the second zone. Set the server-id in the cnf file to 2 instead of 1.

### Set up replication between the two nodes

1. Log in to mysql on node1 as **mysql -u root**
     
2. Create user 'repluser'@'%' identified by < passwd >
     
3. Grant replication slave on '\*.*' to 'repluser'@'%'
     
4. Run the sql command SHOW MASTER STATUS and note the file name and position on both nodes and use the values that are returned from the previous steps, into the replication command.

5. Before you issue replication commands, make sure that the port 3306 is open between the two nodes. Run on both the nodes for 2-way replication:

        nc -w5 -z -v <ip address> 3306
        
6. To avoid errors with sha authentication, run this command on both nodes:

        Alter user 'repluser'@'% identified with mysql_native_password by '<password>';

7. On node2 enter the details of node1 in mysql:
    
        stop slave;

        change master to master_host='<node1 ip>',  

        master_user='repluser',  
        master_password=’<passwd>'
        master_log_file=' mysql-bin.000001',  
        master_log_pos= 697;
        start slave;
8. On **node1** enter the details of **node2** as in mysql:

        stop slave
        change master to master_host='<node2 ip>',  
        master_user='repluser',  
        master_password=’<password>',  
        master_log_file=' mysql-bin.000002',  
        master_log_pos= 156;
        start slave

9. Check status of both the nodes.

         show slave status\G
     
10. Verify that the replication is working fine.

    a. Log in to **node1 mysql**

    Create database testdb1;

        Use testdb1

    Create a test table and insert a couple of rows.

         create table testtable1 (owner varchar(20), department varchar(20));

         insert into testtable1 values(‘abc’,’hr’);

         insert into testtable1 values(‘xyz’,’eng’);

         commit;

    b. Log in to **Node 2 mysql**

        Use testdb1

        Select * from testtable1. 
        
    You can now see the table of contents that you created on **node1**.

12. Repeat step 10 and create a table on **node2** and check on **node1** to make sure that 2-way replication is working.

### Install Tomcat on Middle tier nodes
{: #install-Tomcat-on-middle-tier-mzr}

The following steps are a snippet for Tomcat install on the two middle tier servers. You can also install any other middle tier of your choice and make the changes.

1.  Log in to one of the middle tiers Virtual Server Instances you created earlier.
2.  Install tomcat.  

        apt install -y tomcat9 tomcat9-admin

3.  Update **/etc/hosts** with both mysql and middle tier node addresses.
4.  Install jdbc drivers for your operating system from [https://dev.mysql.com/downloads/connector/j/](https://dev.mysql.com/downloads/connector/j/)

     Download and install the deb pkg as **dpkg -i <.deb>**

     Depending on your application, you can download other JAR files like **jstl-1.2** JAR files.

    Alternatively, you can extract and copy the **mysql-connector-java-8.0.23.JAR** and **jstl-1.2** JAR files to $CATALINA\_HOME/lib directory.

5.  Add or update these parameters to the context.xml file /var/lib/tomcat9/webapps/ROOT/META-INF.

          < Context >
          < Resource name="jdbc/[YourDatabaseName]"
          auth="Container"
           type="javax.sql.DataSource"
           username="[DatabaseUsername]"
           password="[DatabasePassword]"
           driverClassName="com.mysql.jdbc.Driver"
           url="jdbc:mysql://<mysql Load Balancer>:3306/<DatabaseName>"/>
           < /Context >
6. Update **web.xml** with appropriate res-name and types.
    You might have to update this context file with the name of the database Load balancer after all the three load balancers are set up.
7. Restart and enable the tomcat service. 
 
          systemctl restart tomcat9.service  
          systemctl enable tomcat9.service

8.  Repeat steps 1-7 on the second middle tier node.

### Install Nginx on web nodes
{: #install-Nginx-on-web-nodes-mzr}

1.  Log in to one of the web Virtual Server Instances you created earlier.
2.  Install nginx.  

          apt install nginx
3.  Update the **/etc/hosts** with both nginx and middle-tier node details.
4.  Restart and enable the nginx service.  

         systemctl restart nginx.service 
         systemctl enable nginx.service
5.  Repeat steps 1-4 on the second web node.

### Install Load Balancers
{: #install-load-balancers-mzr}

Install three Application load balancers.

The web load balancer receives traffic from internet and forwards the requests to the web servers. The web load balancer is your public load balancer. The other two load balancers are private load balancers. The middle tier load balancer receives traffic from web tier and forwards to Apache Tomcat middle tier servers while the database load balancer forwards to mysql databases from middle tier.

The following table lists the subnet connectivity and security groups:

| Type | Load Balancer | Subnets | Security Groups | Comments |
| ----------- | ----------- | ----------- | ----------- | ----------- |
| Public | Web Load Balancer | vpcname-zone1web-subnet, vpcname-zone2web-subnet | nginx-web-sg | This being public ALB then receives http traffic from the internet on port 80/443. |
| Private | Middle Tier Load Balancer | vpcname-zone1web-subnet, vpcname-zone2web-subnet, vpcname-zone1mt-subnet, vpcname-zone2mt-subnet| nginx-web-sg, tomcat-mt-sg | Receives HTTP traffic on port 8080 from web server and forwards the traffic to Middle tier servers |
| Private | Database Load Balancer | vpcname-zone1mt-subnet, vpcname-zone2mt-subnet, vpcname-zone1db-subnet, vpcname-zone2db-subnet|  tomcat-mt-sg, mysql-db-sg | Receives TCP traffic on port 3306 and forwards the traffic to port 3306 |


Install all the three load balancers with the prior configuration. 

For each load balancer, use these steps:

1. Log in to the [{{site.data.keyword.cloud}} console](https://cloud.ibm.com/login){: external}.
2. Click **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > Menu > VPC infrastructure > Load Balancers**.
3. Click **Create**.
4. Enter the information for the load balancer.
5. Select the Type for the Load Balancers.  
   * Set the **Web Load Balancer** to *Public*
   * Set the **Middle Tier** and **Database** load balnacers to *Private*
      
6. Select your resource group, region, and your VPC.
7. Select the checkboxes for the subnets.  
      
   * Assign these subnets to the Web Load Balancer: 
      - vpcname-zone1web-subnet 
      - vpcname-zone2web-subnet
   * Assign these subnets to the Middle Tier Load Balancer: 
      - vpcname-zone1web-subnet 
      - vpcname-zone2web-subnet 
      - vpcname-zone1mt-subnet 
      - vpcname-zone2mt-subnet
   * Assign these subnets to the Database Load Balancer:
      - vpcname-zone1mt-subnet 
      - vpcname-zone2mt-subnet
      - vpcname-zone1db-subnet 
      - vpcname-zone2db-subnet

8. Select the security groups.  
      
   * Assign the *nginx-web-sg* security group to the Web Load Balancer 
   * Assign the *nginx-web-sg* and *tomcat-mt-sg* security groups to the Middle Tier Load Balancer
   * Assign the *tomcat-mt-sg* and *mysql-db-sg* security groups to the Database Load Balancer

9. Configure each load balancer back-end poolsand front-end listeners: 
      
    * Public Web Tier LB (nginx-alb) Back-End Pool:
      - Protocol – HTTP, Method – Round Robin
      - Health Check – Path /
      - HTTP 80
      - Attach Virtual Servers Webserver1 and Webserver2 
    * Public Web Tier LB (nginx-alb) Front-End Listener:
      - HTTP on Port 80 and attach to the back-end pool created. |
    * Private Middle Tier LB (tomcat-alb) Back-End Pool:
      - Protocol – HTTP, 
      - Method – Round Robin, 
      - Health Check – Path /
      - HTTP 8080
      - Attach Virtual Servers, MT1 and MT2 servers 
    * Private Middle Tier LB (tomcat-alb) Front-End Listener:
      - HTTP on Port 8080 and attach to the back-end pool created.
    * Private Database Tier LB (mysql-alb) Back-End Pool:
      - Protocol – TCP 
      - Method – Round Robin 
      - Health Check – TCP 3306 
      - Attach Virtual Servers, mysql1 and mysql2 database servers
    * Private Database Tier LB (mysql-alb) Front-End Listener: 
      - TCP on Port 3036 and attach to the back-end pool created
    
10. Click **Create Load Balancer**.
11. Make a note of the hostname of each load balancer.

Now that the services are up and running on VM’, verify that the ports are open from the VMs to the load balancers.

1.  From external network nc -w5 -z -v < hostname of Web Load Balancer > 80
2.  From web1 and web2 nc -w5 -z -v < hostname of Web Load Balancer > 80
3.  From MT1 and MT2 nc -w5 -z -v < hostname of MT Load Balancer > 8080
4.  From MT1 and MT2 nc -w5 -z -v < hostname of DB Load Balancer >3306
5.  From DB1 and DB2 nc -w5 -z -v  < hostname of DB Load Balancer >3306

### Update Configuration files with your Load Balancer names
{: #update-config-files-lb-mzr}

1.  Update the **/etc/nginx/sites-enabled/default** file on each of the nginx servers to pass the requests to middle-tier ALB:

        proxy_pass http://<tomcat-alb>:8080;

        Restart nginx server on both the nodes.

2.  Update **/var/lib/tomcat9/webapps/ROOT/META-INF/context.xml**, and **jdbc** url with your database load balancer with this url:

         url="jdbc:mysql://<DB load balancer>:3306/<DatabaseName> "

        
3. Restart tomcat service on both the nodes.

These configurations can vary depending on your application. Make the necessary changes.

The configuration of all the components of a three tier architecture that includes the load balancers is now complete.
