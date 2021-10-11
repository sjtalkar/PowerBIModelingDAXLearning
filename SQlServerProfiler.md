**
READ MORE ABOUT SQL SERVER PROFILER**

https://docs.microsoft.com/en-us/sql/tools/sql-server-profiler/sql-server-profiler?view=sql-server-ver15&WT.mc_id=5003466.

We have the following two options in the way we use SQL Server Profiler:

We can register SQL Server Profiler as an external tool. Here, we can open SQL Server Profiler directly from Power BI Desktop from the External Tools tab. With this method, SQL Server Profiler automatically connects to our Power BI data model via the Power BI Diagnostic Port.
READ MORE ABOUT HOW TO REGISTER SQL SERVER PROFILER IN POWER BI:

https://www.biinsight.com/quick-tips-registering-sql-server-profiler-as-an-external-tool/.

We can open SQL Server Profiler and manually connect to our Power BI data model through the Power BI Desktop Diagnostic Port. To do so, we must find the Diagnostic Port number first, then connect to the data model using the port number.
LEARN MORE ABOUT THE DIFFERENT WAYS TO FIND THE POWER BI DESKTOP DIAGNOSTIC PORT:

https://www.biinsight.com/four-different-ways-to-find-your-power-bi-desktop-local-port-number/.

We use the first option as it is simpler than the second option. We have to trace the following events within the SQL Server Profiler:
