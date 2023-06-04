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
To use MQ Test Step to interact with IBM MQ, download the `MQ allclient` jar from Maven Central to `<APITestBase_Data>/lib/mq`.

For example, version 9.3.2.1 jar can be downloaded at https://repo1.maven.org/maven2/com/ibm/mq/com.ibm.mq.allclient/9.3.2.1/com.ibm.mq.allclient-9.3.2.1.jar.

Latest version jar is recommended.

To interact with multiple versions of MQ at the same time, use the jar of the highest version.

## IBM ACE
To use ACE Test Step to interact with ACE, copy the `<ACE_Install_Dir>/common/classes/IntegrationAPI.jar` to the `<APITestBase_Data>/lib/ace` folder.

## Solace
To use JMS Test Step with Solace provider to interact with Solace broker, download the Solace jars from Maven Central to `<APITestBase_Data>/lib/solace`.

For example, version 10.19.0 jars can be downloaded at

    https://repo1.maven.org/maven2/com/solacesystems/sol-jcsmp/10.19.0/sol-jcsmp-10.19.0.jar
    https://repo1.maven.org/maven2/com/solacesystems/sol-jms/10.19.0/sol-jms-10.19.0.jar

Latest version jars are recommended.