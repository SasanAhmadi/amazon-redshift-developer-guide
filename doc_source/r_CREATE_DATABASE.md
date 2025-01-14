# CREATE DATABASE<a name="r_CREATE_DATABASE"></a>

Creates a new database\.

You can't run CREATE DATABASE within a transaction block \(BEGIN \.\.\. END\)\. For more information about transactions, see [Serializable isolation](c_serial_isolation.md)\. 

## Syntax<a name="r_CREATE_DATABASE-synopsis"></a>

```
CREATE DATABASE database_name [ WITH ]
[ OWNER [=] db_owner ]
[ CONNECTION LIMIT { limit | UNLIMITED } ]
```

## Parameters<a name="r_CREATE_DATABASE-parameters"></a>

 *database\_name*   
Name of the new database\. For more information about valid names, see [Names and identifiers](r_names.md)\.

WITH   
Optional keyword\.

OWNER   
Specifies a database owner\.

=   
Optional character\.

 *db\_owner*   
Username for the database owner\.

CONNECTION LIMIT \{ *limit* \| UNLIMITED \}   
The maximum number of database connections users are permitted to have open concurrently\. The limit isn't enforced for superusers\. Use the UNLIMITED keyword to permit the maximum number of concurrent connections\.  A limit on the number of connections for each user might also apply\. For more information, see [CREATE USER](r_CREATE_USER.md)\. The default is UNLIMITED\. To view current connections, query the [STV\_SESSIONS](r_STV_SESSIONS.md) system view\.  
If both user and database connection limits apply, an unused connection slot must be available that is within both limits when a user attempts to connect\.

## Syntax for using CREATE DATABASE with a datashare<a name="r_CREATE_DATABASE-datashare-synopsis"></a>

The following syntax describes the CREATE DATABASE command used to create databases from a datashare\.

```
CREATE DATABASE database_name
FROM DATASHARE datashare_name OF NAMESPACE namespace_guid
```

### Parameters for using CREATE DATABASE with a datashare<a name="r_CREATE_DATABASE-parameters-datashare"></a>

FROM DATASHARE   
A keyword that indicates where the datashare is located\.

 *datashare\_name*   
The name of the datashare that the consumer database is created on\.

 NAMESPACE *namespace\_guid*   
A value that specifies the namespace that the datashare uses\.

## CREATE DATABASE limits<a name="r_CREATE_DATABASE-create-database-limits"></a>

Amazon Redshift enforces these limits for databases:
+ Maximum of 60 user\-defined databases per cluster\.
+ Maximum of 127 bytes for a database name\.
+ Cannot be a reserved word\.

## Examples<a name="r_CREATE_DATABASE-examples"></a>

The following example creates a database named TICKIT and gives ownership to the user DWUSER: 

```
create database tickit
with owner dwuser;
```

Query the PG\_DATABASE\_INFO catalog table to view details about databases\. 

```
select datname, datdba, datconnlimit 
from pg_database_info
where datdba > 1;

 datname     | datdba | datconnlimit
-------------+--------+-------------
 admin       |    100 | UNLIMITED   
 reports     |    100 | 100         
 tickit      |    100 | 100
```

The following example creates the database Salesdb from the datashare SalesShare\.

```
CREATE DATABASE Sales_db FROM DATASHARE SalesShare OF NAMESPACE '13b8833d-17c6-4f16-8fe4-1a018f5ed00d';
```