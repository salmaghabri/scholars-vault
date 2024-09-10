- When a user or an application tries to connect to SQL Server, it's the login credentials that are checked for authentication.
- Windows authentification / SQL server authentification
-  ``` CREATE LOGIN [DESKTOP-ICEQ7B9\SQLTest] FROM WINDOWS WITH DEFAULT_DATABASE=[AdventureWorks2014]
- ## fixed server roles in SQL Server
### 1. **Serveradmin: settings shut down any endpoint**
-  highest level of administrative privileges on the SQL Server instance.
- They can alter server-wide configuration settings, shut down the server, and manage endpoints
- Members of this role can also create and alter any endpoint.

### 2. **Securityadmin: logins**

- manage logins and their properties.
- They can create, alter, and drop logins. They also have the ability to reset passwords for SQL Server logins.
- `Securityadmin` role members can manage server-level security configurations.

### 3. **Processadmin: connection**

- Members of the `Processadmin` role can manage processes running in an instance of SQL Server.
- They can kill processes that are running in an instance, which can be useful for managing resource-intensive or long-running queries.

### 4. **Setupadmin: linked server**

- Members of the `Setupadmin` role can manage linked servers and their definitions.
- They can create, alter, and drop linked servers, which are used to establish communication between SQL Server instances.

### 5. **Bulkadmin: bulk operation **

- Members of the `Bulkadmin` role can execute bulk insert operations.
- Bulk insert operations are typically used to efficiently import large amounts of data into SQL Server from external sources.
### 6. **Diskadmin:**
- Members of the `Diskadmin` role can manage disk files.
- They can add or remove data or log files from a database.

### 7. **Alter Resources:**
- The `Alter Resources` permission allows users to alter resource governor settings.
- Resource Governor is a feature in SQL Server that enables you to manage SQL Server workload and system resource consumption.

### 8. **Dbcreator:**Alter Any Database, Create Any Databas**

- Members of the `Dbcreator` role can create, alter, and drop databases.
- They have the authority to create new databases as well as modify the structure of existing databases.

### 10. **Sysadmin: any activity**

- Members of the `Sysadmin` role have the highest level of access within SQL Server.
- They can perform any activity on the server, including configuring server options, managing logins, and accessing any database.

### 11. **Public: default view any db**
- By default, all logins are a member of the `Public` role.
- Additionally, they have the view any database permission, allowing
### syntax
ALTER SERVER ROLE [sysadmin] ADD MEMBER [DESKTOP-ICEQ7B9\SQLTest] ALTER SERVER ROLE [sysadmin] DROP MEMBER [DESKTOP-ICEQ7B9\SQLTest

# user-defined server roles
CREATE SERVER ROLE [myServerRole1] ALTER SERVER ROLE [myServerRole1] ADD MEMBER [DESKTOP-ICEQ7B9\SQLTest] GRANT ALTER ANY LOGIN TO [myServerRole1
# create db user account
CREATE USER [SQLTest] FOR LOGIN [DESKTOP-ICEQ7B9\SQLTest] WITH DEFAULT_SCHEMA=[dbo]
# Protect objects from being modified
```
DENY ALTER ON [Production].[Culture] TO [SQLTest] DENY CONTROL ON [Production].[Culture] TO [SQLTest]

DENY DELETE ON [Production].[Culture] TO [SQLTest] 

DENY INSERT ON [Production].[Culture] TO [SQLTest]


DENY TAKE OWNERSHIP ON [Production].[Culture] TO [SQLTest] DENY UPDATE ON [Production].[Culture] TO [SQLTest]
```
# database-level roles
### 1. **db_owner:** all

- Members of the `db_owner` role have full control over a specific database.
- They can perform all activities within the database, including creating, altering, and dropping objects, as well as managing permissions.

### 2. **db_securityadmin:** roles

- `db_securityadmin` members can manage database roles, including altering existing roles, creating new roles, and viewing their definitions.
- They are responsible for configuring and managing security within the database.

### 3. **db_accessadmin:**  manager user access

- `db_accessadmin` members can alter any user within the database and grant or revoke connect permissions.
- They have the authority to manage user access, allowing or denying connections to the database.

### 4. **db_backupoperator:**

- `db_backupoperator` members are tasked with performing backup operations within the database.
- They can backup the entire database, transaction logs, and execute checkpoints to optimize database recovery.

### 5. **db_ddladmin:**

- `db_ddladmin` members have the ability to run Data Definition Language (DDL) commands within the database.
- They can create, modify, and drop schema-bound objects, such as tables and views, controlling the structure of the database.
### 6. **db_datawriter:**

- Members of the `db_datawriter` role have the authority to modify data within the database.
- They are granted permissions to perform insert, update, and delete operations on tables and other data objects in the database.

### 7. **db_denydatawriter:**

- `db_denydatawriter` members are explicitly denied the permission to insert, update, or delete data within the database.
- This role restricts users from making any changes to the data, ensuring data integrity in specific scenarios.

### 8. **db_datareader:**

- Members of the `db_datareader` role have the privilege to read data from all tables and views within the database.
- They are allowed to perform select operations, enabling them to view the data stored in the database.

### 9. **db_denydatareader:**

- `db_denydatareader` members are explicitly denied the permission to read data from tables and views within the database.
- Users in this role are restricted from performing select operations, ensuring data confidentiality in certain situations.

### 10. **public:**

- The `public` role in a database is a default database role to which all users are automatically added.
- By default, members of the `public` role have no specific permissions assigned, except for certain database-level permissions such as viewing any column master key definition and select permission on system tables.
- Users in the `public` role inherit permissions granted to the role but can also have additional permissions assigned individually.