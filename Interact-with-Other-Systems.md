Iron Test interacts with other systems through related Java libraries. When the libraries are not open source, they need to be copied to corresponding folders under `<IronTest_Home>/lib`.

## Databases
To use Database Test Step to interact with a database, such as Oracle or SQL Server, as part of a test case, copy related JDBC drivers like below. At Iron Test runtime, JDBC URL is used in database endpoint for connecting to the database.

### Oracle
Oracle JDBC driver comes with Oracle installation. For example, C:\oraclexe\app\oracle\product\11.2.0\server\jdbc\lib. Copy ojdbc6.jar to `<IronTest_Home>/lib/jdbc/oracle` folder.

Sample JDBC URLs:
 
    jdbc:oracle:thin:@myhost:1521:mySID
    jdbc:oracle:thin:@myhost:1521/myServiceName

### SQL Server    
SQL Server JDBC driver can be downloaded from Microsoft [web site](https://msdn.microsoft.com/en-us/library/mt484311(v=sql.110).aspx). Copy sqljdbc41.jar to `<IronTest_Home>/lib/jdbc/sqlserver` folder.

Sample JDBC URL:

    jdbc:sqlserver://myhost:1433;database=myDatabase;

## WebSphere MQ
