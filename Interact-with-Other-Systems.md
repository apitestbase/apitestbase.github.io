Iron Test interacts with other systems through related Java libraries. When the libraries are not open source, they need to be copied to corresponding folders under `<IronTest_Home>/lib`.

## Databases
To use Database Test Step to interact with a database, such as Oracle or SQL Server, as part of a test case, copy related JDBC drivers like below. At Iron Test runtime, JDBC URL is used in database endpoint for connecting to the database.

### Oracle
Oracle JDBC driver comes with Oracle installation. For example, C:\oraclexe\app\oracle\product\11.2.0\server\jdbc\lib. Copy `ojdbc6.jar` to `<IronTest_Home>/lib/jdbc/oracle` folder.

Sample JDBC URLs:
 
    jdbc:oracle:thin:@myhost:1521:mySID
    jdbc:oracle:thin:@myhost:1521/myServiceName

### SQL Server    
SQL Server JDBC driver can be downloaded from Microsoft [web site](https://msdn.microsoft.com/en-us/library/mt484311(v=sql.110).aspx). Copy `sqljdbc41.jar` to `<IronTest_Home>/lib/jdbc/sqlserver` folder.

Sample JDBC URL:

    jdbc:sqlserver://myhost:1433;database=myDatabase;

## IBM MQ
To use MQ Test Step to interact with IBM MQ as part of a test case, copy below jars to `<IronTest_Home>/lib/mq`.

    //  For MQ 8.0.0.x
    com.ibm.mq.headers.jar
    com.ibm.mq.jar
    com.ibm.mq.jmqi.jar
    com.ibm.mq.pcf.jar

    //  For MQ 7.5.0.x
    com.ibm.mq.commonservices.jar
    com.ibm.mq.headers.jar
    com.ibm.mq.jar
    com.ibm.mq.jmqi.jar
    com.ibm.mq.pcf.jar
    connector.jar
    
These jars can be found at `<MQ_Install_Dir>/java/lib`.

To interact with multiple versions of MQ at the same time, use the jars of the highest version.

## IIB
To use IIB Test Step to interact with IIB as part of a test case, copy IBM jars to corresponding folders.

For IIB 10, copy below jars to `<IronTest_Home>/lib/iib/v100`.

    IntegrationAPI.jar
    jetty-io.jar
    jetty-util.jar
    websocket-api.jar
    websocket-client.jar
    websocket-common.jar

IIB 10 jars can be found at `<IIB_Install_Dir>/common/classes` and `<IIB_Install_Dir>/common/jetty/lib`.

For IIB 9, first copy IBM MQ (either 7.5 or 8.0) jars as described above, then copy below jars to `<IronTest_Home>/lib/iib/v90`.

    ibmjsseprovider2.jar
    ConfigManagerProxy.jar

IIB 9 jars can be found at `<IIB_Install_Dir>/classes` and `<IIB_Install_Dir>/jre17/lib`.

Normally you only want to interact with IIB 10.0 OR 9.0. In case you want to interact with BOTH at the same time, copy all related jars as described above.