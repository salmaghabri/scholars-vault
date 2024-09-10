---
cssclasses:
---

# T-SQL
```SQL
RESTORE DATABASE [AdventureWorks2014] FROM DISK=N'E:\adminBD\Adventure+Works+2014+Full+Database+Backup\AdventureWo rks2014.bak' WITH FILE = 1, MOVE N'AdventureWorks2014_Data' TO N'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\DATA\AdventureWorks2014_Data.mdf', MOVE N'AdventureWorks2014_Log' TO N'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\DATA\AdventureWorks2014_Log.ldf', NOUNLOAD, STATS = 5"
```
	1 . `RESTORE DATABASE [AdventureWorks2014]`: This part of the statement indicates that you want to restore a database named "AdventureWorks2014."
1. `FROM DISK=N'E:\adminBD\Adventure+Works+2014+Full+Database+Backup\AdventureWorks2014.bak'`: This specifies the path to the backup file from which you want to restore the database. In this case, the backup file is located at "E:\adminBD\Adventure+Works+2014+Full+Database+Backup\AdventureWorks2014.bak."
2. `WITH FILE = 1`: This clause specifies the backup file to use ==if there are multiple backup files in the backup set== In this case, it's using the first file in the backup set (FILE = 1).

3. `MOVE N'AdventureWorks2014_Data' TO N'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\DATA\AdventureWorks2014_Data.mdf'`: This part of the statement is used to specify the new file location for the data file (usually with an .mdf extension) of the restored database. It's moving the data file to "C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\DATA\AdventureWorks2014_Data.mdf."
	
4. `MOVE N'AdventureWorks2014_Log' TO N'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\DATA\AdventureWorks2014_Log.ldf'`: This part of the statement specifies the new file location for the transaction log file (usually with an .ldf extension) of the restored database. It's moving the log file to "C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\DATA\AdventureWorks2014_Log.ldf."
    
5. ==`NOUNLOAD`==: This option indicates that the tape drive should not be unloaded after the backup is restored. This is typically used for tape-based backup devices.
    
6. `STATS = 5`: This specifies that progress reporting should occur every 5 percent of  completion during the restore operation. It provides updates on the restore progress.
    

In summary, the SQL statement restores the "AdventureWorks2014" database from a backup file, moves the data and log files to new locations, and includes options for controlling the restore process and reporting progress.

---
Here's a scenario where `NOUNLOAD` could be beneficial:

Suppose you have a series of database backups on a single tape, and you want to restore multiple databases one after the other without removing the tape each time. By using `NOUNLOAD`, you can ensure that the tape remains loaded in the drive, and you can initiate the next restore operation without interruption.


----
in SQL Server, if you don't specify the `FILE` option in a `RESTORE DATABASE` statement, the default value for `FILE` is 1. Therefore, if you omit the `FILE` parameter, SQL Server assumes you are referring to the first file in the backup set (FILE = 1).

---
```
Use [AdventureWorks2014] GO select count(*) from [Person].[Address]
```
1. `USE [AdventureWorks2014]`: This statement instructs SQL Server to switch to the "AdventureWorks2014" database as the current database. In SQL Server, you can have multiple databases on a server,
 
2. `GO`: The `GO` statement is not a SQL statement itself but a batch separator used in SQL Server Management Studio (SSMS) and some other SQL tools. It separates batches of SQL statements and sends them to the server one batch at a time. It is often used to group multiple SQL statements into a single unit of work.

3. `SELECT COUNT(*) FROM [Person].[Address]`: This is a SQL query that counts the number of rows in the "[Person].[Address]" and returns the total count of rows 
----
```
BACKUP DATABASE [AdventureWorks2014] TO DISK = N'E:\adminBD\DBA\AdventureWorksBackup' WITH NOFORMAT, NOINIT, NAME = N'AdventureWorks2014-Full-Database-Backup’, SKIP, NOREWIND, NOUNLOAD, STATS = 1
```
1. `BACKUP DATABASE [AdventureWorks2014]`: back up the "AdventureWorks2014" database.
    
2. `TO DISK = N'E:\adminBD\DBA\AdventureWorksBackup'`:  the path and filename where the backup will be written.
    
3. `WITH NOFORMAT, NOINIT`: These are backup options.
    
    - `NOFORMAT`: This option specifies that the backup should not be in any specific format. It means that the backup will be compatible with different versions of SQL Server.
    - `NOINIT`: This option specifies that the backup should not overwrite the existing backup set on the specified backup media. If you want to append to an existing backup set, you would use `NOINIT`.
4. `NAME = N'AdventureWorks2014-Full-Database-Backup'`: This specifies a name for the backup set. The name you provide here is a label for the backup set and can be useful for identifying it later.
    
5. `SKIP, NOREWIND, NOUNLOAD`: These options are related to the tape device, but they are used for disk backups as well.
    
    - `SKIP`: This option indicates that the tape (or disk) drive should skip any empty tape blocks.
    - `NOREWIND`: This option specifies that the tape (or disk) should not be rewound after the backup is completed.
    - `NOUNLOAD`: Similar to the previous explanation, this option instructs the tape (or disk) drive not to unload the media after the backup is done. This is typically used when you plan to perform additional backup operations or other tasks with the same media.
6. `STATS = 1`: This option specifies that progress reporting should occur every 1 percent of completion during the backup operation. It provides updates on the backup progress.
---
# Recovery Models
## Full recovery model
- db keeps a detailed record of all transactions in the transaction log.
- the highest level of data recoverability.
- It allows for complete point-in-time recovery, meaning you can restore the database to any specific moment in time as long as you have the necessary transaction log backups. 
-  typically used for critical systems where data loss is unacceptable.
## Simple recovery model 
- the transaction log is truncated (emptied) regularly, and only minimal log information is retained for crash recovery. You can only recover to the most recent full or differential backup.
- suitable for databases where a small amount of data loss is acceptable, and you don't need point-in-time recovery.
## Bulk-Logged Recovery Model:
- transaction logs retain a summary of bulk data modification operations. Instead of recording individual changes for each row, it logs the fact that a bulk operation occurred, reducing the volume of log data generated.
- While it provides some benefits in terms of performance for certain operations, it doesn't support fine-grained point-in-time recovery to the same extent as the Full Recovery Model.
- It's used in scenarios where bulk data imports are common.
## difference between the Full Recovery Model and the Bulk-Logged Recovery Model 
is the level of detail recorded in the transaction log
**Scenario:** Suppose you have a SQL Server database with the Bulk-Logged Recovery Model, and you want to insert a large number of records into a table.

**Bulk-Logged Recovery Model:**

1. You initiate the bulk insert operation, which inserts thousands of records into the table.
    
2. In the Bulk-Logged Recovery Model, SQL Server logs this bulk insert operation differently compared to the Full Recovery Model.
    
    - Instead of logging every individual record insert separately, it records a summary of the operation. Specifically, it logs the extents that were modified by the bulk insert operation.
    - The transaction log entry for the bulk insert operation might include information like which pages were affected and the extent of the changes made.
    - This minimizes the amount of log data generated during the bulk insert because it doesn't log every individual record. However, it does record that a bulk operation occurred.
3. If you perform a transaction log backup after the bulk insert operation, it captures this summarized information, allowing you to recover to the point of the transaction log backup.
    

**Full Recovery Model:**

1. You initiate the same bulk insert operation in a database with the Full Recovery Model.
2. In the Full Recovery Model, SQL Server logs the bulk insert operation differently compared to the Bulk-Logged Recovery Model.
    - It logs every individual record insert as a separate transaction log entry.
    - Each record inserted is recorded in the transaction log, allowing for precise point-in-time recovery.
3. If you perform a transaction log backup after the bulk insert operation, it captures all the individual insert transactions, enabling precise point-in-time recovery to any specific moment in time.
    

**Comparison:**

- In the Bulk-Logged Recovery Model, the bulk insert operation generates less log data because it summarizes the operation by logging the extents affected.
- In the Full Recovery Model, the same bulk insert operation generates more log data because it logs each individual record insert separately.

**Recovery Implications:**

- In the Bulk-Logged Recovery Model, while it's more efficient in terms of log space during bulk operations, there's a ==potential risk of data loss for changes made during the bulk operation if a failure occurs before the next transaction log backup==
- In the Full Recovery Model, you have precise point-in-time recovery, but the trade-off is larger transaction logs and potentially longer backup times.

It's important to choose the appropriate recovery model based on your specific requirements for data recoverability and performance in your database environment.
# Backup models
## Full Backup
- A full backup is a complete copy of the entire database, including both data and transaction logs at the time the backup was taken.
- Full backups are the foundation of most backup strategies.
- You can restore a database using only a full backup, but you may also need transaction log backups for point-in-time recovery.
## Differential Backup
- captures all changes made to the database **since the last full backup**.
- It reduces the size and time required for backups compared to full backups.
- When ==restoring==, you need **the most recent full backup** and **the most recent differential backup** to bring the database up to a specific point in time.
- Unlike regular transaction log backups, which capture changes since the last log backup, differential log backups capture changes since the last full backup.
## Transaction Log Backups
- capture changes to the transaction log since the last transaction log or full backup.
- crucial for point-in-time recovery in Full and Bulk-Logged Recovery Models.
- You need a **series of transaction log backups**, in addition **to a full backup**, to ==restore== a database to a specific moment in time.
## Differential Backup vs  Transaction Log Backups
### Diffrential backup 
1. **Scope:** A differential backup captures all changes made to a database since the last full backup. It includes all data modifications, such as inserts, updates, and deletes, that have occurred since the last full backup.
    
2. **Size:** Differential backups tend to be larger than transaction log backups because they contain a snapshot of the changes made to the entire database since the last full backup.
    
3. **Frequency:** Differential backups are typically taken less frequently than transaction log backups. They are often used as an intermediary step between full backups to reduce backup time and storage requirements.
    
4. **Restore Process:** To restore a database using a differential backup, you start with the last full backup and apply the most recent differential backup. This brings the database up to a more recent point in time than the full backup.
    
5. **Point-in-Time Recovery:** Differential backups do not provide fine-grained point-in-time recovery. They bring the database to a specific point in time based on the last full backup and the most recent differential backup.
6. **usecase**: suitable when you need to reduce the time and storage required for backups compared to full backups, and you are willing to accept some data loss since they capture changes since the last full backup.
### Transaction log backup
1. **Scope:** A transaction log backup captures all changes made to a database since the last transaction log or full backup. It includes the individual transactions that have occurred, making it suitable for precise point-in-time recovery.
    
2. **Size:** Transaction log backups are typically smaller than differential backups because they contain only the log records of individual transactions.
    
3. **Frequency:** Transaction log backups are taken more frequently, often at regular intervals (e.g., every hour or every few minutes). This allows for fine-grained recovery and minimizes data loss.
    
4. **Restore Process:** To restore a database using transaction log backups, you start with the last full backup and apply all transaction log backups made since that full backup. This allows you to recover the database to any specific point in time within the range covered by the log backups.
    
5. **Point-in-Time Recovery:** Transaction log backups provide precise point-in-time recovery. You can restore a database to any moment in time covered by the transaction log backups.
6. **usecase**: crucial for scenarios where precise point-in-time recovery is necessary, such as financial systems, where even small data loss can be unacceptable.
# Relationship Between Recovery and Backup Models
- In ==the Full Recovery Model==, you can perform full backups, differential backups, and transaction log backups to achieve point-in-time recovery.
- In ==the Simple Recovery Model==, you typically only perform full backups because **transaction log backups are not allowed.**
- In ==the Bulk-Logged Recovery Model==, you can use full backups, differential backups, and transaction log backups, but the ability to recover to a specific point in time may be limited due to minimally logged operations.
- The choice of recovery model and backup strategy depends on factors such as the criticality of the data, the desired recovery objectives, and performance considerations
# how it typically works
# Recovery vs No Recovery
# system databases
❖ Master ❖ Model ❖ Msdb ❖ TempDB ❖ Resource

