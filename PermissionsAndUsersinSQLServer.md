--Read the current status
EXEC sp_configure 'CONTAINED DATABASE AUTHENTICATION'


--To create a Contained database Set the configure to 1
EXEC sp_configure 'CONTAINED DATABASE AUTHENTICATION', 1
GO
RECONFIGURE
GO

--Read using first query
--contained database authentication	0	1	1	1


--Right click on database --In Tasks -- Make offline/Bring Online (When in master database) 

ALTER DATABASE [DBForAzureDF] SET CONTAINMENT = PARTIAL
-- In the DB
CREATE USER azuresql WITH PASSWORD = 'foradf21!'

CREATE USER azuresql WITH PASSWORD = 'foradf21!'

sp_addrolemember 'db_owner', 'azuresql'![image](https://user-images.githubusercontent.com/7129567/137559044-ea737ca5-e008-4c6f-8eee-9c46ef61653f.png)
