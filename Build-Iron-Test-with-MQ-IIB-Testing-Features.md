Notice: currently only IIB 9.0.0.x is supported.

Download the latest Iron Test release from [here](https://github.com/zheng-wang/irontest/releases) to your local machine. Extract it, and cd to the project directory (in which there is README.md).

Run below Maven commands to install MQ and IIB libraries to your local Maven repository 

    mvn install:install-file -Dfile="<MQ_Home>/java/lib/com.ibm.mq.jar" -DgroupId=com.ibm -DartifactId=com.ibm.mq -Dversion=<MQ_Version> -Dpackaging=jar
    mvn install:install-file -Dfile="<MQ_Home>/java/lib/com.ibm.mq.jmqi.jar" -DgroupId=com.ibm -DartifactId=com.ibm.mq.jmqi -Dversion=<MQ_Version> -Dpackaging=jar
    mvn install:install-file -Dfile="<MQ_Home>/java/lib/com.ibm.mq.commonservices.jar" -DgroupId=com.ibm -DartifactId=com.ibm.mq.commonservices -Dversion=<MQ_Version> -Dpackaging=jar
    mvn install:install-file -Dfile="<MQ_Home>/java/lib/com.ibm.mq.pcf.jar" -DgroupId=com.ibm -DartifactId=com.ibm.mq.pcf -Dversion=<MQ_Version> -Dpackaging=jar
    mvn install:install-file -Dfile="<MQ_Home>/java/lib/com.ibm.mq.headers.jar" -DgroupId=com.ibm -DartifactId=com.ibm.mq.headers -Dversion=<MQ_Version> -Dpackaging=jar
    mvn install:install-file -Dfile="<MQ_Home>/java/lib/connector.jar" -DgroupId=javax.resource -DartifactId=connector -Dversion=1.3.0 -Dpackaging=jar
    mvn install:install-file -Dfile="<IIB_Home>/classes/ConfigManagerProxy.jar" -DgroupId=com.ibm -DartifactId=ConfigManagerProxy -Dversion=<IIB_Version> -Dpackaging=jar
    mvn install:install-file -Dfile="<IIB_Home>/jre17/lib/ibmjsseprovider2.jar" -DgroupId=com.ibm -DartifactId=ibmjsseprovider2 -Dversion=<IIB_Version> -Dpackaging=jar

Notice that the `-Dfile` parameter is for Maven to find the jar file to install. If there is no MQ/IIB on the machine where you build Iron Test, change the -Dfile value to corresponding file path. You may also need to replace / in the file path with \ on a Windows machine.

Check MQ/IIB versions in `irontest-mqiib/pom.xml`. If your MQ or IIB version falls outside the range, modify the POM. I haven't tested that version, but Iron Test might work with it. Refer to [this doc](http://maven.apache.org/enforcer/enforcer-rules/versionRanges.html) for more information about Maven version ranges.

    <mq.version>...</mq.version>
    <iib.version>...</iib.version>
 
Run below Maven command

    mvn clean package -pl irontest-mqiib -am -P prod

An `irontest-mqiib/dist` folder is created containing the files and folders for [deployment](https://github.com/zheng-wang/irontest#deploy).