To use Database Test Step to invoke a database, such as Oracle or SQL Server, as part of a test case, you need to make corresponding JDBC driver(s) available to Iron Test.

## JDBC Drivers
Oracle JDBC driver comes with Oracle installation. For example, C:\oraclexe\app\oracle\product\11.2.0\server\jdbc\lib. Copy ojdbc6.jar to `<IronTest_Home>/lib/jdbc/oracle` folder.

SQL Server JDBC driver can be downloaded from Microsoft [web site](https://msdn.microsoft.com/en-us/library/mt484311(v=sql.110).aspx). Copy sqljdbc41.jar to `<IronTest_Home>/lib/jdbc/sqlserver` folder.
