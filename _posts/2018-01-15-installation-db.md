---
layout: page
title: "Database Installation and Configuration"
category: installation
date: 2018-01-05 15:17:55
order: 2
---
### Install and Congigure MySQL

Keter uses MySQL as our the default database. You can install [MySQL](https://dev.mysql.com/downloads/mysql/) community edition.  


**Create Database**   

Connect to the MySQL server and use MySQL command to create database. 
``` 
mysql> create database keter;
```  

**Execute DB Scripts**  

##### Notes:
- You will want to change [Keter_HOME]/sql/data-mysql.sql script so that  the organization matches your companies name. Please replace ‘keter’ with your company name as outlined below.  The default keter login name and password is “KeterAdmin”/”KeterAdmin”.  


Connect to the MySQL server and switch to the **keter** DB and execute DB scripts **schema-mysql.sql**, **data-mysql.sql** to create the database tables and populate data. These 2 SQL scripts can be found in the **sql** folder of Keter installation package.

You can execute the script with the MySQL source command to execute script. Pls replace **yoursqlpath** to path of the SQL folder location.

``` 
mysql> use keter ;
mysql> source yoursqlpath\schema-mysql.sql;
mysql> source yoursqlpath\data-mysql.sql;
```  

In MySQL 8.0, the default authentication plugin has changed from **mysql_native_password** to **caching_sha2_password**, and the 'root'@'localhost' administrative account uses caching_sha2_password by default. Please execute below script to use the previous default authentication plugin (**mysql_native_password**) in order to work with Keter.

``` 
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

**Notes:**   

You might need to change **data-mysql.sql** script for the your organization name. Please  replace 'keter' to your company name occur in the below script. The default keter login name and password is "KeterAdmin"/"KeterAdmin". Please don't change this username and password in the SQL file.

``` 
INSERT INTO organization (company_name, active) VALUES ('keter', true);
INSERT INTO user (user_name, password, role, active, organization_id) 
VALUES ('keterAdmin', '12d9f16eff974ae7730525b0dda228e2', 'ADMIN', true, (SELECT id FROM organization where company_name = 'keter'));
```  

### Install and Configure DB2

In addition to MySQL, Keter also supports DB2 v10.x+.  Please see the following documentation on how to install DB2 within your environment. 

**Create DB Database**   

Login into the DB2 server and execute below DB2 commands to create database. 
``` 
db2 create database KETER automatic storage yes using codeset UTF-8 territory US pagesize 32768
db2 connect to KETER
db2 CREATE BUFFERPOOL BP32K IMMEDIATE ALL DBPARTITIONNUMS SIZE AUTOMATIC NUMBLOCKPAGES 0 PAGESIZE 32 K
``` 

**Execute DB Scripts**  


Start DB2 server and switch to the **keter** DB and execute DB scripts **schema-db2.sql**, **data-db2.sql** to create the database tables and populate data. These 2 SQL scripts can be found in the **sql** folder of Keter installation package.


``` 
db2 connect to KETER
db2 -stvf schema-db2.sql
db2 -stvf data-db2.sql
db2 connect reset
```  

You can use DB2 client tool (Data Studio) to verify database tables are created and populated with data.

![][db2]   

##### Notes:
- You will want to change [Keter_HOME]/sql/data-db2.sql script so that   the organization matches your companies name. Please replace ‘Keter’ with your company name as outlined below.  The default keter login name and password is “KeterAdmin”/”KeterAdmin”.  



**Download DB2 Driver**  
You can download JDBC for DB2 version 11.1 from below link. 
[https://www-01.ibm.com/marketing/iwm/iwm/web/download.do?source=swg-idsdjs&pageType=urx&S_PKG=dl](https://www-01.ibm.com/marketing/iwm/iwm/web/download.do?source=swg-idsdjs&pageType=urx&S_PKG=dl) 

After downloading, extract the **db2jcc4.jar** from driver package and copy it to the lib folder of Keter installation package. Please note that you can also get the **db2jcc4.jar** from your DB2 server.

![][db2driver]  

[db2]: ../images/install/dbtable.png 
[db2driver]: ../images/install/db2driver.png 
