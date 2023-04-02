---
title: Interact with Other Systems
permalink: /docs/en/interact-with-other-systems
key: docs-interact-with-other-systems
---
API Test Base interacts with other systems through related Java libraries. When the libraries are not open source, they need to be copied to corresponding folders under `<APITestBase_Data>/lib`.

## Databases
To use Database Test Step to interact with a database, such as Oracle or SQL Server, copy related JDBC drivers like below. JDBC URL is used in database endpoint for connecting to the database.

### Oracle
Oracle JDBC driver comes with Oracle installation. For example, C:\oraclexe\app\oracle\product\11.2.0\server\jdbc\lib. Copy `ojdbc8.jar` or `ojdbc6.jar` to `<APITestBase_Data>/lib/jdbc/oracle` folder.

Sample JDBC URLs:
 
    jdbc:oracle:thin:@myhost:1521:mySID
    jdbc:oracle:thin:@myhost:1521/myServiceName

### SQL Server    
SQL Server JDBC driver can be downloaded from Microsoft [web site](https://msdn.microsoft.com/en-us/library/mt484311(v=sql.110).aspx). After unzipping it, copy `sqljdbc41.jar` to `<APITestBase_Data>/lib/jdbc/sqlserver` folder.

To enable API Test Base to use Windows authentication to connect to SQL Server, also copy `sqljdbc_auth.dll` from the unzipped folder to `<APITestBase_Data>/lib/jdbc/sqlserver`, and add `<APITestBase_Data>/lib/jdbc/sqlserver` to the `PATH` environment variable of the Windows OS where API Test Base is running.

Notice that there are two `sqljdbc_auth.dll` files in the unzipped folder. Use the one from folder x64 or x86 for 64 or 32 bit Windows OS.

Sample JDBC URLs:

    //  When using SQL Server authentication
    jdbc:sqlserver://myhost:1433;database=myDatabase;

    //  When using Windows authentication
    jdbc:sqlserver://myhost:1433;database=myDatabase;integratedSecurity=true

## IBM MQ
To use MQ Test Step to interact with IBM MQ, copy below jars to `<APITestBase_Data>/lib/mq`.

    //  For MQ 8.0.0.x
    com.ibm.mq.headers.jar
    com.ibm.mq.jar
    com.ibm.mq.jmqi.jar
    com.ibm.mq.pcf.jar
   
These jars can be found at `<MQ_Install_Dir>/java/lib`.

To interact with multiple versions of MQ at the same time, use the jars of the highest version.

## IIB
To use IIB Test Step to interact with IIB (only v10 is supported), copy IBM jars to corresponding folders.

Copy below jars to `<APITestBase_Data>/lib/iib/v100`.

    IntegrationAPI.jar
    jetty-io.jar
    jetty-util.jar
    websocket-api.jar
    websocket-client.jar
    websocket-common.jar

These jars can be found at `<IIB_Install_Dir>/common/classes` and `<IIB_Install_Dir>/common/jetty/lib`.

## Solace
To use JMS Test Step with Solace provider to interact with Solace broker, download following jar files from Maven Central

    https://repo1.maven.org/maven2/com/solacesystems/sol-jcsmp/10.12.0/sol-jcsmp-10.12.0.jar
    https://repo1.maven.org/maven2/com/solacesystems/sol-jms/10.12.0/sol-jms-10.12.0.jar

and place the jar files under `<APITestBase_Data>/lib/solace`.