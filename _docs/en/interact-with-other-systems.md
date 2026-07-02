---
title: Interact with Other Systems
permalink: /docs/en/interact-with-other-systems
key: docs-interact-with-other-systems
---
API Test Base interacts with other systems through related Java libraries. When these libraries are not open source (and so can't be bundled with API Test Base), they need to be copied to the corresponding folder under `<ATB_DATA_DIR>/lib`, as shown below.

## Databases
To use a database request to interact with a database, such as Oracle or SQL Server, copy the related JDBC driver as shown below. The JDBC URL is used in the database endpoint to connect to the database.

API Test Base bundles the H2 and PostgreSQL JDBC drivers, so no driver needs to be copied for those two databases.

### Oracle
The Oracle JDBC driver comes with an Oracle installation, for example under `<Oracle_Install_Dir>/jdbc/lib`. Copy `ojdbc8.jar` (it supports Java 8 and later) to the `<ATB_DATA_DIR>/lib/jdbc/oracle` folder.

Sample JDBC URLs:

    jdbc:oracle:thin:@myhost:1521:mySID
    jdbc:oracle:thin:@myhost:1521/myServiceName

### SQL Server
The SQL Server JDBC driver can be downloaded from the Microsoft [download page](https://learn.microsoft.com/en-us/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server){:target="_blank"}. After unzipping it, copy the driver jar that matches your Java runtime to the `<ATB_DATA_DIR>/lib/jdbc/sqlserver` folder. For Java 8, use the `jre8` build, for example `mssql-jdbc-13.4.0.jre8.jar`.

The jar alone is sufficient for most cases, where SQL Server Authentication is used.

To enable API Test Base to use Windows Authentication to connect to SQL Server, also copy the matching `mssql-jdbc_auth-<version>.<arch>.dll` (for example `mssql-jdbc_auth-13.4.0.x64.dll`) from the unzipped folder to `<ATB_DATA_DIR>/lib/jdbc/sqlserver`, and add `<ATB_DATA_DIR>/lib/jdbc/sqlserver` to the `PATH` environment variable of the Windows OS where API Test Base is running. The unzipped folder contains an `x64` and an `x86` version of the DLL; use `x64` for 64-bit Windows or `x86` for 32-bit Windows. The DLL version must match the driver jar version.

Sample JDBC URLs:

    //  When using SQL Server Authentication
    jdbc:sqlserver://myhost:1433;database=myDatabase

    //  When using Windows Authentication
    jdbc:sqlserver://myhost:1433;database=myDatabase;integratedSecurity=true

## IBM MQ
To use an MQ request to interact with IBM MQ, download the `MQ allclient` jar from Maven Central to `<ATB_DATA_DIR>/lib/mq`.

For example, version 9.3.2.1 jar can be downloaded at

    https://repo1.maven.org/maven2/com/ibm/mq/com.ibm.mq.allclient/9.3.2.1/com.ibm.mq.allclient-9.3.2.1.jar.

Latest version jar is recommended.

To interact with multiple versions of MQ at the same time, use the jar of the highest version.

## IBM ACE
To use an ACE request to interact with ACE, copy the `<ACE_Install_Dir>/common/classes/IntegrationAPI.jar` to the `<ATB_DATA_DIR>/lib/ace` folder.

## JMS
A JMS request supports the ActiveMQ and Solace providers. The ActiveMQ jars are bundled with API Test Base, so nothing needs to be copied to use ActiveMQ. To use the Solace provider, copy the Solace jars as shown below.

### Solace
To use a JMS request with the Solace provider to interact with a Solace broker, download the Solace jars from Maven Central to `<ATB_DATA_DIR>/lib/solace`.

For example, version 10.19.0 jars can be downloaded at

    https://repo1.maven.org/maven2/com/solacesystems/sol-jcsmp/10.19.0/sol-jcsmp-10.19.0.jar
    https://repo1.maven.org/maven2/com/solacesystems/sol-jms/10.19.0/sol-jms-10.19.0.jar

Latest version jars are recommended.